---
layout: default
title: Device Information
description: Information about the device
parent: Odin
nav_order: 1
---

## Format
### Header

| Argument Type      | Value      | Information           |
|:-------------------|:-----------|:----------------------|
| Magic Number       | 0x12345678 | Self-explanatory      |
| 32-bit integer     | dynamic    | Count of items        |
| Array of Location  | dynamic    | Self-explanatory      |
| Array of Data      | dynamic    | Self-explanatory      |

### Location Structure

| Argument Type     | Information           |
|:------------------|:----------------------|
| 32-bit integer    | DevInfo type (below)) |
| 32-bit integer    | Offset, in bytes      |
| 32-bit integer    | Size, in bytes        |

### Data Structure

| Argument Type     | Information           |
|:------------------|:----------------------|
| 32-bit integer    | DevInfo type (below)) |
| 32-bit integer    | Size, in bytes        |
| Raw byte buffer   | DevInfo data          |

### DevInfo Types

| Name                      | Value | Information  |
|:--------------------------|:------|:-------------|
| DEVINFO_TYPE_MODEL_NAME   | 0x00  | Model's Name |
| 3DEVINFO_TYPE_SERIAL      | 0x01  | Serial Code  |
| DEVINFO_TYPE_OMCSALESCODE | 0x02  | Region Code  |
| DEVINFO_TYPE_CARRIERID    | 0x03  | Carrier ID   |


## Packets
stub