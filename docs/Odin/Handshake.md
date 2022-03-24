---
layout: default
title: Handshake
description: Odin Handshake
parent: Odin
nav_order: 3
---

## Handshakes
Before any upload or download can take place, a handshake is done.

### Mjolnir2
Found this by ~~of course not using the samsung leak~~. Before documenting this protocol, I would like to check if it's even present on modern devices. You can volounteer your device [here](https://github.com/Samsung-Loki/Hreidmar/blob/main/README.md) (it is just a check if this command is present). \
Write: `MJOLNIR` \
Read: `THOR`

### ODIN
Write: `ODIN` \
Read: `LOKE`

### FPGM
Does the same thing as the usual ODIN handshake. \
Write: `FPGM` \
Read: `OK`

### THOR **(Unknown)**
It used by the Thor flashing software (newer Odin). \
Does the same as the usual `ODIN` handshake. \
Write: `THOR` \
Read: `LOKE`
