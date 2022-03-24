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
`THOR` Seem to be a synonym for `ODIN` on some (newer?) devices, file transfers work just as when `ODIN` is sent. There also exists a [thor download protocol](https://lists.denx.de/pipermail/u-boot/2013-October/164088.html) that Tizen/Samsung uses in their development boards so `THOR` could possibly have something to do with this mode. Simply doing a `THOR` handshake and trying to flash something with [lthor](https://git.tizen.org/cgit/tools/lthor/) does not work though. P.S. it is probably used for the Thor flashing software (newer Odin). \
Write: `THOR` \
Read: `LOKE`
