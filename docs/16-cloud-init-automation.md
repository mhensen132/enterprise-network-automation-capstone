# 16 - Cloud-Init Automation

## Objective

Implement cloud-init as the guest customization mechanism for automated virtual machine deployment.

The goal of this phase was to eliminate manual operating system installation and post-installation configuration by allowing virtual machines to self-configure during first boot.

This capability established the foundation for repeatable Infrastructure as Code deployments.

---

## Why Cloud-Init

Traditional virtual machine deployment typically requires:

* Manual operating system installation
* User account creation
* Network configuration
* SSH configuration
* Initial system customization

These tasks become increasingly difficult to manage as the number of virtual machines grows.

Cloud-init allows virtual machines to automatically perform these tasks during their first boot.

Benefits include:

* Consistency
* Repeatability
* Reduced deployment time
* Infrastructure as Code support

---

## Platform Integration

Cloud-init was integrated into the KVM automation environment established during the previous phase.

Platform:

```text id="xq8krh"
Ubuntu Controller
        |
        v
      ub1
 KVM / Libvirt Host
        |
        +---- Cloud-Init Enabled VMs
```

This allowed Ansible to provision virtual machines that automatically configured themselves during deployment.

---

## Environment Validation

Before implementing cloud-init automation, the required tooling was verified.

Command:

```bash id="1h6oqt"
ansible kvm_hosts \
-m ansible.builtin.command \
-a "which cloud-localds" \
--become \
--ask-pass \
--ask-become-pass
```

Result:

Successful output confirmed that cloud-init image creation tools were available on the KVM host.

---

## SSH Trust Foundation

Cloud-init deployment relied on the SSH trust relationship established during the previous phase.

SSH key creation:

```bash id="n7h62x"
ssh-keygen -t ed25519 -C "ansible-controller"
```

Public key installation:

```bash id="qjywh7"
ssh-copy-id -i ~/.ssh/id_ed25519.pub mikeh@10.102.1.4
```

This allowed the controller to automate KVM operations without repeated password prompts.

---

## Cloud-Init Design

The cloud-init workflow was designed to automatically configure newly deployed Ubuntu virtual machines.

Automated configuration included:

* Hostname assignment
* User creation
* SSH key installation
* Initial network configuration
* First-boot customization

The objective was to produce usable servers immediately after deployment.

---

## Automation Workflow

The deployment process followed this sequence:

```text id="4o2mbg"
Ansible Playbook
        ↓
Cloud-Init Data
        ↓
Virtual Machine Creation
        ↓
First Boot
        ↓
Automatic Configuration
        ↓
Ready for Management
```

This removed the need for manual guest configuration.

---

## Inventory Preparation

Infrastructure inventories were expanded to support future guest management.

Artifacts:

```text id="srhhkp"
automation/kvm/
├── inventory.yml
└── vm-inventory.yml
```

Purpose:

* Define virtualization hosts
* Track deployed virtual machines
* Support future playbook execution

---

## Relationship to VM Deployment

Cloud-init itself does not create virtual machines.

Instead, cloud-init provides the customization mechanism used by the deployment playbooks that follow.

Future playbooks would leverage cloud-init to:

* Deploy Ubuntu guests
* Configure users
* Install SSH keys
* Prepare systems for automation

This capability is demonstrated in the next phase.

---

## Automation Artifacts

Artifacts associated with cloud-init preparation:

```text id="cjlwm6"
automation/kvm/
├── inventory.yml
├── vm-inventory.yml
├── discover-kvm-host.yml
├── prepare-kvm-host.yml
└── create-one-ubuntu-vm.yml
```

The VM deployment playbook is introduced in the next phase.

---

## Operational Validation

Successfully validated:

* Cloud-init tooling availability
* SSH trust relationships
* KVM host readiness
* Infrastructure inventory preparation
* Automated guest customization design

---

## Engineering Significance

Cloud-init represented a major advancement in the automation platform.

Previous infrastructure deployment required manual operating system installation and configuration.

Cloud-init introduced the ability for servers to self-configure during deployment, significantly reducing administrative effort and improving consistency.

This capability became the foundation for fully automated server deployment.

---

## Evidence

Supporting screenshots and validation artifacts are stored in:

```text id="3hms2s"
evidence/phase-3/16-cloud-init-automation/
```

---

## Outcome

The enterprise automation platform successfully integrated cloud-init into the virtualization workflow.

The environment was now prepared to deploy fully customized Ubuntu virtual machines through automated playbook execution, eliminating the need for manual guest configuration.

