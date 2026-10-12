# Ansible

What is Ansible, and Why Do You Need It?

What it is

Ansible is an open-source automation tool (created by Michael DeHaan, now owned by Red Hat). You write plain-text YAML files describing what you want, and Ansible makes your infrastructure match that description.

Three properties explain its popularity:
**Agentless**: nothing to install on the target. SSH or an API is enough.
**Human-readable**: YAML playbooks read almost like a change plan.
**Idempotent**: running it repeatedly is safe. It only changes what is out of line.

<img width="983" height="426" alt="image" src="https://github.com/user-attachments/assets/a487d356-0b3a-4a90-8825-321be7a49fcc" />

Ansible turns this into: write once, review, test, apply to all, and keep it in Git. The playbook becomes your source of truth and documentation.

# Ansible Use Cases: Network and Cloud

## Network Use Cases (Your Daily World)

- **Config backup:** Pull running configs from all devices nightly and store them in Git.
- **Bulk changes:** Push NTP, SNMP, syslog, banners, AAA, or ACLs to hundreds of devices consistently.
- **VLAN/interface provisioning:** Create a VLAN on 50 switches in one run.
- **Compliance audit:** Check that every device has the required config, and report the ones that don't.
- **Health checks and reporting:** Collect `show version`, interface errors, and BGP neighbor state, then build an inventory report. This is useful before and after a change window.
- **Pre/post-change validation:** Capture the state before, apply the change, capture the state after, and compare.
- **OS upgrades:** Automate the image copy, verify the checksum, set the boot variable, reload, and verify.
- **Day-0 provisioning:** Generate a full config from a template and push it to a new device.
- **Firewall and load balancer rules:** Manage policy objects and rules on Palo Alto, Fortinet, Check Point, and F5.
- **Multi-vendor consistency:** Use one workflow for Cisco, Juniper, Arista, and others.

## Cloud Use Cases

- **Provisioning:** Create VPCs/VNets, subnets, security groups, route tables, VMs, and load balancers on AWS, Azure, or GCP, using the cloud collections (`amazon.aws`, `azure.azcollection`, `google.cloud`).
- **Hybrid connectivity:** Build site-to-site VPNs or Direct Connect/ExpressRoute attachments, and configure your on-prem router side in the same playbook.
- **Cloud networking security:** Standardize security group and NSG rules, and audit for risky open ports.
- **Server configuration after provisioning:** Install packages, harden the OS, and deploy apps.
- **Multi-cloud:** The same tool and skills work across all providers.
- **Scheduled operations:** Snapshot, patch, or clean up unused resources.

A strong real-world pattern for you: one playbook that creates a cloud VPN gateway and also configures the matching tunnel on your on-prem router. Doing that manually across two consoles is error-prone. Ansible does both from one place.

<img width="969" height="303" alt="image" src="https://github.com/user-attachments/assets/33f080cb-de61-40fe-b55d-e3bbaa7d2a39" />


They are complementary. Many teams use Terraform to build cloud infrastructure and Ansible to configure what runs on or around it. Ansible can also provision cloud resources, but it keeps no state file the way Terraform does.

**Why it matters for your career**

As a Senior Network Engineer, this is the shift from operator to network automation engineer. Job descriptions for senior network roles now routinely ask for Ansible, Git, and Python. Ansible is the easiest entry point because you don’t need to be a programmer.

**Simple summary**

Ansible = a tool that logs in to many devices for you and makes them match the configuration you wrote down, safely and repeatably.

