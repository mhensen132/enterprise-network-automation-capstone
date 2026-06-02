
# 11 - Multi-Branch Expansion

## Objective

Expand the automation platform beyond the single-router environment established in Phase 1 and validate centralized management of multiple branch locations from a single Ubuntu automation controller.

The goal was to demonstrate that the controller could manage routers from multiple enterprise sites using Ansible inventories and reusable automation workflows.

---

## Initial Environment

At the conclusion of Phase 1, the automation controller successfully managed a single Cisco IOS-XE router through:

* SSH
* Ansible
* NETCONF
* YANG

The environment validated controller-driven automation but remained limited to a single managed device.

Current topology:

```text
Ubuntu Controller
        |
        +---- R1
```

---

## Expansion Strategy

To simulate a more realistic enterprise environment, two branch locations were added:

### Morning Glory Branch

Devices:

* MG-R1
* MG-R2

Inventory file:

```text
inventory.yml
```

### Anubis Branch

Devices:

* A-R1
* A-R2

Inventory file:

```text
anubisinventory.yml
```

This separation allowed each branch to maintain its own inventory while preserving a centralized automation controller.

---

## Inventory Development

Separate inventories were created for each branch environment.

Morning Glory inventory:

```text
inventory.yml
```

Anubis inventory:

```text
anubisinventory.yml
```

The inventories contained:

* Device names
* Management IP addresses
* SSH credentials
* Connection parameters

This allowed playbooks to target either an individual branch or multiple devices simultaneously.

---

## Inventory Validation

Before testing automation, inventory structures were verified.

Command:

```bash
ansible-inventory -i anubisinventory.yml --graph
```

Purpose:

* Verify inventory syntax
* Confirm host grouping
* Validate Ansible inventory parsing

Result:

Inventory parsed successfully.

---

## Direct SSH Validation

Before introducing automation, direct SSH connectivity was validated to each router.

Example:

```bash
ssh a-r1
```

```bash
ssh a-r2
```

This verified:

* Reachability
* Authentication
* Management access
* Router readiness

The same process was used for Morning Glory branch devices.

---

## Ansible Reachability Testing

After SSH validation, Ansible connectivity was tested.

Command:

```bash
ansible all -i anubisinventory.yml -m ping
```

Result:

```text
SUCCESS => "ping": "pong"
```

This validated:

* Controller connectivity
* Inventory correctness
* SSH transport operation
* Python availability

---

## Command Execution Validation

Ansible was then used to execute operational commands against multiple routers.

Example:

```bash
ansible all -i anubisinventory.yml \
-m cisco.ios.ios_command \
-a "commands='show version'"
```

Result:

Cisco IOS-XE platform information was successfully returned from managed routers.

Validated:

* Multi-device execution
* SSH automation
* Cisco collections
* Inventory-driven management

---

## Engineering Significance

This phase marked the transition from a proof-of-concept automation environment into a multi-site management platform.

The controller could now manage infrastructure across multiple branch environments without requiring direct administration from individual workstations.

Benefits included:

* Centralized administration
* Reusable automation workflows
* Consistent configuration management
* Reduced manual effort

---

## Operational Validation

Successfully validated:

* Morning Glory inventory
* Anubis inventory
* Multi-router SSH connectivity
* Multi-router Ansible execution
* Inventory-driven automation
* Centralized branch management

---

## Evidence

Supporting screenshots and verification artifacts are stored in:

```text
evidence/phase-2/11-multi-branch-expansion/
```

---

## Outcome

The automation platform successfully expanded from a single-router lab into a multi-branch enterprise management environment.

This phase established the inventory structure and management model required for the NETCONF-based automation workflows implemented in the following phase.
