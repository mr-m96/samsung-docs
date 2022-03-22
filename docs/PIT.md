---
layout: default
title: PIT
description: Samsung Partition Table Format
nav_order: 4
---

## Warning
The PIT format is not yet fully documented and most of it's parts are unknown. \
Also things vary from device to device.

## Header
`0x12349876` is the magic number that is located in first 4 bytes of a PIT file. \
### Magic Number
It's value is always `0x12349876`.
### Count of partitions
Type: 32-bit integer
### GANG name
It's value is always `COM_TAR2`. \
Type: 8-char long string
### Project Name (???)
Type: 8-char long string
### Protocol Version or LUN count
Type: 32-bit integer

## Entries
Entries begin after first 28 bytes. \
One entry is 132 bytes long.
### Binary Type
Type: 32-bit number \
Value: 
* AP/Phone = 0
* CP/Modem = 1

### Device Type
Block Size: 512 (4096 for rpmb) \
Type: 32-bit number \
Value:
* NAND = 1
* EMMC = 2
* SPI = 3
* IDE = 4
* NANDX16 = 5
* NOR = 6
* NANDWB1 = 7
* UFS = 8

### Identitifier
Type: 32-bit number

### Attributes
Type: 32-bit number \
Flags:
* STL = 0x0001
* Write = 0x0010

### Update Attributes
Type: 32-bit number \
Flags:
* FOTA = 0x0001
* Secure = 0x0010

### Start Block
Type: 32-bit number
### Block count
Type: 32-bit number
### LUN (used unly for UFS)
Type: 32-bit number
### File Size
Type: 32-bit number
### Partition name
Type: 32 characters long string
### File name
Type: 32 characters long string
### FOTA name
Type: 32 characters long string
