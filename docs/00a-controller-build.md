# Ubuntu Automation Controller Build

## Objective

Prepare the Ubuntu controller to manage Cisco IOS-XE devices using Ansible and NETCONF.

## Platform

Ubuntu 24.04 LTS

## Package Installation

Update package index:

```bash
sudo apt update
```

Install Ansible:

```bash
sudo apt install ansible
```

Install required collections:

```bash
ansible-galaxy collection install cisco.ios
ansible-galaxy collection install ansible.netcommon
```

## SSH Tools Used

Verify NETCONF port:

```bash
nc -zv 192.168.50.1 830
```

Add NETCONF host key:

```bash
ssh-keyscan -p 830 192.168.50.1 >> ~/.ssh/known_hosts
```

## Result

Ubuntu controller prepared for:

* CLI automation
* NETCONF retrieval
* NETCONF configuration push
