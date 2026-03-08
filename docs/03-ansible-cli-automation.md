# Ansible CLI Automation

## Objective

Validate that the Ubuntu automation controller could execute Cisco IOS-XE commands through Ansible over SSH.

---

## Required Ansible Collections

Cisco collections installed on the controller:


ansible-galaxy collection install cisco.ios
ansible-galaxy collection install ansible.netcommon


---

## Initial Inventory File

Inventory used for CLI testing:


all:
  hosts:
    R1:
      ansible_host: 192.168.50.1
      ansible_user: admin
      ansible_password: <redacted>
      ansible_network_os: ios
      ansible_connection: network_cli


---

## Initial Problem

First Ansible execution failed because the command was launched outside the correct working directory.

Inventory could not be parsed correctly until the controller was moved into the project directory.

---

## Working Directory Correction


cd ~/ansible-lab


---

## Connectivity Validation

Basic Ansible reachability test:


ansible R1 -i inventory.yml -m ping

---

## Result


SUCCESS => "ping": "pong"


---

## First CLI Module Test

Command used:


ansible R1 -i inventory.yml -m cisco.ios.ios_command -a "commands='show version'"


---

## Result

Router returned IOS-XE platform output successfully.

Validated:

* SSH authentication
* inventory correctness
* Cisco collection operation
* CLI module execution

---

## Transport Note

Ansible reported:

ansible-pylibssh not installed, falling back to paramiko


This did not prevent successful execution.

---

## Engineering Meaning

This was the first successful controller-driven operational command executed against the router using Ansible.
