# 13 - Firepower Integration

## Objective

Integrate Cisco Firepower 1010 firewalls into the enterprise automation architecture and establish secure management access between the Ubuntu automation controller and branch infrastructure.

The goal of this phase was not to automate the firewalls themselves, but to use them as controlled security boundaries that would allow Ansible, SSH, and NETCONF management traffic to reach infrastructure devices while maintaining enterprise security controls.

---

## Branch Firewall Architecture

Each branch environment was protected by a Cisco Firepower 1010 firewall.

Branch environments included:

### Morning Glory

Protected devices:

* MG-R1
* MG-R2
* MG-SW1
* MG-SW2

### Anubis

Protected devices:

* A-R1
* A-R2
* A-SW1
* A-SW2

The Firepower platforms provided:

* Routing
* NAT
* Access Control Lists
* Secure management access
* Traffic filtering

The firewalls became the gateway through which all automation traffic passed.

---

## Platform Validation

Firewall software versions were verified.

Command:

```bash
show version
```

Result:

```text
Cisco Firepower 1010
ASA Version 9.18(2)
SSP Operating System Version 2.12(0.438)
```

Verification confirmed:

* Platform health
* Software version
* Licensing status
* Hardware operation

---

## Interface Architecture Validation

Firewall interfaces were validated.

Command:

```bash
show interface ip brief
```

Example interface assignments:

```text
outside   10.104.104.3
inside1   172.16.150.1
inside2   172.16.150.17
```

Validation confirmed:

* Interface status
* Addressing
* Branch network connectivity

---

## Routing Validation

Routing functionality was verified.

Command:

```bash
show route
```

Validation confirmed:

* Default route operation
* Connected networks
* Branch reachability
* Upstream connectivity

The firewall successfully provided Layer 3 forwarding between branch infrastructure and external networks.

---

## NAT Architecture

The automation controller required access to infrastructure devices located behind each firewall.

To support management access, static NAT translations were created.

### Morning Glory Example

Configuration:

```text
object network MG-R1-REAL
 host 172.16.150.2

object network MG-R1-PUBLIC
 host 10.104.104.10

nat (inside1,outside) source static MG-R1-REAL MG-R1-PUBLIC
```

Additional static translations were created for:

* MG-R2
* MG-SW1
* MG-SW2

Morning Glory NAT configuration is preserved in:

```text
automation/firepower/morning-glory-firewall-acl.txt
```

### Anubis Example

Configuration:

```text
object network A-R1-REAL
 host 172.16.100.2

object network A-R1-PUBLIC
 host 10.104.104.14

nat (inside1,outside) source static A-R1-REAL A-R1-PUBLIC
```

Additional static translations were created for:

* A-R2
* A-SW1
* A-SW2

Anubis NAT configuration is preserved in:

```text
automation/firepower/anubis-firewall-acl.txt
```

---

## NAT Verification

NAT operation was verified using:

```bash
show running-config nat
```

and

```bash
show nat
```

Validation confirmed:

* Static translation presence
* Active translation usage
* Controller-to-device reachability

---

## SSH Access Design

A dedicated object was created representing the Ubuntu automation controller.

Configuration:

```text
object network ANSIBLE-STATION
 host 10.104.104.5
```

SSH access was permitted only from the automation controller to managed infrastructure devices.

Morning Glory example:

```text
access-list OUTSIDE-IN extended permit tcp object ANSIBLE-STATION host 10.104.104.10 eq 22
access-list OUTSIDE-IN extended permit tcp object ANSIBLE-STATION host 10.104.104.11 eq 22
```

Anubis example:

```text
access-list OUTSIDE-IN extended permit tcp object ANSIBLE-STATION host 10.104.104.14 eq 22
access-list OUTSIDE-IN extended permit tcp object ANSIBLE-STATION host 10.104.104.15 eq 22
```

---

## NETCONF Access Design

To support model-driven automation, NETCONF access was permitted only to routers requiring NETCONF management.

Morning Glory:

```text
access-list OUTSIDE-IN extended permit tcp object ANSIBLE-STATION host 10.104.104.10 eq 830
access-list OUTSIDE-IN extended permit tcp object ANSIBLE-STATION host 10.104.104.11 eq 830
```

Anubis:

```text
access-list OUTSIDE-IN extended permit tcp object ANSIBLE-STATION host 10.104.104.14 eq 830
access-list OUTSIDE-IN extended permit tcp object ANSIBLE-STATION host 10.104.104.15 eq 830
```

This allowed the controller to access NETCONF services while minimizing exposure.

---

## ACL Validation

Access control policies were verified.

Commands:

```bash
show access-list
```

```bash
show running-config access-group
```

Validation confirmed:

* SSH permissions
* NETCONF permissions
* ICMP testing permissions
* ACL application to the outside interface

The firewall successfully enforced management access policies while permitting automation traffic.

---

## SSH Service Validation

Firewall SSH configuration was reviewed.

Commands:

```bash
show ssh
```

```bash
show run ssh
```

```bash
show run aaa
```

```bash
show run username
```

Validation confirmed:

* SSH Version 2
* Authentication configuration
* Allowed management networks
* Administrative accounts

---

## Configuration Artifacts

Firewall automation-support configurations are stored in:

```text
automation/firepower/
├── morning-glory-firewall-acl.txt
└── anubis-firewall-acl.txt
```

These files contain:

* Static NAT translations
* SSH access controls
* NETCONF access controls
* ICMP testing policies
* ACL assignments

The configuration artifacts provide the exact commands used to implement controller access through the Firepower platforms.

---

## Security Considerations

Several security principles were applied:

* Management access restricted to the automation controller
* NETCONF access limited to required routers
* Static NAT used only for managed infrastructure
* ACL enforcement at the perimeter
* Explicit routing and policy control

This reduced unnecessary exposure while preserving automation functionality.

---

## Engineering Significance

The Firepower platforms became critical components of the automation architecture.

Rather than exposing routers directly, all management traffic passed through controlled firewall policies.

Benefits included:

* Centralized access control
* Secure management-plane design
* Controlled NETCONF exposure
* Enterprise-style segmentation
* Scalable branch security

---

## Operational Validation

Successfully validated:

* Firepower platform operation
* Interface architecture
* Routing functionality
* Static NAT translations
* SSH management access
* NETCONF traffic forwarding
* ACL enforcement
* Controller-to-branch connectivity

---

## Evidence

Supporting screenshots and validation artifacts are stored in:

```text
evidence/phase-2/13-firepower-integration/
```

---

## Outcome

The Cisco Firepower 1010 platforms were successfully integrated into the enterprise automation environment and became the secure gateway between the Ubuntu automation controller and branch infrastructure.

This phase established the security foundation required for scalable automation across multiple enterprise sites.
