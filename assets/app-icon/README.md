# Iris Photo app icon

**Drawing:** a photo (mountains and a sun) between two crop marks. It is the same drawing the Iris
Workspace shows for Iris Photo (`../Iris/src/shared/types/workspace.types.ts`, entry `irisphoto`).

**Palette:**

| Colour | Hex | Used for |
|---|---|---|
| Cyan | `#00b1cc` | gradient start (top left), `oklch(0.66 0.2 205)` |
| Blue | `#0083f4` | gradient end (bottom right), `oklch(0.60 0.21 247)` |
| White | `#ffffff` | the photo frame and the crop marks |

**Tile:** `viewBox="0 0 512 512"`; the Workspace's 64-grid drawing scaled so its rounded tile
(`rx` 15 on 56) fills the canvas edge to edge. The macOS files add Apple's transparent margin
(tile = 824/1024 of the canvas); Windows and Linux use the tile edge to edge.

**Licence:** part of the Iris brand, see `LICENSE.txt`.

## Files

| File | What | Used by |
|---|---|---|
| `photocraft.svg` | master vector | source for everything below |
| `photocraft-small.svg` | copy of the master (kept so `packaging/icons.sh` keeps working) | `hicolor/scalable` |
| `photocraft-1024.png` | tile with Apple margin, 1024 px | runtime Dock icon on macOS (`apps/photocraft/src/app_icon.rs`) |
| `photocraft.icns` | macOS icon set, 16 to 1024 px | `packaging/macos/package.sh` (`CFBundleIconFile`) |
| `photocraft.ico` | Windows icon, 16 to 256 px | `apps/photocraft/build.rs` (embedded in the `.exe`), the MSI |
| `hicolor/<size>/apps/com.iris.irisphoto.png` | Linux theme icons, 16 to 512 px; the 256 one is also the runtime window icon on Windows and Linux, the 128 one the brand mark in the title bar (`crates/ui-egui/src/panels/brand.rs`) and the update prompt | `packaging/linux/com.iris.irisphoto.desktop` |
| `hicolor/scalable/apps/com.iris.irisphoto.svg` | Linux scalable icon | as above |

## Regenerate

Edit `photocraft.svg`, copy it over `photocraft-small.svg`, then run `packaging/icons.sh` (needs
`resvg`; `iconutil` on macOS for the `.icns`). Without `resvg`, any SVG rasteriser works as long
as it writes the same files: on Windows the Iris Photo icons were rendered with the Electron in
`../Iris/node_modules` (unset `ELECTRON_RUN_AS_NODE`), the `.ico` built with `cargo xtask ico` and
the `.icns` written from PNGs.

PhotoCraft's original icon (the kitsune) is no longer used. A copy of its light trace stays in
`crates/io/src/svg/clip-path-tile.svg` only as an SVG-import test case.
