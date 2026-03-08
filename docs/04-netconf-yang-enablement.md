# NETCONF and YANG Enablement

## Objective

Enable model-driven programmability on the Cisco IOS-XE router and validate NETCONF transport from the Ubuntu automation controller.

---

## Why This Step Was Required

CLI automation works through command execution.

NETCONF enables structured configuration and operational data exchange using YANG models.

This was required to move beyond traditional command scraping.

---

## Router Configuration

NETCONF was enabled manually on the router:


conf t
netconf-yang
end
write memory


---

## Port Validation

NETCONF listens on TCP port 830.

Validation command:

show tcp brief all | include 830


Expected result:


0.0.0.0.830 LISTEN


---

## Controller Validation

From Ubuntu controller:

ssh -p 830 admin@192.168.50.1


---

## Expected NETCONF Response

Successful connection returns NETCONF subsystem banner rather than normal shell access.

This confirms:

* SSH transport operational
* NETCONF subsystem active
* port 830 reachable

---

## SSH Compatibility Still Applied

Because IOS-XE 16.9 uses older crypto defaults, SSH client compatibility remained necessary through the existing SSH config file.

---

## YANG Process Validation

Router process check:


show platform software yang-management process


Confirmed YANG services running.

---

## Engineering Meaning

This step established the management interface required for structured model-driven automation through Ansible NETCONF modules.
