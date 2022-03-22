---
layout: default
title: PIT
description: PIT flashing and dumping 
parent: Odin
nav_order: 2
---

## How-to guide
// TODO: How-to guide

## Packets
### Flashing
#### Request PIT flash
Write: `0x65(PIT) 0x00(Flash)` \
Read: `0x65(PIT) 0x00(Always)`
#### Begin PIT Flash
Write: `0x65(PIT) 0x02(Begin) <32bit>(Byte Length)` \
Read: `0x65(PIT) 0x00(Always)`
#### Send PIT data
Write: `<PIT data buffer>` \
Read: `0x65(PIT) 0x00(Always)`
#### End PIT Flash
Write: `0x65(PIT) 0x03(End)` \
Read: `0x65(PIT) 0x00(Always)`
### Dumping
#### Request PIT data dump
Write: `0x65(PIT) 0x01(Dump)` \
Read: `0x65(PIT) 0x4000(File Size)`
#### Dump PIT data block
One block is 500 bytes. \
Send an empty packet after last block. \
Write: `0x65(PIT) 0x02(FilePart) <32bit>(Index)` \
Read: `<PIT data block>`
#### End PIT dump
Identical to End PIT flash. \
Write: `0x65(PIT) 0x03(End)` \
Read: `0x65(PIT) 0x00(Always)`
