# Ansible Playbook Automation

## Objective

Move from single Ansible ad hoc commands to repeatable playbook-based automation against the Cisco router.

---

## Why This Step Matters

Ad hoc commands validate connectivity.

Playbooks create repeatable automation workflows that can be expanded safely.

---

## First Playbook Structure

A basic playbook was created to execute operational show commands.

```yaml id="8d4p7x"
---
- name: Sample IOS playbook to run show commands
  hosts: R1
  gather_facts: no

  tasks:
    - name: run show ip interface brief
      cisco.ios.ios_command:
        commands:
          - show ip interface brief
      register: myinterfaces

    - name: display value of myinterfaces variable
      debug:
        var: myinterfaces["stdout_lines"][0]
```

---

## Execution Command

```bash id="4v1r8w"
ansible-playbook -i inventory.yml 02-ios_show_commands.yaml
```

---

## Result

The router returned structured command output successfully.

Validated:

* playbook syntax correct
* Cisco module execution working
* registered variable storage working

---

## Registered Variable Use

The command result was stored in:

```text id="6n3m5k"
myinterfaces
```

This allowed later tasks to reference returned data.

---

## Example Output Use

```text id="5k2r1z"
myinterfaces["stdout_lines"][0]
```

This displayed interface output line-by-line.

---

## Why Register Matters

Registering output allows later logic such as:

* condition checks
* parsing
* comparisons
* reporting

---

## Engineering Meaning

This was the first repeatable operational workflow executed through Ansible playbooks instead of one-time commands.
