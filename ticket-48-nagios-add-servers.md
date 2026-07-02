**Ticket #48 — Add Servers to Nagios Monitoring**

*Category: Monitoring Services / Nagios*

| **Field** | Value |
| --- | --- |
| **Ticket #** | 48 |
| **Title** | Add Servers to Nagios Monitoring |
| **Category** | Monitoring Services / Nagios |
| **Prepared by** | Romain Sinclair |
| **Environment** | Procore-Plus Lab (CentOS Stream / RHEL-based) |

# **Objective**

Add virtual machines dev-app and stage-web to Nagios monitoring by creating host configuration files based on the yt-templates.cfg.bak template and registering them in nagios.cfg.

# **Requirements**

- VM server: stage-nagios

- VM clients: dev-app-[initials].procore.prod1-IP, stage-web-[initials].procore.prod1-IP

- Use yt-templates.cfg.bak to create host config under /usr/local/nagios/etc/objects

- Add host config path to /usr/local/nagios/etc/nagios.cfg

- Validate configuration and restart Nagios

# **Environment Details**

- Server: stage-nagios

- Clients: dev-app-rs1.procore.prod1, stage-web-rs1.procore.prod1

- Files: /usr/local/nagios/etc/objects/*.cfg, /usr/local/nagios/etc/nagios.cfg

# **Implementation Steps**

Step 1 — Create host config file for stage-web:

sudo vi /usr/local/nagios/etc/objects/stage-web.cfg

Define: host_name, alias, address (IP), use linux-server, check_period 24x7, notification_period 24x7

Step 2 — Create host config file for dev-app:

sudo vi /usr/local/nagios/etc/objects/dev-app.cfg

Define same fields with dev-app's hostname and IP address

Step 3 — Register both config files in nagios.cfg:

sudo vi /usr/local/nagios/etc/nagios.cfg

Add: cfg_file=/usr/local/nagios/etc/objects/stage-web.cfg

Add: cfg_file=/usr/local/nagios/etc/objects/dev-app.cfg

Step 4 — Run pre-flight validation:

sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg

Step 5 — Restart Nagios to apply:

sudo systemctl restart nagios

# **Troubleshooting ****&**** Root Cause Analysis**

There were multiple blockers during this work. First, there was directory access confusion — navigating to the correct /usr/local/nagios/etc/objects/ directory versus the default /etc/nagios path.

Hostname resolution issues arose when trying to reach stage-nagios from the shell — the short hostname alone failed and the FQDN (stage-nagios-rs1.procore.prod1) was needed.

Attempted reload on Nagios failed because the service unit did not support reload, requiring a full restart instead.

The final successful check came from the Nagios pre-flight validation returning: Total Warnings: 0 and Total Errors: 0.

# **Validation ****&**** Testing**

sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg

sudo systemctl status nagios

- Verify both hosts appear in the Nagios web interface under Hosts

# **Key Lessons Learned**

- Always run nagios -v before restarting to catch config errors early

- Use exact FQDNs for server addresses in host definitions to avoid resolution failures

- Nagios restart is required (not reload) when adding new cfg_file entries to nagios.cfg

# **Screenshots**

***Figure 1: Nagios host config file (dev-app-kr.cfg) sample template structure***

***Figure 2: Nagios configuration pre-flight validation output***

***Figure 3: Nagios service restart (reload not supported — restart required)***

Ticket #48 | Add Servers to Nagios Monitoring | Procore Linux Jira  |  Page  of