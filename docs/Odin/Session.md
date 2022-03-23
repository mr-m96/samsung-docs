---
layout: default
title: Session
description: Manage the Session
parent: Odin
nav_order: 4
---

## Packets
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
### OEM State **(Unknown)**
Write: `0x64(Session) 0x03(OEM State)`
### No OEM check **(Unknown)**
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
