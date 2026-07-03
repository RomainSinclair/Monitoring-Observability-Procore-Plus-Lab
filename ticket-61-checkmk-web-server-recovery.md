**Ticket #61 — Fix the Issue on Your Web Server (via CheckMK)**

*Category: Monitoring Services / Web Server Recovery*

| **Field** | Value |
| --- | --- |
| **Ticket #** | 61 |
| **Title** | Fix the Issue on Your Web Server (via CheckMK) |
| **Category** | Monitoring Services / Web Server Recovery |
| **Prepared by** | Romain Sinclair |
| **Environment** | Procore-Plus Lab (CentOS Stream / RHEL-based) |

# **Objective**

Use CheckMK to identify and fix the critical issues on stage-web. Restore Apache (httpd) and NFS server services to a running, healthy state.

# **Requirements**

- VM: stage-web

- Use CheckMK to identify failing services

- Verify and restore httpd (Apache HTTP Server)

- Verify and restore nfs-server

# **Environment Details**

- VM: stage-web

- Services: httpd, nfs-server, CheckMK agent

- Files: /etc/httpd/conf/httpd.conf, /etc/exports

# **Implementation Steps**

Step 1 — Check Apache status:

systemctl status httpd

Step 2 — Verify the httpd package and config exist:

rpm -q httpd

ls -ld /etc/httpd/conf

ls -l /etc/httpd/conf/httpd.conf

Step 3 — If httpd.conf is missing, reinstall httpd to restore it:

sudo dnf reinstall -y httpd

Step 4 — Test Apache config syntax:

sudo apachectl configtest

Step 5 — Enable and start httpd:

sudo systemctl enable --now httpd

Step 6 — Enable and start nfs-server:

sudo systemctl enable --now nfs-server

sudo systemctl status nfs-server

# **Troubleshooting ****&**** Root Cause Analysis**

The primary issue was that Apache was installed but its main config file /etc/httpd/conf/httpd.conf was missing, likely due to earlier destructive changes made on the system.

Reinstalling httpd alone (dnf reinstall -y httpd) initially did not recreate the file as expected, which required verifying the RPM's file list and checking whether config files had been explicitly deleted.

NFS restart initially failed due to authentication/session privilege issues rather than a configuration problem itself. Running the command with the correct sudo context resolved the service start.

The final resolution path was to restore required service configuration files and restart the services with appropriate privileges.

# **Validation ****&**** Testing**

sudo apachectl configtest

sudo systemctl status httpd

sudo systemctl status nfs-server

ss -tulnp | grep :80

curl -I http://localhost

# **Key Lessons Learned**

- CheckMK makes it easy to identify which services are in CRITICAL state — check the service list first

- dnf reinstall restores config files only if they are still tracked by RPM; manually deleted files may need manual recreation

- NFS server needs proper privileges to start — use sudo or root context


Ticket #61 | Fix the Issue on Your Web Server (via CheckMK) | Procore Linux Jira  | 
