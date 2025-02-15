---
layout: default
title: Flashing
description: Flash partitions with Odin
parent: Odin
nav_order: 4
---

## Defaults
* Read/Write timeout for file transfer: 30000 (30 seconds)
* Packet size for file transfer: 131072 (128 KiB)
* File transfer max sequence size: 800 (Packets)

## Terminology
* Sequence: Size is *Packet size for file transfer* multiplied by *File transfer max sequence size*
* File Part: Size is *Packet size for file transfer*.

## How-to guide
1) Request a file flash \
2) Divide the file into sequences \
3) Begin file sequence flash \
4) Divide the sequence into file parts \
5) Send the file part (in raw format) \
6) Read the file part response \
7) Send End file sequence flash:
* If you finished sending file parts
* Send the AP/CP packet accordingly

8) Repeat until the end of the file

## Documentation
### Request file flash
Request:

| Value        | Argument Type     | Information        |
|:-------------|:------------------|:-------------------|
| 0x66         | 32-bit integer    | Packet type        |
| 0x00         | 32-bit integer    | Packet's command   |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x66         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. Is always zero.                |

### Begin file sequence flash
Request:

| Value        | Argument Type     | Information        |
|:-------------|:------------------|:-------------------|
| 0x66         | 32-bit integer    | Packet type        |
| 0x02         | 32-bit integer    | Packet's command   |
| dynamic      | 32-bit integer    | Length (in bytes)  |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x66         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. May not be zero.               |

### Flash a file part
Send an empty packet for the first file part. \
Request:

| Value        | Argument Type     | Information        |
|:-------------|:------------------|:-------------------|
| dynamic      | Raw byte buffer   | File Part buffer   |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x66         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| dynamic      | 32-bit integer    | Current file part index on LOKE's side      |

### End file sequence flash: MODEM
Send an empty packet before and after. \
Request:

| Value        | Argument Type     | Information                 |
|:-------------|:------------------|:----------------------------|
| 0x66         | 32-bit integer    | Packet type                 |
| 0x03         | 32-bit integer    | Packet's command            |
| 0x01         | 32-bit integer    | Modem/CP                    |
| dynamic      | 32-bit integer    | Sequence byte length        |
| 0x00         | 32-bit integer    | Unknown                     |
| dynamic      | 32-bit integer    | Device Type (PIT)           |
| dynamic      | 32-bit integer    | Is last sequence, 0/1 bool  |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x66         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. May not be zero.               |

### End file sequence flash: PHONE
Send an empty packet before and after. \
Request:

| Value        | Argument Type             | Information                 |
|:-------------|:--------------------------|:----------------------------|
| 0x66         | 32-bit integer            | Packet type                 |
| 0x03         | 32-bit integer            | Packet's command            |
| 0x00         | 32-bit integer            | Phone/AP                    |
| dynamic      | 32-bit integer            | Sequence byte length        |
| 0x00         | 32-bit integer            | Unknown                     |
| dynamic      | 32-bit integer            | Device Type (PIT)           |
| dynamic      | 32-bit integer            | Partition Identifier (PIT)  |
| dynamic      | 32-bit integer Boolean    | Is last sequence            |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x66         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. May not be zero.               |
