# Troubleshooting Log

## Objective

Record major technical problems encountered during the project and the corrective actions used to resolve them.

---

## 1. SSH Crypto Negotiation Failure

### Problem

Ubuntu 24.04 OpenSSH rejected older IOS-XE default cryptographic algorithms.

Initial SSH attempts failed.

---

### Cause

IOS-XE 16.9.6 still offered legacy key exchange and RSA host key behavior blocked by modern OpenSSH defaults.

---

### Correction

Temporary working command:


ssh -oKexAlgorithms=+diffie-hellman-group14-sha1 \
-oHostKeyAlgorithms=+ssh-rsa \
-oPubkeyAcceptedAlgorithms=+ssh-rsa admin@192.168.50.1


Permanent fix added to:


~/.ssh/config


---

## 2. Ansible Inventory Execution Failure

### Problem

Ansible inventory was not recognized correctly.

---

### Cause

Command executed outside project working directory.

---

### Correction

Moved into project folder:


cd ~/ansible-lab


---

## 3. IOS Image Filename Issue During Upgrade

### Problem

Copied IOS image appeared incorrectly as:


bootflash:y


---

### Cause

Filename handling during USB copy did not preserve full image name.

---

### Correction

Verified file integrity and renamed:


rename bootflash:y bootflash:isr4300-universalk9.16.09.06.SPA.bin


---

## 4. NETCONF Validation Required Explicit Port Check

### Problem

NETCONF enablement alone did not guarantee transport readiness.

---

### Correction

Port verified:


show tcp brief all | include 830


Expected result:


0.0.0.0.830 LISTEN


---

## 5. Ansible SSH Transport Warning

### Problem

Ansible reported transport fallback:


ansible-pylibssh not installed, falling back to paramiko


---

### Result

Execution still succeeded.

No functional failure occurred.

---

## Engineering Meaning

Troubleshooting was not separate from the project; it was part of building a stable automation path.
