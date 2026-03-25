# cubiboot for Dolphin Consolizer

A fork of [cubiboot](https://github.com/makeo/cubiboot) (itself a fork of [cubeboot](https://github.com/OffBroadway/cubeboot)) modified for use with [Dolphin Consolizer](https://github.com/Ma-Rang/dolphin-consolizer). It provides a GameCube BIOS replacement menu that displays game artwork with animated icons, lets the user pick a game, then signals Consolizer to boot it.

## Changes from upstream cubiboot

- Game list metadata embedded in the DOL by Consolizer's `dol_patcher.py`; visual assets (banners, animated save icons) streamed over USB Gecko at boot
- USB Gecko communication link for asset streaming and sending the selected game back to Consolizer
- Runs entirely in the Dolphin emulator — real hardware is not supported

## How it works

1. Consolizer patches a GameCube IPL ROM using `gc_ipl_patcher`, embedding this fork's code, the IPL, and a lightweight game list (IDs, titles, descriptions)
2. Dolphin boots the patched IPL as its GC BIOS
3. The menu loads immediately with placeholder art, then Consolizer streams banners and animated save icons over USB Gecko — the emulated Gecko is fast enough that this is near-instant
4. When the user selects a game, a 2-byte LAUNCH command is sent over USB Gecko to Consolizer, which tells Dolphin to boot it

## Building

Requires [devkitPPC](https://devkitpro.org/) (part of devkitPro) with libogc2.

Build order: patches first, then cubeboot (which embeds `patches.elf`).

```
./build.sh
```

Output: `cubeboot/cubeboot.dol`

## License

GPL-2.0-or-later — see [CREDIT.md](CREDIT.md) for full acknowledgements.

## Acknowledgements

- [cubeboot](https://github.com/OffBroadway/cubeboot) by [TeamOffBroadway](https://github.com/OffBroadway) (GPL-2.0)
- [cubiboot](https://github.com/makeo/cubiboot) by [makeo](https://github.com/makeo) (GPL-2.0)
- [apploader](https://github.com/makeo/cubeboot-tools) (GPL-2.0)
- [packer](https://github.com/emukidid/swiss-gc/tree/master/cube/packer) for apploader.img (GPL-2.0)
