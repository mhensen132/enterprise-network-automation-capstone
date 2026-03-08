# Management Plane Design

## Objective

Create a dedicated management path between the Ubuntu automation controller and the Cisco infrastructure so automation traffic remains isolated and predictable.

---

## Dedicated Management Subnet

192.168.50.0/24

---

## Device Roles

| Device            | Role              | Address        |
| ----------------- | ----------------- | -------------- |
| Ubuntu Controller | Automation Host   | 192.168.50.100 |
| SW1               | Management Switch | 192.168.50.2   |
| R1                | Managed Router    | 192.168.50.1   |

---

## Physical Topology

Ubuntu controller connected to switch infrastructure, with switch forwarding management traffic to router R1.

Traffic path:

Ubuntu → SW1 → R1

---

## Why a Dedicated Management Plane Was Used

A dedicated management plane improves:

* Predictability during troubleshooting
* Isolation of automation traffic
* Reduced dependency on production forwarding paths
* Cleaner protocol validation during early automation testing

---

## Validation Performed

### Ping test to router

ping 192.168.50.1

### Ping test to switch

ping 192.168.50.2


---

## Result

Layer 3 connectivity to both switch and router was confirmed before beginning SSH and automation validation.

---

## Engineering Importance

Successful reachability to both devices confirmed that the management subnet was functioning before introducing Ansible and NETCONF.

