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

```text id="c6u7r8"
conf t
netconf-yang
end
write memory
```

---

## Port Validation

NETCONF listens on TCP port 830.

Validation command:

```text id="k2a0v4"
show tcp brief all | include 830
```

Expected result:

```text id="s3v8n1"
0.0.0.0.830 LISTEN
```

---

## Controller Validation

From Ubuntu controller:

```bash id="q1e4n6"
ssh -p 830 admin@192.168.50.1
```

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

```text id="r8w5g2"
show platform software yang-management process
```

Confirmed YANG services running.

---

## Engineering Meaning

This step established the management interface required for structured model-driven automation through Ansible NETCONF modules.
