---
layout: default
title: Odin
description: Odin Protocol v3/v4
nav_order: 3
---

## Warning
If you just wanna to flash firmware onto a Samsung phone, click Hreidmar button below, or use leaked Odin. \
It is an implementation of this protocol, tested on Samsung Galaxy A20s.

## Defaults
* Read/Write timeout for file transfer: 30000 (30 seconds)
* Packet size for file transfer: 131072 (128 KiB)
* File transfer max sequence size: 800 (Packets)

## Terminology
* Sequence: Size is *Packet size for file transfer* multiplied by *File transfer max sequence size*
* File Part: Size is *Packet size for file transfer*.
* IPoRE = In the Process of Reverse-Engineering

## Other
* `Unknown` should be always 0x00. If it's not, you're in trouble.
* All non-flash packets should be sent with buffer length 1024, else the packet will be rejected.

## Handshakes
Before any upload or download can take place, a handshake is done.

### ODIN
Write: `ODIN` \
Read: `LOKE`

### FPGM
Does the same thing as the usual ODIN handshake. \
Write: `FPGM` \
Read: `OK`

### THOR **(IPoRE)**
`THOR` Seem to be a synonym for `ODIN` on some (newer?) devices, file transfers work just as when `ODIN` is sent. \
There also exists a [thor download protocol](https://lists.denx.de/pipermail/u-boot/2013-October/164088.html) that Tizen/Samsung uses in their development boards so `THOR` could possibly have something to do with this mode. Simply doing a `THOR` handshake and trying to flash something with [lthor](https://git.tizen.org/cgit/tools/lthor/) does not work though. \
Write: `THOR` \
Read: `LOKE`

## Other commands
### Rooting **(IPoRE)**
Does nothing according to source code. \
Write: `ROOTING` \
Read: `DDI`

### ATQ0
Does nothing according to source code. \
Write: `ATQ0` \
Read: `OKAY`

### PROMPT **(IPoRE)**
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
Write: `SECCMD<command>` \
Read: `<unknown>`

### SECCMD **(IPoRE)**
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
@#MODEL=<model>;UN=<unknown>;CAPA=<unknown>;VENDOR=SAMSUNG;FWVER=<probably efuse>;PRODUCT=<flash storage name>;SALES=<region>;VER=<firmware version>;DID=<unknown>;RAND=(base64 encoded random string)@#
```
or
```
@#MODEL=<model>;UN=<unknown>;CAPA=<unknown>;VENDOR=SAMSUNG;FWVER=<probably efuse>;PRODUCT=<flash storage name>;PROV=<unknown>;SALES=<region>;VER=<firmware version>;DID=<device/chip id>;@#
```

## Session (0x64)
### Begin Session
Write: `0x64(Session) 0x00(Begin) <Protocol Version>(0x00, 0x03, 0x04 or 0x05)`
Read: `0x64(Session) 0x00(Response Ack)`

The Response Ack would be:
* If the `<Protocol Version>` is 0, would return `0x20000`
* If the `<Protocol Version>` is lower than bootloader's,
    * it would return `(<Protocol Version> << 16) | 0x0`
* In other cases it would return:
    * `(<Bootloader Protocol Version> << 16) | 0x0`

If Response Ack is not 0:
* Read/Write timeout for file transfer: 120000 (2 minutes)
* Packet size for file transfer: 1048576 (1 MiB)
* File transfer max sequence size: 30 (Packets)
* Send `0x64(Session) 0x05(FilePartSize) 0x100000(Packet Size)`
* Read `0x64(Session) 0x00(Always)`

### Device Type (or Reset Timer)
May always return zero. \
Write: `0x64(Session) 0x01(Device Type)` \
Read: `0x64(Session) <32bit>(Device Type)`
### Erase userdata partition
**WARNING!** This will do a factory reset. \
Write: `0x64(Session) 0x07(Erase)` \
Read: `0x64(Session) 0x00(Status Code)`
### Set Total Bytes
Write: `0x64(Session) 0x02(Total Bytes) <64bit>(Bytes Length)` \
Read: `0x64(Session) 0x00(Always)`
### OEM State **(IPoRE)**
Write: `0x64(Session) 0x03(OEM State)`
### No OEM check **(IPoRE)**
Write: `0x64(Session) 0x04(No OEM Check)`
### Enable T-Flash
Write: `0x64(Session) 0x08(T-Flash)` \
Read: `0x64(Session) 0x00(Always)`
### Change device's region code
Write: `0x64(Session) 0x09(Sales Code) <3bytes>(RegionCode)` \
Read: `0x64(Session) 0x00(Always)`
### Enable RTN config on boot
Write: `0x64(Session) 0x0A(RTN Config)` \
Read: `0x64(Session) 0x00(Always)`
## End Session (0x67)
### End Session
Write: `0x67(End Session) 0x00(End)` \
Read: `0x67(End Session) 0x00(Always)`
### Reboot
Write: `0x67(End Session) 0x01(Reboot)` \
Read: `0x67(End Session) 0x00(Always)`
### Reboot into ODIN
Would restart the device, and boot into ODIN again. \
Write: `0x67(End Session) 0x02(Reboot into ODIN)` \
Read: `0x67(End Session) 0x00(Always)`
### Shutdown
This command is unsupported on some devices. \
Write: `0x67(End Session) 0x03(Shutdown)` \
Read: `0x67(End Session) 0x00(Always)`
## PIT (0x65)
### Flashing
#### Request PIT flash
Write: `0x65(PIT) 0x00(Flash)` \
Read: `0x65(PIT) <32bit>(Untested)`
#### Begin PIT Flash
Write: `0x65(PIT) 0x02(Begin) <32bit>(Byte Length)` \
Read: `0x65(PIT) <32bit>(Untested)`
#### Send PIT data
Write: `<PIT data buffer>` \
Read: `0x65(PIT) <32bit>(Untested)`
#### End PIT Flash
Write: `0x65(PIT) 0x03(End)` \
Read: `0x65(PIT) <32bit>(Untested)`
### Dumping
#### Request PIT data dump
Write: `0x65(PIT) 0x01(Dump)` \
Read: `0x65(PIT) <32bit>(File Size)`
#### Dump PIT data block
One block is 500 bytes. \
Send an empty packet after last block. \
Write: `0x65(PIT) 0x02(FilePart) <32bit>(Index)` \
Read: `<PIT data block>`
#### End PIT dump
Identical to End PIT flash. \
Write: `0x65(PIT) 0x03(End)` \
Read: `0x65(PIT) <32bit>(Untested)`
## File (0x66)
### Request file flash
Write: `0x66(File) 0x00(Flash)` \
Read: `0x66(File) <32bit>(Untested)`
### Begin file sequence flash
Write: `0x66(File) 0x02(FilePart) <32bit>(Byte Length)` \
Read: `0x66(File) <32bit>(Untested)`
### Flash a file part
Send an empty packet for the first file part. \
Write: `<File part data>` \
Read: `0x00(File Part) <32bit>(File Part Index)`
### End file sequence flash: MODEM
Send an empty packet before and after. \
Write: `0x66(File) 0x03(End) 0x01(Modem/CP) <32bit>(Sequence Byte Length) 0x00(Unknown) <32bit>(Device Type) <32bit>(Is last Sequence)` \
Read: `0x66(File) <32bit>(Untested)`
### End file sequence flash: PHONE
Send an empty packet before and after. \
Write: `0x66(File) 0x03(End) 0x00(Phone/AP) <32bit>(Sequence Byte Length) 0x00(Unknown) <32bit>(Device Type) <32bit>(Identifier) <32bit>(Is last Sequence)` \
Read: `0x66(File) <32bit>(Untested)`
