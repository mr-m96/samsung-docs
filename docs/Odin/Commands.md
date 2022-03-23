---
layout: default
title: Commands
description: Pre-handshake commands
parent: Odin
nav_order: 1
---

## Other commands
### Rooting
Does nothing according to source code. \
Write: `ROOTING` \
Read: `DDI`

### ATQ0
Does nothing according to source code. \
Write: `ATQ0` \
Read: `OKAY`

### RESET
Shuts down the device. \
Write: `RESET` \
Read: `+RESET: OK\n`

### PROMPT
Allowed commands:
```
getenv: Get environment data
setenv REBOOT_MODE: Sets REBOOT_MODE
setenv FORCE_UPLOAD: Sets FORCE_UPLOAD
setenv DEBUG_LEVEL: Sets DEBUG_LEVEL
setenv CMDLINE: Sets CMDLINE
setenv POWER_MARGIN: Sets POWER_MARGIN
saveenv: Saves environment data
reset: Resets environment data
```
Write: `PROMT<command>` \
Read: `<response>`

### SECCMD
Commands:
```
0x01 - Forcefully set the warranty bit
0x02 - Clear EMC token
```
Write: `SECCMD<command>` \
Read: `<response>`

### DVIF
Outputt differs between device models. \
Write: `DVIF` \
Read: 
```
General: @#MODEL=<Product Name>;VENDOR=<Chip Vendor>;FWVER=<Revision>;SALES=<Region Code>;VER=<Build Version>;DID=<DID>;RAND=<Base64 Encoded random string>;TMU_TEMP=<CPU Temperature>@#
For UFS: @#UN=<UFS ID>;CAPA=<UFS Size>;PRODUCT=<UFS name>@#
For MMC: @#UN=<MMC ID>;CAPA=<GB Size>PRODUCT=<MMC name>@#
```
