
# Enterprise Network Automation Capstone

## Overview

This project documents the build of a model-driven network automation environment using Cisco IOS-XE, Ansible, NETCONF, and YANG.

The objective is to move from traditional manual CLI administration toward structured automation using a dedicated management plane and standards-based device interfaces.

---

## Project Scope

This repository covers:

* Ubuntu automation controller preparation
* Cisco IOS-XE router management-plane connectivity
* SSH compatibility troubleshooting between modern Linux and older IOS-XE crypto defaults
* Ansible CLI automation
* NETCONF transport validation
* First model-driven configuration push using YANG-backed XML payloads

---

## Platform Stack

### Automation Controller

* Ubuntu 24.04 LTS
* Ansible
* OpenSSH

### Managed Device

* Cisco ISR4321
* IOS-XE 16.9.6

### Management Protocols

* SSH (TCP 22)
* NETCONF over SSH (TCP 830)

---

## Management Network

| Device            | Role            | Address        |
| ----------------- | --------------- | -------------- |
| Ubuntu Controller | Automation Host | 192.168.50.100 |
| R1                | Managed Router  | 192.168.50.1   |


## Phase 1 Achievements

Completed:

* Ubuntu controller prepared for Ansible
* Required Ansible collections installed
* Router reachable across dedicated management subnet
* SSH access established
* SSH crypto compatibility issues resolved
* Ansible inventory validated
* CLI automation operational
* NETCONF retrieval operational
* First hostname change completed through NETCONF

---

## First Successful NETCONF Configuration

Hostname changed using XML payload:


<hostname>R1-NETCONF</hostname>


This was the first successful model-driven configuration push in the project.

---

## Repository Structure


docs/        -> design, build process, troubleshooting, validation
automation/  -> inventories and playbooks
evidence/    -> screenshots and outputs
diagrams/    -> topology visuals

---

## Current Status

Phase 1 baseline automation is operational.

## Next Milestones

* Expand automation to multiple devices
* Build reusable Ansible roles
* Automate NTP and syslog deployment
* Add compliance validation
* Introduce drift detection
