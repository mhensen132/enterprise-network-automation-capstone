# 15 - KVM Libvirt Platform

## Objective

Establish a Linux-based virtualization platform capable of supporting Infrastructure as Code workflows using Ansible, KVM, and Libvirt.

The goal of this phase was to expand the project beyond network automation and build a platform capable of deploying and managing virtual servers programmatically.

---

## Why KVM

The previous phases focused on automating network infrastructure:

* Cisco IOS-XE Routers
* NETCONF
* YANG
* Firepower Security Controls

The next objective was to automate server infrastructure.

Requirements included:

* Linux-native virtualization
* Enterprise scalability
* Ansible integration
* Infrastructure as Code support
* Automated virtual machine deployment

KVM and Libvirt were selected because they provide enterprise-class virtualization capabilities directly integrated into Linux.

---

## Platform Architecture

The Ubuntu automation controller managed a dedicated virtualization host.

```text
Ubuntu Automation Controller
            |
            v
      ub1 (10.102.1.4)
      KVM / Libvirt Host
            |
            +---- Virtual Machines
```

The controller communicated with the KVM host using SSH and Ansible.

---

## KVM Host Information

| Item     | Value              |
| -------- | ------------------ |
| Hostname | ub1                |
| Address  | 10.102.1.4         |
| User     | mikeh              |
| Platform | Ubuntu Linux       |
| Role     | KVM / Libvirt Host |

---

## Initial Host Validation

Before automation could begin, direct SSH connectivity was verified.

Command:

```bash
ssh mikeh@10.102.1.4
```

Successful login confirmed:

* Network connectivity
* SSH functionality
* Administrative access
* Host readiness

This established the management path between the controller and the virtualization host.

---

## KVM Automation Workspace

A dedicated project workspace was created.

Commands:

```bash
mkdir kvm-automation
cd kvm-automation
```

Inventory and controller configuration files were then created:

```bash
nano inventory.yml
nano ansible.cfg
```

This separated virtualization automation from the network automation project used during earlier phases.

---

## Initial Ansible Validation

Connectivity between the controller and the KVM host was validated.

Command:

```bash
ansible kvm_hosts -m ping --ask-pass --ask-become-pass
```

Additional validation:

```bash
ansible kvm_hosts -m ansible.builtin.command -a "hostname" --ask-pass --ask-become-pass
```

Result:

The controller successfully communicated with the KVM host and retrieved host information.

---

## KVM Discovery

A discovery playbook was created to inspect the virtualization platform.

Artifact:

```text
automation/kvm/discover-kvm-host.yml
```

Execution:

```bash
ansible-playbook discover-kvm-host.yml --ask-pass --ask-become-pass
```

Discovery results were reviewed using:

```bash
cat ub1-kvm-discovery.txt
```

The playbook collected information about:

* Operating system
* CPU resources
* Memory
* KVM support
* Libvirt status

---

## Host Preparation

A dedicated preparation playbook was created.

Artifact:

```text
automation/kvm/prepare-kvm-host.yml
```

Execution:

```bash
ansible-playbook prepare-kvm-host.yml --ask-pass --ask-become-pass
```

After execution, KVM functionality was validated.

Command:

```bash
ansible kvm_hosts \
-m ansible.builtin.command \
-a "which cloud-localds" \
--become \
--ask-pass \
--ask-become-pass
```

Successful output confirmed cloud-init tooling was available.

---

## SSH Key Authentication

To support unattended Infrastructure as Code workflows, SSH key-based authentication was implemented.

Generate SSH key pair:

```bash
ssh-keygen -t ed25519 -C "ansible-controller"
```

Verify key creation:

```bash
ls -l ~/.ssh/id_ed25519*
cat ~/.ssh/id_ed25519.pub
```

Install the public key on the KVM host:

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub mikeh@10.102.1.4
```

Verification:

```bash
ansible kvm_hosts -m ping
```

Result:

```text
SUCCESS => "ping": "pong"
```

The controller could now communicate with the KVM host without interactive password prompts.

Benefits included:

* Non-interactive automation
* Improved Ansible reliability
* Infrastructure as Code support
* Simplified playbook execution

---

## Libvirt Validation

The virtualization environment was validated using:

```bash
ansible kvm_hosts \
-m ansible.builtin.command \
-a "virsh list --all" \
--become \
--ask-become-pass
```

Additional validation:

```bash
ansible kvm_hosts \
-m command \
-a "free -h" \
--become \
--ask-become-pass
```

These commands confirmed:

* Libvirt operation
* Virtualization readiness
* Available host resources

---

## Automation Artifacts

Artifacts stored in:

```text
automation/kvm/
├── discover-kvm-host.yml
├── prepare-kvm-host.yml
├── inventory.yml
├── vm-inventory.yml
└── ub1-kvm-discovery.txt
```

---

## Operational Validation

Successfully validated:

* SSH connectivity to ub1
* Ansible communication
* KVM host discovery
* Libvirt readiness
* Cloud-init tooling
* SSH key authentication
* Infrastructure inventory creation

---

## Engineering Significance

This phase marked the transition from network automation into infrastructure automation.

Previous phases focused on:

```text
Network Devices
    |
    +-- Routers
    +-- Switches
```

This phase introduced:

```text
Virtual Infrastructure
    |
    +-- Hypervisor
    +-- Virtual Machines
    +-- Services
```

The project now possessed the capability to automate both network infrastructure and server infrastructure from a centralized controller.

---

## Evidence

Supporting screenshots and validation artifacts are stored in:

```text
evidence/phase-3/15-kvm-libvirt-platform/
```

---

## Outcome

The ub1 KVM host was successfully integrated into the enterprise automation environment and prepared for Infrastructure as Code workflows.

This platform became the foundation for automated virtual machine deployment, cloud-init customization, web services, and centralized logging implemented in the following phases.

