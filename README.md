# AGI Driver for Olivetti Prodest PC1

Brings true EGA-quality 16-color graphics to Sierra AGI v2 games on the Olivetti Prodest PC1, using the Yamaha V6355D hidden 160×200×16 graphics mode.

By **Retro Erik** — [YouTube: Retro Hardware and Software](https://www.youtube.com/@RetroErik)

![Olivetti Prodest PC1](https://img.shields.io/badge/Platform-Olivetti%20Prodest%20PC1-blue)
![License](https://img.shields.io/badge/License-CC%20BY--NC%204.0-green)

### 📥 [Download AGI-PC1.zip — everything you need](AGI-PC1.zip)

| File | Description |
|------|-------------|
| `AGIPAL.COM` | EGA palette loader — run once before launching a game |
| `KQ1/CGA_GRAF.OVL` | Patched overlay for King's Quest I |
| `KQ2/CGA_GRAF.OVL` | Patched overlay for King's Quest II |
| `KQ3/CGA_GRAF.OVL` | Patched overlay for King's Quest III |
| `LSL/CGA_GRAF.OVL` | Patched overlay for Leisure Suit Larry |
| `PQ1/CGA_GRAF.OVL` | Patched overlay for Police Quest I |
| `SQ1/CGA_GRAF.OVL` | Patched overlay for Space Quest I |
| `SQ2/CGA_GRAF.OVL` | Patched overlay for Space Quest II |

## Usage

### 1. Copy the overlay to your game disk

Each game has its own patched overlay. Copy the correct `CGA_GRAF.OVL` for your game to your game disk, replacing the original:
```
copy KQ1\CGA_GRAF.OVL A:\CGA_GRAF.OVL
```

**Important:** Each overlay is specific to the game and version it was built for. Different versions of the same game may have a different overlay. If you have a version that doesn't work, or if you want support for an AGI game not listed here, send me your original `CGA_GRAF.OVL` and I will patch it for you.

### 2. Set the EGA palette

Run `AGIPAL.COM` once before launching the game to set the standard 16 EGA colors:
```
AGIPAL
KQ1
```

The V6355D palette is mode-independent and survives INT 10h calls, so it stays set for the entire session. Use `AGIPAL /R` to reset to defaults.

## How It Works

Sierra AGI games store their graphics as vector drawing commands and sprites using 16 EGA color indices. The AGI interpreter renders these into an internal framebuffer at **160×200 with 16 colors** — this is the true native resolution of all AGI graphics, regardless of display adapter (CGA, EGA, Tandy, Hercules).

On EGA, the overlay doubles each pixel horizontally to fill a 320×200 screen. On the Olivetti Prodest PC1, the V6355D hidden mode displays 160×200×16 natively — the exact resolution of AGI's internal framebuffer, with no pixel doubling needed.

The patch modifies the CGA overlay (`CGA_GRAF.OVL`), which already produces a byte format identical to the V6355D hidden mode's VRAM layout — 4bpp packed nibbles at 80 bytes/row with interlaced banks. Only **10 bytes** are changed to redirect the output to the V6355D:

1. **Mode register** — activates the V6355D hidden 160×200×16 mode
2. **VRAM segment** — redirects output from `B800h` to `B000h`

No rendering code is modified — the original blit routine produces the correct byte format natively. With `AGIPAL.COM` setting the V6355D palette to the standard 16 EGA colors, the result is **100% true EGA graphics** displayed at the game's native resolution.

### Note on text

Game graphics are rendered through the overlay and look perfect. However, the AGI interpreter draws status bar and dialog text using INT 10h BIOS calls, which write directly to VRAM in CGA format. This produces minor colored fringing artifacts at character edges (typically cyan and red pixels). The text is fully readable — the artifacts are purely cosmetic.

## Screenshots

Captured from real Olivetti Prodest PC1 hardware.

<img src="Screenshots/KQ1 AGI overlay.png" width="50%">

*King's Quest I*

<img src="Screenshots/KQ2 AGI overlay.png" width="50%">

*King's Quest II*

<img src="Screenshots/KQ3 AGI overlay.png" width="50%">

*King's Quest III*

<img src="Screenshots/PQ1 AGI overlay.png" width="50%">

*Police Quest I*

<img src="Screenshots/SQ1 AGI overlay.png" width="50%">

*Space Quest I*

<img src="Screenshots/SQ2 AGI overlay.png" width="50%">

*Space Quest II*

## Supported Games

- King's Quest I
- King's Quest II
- King's Quest III
- Leisure Suit Larry
- Police Quest I
- Space Quest I
- Space Quest II

## Requirements

- Olivetti Prodest PC1 (or compatible with Yamaha V6355D)
- Original AGI game files

## YouTube

Check out my YouTube channel for demos and more retro projects: https://www.youtube.com/@RetroErik

## License

This project is provided for personal, non-commercial use. The patched overlays are derived from Sierra's original CGA_GRAF.OVL files — you must own the original games to use them.

## YouTube

For more retro computing content, visit my YouTube channel **Retro Hardware and Software**:
[https://www.youtube.com/@RetroErik](https://www.youtube.com/@RetroErik)
