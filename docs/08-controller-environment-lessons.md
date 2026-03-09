# Controller Environment Lessons

## Objective

Document controller-side requirements and lessons learned that were necessary to keep automation working reliably.

---

## Why This Matters

Successful automation depended on controller preparation as much as router configuration.

Several failures occurred when controller environment details were missing or inconsistent.

---

## Python Virtual Environment

A Python virtual environment was used to isolate automation tools.

Activation command:

```bash id="2x5m8k"
source venv/bin/activate
```

---

## Why Virtual Environment Was Important

This prevented package conflicts between system Python and automation libraries.

---

## Core Packages Installed

Installed tools included:

```bash id="7w2r4f"
pip install ansible
pip install ncclient
pip install pyang
```

---

## Ansible Collections Installed

Cisco automation collections required:

```bash id="6u1p9n"
ansible-galaxy collection install cisco.ios
ansible-galaxy collection install ansible.netcommon
```

---

## SSH Client Configuration Became Persistent

Compatibility settings were moved into:

```bash id="5f1q7v"
~/.ssh/config
```

This avoided repeating legacy crypto options manually.

---

## Working Directory Discipline

Ansible execution failed when launched outside the project folder.

Correct working directory:

```bash id="4j9t2w"
cd ~/ansible-lab
```

---

## Transport Warning Observed

Ansible reported:

```text id="1m8n6z"
ansible-pylibssh not installed, falling back to paramiko
```

Execution still succeeded.

---

## Lesson Learned

Controller preparation must be treated as part of the automation system, not just the host running commands.

---

## Engineering Meaning

Stable automation required repeatable controller-side preparation before device automation became reliable.
