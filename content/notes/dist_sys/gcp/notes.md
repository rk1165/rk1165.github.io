+++
title = 'Google Cloud Notes'
date = 2024-02-11

+++

### Google Cloud Fundamentals

- GC resource hierarchy
  - Organization Node (L4)
  - Folder (L3)
  - Project (L2)
  - Resources (L1) (VM, Cloud Storage Buckets)
- Policies can be defined at L4, L3, L2 and on some L1
- Policies are inhertied downwards
- Project ID, Name and Number
- PID is globally unique
- PName are user created and can be changed
- PNumber : Immutable, used internally
- Resource Manager Tool: gathers all resources associated with an account
- "Who" is also called a principal.
- "Can do what part" of an IAM policy is defined by Role. An IAM role is a collection of permissions.
- When you grant a role to a principal, you grant all the permissions that are granted to the role
- Basic Role, Predefined Roles and Custom Roles
- Instance Admin Role - Pre Defined role
- Service account allows say Compute Engine to save Data that no one else can access
- With Cloud Identity, organizations can define policies and manage their users and groups using the Google Admin Console.
- Admins can log in and manage Google Cloud resources using the same user names and passwords they already used in existing Active Directory or LDAP systems
- VPC networks :
  - using firewall rules to restrict access to instances
  - create static routes to forward traffic to specific destination
- Google VPC networks are global and can have subnets in any GC region WW.
- Cloud Router lets other networks and Google VPC exchange route information over the VPN using BGP.
- Routes tell VM instances and the VPC network how to send traffic from an instance to a destination, either inside the network or outside Google Cloud. Each VPC network comes with some default routes to route traffic among its subnets and send traffic from eligible instances to the internet.
- You can ping `mynet-eu-vm's` internal IP because of the `allow-custom` firewall rule.
- firewall rule that allows to ping `mynet-eu-vm's` external IP : `mynetwork-allow-icmp`
- GC Storage options
  - Cloud Storage : Object package storage examples include Video, pictures and audio. BLOB storage. Organized into buckets.
    - Storage classes : Standard Storage, Nearline Storage (once per month updating), Coldline Storage (read or modify once every 90 days), Archive Storage (access less than a year)
  - Cloud SQL : managed RDBs. Storing Customer orders
  - Cloud Spanner : Built in HA, High IO.
  - Firestore : Flexible, Horizontally Scalable, NoSQL.
  - Cloud Bigtable : NoSQL big data database service. IOTs.
- BigQuery : on the edge between data storage and data processing
- `gcloud storage buckets create -l $LOCATION gs://$DEVSHELL_PROJECT_ID`
- `gcloud storage cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png`
- `gcloud storage cp my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png`
- `gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png`
- A Pod is the smallest unit in K8s that you create or deploy
- It represents a running process on cluster.
- Generally one container per pod
- `kubectl get pods`
- `kubectl get service`
- Service is a logical abstraction
- `kubectl scale`
- `kubectl get deployments`
- `kubectl get services`
