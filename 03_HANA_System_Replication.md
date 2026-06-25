# SAP HANA System Replication (HSR)

## Introduction

SAP HANA System Replication (HSR) is a built-in high availability and disaster recovery feature that continuously replicates data from a primary SAP HANA database to a secondary SAP HANA database.

It enables organizations to minimize downtime and data loss by maintaining a synchronized copy of the production database on another server or data center.

SAP HANA System Replication is one of the most widely used disaster recovery solutions in enterprise SAP landscapes.

---

# Objectives of HANA System Replication

The primary objectives of HSR are:

- Ensure continuous database availability
- Minimize business downtime
- Reduce data loss
- Support Disaster Recovery (DR)
- Improve Business Continuity
- Enable automatic or manual failover

---

# HSR Architecture

```
                SAP Application

                       │

                SAP HANA Primary

                       │
             System Replication
        (Sync / SyncMem / Async)

                       │

              SAP HANA Secondary

                       │

            Disaster Recovery Site
```

---

# Components

## Primary System

- Production SAP HANA Database
- Accepts all read and write operations
- Replicates transaction logs to the secondary system

---

## Secondary System

- Receives replicated data
- Remains in standby mode
- Can be promoted to Primary during failover

---

# Replication Modes

## 1. Synchronous (SYNC)

- Zero or near-zero data loss
- Highest data protection
- Primary waits until Secondary confirms the transaction

### Advantages

- Maximum protection
- Suitable for nearby data centers

### Disadvantages

- Higher network latency
- Slight impact on transaction response time

---

## 2. Synchronous in Memory (SYNCMEM)

- Transaction is confirmed after data reaches secondary memory
- Faster than full synchronous mode
- Very commonly used in production environments

### Advantages

- Good performance
- Minimal data loss
- Lower latency

---

## 3. Asynchronous (ASYNC)

- Primary system does not wait for Secondary confirmation
- Suitable for geographically distant DR sites

### Advantages

- Better performance
- Supports long-distance replication

### Disadvantages

- Possible data loss during unexpected failures

---

# Operation Modes

## Log Replay

- Most commonly used
- Secondary continuously replays transaction logs
- Fast failover
- Recommended by SAP

---

## Delta Datashipping

- Transfers changed data pages
- Lower CPU usage
- Slower recovery

---

## Log Replay Read Enabled

- Allows reporting queries on Secondary
- Useful for analytics
- Reduces workload on Primary

---

# Replication Workflow

```
User Transaction

       │

Primary Database

       │

Transaction Log Generated

       │

Replication Service

       │

Secondary Database

       │

Log Replay

       │

Standby Ready
```

---

# Important Commands

## Check HANA Processes

```bash
HDB info
```

---

## Check Replication Status

```bash
systemReplicationStatus.py
```

---

## Display Replication Information

```bash
hdbnsutil -sr_state
```

---

## Register Secondary System

```bash
hdbnsutil -sr_register
```

---

## Enable System Replication

```bash
hdbnsutil -sr_enable
```

---

## Disable Replication

```bash
hdbnsutil -sr_disable
```

---

# Monitoring Replication

SAP BASIS administrators should regularly monitor:

- Replication status
- Log shipping delay
- Network latency
- Synchronization status
- Primary/Secondary roles
- Backup status
- HANA alerts

---

# Failover Process

```
Primary Failure

      │

Cluster Detects Failure

      │

Secondary Promoted

      │

Applications Reconnect

      │

Business Continues
```

---

# Failback Process

Once the failed Primary server is repaired:

- Reinstall or recover HANA if required
- Register the old Primary as Secondary
- Synchronize data
- Perform planned failback during maintenance window

---

# Best Practices

- Use dedicated replication network
- Monitor replication daily
- Perform regular failover testing
- Keep HANA revisions consistent
- Schedule regular backups
- Monitor disk usage
- Verify replication after maintenance

---

# Common Monitoring Tools

| Tool | Purpose |
|------|---------|
| HANA Cockpit | Database Monitoring |
| HANA Studio | Administration |
| SAP Host Agent | Host Monitoring |
| HDB info | Process Status |
| systemReplicationStatus.py | Replication Status |
| hdbnsutil | Replication Configuration |

---

# Real-Time Production Scenario

**Issue:**

The primary SAP HANA server unexpectedly crashed due to a hardware failure.

**Action Taken:**

- Verified the replication status on the secondary server.
- Confirmed that all services were synchronized.
- Promoted the secondary server to the primary role.
- Redirected SAP application servers to the new primary database.
- Validated user logins and business transactions.
- After repairing the original server, it was re-registered as the secondary system.

**Result:**

Business operations resumed with minimal downtime and no significant data loss.

---

# Interview Questions

### What is SAP HANA System Replication?

A built-in SAP HANA feature that replicates data from a primary database to a secondary database for High Availability and Disaster Recovery.

---

### What are the replication modes?

- Synchronous
- Synchronous in Memory (SyncMem)
- Asynchronous

---

### Which replication mode is commonly used in production?

Synchronous in Memory (SyncMem), as it provides a balance between performance and data protection.

---

### Which command checks replication status?

```bash
systemReplicationStatus.py
```

---

### Which command displays the replication role?

```bash
hdbnsutil -sr_state
```

---

### What is Log Replay mode?

A replication operation mode where transaction logs are continuously applied on the secondary database, enabling fast recovery.

---

# Summary

SAP HANA System Replication is a critical feature for achieving High Availability and Disaster Recovery. By maintaining synchronized database copies, monitoring replication health, and regularly testing failover procedures, SAP BASIS administrators can ensure business continuity and minimize service disruptions.
