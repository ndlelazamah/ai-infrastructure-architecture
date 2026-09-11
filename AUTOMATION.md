# Automation & Self-Healing Monitoring Suite

A 13-job cron automation layer that turned a manually-managed infrastructure into one that monitors, alerts on, and partially heals itself.

## What it does

| Frequency | Job | Purpose |
|---|---|---|
| Every 5 min | Gateway watchdog | Detects a crashed automation service and restarts it automatically |
| Every 10 min | Firewall reachability check | Alerts immediately if the edge firewall admin interface goes unreachable |
| Every 15 min | Full service monitor | Checks all containers and public-facing services, alerts on any outage |
| Every 30 min | Lead notification | Pushes an instant alert the moment a new enquiry lands in the database |
| Hourly | Render queue monitor | Flags jobs stuck in the processing queue for 2+ hours |
| Every 6 hrs | Memory usage alert | Warns if any host crosses 85% RAM |
| 2x daily | Disk space alert | Warns if any host crosses 80% disk usage |
| Weekly | Analytics digest | Traffic, pageviews, and new leads for the week, pushed as a report |
| Weekly | SSL expiry check | Warns 30 days ahead of any certificate expiring |
| Weekly | Security report | Summarizes banned IPs (intrusion-prevention) and certificate health across all hosts |
| Daily (upgraded from weekly) | Database backup | Automated dump of production databases |
| Weekly | Docker maintenance | Prunes unused images/cache, reports space freed |
| Daily | Morning status brief | One consolidated health report across the whole stack |

## Design notes

- **Alerting channel:** all jobs report to a chat-based notification channel rather than email, so incidents are seen within minutes, not at the next inbox check.
- **Self-healing, not just self-reporting:** the watchdog job doesn't just alert on a crashed service — it restarts it and *then* alerts, so most outages resolve before a human even looks.
- **Backup cadence upgraded from experience:** database backups moved from weekly to daily after reviewing acceptable data-loss windows for the business — a good example of iterating an ops decision post-launch rather than treating "done" as final.
- **Snapshot before automating:** a full VM snapshot was taken once the suite was verified working, so the entire automation layer can be restored in one step if something regresses.
- **Every job manually tested** before being left to run unattended — timestamps and log paths were checked for a full day before trusting the schedule.

## Outcome

Infrastructure that used to require manual checking now surfaces its own problems — outages, low disk, expiring certificates, stuck jobs — usually before a person notices, and recovers automatically from the most common failure mode (a crashed service).

---
*Script paths, internal IPs, hostnames, and credentials are omitted — this describes the design and outcome, not the live configuration.*
