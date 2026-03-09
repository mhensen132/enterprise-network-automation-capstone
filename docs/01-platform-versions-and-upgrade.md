
# Platform Versions and Upgrade

## Objective

Stabilize the router platform on a known IOS-XE version before applying baseline configuration or enabling management services.

---

## Router Platform

Cisco ISR4321

---

## Starting Version

IOS-XE 16.3.6

Validated with:

```text
show version
```

The router initially booted from:

```text
bootflash:isr4300-universalk9.16.03.06.SPA.bin
```

---

## Target Version

IOS-XE 16.9.6

Image used:

```text
isr4300-universalk9.16.09.06.SPA.bin
```

---

## Upgrade Method Used

Upgrade performed from USB flash drive.

---

## Pre-Upgrade Validation

### Verify available flash space

```text
dir bootflash:
```

Confirmed sufficient free space before copying image. 

### Verify USB image present

```text
dir usb0:
```

Confirmed router could read USB image before copy. 

---

## Image Copy

Image copied from USB into bootflash:

```text
copy usb0:isr4300-universalk9.16.09.06.SPA.bin bootflash:
```

Copy completed successfully. 

---

## Image Verification

Bootflash checked for new image:

```text
dir bootflash: | include 16.09
```

MD5 verification performed:

```text
verify /md5 bootflash:isr4300-universalk9.16.09.06.SPA.bin
```



---

## Filename Correction

During copy, the image initially appeared in bootflash with incorrect filename:

```text
bootflash:y
```

The file verified successfully, so it was renamed rather than copied again:

```text
rename bootflash:y bootflash:isr4300-universalk9.16.09.06.SPA.bin
```



---

## Boot Variable Update

Existing boot statement cleared:

```text
conf t
no boot system
```

New boot image set:

```text
boot system bootflash:isr4300-universalk9.16.09.06.SPA.bin
```

Saved configuration:

```text
end
write memory
```



---

## Boot Verification

Before reload:

```text
show boot
```

Confirmed boot variable pointed to 16.9.6 image. 

---

## Reload

Router reloaded:

```text
reload
```

---

## Post-Upgrade Validation

After reboot:

```text
show version
```

Confirmed:

* IOS-XE 16.9.6
* ISR4321 platform
* new system image active



---

## Immediate Automation Validation

After upgrade:

```text
show platform software yang-management process
```

Confirmed YANG processes active. 

NETCONF enabled:

```text
conf t
netconf-yang
end
write memory
```

Port 830 verified:

```text
show tcp brief all | include 830
```

Expected result:

```text
0.0.0.0.830 LISTEN
```



---

## Engineering Meaning

The upgrade to 16.9.6 was the turning point where NETCONF became stable enough for Ansible model-driven automation.
