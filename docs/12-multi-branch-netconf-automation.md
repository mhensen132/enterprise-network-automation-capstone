
# 12 - Multi-Branch NETCONF Automation

## Objective

Extend the NETCONF capabilities established in Phase 1 to multiple branch environments and validate model-driven data collection across enterprise infrastructure.

The goal was to demonstrate that the Ubuntu automation controller could retrieve operational state information from multiple routers using NETCONF and Ansible rather than traditional CLI-based methods.

---

## Prerequisites

The following capabilities were successfully validated during previous phases:

* SSH connectivity
* Ansible controller operation
* NETCONF enablement
* YANG service operation
* Multi-branch inventory management

Branch environments included:

### Morning Glory

Inventory:

```text
inventory.yml
```

Managed devices:

* MG-R1
* MG-R2

### Anubis

Inventory:

```text
anubisinventory.yml
```

Managed devices:

* A-R1
* A-R2

---

## NETCONF Inventory Development

A dedicated inventory was created for NETCONF operations.

Inventory file:

```text
inventory-cli.yml
```

The inventory defined:

* Device management addresses
* Authentication parameters
* NETCONF connection settings
* Ansible network transport configuration

Unlike SSH CLI automation, NETCONF operations communicate directly with the NETCONF service running on TCP port 830.

---

## NETCONF Connectivity Validation

Initial validation focused on confirming that routers could accept NETCONF connections from the controller.

Testing verified:

* NETCONF service availability
* Authentication
* Transport connectivity
* Ansible NETCONF module operation

This ensured that routers were prepared for model-driven management workflows.

---

## NETCONF Data Retrieval

NETCONF data collection was performed using:

```text
netconf-get.yml
```

Purpose:

* Retrieve operational data
* Validate NETCONF communication
* Confirm YANG-based management access

The playbook successfully retrieved information directly from the router's management datastore.

---

## Router State Collection

To collect broader operational information, a dedicated playbook was created:

```text
collect-router-state.yml
```

Purpose:

* Gather router operational state
* Retrieve management information
* Validate multi-device data collection

This approach provided structured data rather than relying on traditional CLI output parsing.

---

## NETCONF State Collection

Additional NETCONF service validation was performed using:

```text
collect-netconf-state.yml
```

Purpose:

* Verify NETCONF service status
* Validate YANG management processes
* Confirm operational readiness

This provided visibility into the model-driven management infrastructure running on IOS-XE.

---

## Multi-Branch Execution

The same automation workflows could be executed against multiple branch environments.

Examples included:

```bash
ansible-playbook -i inventory-cli.yml netconf-get.yml
```

```bash
ansible-playbook -i inventory-cli.yml collect-router-state.yml
```

```bash
ansible-playbook -i inventory-cli.yml collect-netconf-state.yml
```

This demonstrated that the automation platform could collect structured management data from multiple routers without modifying playbook logic.

---

## NETCONF vs Traditional CLI

### Traditional CLI Approach

```text
SSH
 ↓
show commands
 ↓
parse output
```

### NETCONF Approach

```text
NETCONF
 ↓
Structured XML data
 ↓
YANG models
 ↓
Machine-readable output
```

The NETCONF workflow provided a more scalable foundation for enterprise automation.

---

## Engineering Significance

This phase demonstrated a transition from command-based automation to model-driven automation.

Rather than collecting text-based CLI output, the controller interacted directly with the router's management datastore through standards-based protocols.

This approach provides:

* Structured data collection
* Improved automation reliability
* Vendor-supported management interfaces
* Reduced dependence on CLI parsing

---

## Operational Validation

Successfully validated:

* Multi-router NETCONF communication
* Inventory-driven NETCONF execution
* Structured state retrieval
* Router operational state collection
* NETCONF service state collection
* Multi-branch model-driven management

---

## Evidence

Supporting screenshots and verification artifacts are stored in:

```text
evidence/phase-2/12-multi-branch-netconf/
```

Automation artifacts are stored in:

```text
automation/netconf/
├── netconf-get.yml
├── collect-router-state.yml
├── collect-netconf-state.yml
└── inventory-cli.yml
```

---

## Outcome

The automation platform successfully expanded NETCONF operations beyond a single router and demonstrated model-driven management across multiple branch environments.

This phase established the foundation for larger-scale network automation by proving that structured operational data could be collected consistently from multiple enterprise sites using NETCONF and Ansible.
