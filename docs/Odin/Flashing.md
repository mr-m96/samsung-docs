---
layout: default
title: Flashing
description: Flash partitions with Odin
nav_order: 1
---

## Defaults
* Read/Write timeout for file transfer: 30000 (30 seconds)
* Packet size for file transfer: 131072 (128 KiB)
* File transfer max sequence size: 800 (Packets)

## Terminology
* Sequence: Size is *Packet size for file transfer* multiplied by *File transfer max sequence size*
* File Part: Size is *Packet size for file transfer*.

## How-to guide
1) Request a file flash
2) Divide the file into sequences
3) Begin file sequence flash
4) Divide the sequence into file parts
5) Send the file part (in raw format)
6) Read the file part response
7) Send End file sequence flash:
    * If you finished sending file parts
    * Send the AP/CP packet accordingly
8) Repeat until the end of the file

## Documentation
### Request file flash
Write: `0x66(File) 0x00(Flash)` \
Read: `0x66(File) 0x00(Always)`
### Begin file sequence flash
Write: `0x66(File) 0x02(FilePart) <32bit>(Byte Length)` \
Read: `0x66(File) 0x00(On Success)`
### Flash a file part
Send an empty packet for the first file part. \
Write: `<File part data>` \
Read: `0x00(File Part) <32bit>(File Part Index)`
### End file sequence flash: MODEM
Send an empty packet before and after. \
Write: `0x66(File) 0x03(End) 0x01(Modem/CP) <32bit>(Sequence Byte Length) 0x00(Unknown) <32bit>(Device Type) <32bit>(Is last Sequence)` \
Read: `0x66(File) 0x00(On Success)`
### End file sequence flash: PHONE
Send an empty packet before and after. \
Write: `0x66(File) 0x03(End) 0x00(Phone/AP) <32bit>(Sequence Byte Length) 0x00(Unknown) <32bit>(Device Type) <32bit>(Identifier) <32bit>(Is last Sequence)` \
Read: `0x66(File) 0x00(On Success)`
