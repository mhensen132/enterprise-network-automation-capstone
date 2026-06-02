# 18 - Multi-VM Provisioning

## Objective

Expand the Infrastructure as Code platform from a single virtual machine deployment to a repeatable multi-server deployment model.

The goal of this phase was to demonstrate that Ansible, KVM, Libvirt, and cloud-init could consistently deploy multiple servers from a centralized automation controller.

This phase established the foundation for service deployment and enterprise application hosting.

---

## Previous State

At the conclusion of Phase 17, the environment successfully deployed a single Ubuntu virtual machine through automation.

Architecture:

```text
Ubuntu Controller
        |
        v
      ub1
 KVM / Libvirt Host
        |
        +---- Ubuntu VM
```

While successful, enterprise environments rarely consist of a single server.

The next objective was to automate the deployment of multiple servers simultaneously.

---

## Multi-VM Deployment Playbook

A dedicated playbook was created.

Artifact:

```text
automation/kvm/create-two-ubuntu-vms.yml
```

Purpose:

* Deploy multiple Ubuntu virtual machines
* Apply cloud-init customization
* Configure networking
* Prepare servers for automation

The playbook expanded the deployment model beyond a single guest.

---

## Inventory Validation

Before deployment, the KVM inventory was reviewed.

Command:

```bash
ansible-inventory -i inventory.yml --graph
```

This verified:

* Inventory structure
* Host group membership
* Ansible inventory parsing

---

## Controller Validation

Connectivity to the KVM host was confirmed.

Command:

```bash
ansible kvm_hosts -m ping
```

Result:

```text
SUCCESS => "ping": "pong"
```

This confirmed that the controller could communicate with the virtualization platform.

---

## Deployment Execution

Multiple virtual machines were deployed using:

```bash
ansible-playbook create-two-ubuntu-vms.yml --ask-become-pass
```

The controller instructed Libvirt to provision and start multiple Ubuntu guests.

---

## Libvirt Validation

Deployment results were verified using:

```bash
ansible kvm_hosts \
-m ansible.builtin.command \
-a "virsh list --all" \
--become \
--ask-become-pass
```

Result:

Multiple Ubuntu virtual machines appeared within the Libvirt inventory.

Validation confirmed:

* Successful provisioning
* Guest registration
* Running VM state

---

## DHCP Lease Validation

Guest network assignments were verified.

Command:

```bash
ansible kvm_hosts \
-m ansible.builtin.command \
-a "virsh net-dhcp-leases default" \
--become \
--ask-become-pass
```

Result:

Multiple DHCP leases were assigned to deployed guests.

Example addresses:

```text
192.168.122.118
192.168.122.157
```

This confirmed:

* Virtual network operation
* DHCP functionality
* Guest connectivity

---

## SSH Connectivity Validation

The newly deployed guests were tested using SSH jump-host access through ub1.

Examples:

```bash
ssh -J mikeh@10.102.1.4 ubuntu@192.168.122.118
```

```bash
ssh -J mikeh@10.102.1.4 ubuntu@192.168.122.157
```

Successful login confirmed:

* Cloud-init completion
* User creation
* SSH key installation
* Guest readiness

---

## Virtual Machine Inventory Development

A dedicated inventory was created for guest management.

Artifact:

```text
automation/kvm/vm-inventory.yml
```

Purpose:

* Track deployed guests
* Support service deployment
* Enable centralized management

The inventory allowed future playbooks to target virtual machines directly rather than the KVM host.

---

## Guest Management Validation

The new inventory was validated.

Command:

```bash
ansible all -i vm-inventory.yml -m ping
```

Result:

```text
SUCCESS => "ping": "pong"
```

The controller successfully communicated with all deployed guests.

---

## Architecture Evolution

Previous architecture:

```text
Ubuntu Controller
        |
        v
      ub1
        |
        +---- Ubuntu VM
```

New architecture:

```text
Ubuntu Controller
        |
        v
      ub1
        |
   +----+----+
   |         |
   v         v

Ubuntu   Ubuntu
 VM 01    VM 02
```

The environment now resembled a small enterprise server infrastructure.

---

## Automation Artifacts

Artifacts stored in:

```text
automation/kvm/
├── create-one-ubuntu-vm.yml
├── create-two-ubuntu-vms.yml
├── inventory.yml
└── vm-inventory.yml
```

---

## Operational Validation

Successfully validated:

* Multi-VM deployment
* Cloud-init customization
* DHCP lease assignment
* SSH access
* Inventory-driven guest management
* Multi-server Ansible communication

---

## Engineering Significance

This phase demonstrated that the automation platform could scale beyond a single server.

The controller could now deploy and manage multiple virtual machines through repeatable Infrastructure as Code workflows.

This capability established the foundation required for service deployment and centralized management.

---

## Evidence

Supporting screenshots and validation artifacts are stored in:

```text
evidence/phase-3/18-multi-vm-provisioning/
```

---

## Outcome

The enterprise automation platform successfully evolved from single-server provisioning into multi-server deployment.

The environment now possessed the infrastructure necessary to host enterprise services and support higher-level automation workflows implemented in subsequent phases.

