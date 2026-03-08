# Project Architecture Summary

## Objective

Summarize the full technical architecture used to establish model-driven automation between the Ubuntu controller and Cisco infrastructure.

---

## Core Components

| Component            | Role                            |
| -------------------- | ------------------------------- |
| Ubuntu 24.04 VM      | Automation controller           |
| Cisco Switch         | Management path                 |
| Cisco ISR4321 Router | Automation target               |
| Ansible              | Automation framework            |
| SSH                  | Secure transport                |
| NETCONF              | Structured management transport |
| YANG                 | Data model definition           |

---

## Management Topology

Traffic path used during project:


Ubuntu Controller → Switch → Router


---

## Management Addressing

| Device            | Address        |
| ----------------- | -------------- |
| Ubuntu Controller | 192.168.50.100 |
| Switch            | 192.168.50.2   |
| Router            | 192.168.50.1   |

---

## Protocol Layering

### First Layer

SSH enabled remote authenticated access.

---

### Second Layer

Ansible CLI modules executed operational commands.

Example:


cisco.ios.ios_command


---

### Third Layer

NETCONF enabled structured data retrieval over port 830.

---

### Fourth Layer

YANG models defined XML hierarchy used by NETCONF.

---

## Controller Responsibilities

Ubuntu controller handled:

* SSH client compatibility adjustments
* Ansible execution
* playbook storage
* Python package management
* YANG inspection

---

## Router Responsibilities

Router provided:

* IOS-XE 16.9.6 platform
* NETCONF subsystem
* YANG process support
* CLI automation target

---

## Project Flow


Platform upgrade
→ baseline config
→ management validation
→ SSH access
→ Ansible CLI
→ NETCONF
→ YANG exploration
→ playbook automation


---

## Engineering Meaning

The project built a layered management path where each successful step became the dependency for the next automation capability.
