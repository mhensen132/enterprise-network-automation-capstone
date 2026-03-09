# SSH Access and Crypto Compatibility

## Objective

Establish SSH access from the Ubuntu automation controller to the Cisco router after baseline configuration and management connectivity were confirmed.

---

## Initial Problem

The router accepted SSH, but Ubuntu 24.04 OpenSSH rejected the router’s older default cryptographic algorithms.

Initial connection attempts failed because modern OpenSSH disables several legacy algorithms by default.

---

## Router Side Preparation

SSH had already been enabled during baseline device preparation:

```text id="v7c0g7"
ip domain-name lab.local
crypto key generate rsa
username admin secret <redacted>
line vty 0 4
 login local
 transport input ssh
```

---

## Initial SSH Failure

Ubuntu returned key exchange errors indicating unsupported negotiation parameters.

Typical failure involved:

* legacy key exchange methods
* legacy RSA host key handling

---

## Working SSH Command

SSH succeeded after explicitly allowing older algorithms:

```bash id="lzqu2o"
ssh -oKexAlgorithms=+diffie-hellman-group14-sha1 \
-oHostKeyAlgorithms=+ssh-rsa \
-oPubkeyAcceptedAlgorithms=+ssh-rsa admin@192.168.50.1
```

---

## Why This Was Necessary

Ubuntu 24.04 OpenSSH uses stricter defaults than the IOS-XE 16.9 SSH stack.

The router still offered older algorithms that modern OpenSSH blocks unless explicitly enabled.

---

## Validation

Successful login confirmed:

* SSH transport operational
* router credentials working
* management path stable

---

## Engineering Meaning

This step established the first remote management path required before Ansible inventory testing could begin.

---

## Permanent SSH Client Fix

To avoid entering the full compatibility command repeatedly, an SSH client configuration file was added on the Ubuntu controller.

File used:

```bash id="5fdm4s"
~/.ssh/config
```

Configuration added:

```text id="mrgd9m"
Host 192.168.50.1
    KexAlgorithms +diffie-hellman-group14-sha1
    HostKeyAlgorithms +ssh-rsa
    PubkeyAcceptedAlgorithms +ssh-rsa
```

---

## Result

After this change, normal SSH worked:

```bash id="op8r5v"
ssh admin@192.168.50.1
```

without needing the full compatibility options each time.

---

## Why This Was Better

This moved the compatibility fix into controller configuration rather than repeating manual command-line overrides.



