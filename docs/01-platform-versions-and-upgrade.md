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

show version

The router initially booted from:

bootflash:isr4300-universalk9.16.03.06.SPA.bin

---

## Target Version

IOS-XE 16.9.6

Image used:

isr4300-universalk9.16.09.06.SPA.bin

---

## Upgrade Method Used

Upgrade performed from USB flash drive.

---

## Pre-Upgrade Validation

### Verify available flash space

dir bootflash:


Confirmed sufficient free space before copying image. 

### Verify USB image present

dir usb0:

Confirmed router could read USB image before copy. 

---

## Image Copy

Image copied from USB into bootflash:

copy usb0:isr4300-universalk9.16.09.06.SPA.bin bootflash:


Copy completed successfully. 

---

## Image Verification

Bootflash checked for new image:


dir bootflash: | include 16.09


MD5 verification performed:


verify /md5 bootflash:isr4300-universalk9.16.09.06.SPA.bin


---

## Filename Correction

During copy, the image initially appeared in bootflash with incorrect filename:

bootflash:y

The file verified successfully, so it was renamed rather than copied again:


rename bootflash:y bootflash:isr4300-universalk9.16.09.06.SPA.bin


---

## Boot Variable Update

Existing boot statement cleared:

conf t
no boot system

New boot image set:


boot system bootflash:isr4300-universalk9.16.09.06.SPA.bin


Saved configuration:


end
write memory

---

## Boot Verification

Before reload:


show boot


Confirmed boot variable pointed to 16.9.6 image. 

---

## Reload

Router reloaded:

reload

---

## Post-Upgrade Validation

After reboot:


show version


Confirmed:

* IOS-XE 16.9.6
* ISR4321 platform
* new system image active



---

## Immediate Automation Validation

After upgrade:


show platform software yang-management process

Confirmed YANG processes active. 

NETCONF enabled:

conf t
netconf-yang
end
write memory

Port 830 verified:


show tcp brief all | include 830


Expected result:


0.0.0.0.830 LISTEN


---

## Engineering Meaning

The upgrade to 16.9.6 was the turning point where NETCONF became stable enough for Ansible model-driven automation.
