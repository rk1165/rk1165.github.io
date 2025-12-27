+++
title = 'HFT Engine'
date = 2025-11-21

+++

Designing a High-Frequency Trading (HFT) engine is fundamentally different from designing a web server or a distributed crawler. In web systems, we optimize for **throughput** (how many requests per second). In HFT, we optimize for **latency** (how fast is one request).

We are no longer measuring in milliseconds ($ms$); we are measuring in **microseconds ($\mu s$)** and **nanoseconds ($ns$)**.

Here is the step-by-step architecture of a system where the speed of light is your biggest enemy.

---

### 1. The High-Level Philosophy: The "Hot Path"

In HFT, you do not build a distributed system with microservices for the core trading loop. Network hops are too slow. Serialization is too slow.

**The Architecture:** A Monolithic, Single-Threaded Event Loop pinned to a specific CPU core.

- **Why Single Threaded?** Multithreading requires locks and context switches. A context switch takes $\approx 1-3 \mu s$. That is an eternity. We want **Zero-Lock** architecture.
- **Why Monolithic?** You want the Market Data Handler, Strategy Engine, and Order Entry Gateway in the same memory space to avoid passing data over sockets.

---

### 2. The Input: Market Data Handler (The Feeder)

The exchange sends data via UDP Multicast. Your system needs to ingest this.

**Step A: Kernel Bypass (The Hardware Trick)**
Standard networking involves the Network Card (NIC) copying data to the OS Kernel, which copies it to User Space. This triple-copy is too slow.

- **Solution:** Use **Kernel Bypass** (technologies like **DPDK**, **OpenOnload**, or **Solarflare**).
- The application reads memory directly from the Network Card's ring buffer, skipping the OS entirely.

**Step B: The Protocol Parsers**
Exchanges don't send JSON. They send compact binary formats (like **ITCH** or **OUCH**).

- **Zero-Copy Parsing:** Do not create new objects for incoming messages. Cast the raw memory buffer pointer to a C++ struct and read the values directly.

---

### 3. The State: Limit Order Book (LOB)

The engine must reconstruct the market state (Bids and Asks) from the data stream.

**The Challenge:** Dynamic memory allocation (`malloc`/`new`) is non-deterministic. It can take $10ns$ or $100ns$. You cannot afford uncertainty.
**The Solution:** **Object Pooling & Pre-allocation.**

- At startup, pre-allocate an array of 1,000,000 Order objects.
- When an order arrives, pop an object from the pool. When it closes, push it back. Never ask the OS for memory during trading hours.

**Data Structure:**
Avoid HashMaps (collisions cause jitter). Use **Flat Arrays** or intrusive Doubly Linked Lists for price levels to ensure cache locality (keeping data in the CPU L1/L2 cache).

---

### 4. The Strategy Engine (The Brain)

This is the logic: "If Apple stock drops below $150, Buy."

**The Execution Model:**
The strategy runs on the same thread as the Market Data Handler.

1.  Read Packet.
2.  Update Book.
3.  **Run Strategy Check.**
4.  Send Order.
5.  _Loop._

**Optimization: Branch Prediction**
CPUs try to guess which way an `if` statement will go. If they guess wrong, they flush the pipeline (costing cycles).

- **Code Style:** Write branchless code where possible. Optimize for the "common path" so the CPU predictor is usually right.

---

### 5. The Output: Order Entry Gateway

Once the strategy decides to buy, you must send a TCP/IP packet to the exchange.

**The Risk Check (The Safety Valve):**
Before the packet leaves, you **must** run pre-trade risk checks (e.g., "Do not exceed $1M exposure").

- **Problem:** This adds latency.
- **Solution:** Pre-calculate risk limits. Do not calculate `CurrentExposure + NewOrderCost` on the fly. Maintain a `RemainingBudget` variable and just check `if (OrderCost < RemainingBudget)`.

---

### 6. Major Problems & Mitigation Strategies

In HFT, "tail latency" (the 99th percentile slowness) is the enemy.

#### Problem A: The "Garbage Collection" Pause

If you use Java or Python, the Garbage Collector (GC) might pause your app for 10ms to clean memory. In that time, the market has moved, and you lost money.

- **Solution 1:** Use **C++** or **Rust**. No GC.
- **Solution 2:** If using Java, use **Zero-GC** libraries (e.g., LMAX Disruptor) and pre-allocate everything so the GC has nothing to collect.

#### Problem B: The "Straggler" Packet (Jitter)

Sometimes a TCP packet just gets stuck or a switch buffer fills up.

- **Solution: Colocation.** You physically place your server in the same data center as the exchange (e.g., NASDAQ's data center in Carteret, NJ). You pay for the length of the fiber optic cable to be exactly the same as your competitors.

#### Problem C: "Split Brain" / State Drift

Your internal book says the price is $100.05, but the exchange says $100.06$. You send an order thinking you are getting a deal, but you aren't.

- **Solution:** **Sequence Number Reconciliation.** If the exchange data stream skips a sequence number (Packet 1, Packet 3...), you must immediately stop trading and request a "Snapshot" to reset your state.

#### Problem D: The "Fat Finger" (Runaway Algo)

Your code has a bug and enters an infinite loop of buying.

- **Solution:** **Hardware Kill Switch.** Often, a separate FPGA or gateway sits in front of your box. If it detects > X orders per second, it physically cuts the connection.

---

### 7. Hardware Acceleration (The Final Frontier)

Software is sometimes too slow.

- **FPGA (Field-Programmable Gate Array):** Engineers write logic in hardware (Verilog/VHDL). The strategy is baked into the silicon.
- **Latency:** Software takes $\approx 5-10 \mu s$. FPGAs can trade in $\approx 700-800 ns$ (nanoseconds).

### Summary of the Stack

| Layer              | Technology                                                    |
| :----------------- | :------------------------------------------------------------ |
| **Language**       | C++20 or Rust                                                 |
| **OS**             | Linux (RedHat/CentOS) with Real-Time Kernel patches           |
| **Network**        | Solarflare NICs with OpenOnload (Kernel Bypass)               |
| **CPU**            | Overclocked i9 or Xeon (Pinned Cores, isolated from OS tasks) |
| **Data Structure** | Ring Buffers (LMAX Disruptor pattern)                         |

### A Next Step for You

The **LMAX Disruptor** is a famous concurrency pattern invented for this exact purpose (High-Frequency Trading in Java). It handles millions of events per second with near-zero locking.

Would you like me to explain the **Ring Buffer / Disruptor pattern** conceptually, or perhaps show a C++ snippet of how to build a **Lock-Free Queue**?
