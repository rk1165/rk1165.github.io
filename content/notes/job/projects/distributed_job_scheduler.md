+++
title = 'Distributed Job Scheduler'
date = 2025-11-21

+++

Designing a distributed job scheduler (like a custom version of Airflow, Celery, or Quartz) is a fantastic project because it forces you to solve the "Exactly-Once" delivery problem—the holy grail of distributed systems.

Here is the step-by-step design, moving from the data model to the specific concurrency controls needed to prevent chaos.

---

### 1\. High-Level Architecture

The system needs to separate the "planning" (When should this run?) from the "execution" (Run this code now).

**The Core Components:**

1.  **Job Repository (SQL DB):** Stores metadata (Schedule: "Every Monday at 9am", Payload: "Email User X").
2.  **The Scheduler (The Poller):** Scans the DB for jobs that are due (`next_run_time <= now()`).
3.  **Message Queue (The Buffer):** Decouples the scheduler from the workers (e.g., RabbitMQ, Kafka, SQS).
4.  **Worker Nodes:** Stateless services that pull tasks from the queue and execute the actual logic.
5.  **Coordinator (Zookeeper/Redis):** Handles leader election and distributed locking.

---

### 2\. Data Storage & The "Job" Model

You need a relational database here (Postgres/MySQL). Consistency is more important than raw speed; you cannot afford "eventual consistency" where a job disappears or runs twice.

**The Table Structure:**

- `job_id`: Unique UUID.
- `cron_expression`: e.g., `0 9 * * 1`.
- `next_run_at`: Timestamp of when it should run next.
- `status`: `PENDING` | `QUEUED` | `RUNNING` | `COMPLETED` | `FAILED`.
- `lock_owner`: ID of the scheduler instance handling this job (for crash recovery).

---

### 3\. The Scheduling Loop (The Concurrency Trap)

This is the hardest part. You likely have multiple Scheduler nodes for high availability. If two schedulers query the DB at the same time, they both see the same job is due. They both push it to the queue. The job runs twice.

**Solution A: Row-Level Locking (Postgres `FOR UPDATE`)**
This allows one transaction to lock rows while reading them.

```sql
BEGIN;
-- Select jobs due for execution and lock them so no one else can grab them
SELECT * FROM jobs
WHERE next_run_at <= NOW() AND status = 'PENDING'
FOR UPDATE SKIP LOCKED
LIMIT 100;

-- Immediately update them to 'QUEUED'
UPDATE jobs SET status = 'QUEUED' WHERE id IN (...);
COMMIT;
```

- `SKIP LOCKED` is the magic keyword. If Scheduler A locks rows 1-10, Scheduler B will automatically skip to rows 11-20. No waiting, no race conditions.

**Solution B: Distributed Leasing (Redis)**
If you aren't using a DB with row locking, you use Redis `SETNX` (Set if Not Exists) to acquire a lock on a specific `job_id` for a short duration (e.g., 30 seconds).

---

### 4\. Dispatching to the Queue

Once the Scheduler has successfully locked the job and set status to `QUEUED`, it pushes a message to the Message Queue.

**The Payload:**

```json
{
  "job_id": "12345",
  "task_type": "SEND_EMAIL",
  "payload": { "user_id": 50, "template": "welcome" },
  "retry_count": 0
}
```

> **Why a Queue?** If you have a sudden spike of 10,000 jobs due at 9:00 AM, the Scheduler can push them to the queue in milliseconds. The Workers can then process them at their own pace without crashing the system (Backpressure).

---

### 5\. Worker Execution & Reliability

The Worker pulls the message. It does **not** mark the job as "Completed" yet.

**The Heartbeat Mechanism:**
If a Worker picks up a job and then the server catches fire, the job is lost in limbo (Status is `RUNNING`, but no one is running it).

1.  **Worker:** While processing, sends a "heartbeat" to Redis every 10 seconds: `SET job_123_health "alive" EXPIRE 15`.
2.  **Recovery Monitor:** A background process checks for jobs in `RUNNING` state where the Redis key has expired. It resets those jobs to `PENDING` to be retried.

---

### 6\. Handling "Cron" Logic (The Timing Wheel)

Querying a database every second (`SELECT * WHERE time < now`) is inefficient at massive scale.

**Optimization: Hierarchical Timing Wheels**

[Image of timing wheel data structure]

Instead of polling the DB, the Scheduler loads upcoming jobs into a specialized memory structure (a ring buffer).

- The wheel rotates once per second.
- The current slot contains a list of all jobs due _right now_.
- This changes complexity from $O(N)$ (scanning the DB) to $O(1)$ (checking the current slot).
- _Note:_ This is how Kafka and Linux kernel timers work internally.

---

### 7\. Step-by-Step Workflow Summary

1.  **User** submits a job via API. It is saved to **DB** with `status: PENDING`.
2.  **Scheduler (Master)** runs a loop. It finds jobs where `next_run_at <= NOW`.
3.  **Scheduler** performs a CAS (Compare-And-Swap) or Row Lock to flip status to `QUEUED`.
4.  **Scheduler** pushes job ID to **RabbitMQ/Kafka**.
5.  **Worker** pulls message, sets DB status to `RUNNING`.
6.  **Worker** executes code.
    - _If Success:_ Updates DB to `COMPLETED`. Updates `next_run_at` for the next interval (if repeating).
    - _If Fail:_ Updates DB to `FAILED` or increments retry counter and re-queues.
7.  **Dead Job Monitor** scans for `RUNNING` jobs with no recent heartbeat and resets them.

### A Next Step for You

Understanding **Idempotency** is the final piece of this puzzle. Even with this design, network errors can cause a job to run twice (e.g., Worker finishes, but the "Complete" network packet is lost, so the system retries it).

Would you like me to explain **how to write Idempotent code** so that even if your scheduler triggers the "Charge Credit Card" job twice, the user is only charged once?
