# Monitoring Observability — Procore-Plus Lab

Monitoring & Observability — Nagios, CheckMK, and Graylog deployment, configuration, and incident detection across the Procore-Plus lab environment.

## Environment
CentOS Stream / RHEL-based VMs in the Procore-Plus lab (dev-app, stage-web, dev-performance hosts), managed as a production-style environment with Jira ticket tracking.

## Skills & Tools
Nagios Core, NRPE, CheckMK, check-mk-agent, Graylog, log analysis, service health monitoring, alert-driven troubleshooting

## Tickets

| Ticket | Title | Documentation |
| --- | --- | --- |
| #47 | Create Nagios User and Configure Access | [ticket-47-nagios-user-access.md](tickets/ticket-47-nagios-user-access.md) |
| #48 | Add Servers to Nagios Monitoring | [ticket-48-nagios-add-servers.md](tickets/ticket-48-nagios-add-servers.md) |
| #54 | Provide Log Information from Graylog (MariaDB) | [ticket-54-graylog-log-analysis.md](tickets/ticket-54-graylog-log-analysis.md) |
| #60 | Add Servers to CheckMK Monitoring | [ticket-60-checkmk-add-servers.md](tickets/ticket-60-checkmk-add-servers.md) |
| #61 | Fix Web Server Issue Identified via CheckMK (NTP drift, httpd recovery) | [ticket-61-checkmk-web-server-recovery.md](tickets/ticket-61-checkmk-web-server-recovery.md) |

## About
Each ticket document includes the objective, environment details, step-by-step resolution, commands used, and verification/outcome. These reflect real hands-on system administration work completed in a lab modeled on production operations.
