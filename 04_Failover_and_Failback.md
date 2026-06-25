# Failover and Failback in SAP HANA System Replication

## Introduction

Failover and Failback are essential operations in SAP HANA High Availability (HA) and Disaster Recovery (DR) environments. These processes ensure that business operations continue even when the primary database becomes unavailable.

A properly planned failover strategy minimizes downtime, while a controlled failback process restores the original production environment after the issue has been resolved.

---

# What is Failover?

Failover is the process of switching SAP operations from the Primary HANA database to the Secondary HANA database when the primary system becomes unavailable.

Failover can be:

- Automatic
- Manual

The objective is to restore business services as quickly as possible.

---

# Automatic Failover

Automatic Failover is managed by cluster software such as:

- Pacemaker
- SUSE High Availability Extension (HAE)
- Red Hat High Availability

The cluster continuously monitors the health of the primary node.

If the primary node fails:

1. Failure is detected.
2. Secondary database is promoted.
3. Virtual IP moves to the secondary node.
4. SAP application reconnects automatically.
5. Business users continue working.

---

# Manual Failover

Manual Failover is performed by the SAP BASIS administrator during:

- Planned maintenance
- Hardware replacement
- Operating System upgrades
- Database upgrades
- Disaster Recovery testing

Typical steps include:

1. Verify replication status.
2. Stop application activity if required.
3. Promote Secondary to Primary.
4. Redirect SAP application servers.
5. Validate database connectivity.
6. Resume business operations.

---

# Automatic Failover Workflow

```text
Primary Server Running
          │
          ▼
 Hardware Failure
          │
          ▼
 Cluster Detects Failure
          │
          ▼
 Secondary Promoted
          │
          ▼
 Virtual IP Switch
          │
          ▼
 SAP Application Reconnects
          │
          ▼
 Users Continue Working
```

---

# Manual Failover Workflow

```text
Maintenance Planned
        │
        ▼
Check Replication Status
        │
        ▼
Promote Secondary Database
        │
        ▼
Update Application Connections
        │
        ▼
Verify SAP Services
        │
        ▼
Business Operations Resume
```

---

# What is Failback?

Failback is the process of restoring the repaired original Primary server and returning production operations back to it.

Failback is usually scheduled during a maintenance window to avoid business disruption.

---

# Failback Procedure

1. Repair the original Primary server.
2. Ensure SAP HANA is healthy.
3. Register it as the Secondary system.
4. Synchronize all database changes.
5. Verify replication status.
6. Schedule maintenance.
7. Promote the original Primary back to production.
8. Validate SAP applications.
9. Resume normal operations.

---

# Failover vs Failback

| Failover | Failback |
|----------|----------|
| Switches production to Secondary | Restores production to Original Primary |
| Happens during failure | Happens after recovery |
| Can be automatic or manual | Usually manual |
| Focuses on business continuity | Focuses on restoring normal architecture |

---

# Role of Pacemaker

Pacemaker is responsible for:

- Monitoring node health
- Detecting failures
- Managing cluster resources
- Performing automatic failover
- Managing Virtual IP
- Restarting SAP services

---

# Virtual IP (VIP)

A Virtual IP allows SAP application servers to connect without changing configuration after failover.

Instead of connecting directly to a server hostname, applications connect to the Virtual IP.

During failover:

```text
Primary Server Down

Virtual IP

↓

Automatically Moves

↓

Secondary Server
```

Users continue working without changing connection settings.

---

# Health Checks Before Failover

SAP BASIS administrators should verify:

- HANA services are running
- Replication status is ACTIVE
- No replication backlog
- Network connectivity
- Storage availability
- Cluster status
- SAP application status

---

# Health Checks After Failover

After failover:

- Verify HANA role
- Check SAP application connectivity
- Validate business transactions
- Monitor performance
- Confirm user logins
- Review system logs

---

# Common Failover Issues

| Issue | Possible Cause |
|--------|----------------|
| Replication not synchronized | Network delay |
| Failover failed | Cluster misconfiguration |
| Users cannot connect | Virtual IP not switched |
| SAP application unavailable | Application server not restarted |
| Database inaccessible | HANA services stopped |

---

# Best Practices

- Test failover regularly.
- Monitor replication continuously.
- Maintain consistent HANA revisions.
- Document failover procedures.
- Schedule DR drills.
- Monitor cluster logs.
- Verify backups before maintenance.

---

# Real-Time Production Scenario

## Scenario

The Primary HANA server suddenly became unavailable due to a hardware motherboard failure.

### Action Taken

- Cluster detected the failure.
- Secondary HANA database was promoted.
- Virtual IP switched automatically.
- SAP application servers reconnected.
- User transactions resumed successfully.

### Resolution

The failed server was repaired and reconfigured as the Secondary node.

Replication was re-established, and a planned failback was performed during the next maintenance window.

---

# Interview Questions

## What is Failover?

Failover is the process of switching production from the Primary database to the Secondary database after a failure.

---

## What is Failback?

Failback is restoring production to the repaired original Primary database.

---

## What is the difference between Manual and Automatic Failover?

Manual Failover is initiated by the SAP BASIS administrator.

Automatic Failover is managed by cluster software such as Pacemaker.

---

## Why is Virtual IP used?

It allows SAP applications to reconnect automatically without changing configuration after failover.

---

## Which software commonly manages SAP HANA clusters?

- Pacemaker
- SUSE High Availability Extension
- Red Hat High Availability

---

# Summary

Failover and Failback are critical operations in SAP HANA High Availability and Disaster Recovery environments. A well-designed cluster, synchronized replication, and regular testing ensure minimal downtime and uninterrupted business operations.
