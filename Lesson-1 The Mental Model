Ansible Lesson 1: The Mental Model

As a network engineer, you already do this by hand: log in to a device, type commands, verify, repeat for 200 devices. Ansible is the same work, but you describe what you want once, and it does the logging in, checking, and changing for you across every device.

1. The core idea: desired state, not commands

If you type ip route 10.0.0.0 255.0.0.0 192.168.1.1 twice, the router just accepts it again. Ansible modules work differently. You say “this route should exist”, and the module:

Checks the current state of the device
Compares it to your desired state
Changes something only if they differ
Reports ok (already correct) or changed (it fixed something)

This property is called idempotency. It’s why you can safely run a playbook 10 times and get the same result, and why Ansible works as an audit tool as well as a config tool.

2. How it works for network devices

This is the most important point for you, and many tutorials skip it.

Servers (Linux): Ansible SSHes in, copies a small Python script, runs it there, and removes it.
Network devices (IOS, NX-OS, Junos, EOS): They can’t run Python. So Ansible runs the module on your control node and connects to the device over SSH CLI (network_cli), NETCONF, or an HTTP API.
[Control Node]  ──SSH/NETCONF/API──►  [Router / Switch]
  Ansible runs here                    nothing installed on it

Nothing is installed on your devices (“agentless”). If you can SSH to it, Ansible can manage it.

3. The five building blocks
Concept	What it is	Network analogy
Inventory	List of devices, grouped	Your device list / NMS groups
Module	A unit of work (e.g. cisco.ios.ios_config)	One feature template: “configure NTP”
Task	One module call with parameters	One step in a change plan
Play	Maps a group of devices to a list of tasks	“On all core routers, do these steps”
Playbook	A YAML file with one or more plays	The full change plan (MOP)
4. Your first real example

inventory.ini

ini
[routers]
r1 ansible_host=10.1.1.1
r2 ansible_host=10.1.1.2

[routers:vars]
ansible_network_os=cisco.ios.ios
ansible_connection=ansible.netcommon.network_cli
ansible_user=admin
ansible_password=yourpassword

ntp.yml

yaml
---
- name: Configure NTP on routers
  hosts: routers
  gather_facts: false

  tasks:
    - name: Ensure NTP server is configured
      cisco.ios.ios_config:
        lines:
          - ntp server 10.10.10.10

    - name: Check NTP status
      cisco.ios.ios_command:
        commands: show ntp status
      register: ntp_output

    - name: Display result
      ansible.builtin.debug:
        var: ntp_output.stdout_lines

Run it:

bash
ansible-playbook -i inventory.ini ntp.yml

What happens, in logic order:

Ansible reads the inventory and finds r1 and r2 in routers.
It connects to each device, in parallel (5 at a time by default).
Task 1: ios_config pulls the running config, sees whether the NTP line exists, and adds it only if missing.
Task 2 runs show ntp status and saves the output in ntp_output (register).
Task 3 prints it.
You get a recap: ok=3 changed=1 on the first run, changed=0 on the second. That’s idempotency in action.

(In real life, put the password in Ansible Vault, not the inventory. We’ll cover that.)

5. YAML in 30 seconds
Indentation is structure (2 spaces, never tabs).
key: value is a dictionary.
- item is a list entry.
--- marks the start of a file.

Most beginner errors are indentation mistakes.

6. Useful commands to try right now
bash
pip install ansible                         # install
ansible-galaxy collection install cisco.ios # vendor modules live in collections
ansible-doc cisco.ios.ios_config            # built-in documentation for any module
ansible-playbook ntp.yml --check --diff     # dry run: shows what WOULD change

--check --diff is your pre-change verification tool. Get used to it.

Learning roadmap
Lesson 1 (this one): mental model, first playbook
Lesson 2: inventory deep dive: groups, group_vars, host_vars, and variables
Lesson 3: network modules: ios_command vs ios_config vs resource modules (ios_interfaces, ios_vlans, ios_bgp_global)
Lesson 4: Jinja2 templates: generate full configs from variables
Lesson 5: loops, conditionals, handlers, register, and parsing show output (ntc_templates, ansible.utils.cli_parse)
Lesson 6: Ansible Vault, backups, rollback, and safe change workflows
Lesson 7: roles, Git, and CI pipelines for network automation
