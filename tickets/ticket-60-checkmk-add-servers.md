**Ticket #60 — Add Servers to CheckMK Monitoring**

*Category: Monitoring Services / CheckMK*

| **Field** | Value |
| --- | --- |
| **Ticket #** | 60 |
| **Title** | Add Servers to CheckMK Monitoring |
| **Category** | Monitoring Services / CheckMK |
| **Prepared by** | Romain Sinclair |
| **Environment** | Procore-Plus Lab (CentOS Stream / RHEL-based) |

# **Objective**

Install the CheckMK agent on dev-app and stage-web, open the required firewall port (6556/tcp), and add both servers into the CheckMK monitoring platform.

# **Requirements**

- VMs: dev-app, stage-web

- Install CheckMK agent from the CheckMK server at 10.1.x.x

- Open firewall port 6556/tcp on each client

- Enable and start the check-mk-agent.socket service

- Add both hosts in the CheckMK web interface

# **Environment Details**

- Clients: dev-app, stage-web

- Server: CheckMK at 10.1.x.x (site: procore)

- Port: 6556/tcp (CheckMK agent listener)

- Package: check-mk-agent-2.3.0p2-1.noarch.rpm

# **Implementation Steps**

Step 1 — Install wget if not present:

sudo yum install -y wget

Step 2 — Download the CheckMK agent RPM from the server:

wget http://10.1.30.37/procore/check_mk/agents/check-mk-agent-2.3.0p2-1.noarch.rpm

Step 3 — Install the agent:

sudo yum -y localinstall check-mk-agent-2.3.0p2-1.noarch.rpm

Step 4 — Open firewall port 6556:

sudo firewall-cmd --zone public --add-port 6556/tcp --permanent

sudo firewall-cmd --reload

Step 5 — Enable and start the agent socket:

sudo systemctl enable --now check-mk-agent.socket

Step 6 — Verify the port is listening:

ss -tulnp | grep 6556

Step 7 — In CheckMK web UI: navigate to Setup > Hosts and add each server

# **Troubleshooting ****&**** Root Cause Analysis**

Two major issues surfaced during this ticket. First, the CheckMK UI instructions referenced a DEV folder under Setup > Hosts that did not exist by default. The folder needed to be created manually in the CheckMK interface.

Second, when validating with curl localhost:6556, the command returned an HTTP/0.9 response warning. This is expected — the CheckMK agent does not speak HTTP. It uses a raw TCP protocol. The correct test is nc localhost 6556 which returns the agent output directly.

After both issues were resolved, the agent socket was verified as listening and the hosts were successfully added in the CheckMK web interface.

# **Validation ****&**** Testing**

systemctl status check-mk-agent.socket

ss -tulnp | grep 6556

nc localhost 6556

- Verify hosts appear in CheckMK UI under Setup > Hosts

# **Key Lessons Learned**

- CheckMK agent uses raw TCP on port 6556 — curl will show an HTTP/0.9 warning (expected behavior); use nc for testing

- The CheckMK UI folder (Dev/Prod) must be pre-created before adding hosts

- check-mk-agent.socket is a systemd socket-activated service — always enable via systemctl


Ticket #60 | Add Servers to CheckMK Monitoring | Procore Linux Jira  | 
