# Initial Device Preparation

## Objective

Prepare the router and switch manually so they could participate in the management network and support automation.

---

## Router Initial Configuration Performed Manually

The router was first configured through direct CLI access.

### Baseline items configured

* hostname
* management IP address
* interface activation
* local user credentials
* enable secret
* domain name
* SSH keys
* VTY access

### SSH enablement steps included


ip domain-name lab.local
crypto key generate rsa
username admin secret <redacted>
line vty 0 4
 login local
 transport input ssh


---

## NETCONF Enablement

After SSH was working, NETCONF was enabled manually:


netconf-yang


---

## Switch Initial Configuration Performed Manually

The switch was configured to support the management subnet.

### Baseline items configured

* hostname
* VLAN interface for management
* management IP address
* default gateway

---

## Why Manual Preparation Was Required

Automation cannot begin until devices are reachable and management services are active.

Manual baseline preparation established the first controllable state.

---

## Engineering Meaning

This phase created the minimum viable network state required before Ansible and NETCONF could be tested.
