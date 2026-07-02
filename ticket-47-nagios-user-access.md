**Ticket #47 — Create NAGIOS User and Configure Access**

*Category: Monitoring Services / Nagios*

| **Field** | Value |
| --- | --- |
| **Ticket #** | 47 |
| **Title** | Create NAGIOS User and Configure Access |
| **Category** | Monitoring Services / Nagios |
| **Prepared by** | Romain Sinclair |
| **Environment** | Procore-Plus Lab (CentOS Stream / RHEL-based) |

# **Objective**

Create an HTTP user for the Nagios web interface using the IPA username and authorize that user with full admin rights in the Nagios CGI configuration.

# **Requirements**

- VM: stage-nagios

- Create a Nagios web interface user using the IPA username

- Authorize user access in /usr/local/nagios/etc/cgi.cfg

- Restart Nagios to apply the changes

# **Environment Details**

- VM: stage-nagios (IP: 10.1.10.37)

- Service: Nagios Core

- Files: /usr/local/nagios/etc/htpasswd.users, /usr/local/nagios/etc/cgi.cfg

# **Implementation Steps**

Step 1 — SSH into the Nagios server:

ssh ipauser@10.1.10.37

Step 2 — Create/update the web user password in htpasswd.users:

htpasswd /usr/local/nagios/etc/htpasswd.users <username>

Step 3 — Open cgi.cfg and add the username to all authorized_for_* directives:

sudo vi /usr/local/nagios/etc/cgi.cfg

Directives to update: authorized_for_system_information, authorized_for_configuration_information, authorized_for_system_commands, authorized_for_all_services, authorized_for_all_hosts, authorized_for_all_service_commands, authorized_for_all_host_commands

Step 4 — Restart Nagios to apply:

sudo systemctl restart nagios

# **Troubleshooting ****&**** Root Cause Analysis**

The work depended on creating the user in the correct htpasswd file and updating all relevant authorized_for_* directives inside cgi.cfg. Missing any authorization line would cause partial or failed web console access.

Nagios does not support service reload (systemctl reload nagios returns 'Job type reload not applicable'). A full restart is required every time configuration changes are made.

Final resolution required a Nagios service restart to pick up both the new htpasswd entry and the cgi.cfg updates.

# **Validation ****&**** Testing**

sudo systemctl status nagios

- Browser validation: http://10.1.10.37/nagios/

- Login with the new credentials and confirm full dashboard access

# **Key Lessons Learned**

- Nagios does not support reload — always use restart after config changes

- htpasswd.users controls authentication; cgi.cfg controls authorization — both must be updated

- The authorized_for_* directives in cgi.cfg are comma-separated — append the username carefully

# **Screenshots**

***Figure 1: SSH into stage-nagios and running htpasswd to create Nagios web user***

***Figure 2: Nagios cgi.cfg authorization configuration***

***Figure 3: Nagios service restart and status verification***

Ticket #47 | Create NAGIOS User and Configure Access | Procore Linux Jira  |  Page  of