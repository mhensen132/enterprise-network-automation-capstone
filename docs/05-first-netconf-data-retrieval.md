# First NETCONF Data Retrieval

## Objective

Retrieve structured operational data from the Cisco IOS-XE router using Ansible NETCONF modules.

---

## Why This Step Matters

CLI automation returns plain text.

NETCONF returns structured XML tied to YANG models.

This allows predictable automation and data parsing.

---

## First NETCONF Playbook

Playbook used:


- name: Retrieve interface data using NETCONF
  hosts: R1
  connection: netconf
  gather_facts: no

  tasks:
    - name: Get interface information
      ansible.netcommon.netconf_get:
        source: running
      register: interfaces

    - name: Display result
      debug:
        var: interfaces


---

## Inventory Adjustment for NETCONF

Inventory required NETCONF transport:


all:
  hosts:
    R1:
      ansible_host: 192.168.50.1
      ansible_user: admin
      ansible_password: <redacted>
      ansible_connection: netconf
      ansible_network_os: ios


---

## Playbook Execution

Command used:

ansible-playbook -i inventory.yml netconf-get.yml


---

## Result

Structured XML returned successfully from the router.

This confirmed:

* NETCONF authentication working
* port 830 functional
* Ansible NETCONF transport operational

---

## Initial Output Type

Returned data included XML representing running configuration content.

Typical returned structure:


<data>
  <native xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-native">
    ...
  </native>
</data>


---

## Why XML Matters

XML output maps directly to YANG model hierarchy.

This allows:

* precise subtree retrieval
* structured parsing
* targeted automation

---

## Engineering Meaning

This was the first successful model-driven data extraction from the router using standards-based automation.
