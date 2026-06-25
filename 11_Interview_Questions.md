# SAP HANA High Availability & Disaster Recovery Interview Questions

## Introduction

This document contains frequently asked SAP BASIS interview questions related to High Availability (HA), Disaster Recovery (DR), SAP HANA System Replication (HSR), Backup, Recovery, Failover, and Business Continuity.

---

# Basic Questions

## 1. What is High Availability (HA)?

High Availability (HA) is a system design that ensures SAP applications remain operational with minimal downtime by eliminating single points of failure through redundancy, clustering, and automatic failover.

---

## 2. What is Disaster Recovery (DR)?

Disaster Recovery (DR) is the process of restoring SAP systems and business operations after a major failure or disaster using backups, replication, and recovery procedures.

---

## 3. What is the difference between HA and DR?

| High Availability | Disaster Recovery |
|-------------------|------------------|
| Minimizes downtime | Restores services after disaster |
| Local protection | Remote site protection |
| Automatic failover | Planned recovery |
| Focuses on availability | Focuses on business continuity |

---

## 4. What is SAP HANA System Replication?

SAP HANA System Replication (HSR) is a native SAP HANA feature that continuously replicates data from a Primary database to a Secondary database for High Availability and Disaster Recovery.

---

## 5. What are the replication modes?

- Synchronous (SYNC)
- Synchronous in Memory (SYNCMEM)
- Asynchronous (ASYNC)

---

## 6. Which replication mode is commonly used?

Synchronous in Memory (SYNCMEM) is commonly used because it provides a good balance between performance and data protection.

---

## 7. What are the operation modes in HSR?

- Log Replay
- Delta Datashipping
- Log Replay Read Enabled

---

## 8. Which operation mode is recommended?

Log Replay is the most commonly recommended mode because it provides faster failover and better recovery performance.

---

# Backup & Recovery

## 9. What are the types of SAP HANA backups?

- Data Backup
- Log Backup
- Catalog Backup

---

## 10. Why are Log Backups important?

They capture all committed transactions after the last data backup and enable Point-in-Time Recovery (PITR).

---

## 11. What is Point-in-Time Recovery?

Point-in-Time Recovery restores the database to a specific timestamp using a full data backup and transaction log backups.

---

## 12. What is the purpose of the Backup Catalog?

The Backup Catalog stores metadata about all backups, making it easier to manage and restore backups.

---

## 13. Can a Tenant Database be recovered independently?

Yes. In an MDC environment, Tenant Databases can be recovered independently without affecting other tenants.

---

# Failover & Failback

## 14. What is Failover?

Failover is the process of switching production from the Primary database to the Secondary database after a failure.

---

## 15. What is Failback?

Failback is the process of returning production to the repaired original Primary database after it has been restored.

---

## 16. What is Automatic Failover?

Automatic Failover is performed by cluster software such as Pacemaker without manual intervention.

---

## 17. What is Manual Failover?

Manual Failover is initiated by the SAP BASIS administrator during maintenance or disaster recovery activities.

---

## 18. Why is a Virtual IP used?

A Virtual IP allows SAP application servers to reconnect automatically after failover without changing connection settings.

---

# Monitoring

## 19. How do you monitor HANA System Replication?

Common methods include:

- `systemReplicationStatus.py`
- `hdbnsutil -sr_state`
- SAP HANA Cockpit
- SAP HANA Studio

---

## 20. Which command displays the replication role?

```bash
hdbnsutil -sr_state
```

---

## 21. Which command checks replication health?

```bash
systemReplicationStatus.py
```

---

## 22. Which command displays running HANA services?

```bash
HDB info
```

---

# RPO & RTO

## 23. What is RPO?

Recovery Point Objective (RPO) defines the maximum acceptable amount of data loss measured in time.

---

## 24. What is RTO?

Recovery Time Objective (RTO) defines the maximum acceptable downtime required to restore business services.

---

