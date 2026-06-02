
# 14 - Multi-Site Architecture Summary

## Objective

Summarize the enterprise automation architecture established during Phase 2 and document how the Ubuntu automation controller securely manages infrastructure across multiple branch environments.

---

## Core Components

| Component              | Role                                     |
| ---------------------- | ---------------------------------------- |
| Ubuntu Controller      | Centralized automation platform          |
| Cisco Firepower 1010   | Security boundary and management gateway |
| Morning Glory Routers  | Managed infrastructure targets           |
| Morning Glory Switches | Managed Layer 2 infrastructure           |
| Anubis Routers         | Managed infrastructure targets           |
| Anubis Switches        | Managed Layer 2 infrastructure           |
| Ansible                | Automation framework                     |
| SSH                    | Secure management transport              |
| NETCONF                | Structured management transport          |
| YANG                   | Data model definition                    |

---

## Branch Architecture

### Morning Glory Branch

Managed Devices:

| Device | Role            |
| ------ | --------------- |
| MG-R1  | IOS-XE Router   |
| MG-R2  | IOS-XE Router   |
| MG-SW1 | Catalyst Switch |
| MG-SW2 | Catalyst Switch |

Inventory:

```text id="1dovrt"
inventory.yml
```

---

### Anubis Branch

Managed Devices:

| Device | Role            |
| ------ | --------------- |
| A-R1   | IOS-XE Router   |
| A-R2   | IOS-XE Router   |
| A-SW1  | Catalyst Switch |
| A-SW2  | Catalyst Switch |

Inventory:

```text id="r1w33z"
anubisinventory.yml
```

---

## Management Topology

Traffic path used during Phase 2:

```text id="0jmxfi"
Ubuntu Controller
        |
        v
   Firepower 1010
        |
   +----+----+
   |         |
   v         v

Morning   Anubis
 Glory    Branch
 Branch
```

All management traffic passed through Firepower security policies before reaching managed infrastructure.

---

## Management Protocols

### SSH

SSH provided authenticated management access.

Used for:

* Device administration
* Ansible CLI automation
* Connectivity validation

Default management port:

```text id="85gddq"
22/TCP
```

---

### NETCONF

NETCONF provided model-driven management access.

Used for:

* State retrieval
* Configuration management
* Structured data collection

Default management port:

```text id="ur4w53"
830/TCP
```

---

### YANG

YANG models defined the hierarchical structure used by NETCONF.

Benefits:

* Structured data
* Vendor-supported management
* Machine-readable configuration

---

## Security Architecture

Firepower enforced:

* NAT translations
* SSH access controls
* NETCONF access controls
* ICMP testing policies
* Interface-level ACLs

Management access was restricted to the automation controller.

Controller object:

```text id="uh6glj"
ANSIBLE-STATION
10.104.104.5
```

## Only approved management traffic was permitted through the branch firewalls.

## Inventory Architecture

The automation platform used inventory separation by branch.

Morning Glory:

```text id="vh7hjg"
inventory.yml
```

Anubis:

```text id="83nmz4"
anubisinventory.yml
```

Benefits:

* Logical separation
* Simplified troubleshooting
* Scalable growth
* Reusable playbooks

---

## Automation Artifacts

### NETCONF

```text id="2jlbpb"
automation/netconf/
├── collect-netconf-state.yml
├── collect-router-state.yml
├── inventory-cli.yml
└── netconf-get.yml
```

### Firepower

```text id="39dwa7"
automation/firepower/
├── morning-glory-firewall-acl.txt
└── anubis-firewall-acl.txt
```

---

## Project Flow

```text id="ubdjlwm"
Single Router Automation
        ↓
NETCONF Validation
        ↓
Multi-Branch Expansion
        ↓
Firepower Integration
        ↓
Multi-Site NETCONF Operations
```

---

## Operational Validation

Successfully validated:

* Multi-branch inventory management
* Multi-router SSH connectivity
* Multi-router NETCONF communication
* Firepower NAT operation
* Firepower ACL enforcement
* Secure controller-to-branch connectivity
* Inventory-driven automation

---

## Engineering Meaning

Phase 2 transformed the project from a single-device automation lab into a multi-site enterprise management platform.

The Ubuntu controller became capable of securely managing infrastructure across multiple branch environments through a combination of:

* Ansible
* SSH
* NETCONF
* YANG
* Firepower security controls

This architecture established the foundation required for Phase 3, where automation expanded beyond network devices and into virtualized server infrastructure.
