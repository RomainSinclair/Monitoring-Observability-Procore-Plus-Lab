**Ticket #54 — Provide Log Information from Graylog**

*Category: Monitoring Services / Graylog*

| **Field** | Value |
| --- | --- |
| **Ticket #** | 54 |
| **Title** | Provide Log Information from Graylog |
| **Category** | Monitoring Services / Graylog |
| **Prepared by** | Romain Sinclair |
| **Environment** | Procore-Plus Lab (CentOS Stream / RHEL-based) |

# **Objective**

Query Graylog to determine the exact date and time MariaDB was installed on dev-app. If the installation is older than one week and not visible in current Graylog data, reinstall MariaDB to generate fresh logs.

# **Requirements**

- VM: dev-app

- MariaDB installation logs from the Graylog server

- Proper documentation mandatory — screenshots required

- If installed more than a week ago, reinstall MariaDB to generate fresh, searchable logs

# **Environment Details**

- VM: dev-app

- Monitoring platform: Graylog

- Service: MariaDB (mariadb-server)

# **Implementation Steps**

Step 1 — Check if MariaDB is already installed:

rpm -q mariadb-server

Step 2 — If older than one week, reinstall to generate fresh log entries:

dnf install -y mariadb-server

Step 3 — Open the Graylog web interface and search for the install event:

Navigate to: Graylog UI > Search

Query: source:dev-app AND message:mariadb

Step 4 — Filter by time range to find the installation timestamp

Step 5 — Note the exact date and time from the log entry

Step 6 — Document with screenshots from Graylog showing the event

# **Troubleshooting ****&**** Root Cause Analysis**

A special note in the ticket indicated that older installations might not still be represented in current Graylog data. Graylog retains logs only for a configured retention window.

The resolution was to reinstall MariaDB on dev-app so that new package installation logs were forwarded from the rsyslog agent to the Graylog server and became searchable.

After reinstallation, a fresh search in Graylog returned the exact install timestamp from the dnf transaction log forwarded by rsyslog.

# **Validation ****&**** Testing**

- Search Graylog for MariaDB install events from dev-app

- Confirm matching timestamp after reinstall appears in results

- Screenshot the Graylog entry showing the exact date/time

# **Key Lessons Learned**

- Graylog only retains logs within its configured retention window — reinstalling may be needed for fresh log data

- rsyslog must be forwarding logs from the client to Graylog for events to appear

- Always document with screenshots for Graylog tickets — the timestamp is the deliverable


Ticket #54 | Provide Log Information from Graylog | Procore Linux Jira  |  Page  of