## 25. How can RPO be reduced?

- Frequent backups
- Log backups
- HANA System Replication
- Continuous monitoring

---

## 26. How can RTO be reduced?

- High Availability architecture
- Automated failover
- Disaster Recovery planning
- Regular DR testing

---

# Scenario-Based Questions

## 27. What will you do if HANA System Replication stops?

- Check replication status.
- Verify network connectivity.
- Review HANA logs.
- Restart replication if required.
- Validate synchronization.

---

## 28. What will you do if the Primary HANA server crashes?

- Verify cluster status.
- Promote the Secondary server.
- Validate SAP application connectivity.
- Monitor business transactions.
- Investigate the root cause.

---

## 29. What will you do if Log Backups fail?

- Verify backup destination.
- Check disk space.
- Review backup logs.
- Restart backup services.
- Confirm successful execution.

---

## 30. What should be verified after Failover?

- Database status
- SAP instance status
- User logins
- Business transactions
- Replication status
- System logs

---

# Production Questions

## 31. Have you participated in a DR Drill?

**Sample Answer:**

> Yes. I participated in planned Disaster Recovery testing where we verified backups, checked SAP HANA System Replication, executed failover, validated SAP applications, performed business transaction testing, and completed failback successfully.

---

## 32. What health checks do you perform before Failover?

- Backup verification
- Replication status
- Cluster health
- Network connectivity
- Storage availability
- SAP service status

---

## 33. What health checks do you perform after Failover?

- HANA database status
- SAP application connectivity
- User logins
- Business process validation
- System log review
- Replication verification

---

## 34. Why should DR testing be performed regularly?

Regular DR testing validates recovery procedures, identifies configuration gaps, and ensures the organization can meet defined RPO and RTO targets.

---

## 35. What are common causes of SAP HANA downtime?

- Hardware failures
- Storage issues
- Network failures
- Database corruption
- Human error
- Operating system failures

---

# Advanced Questions

## 36. What is a Split-Brain scenario?

A Split-Brain scenario occurs when both nodes believe they are the Primary database, leading to data inconsistency. Cluster quorum and fencing mechanisms help prevent this.

---

## 37. Why is fencing important in a cluster?

Fencing isolates a failed or unresponsive node to prevent data corruption and ensure only one active Primary node.

---

## 38. What is quorum?

Quorum is the voting mechanism used by cluster software to determine which nodes remain active and to avoid Split-Brain conditions.

---

## 39. What is Business Continuity?

Business Continuity is the ability of an organization to continue delivering critical business services during and after disruptions.

---

## 40. What are the key responsibilities of a SAP BASIS Administrator in HA & DR?

- Monitor HANA System Replication
- Manage backups and recovery
- Perform failover and failback
- Conduct DR testing
- Monitor cluster health
- Validate business applications
- Maintain operational documentation

---

# Quick Revision

| Topic | Key Concept |
|--------|-------------|
| HA | Minimize Downtime |
| DR | Restore After Disaster |
| HSR | Database Replication |
| RPO | Acceptable Data Loss |
| RTO | Acceptable Downtime |
| Failover | Primary → Secondary |
| Failback | Secondary → Primary |
| Log Replay | Recommended Operation Mode |
| SyncMem | Common Replication Mode |
| PITR | Point-in-Time Recovery |

---

# Final Tips for Interviews

- Explain concepts using real-world production scenarios.
- Mention monitoring commands where applicable.
- Discuss backup, replication, and recovery together.
- Highlight DR testing experience.
- Focus on business continuity, RPO, and RTO.
- Use a structured troubleshooting approach when answering scenario-based questions.

---

# Summary

High Availability and Disaster Recovery are essential for ensuring uninterrupted SAP business operations. A strong understanding of HANA System Replication, Backup and Recovery, Failover procedures, and Disaster Recovery planning demonstrates the practical knowledge expected from a SAP BASIS Administrator.
