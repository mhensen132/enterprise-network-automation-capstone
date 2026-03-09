# YANG Model Exploration

## Objective

Understand the YANG models behind NETCONF data returned from the router and map XML output to model structure.

---

## Why This Step Was Required

NETCONF returns structured XML, but the XML only becomes useful when mapped to its YANG model.

Understanding the model hierarchy allows targeted retrieval instead of pulling large sections of configuration blindly.

---

## Primary Models Used

Two model families were examined:

* Cisco native YANG models
* IETF standard models

---

## Cisco Native Model

Primary model observed in returned XML:

```text id="j7m4v3"
Cisco-IOS-XE-native
```

Typical XML namespace:

```xml id="n1r8q2"
xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-native"
```

---

## IETF Model Used for Interface Study

Standard model examined:

```text id="g5u2w9"
ietf-interfaces.yang
```

---

## Model Inspection Tool

The Ubuntu controller used `pyang` to inspect YANG structure.

Command:

```bash id="r2f7y6"
pyang -f tree ietf-interfaces.yang
```

---

## Why pyang Was Useful

`pyang` converted raw model definitions into readable hierarchy.

This made it easier to identify:

* container names
* list keys
* leaf values

---

## Example Hierarchy Observed

```text id="u8w1m5"
+--rw interfaces
   +--rw interface* [name]
      +--rw name
      +--rw description
      +--rw enabled
```

---

## Practical Effect on Automation

Understanding model structure allowed more targeted NETCONF retrievals later instead of requesting full running configuration.

---

## Engineering Meaning

This step connected returned XML to formal YANG definitions and made model-driven automation predictable rather than experimental.
