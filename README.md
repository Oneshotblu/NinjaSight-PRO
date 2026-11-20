# NinjaSight Pro

Offline Windows photo viewer, annotation editor and batch image resizer.

**Open instantly · View naturally · Annotate precisely · Resize efficiently · Save everything · Continue editing anytime.**

- No accounts, no cloud, no telemetry, no internet connection needed.
- HEIC / HEIF support is built in (pure-Rust decoders); no Microsoft HEIF/HEVC extension is required.

## Features

### Viewer (floating overlay)
- Opens pictures in a borderless, translucent overlay above the desktop; the picture stays the focus.
- Formats: JPG/JPEG, PNG, BMP, GIF, TIFF/TIF, TGA, WEBP, HEIC/HEIF (incl. iPhone grid photos, rotation, colour profiles converted to sRGB, image sequences: Ctrl+←/→), AVIF, JPEG 2000 (.jp2/.j2k/.jpf/.jpx) and camera RAW (DNG, CR2, CR3, CRW, NEF, NRW, ARW, SRF, SR2, ORF, RW2, RAF, PEF, SRW, ERF, KDC, DCR, MRW, 3FR, IIQ, MOS, MEF, RWL, ARI).
- Every format can be viewed, edited and saved. Libraries are all free/open-source (no commercial licences): AVIF = avif-parse (MPL-2.0) + rav1d (BSD-2) to read, rav1e/ravif (BSD) to write; JPEG 2000 = dicom-toolkit-jpeg2000 (MIT/Apache, lossless .jp2 output); RAW = rawler (LGPL-2.1); TGA/GIF/TIFF = image (MIT/Apache). Camera RAW files are never overwritten: edits are saved as DNG (Adobe's open, royalty-free RAW format) beside them, or exported to any other format.
- EXIF orientation applied automatically; HEIF container rotation honoured.
- Mouse-wheel zoom centred on the cursor (smooth), drag to pan, middle-button pan, double-click fullscreen, right-click menu.
- Folder navigation with arrow keys (follows the folder order, HEIC included), prefetching of neighbours for instant display.
- Thumbnail strip, information overlay (name, resolution, format, size, zoom, “5 of 42”), auto-hiding floating toolbar.
- Image browser (B): folder grid with sorting (name, date, size, type), recent files, recent projects, favourites.
- Multiple windows: every launch is an independent viewer; “Open in new window” (Ctrl+N).
- Drag and drop of files and folders.

### Annotate (Greenshot-style, non-destructive)
- Text boxes and callouts (font family, size, bold, italic, alignment, colours, background, border, corner radius, shadow, transparency, optional number badge), freehand pen, highlighter, lines, arrows, double arrows, rectangles, ellipses/circles, numbered markers, area highlight, spotlight, blur, pixelate.
- Select, move, resize (handles), rotate (rotation handle or angle field), multi-select (Shift/rubber band), duplicate, copy/paste, bring forward/send backward, group/ungroup, undo/redo (Ctrl+Z / Ctrl+Y), nudge with arrow keys.
- Objects panel (O): list, rename, show/hide, reorder, groups.
- Annotations always stay separate editable objects; the picture is never modified.

### NinjaSight Project (.pvp)
ZIP container:

| Entry | Content |
|---|---|
| `mimetype` | `application/x-ninjasight-project` |
| `manifest.json` | format name, `format_version`, `min_reader_version`, app version, created/modified timestamps, SHA-256 + size of every entry |
| `original_image/<file>` | the original picture, byte-for-byte (stored, not recompressed) |
| `annotations.json` | `schema_version`, image size, rotation, `next_id`, annotations (tagged JSON objects), groups |
| `metadata.json` | title, source path/name/format/size, original size, EXIF summary, resize history |
| `preview.jpg` | flattened 512 px preview (used for thumbnails) |

- Unknown fields from newer versions are kept when saving again (forward compatibility).
- Safe saving: written to `name.pvp.tmp`, verified (checksums), previous file kept as `.bak` during the swap, then renamed. Interrupted saves are detected and can be recovered (“Open recovered copy”).
- Automatic recovery: unsaved annotations are saved every 30 s (configurable); after a crash the next start offers to recover them.

### Resize (Fast-Image-Resizer-style)
- Single picture: presets (3840×2160 … 640×480, 50 %, 25 %, your own), fit within, fill & crop (with on-picture crop preview), exact size, width/height only, percentage, longest side; don't enlarge; portrait-aware presets.
- Lanczos, bicubic, bilinear, nearest; sharpening after resize.
- Output: same as original, JPEG (quality 1–100), PNG (lossless), WEBP (lossy/lossless), BMP, TIFF, HEIC; keep or strip EXIF; keep colour profile; background colour for transparency.
- Live new size, scale %, original file size and estimated output size.
- Projects: **Resize project** (scales picture and every annotation, stays editable) and **Export resized annotated image** (flattened).
- Batch processor extras: optional **upscaling** before resizing (local processing only), **keep the original files' dates**, watermark with a live **preview** and system fonts, and an **Estimate output size** button (between *Include subfolders* and *Remove selected*) that shows the approximate size after processing next to each file's current size.
- Batch processor (Ctrl+B): files/folders (with sub-folders), drag & drop, queue preview and removal, naming pattern (`{name}{suffix}`, `{width}`, `{height}`, `{percent}`, `{counter}`, `{date}`, `{format}`, `{ext}`), same folder / sub-folder / chosen folder (keeps structure) / replace originals (explicit confirmation), conflict handling (number, skip, overwrite), multi-threaded, progress (total, completed, failed, skipped, remaining, current file), cancel without corrupting finished files, reusable profiles.

### Export
PNG, JPEG, WEBP, PDF; with or without annotations; resolution (percentage or width); quality; PDF page size (A4, A3, A5, Letter, Legal, fit), orientation, margins, 1/2/4/6/9 pictures per page, captions; several files (pictures or projects) in one operation.

### Windows integration
Settings › Windows integration (per user, no admin rights): “Open with” for all picture types, right-click “Open with NinjaSight Pro”, “Resize with NinjaSight Pro”, folder “Batch resize with NinjaSight Pro” and “Find duplicates with NinjaSight Pro” (also on the empty area inside a folder), `.pvp` association, Default-apps registration. Paths with spaces and non-ASCII characters are supported.

### Screenshot
Direct capture: the shortcut, the desktop right-click entries and the in-app buttons start a light native overlay (no NinjaSight window) that freezes the screen at its real resolution on every monitor (per-monitor DPI aware) and draws it 1:1. Drag an area (Shift = square, snaps to photo ratios), Space = whole desktop, M = monitor under the mouse, Esc / right-click = cancel; then a small menu: Save directly, Save as, Copy, Open in NinjaSight Edit, Quick Share. Details: Camera button (top right) or the system-wide shortcut (default Ctrl+Shift+X, configurable in Settings › Screenshot, several keys allowed): whole work area (all monitors), one monitor, or an area drawn on a frozen copy of the screen with live W × H pixels and snapping to 1:1, 3:2, 2:3, 4:3, 16:9… The NinjaSight window, when open, always stays in the capture (it is never hidden). The selection frame is drawn off-screen and only the changed parts are repainted, so it follows the mouse smoothly; the instructions are centred at the top of the monitor under the mouse. Afterwards: Save directly (folder from the settings, default Pictures\Screenshot), Save as (chosen in the image browser), Copy to clipboard, Open in Edit, Send with Quick Share (if installed). Also on the Windows right-click menu of the desktop and of folders (*Screenshot with NinjaSight Pro* › area, whole work area, monitor under the mouse, choose…), selectable during installation; on Windows 11 it is under *Show more options*. Command line: `NinjaSightPro.exe --shot area|whole|monitor|menu`. **Always ready** (asked during installation, switch in Settings › Screenshot): a tiny helper (`--shot-helper`) starts with Windows and waits for the shortcut even when NinjaSight Pro is closed; while a NinjaSight window is open that window handles the shortcut. Uninstalling stops and removes the helper.

### Edit retouch tools
Eyedropper (pick a colour from the photo), clone stamp (Alt+click a source, then paint; zoom and position stay put between strokes), patch in two styles: **Rectangle** (select a damaged area, drag it onto a clean one: seamless blend) or **Brush** (paint the damage in as many passes as needed with a resizable soft brush, Alt or Erase removes parts; press *Replace…* only when the selection is complete, drag onto a clean area, check the preview and *Apply replacement*). Pen strokes can be normalized (smoothed) and vectorized: a vectorized stroke shows its curve points (squares) and direction handles (circles); drag a point to move it, a handle to bend the curve (the opposite handle follows; Alt = corner point), *Remove handles* keeps the curve as it is. Spotlight areas can be rectangular or round; the rest of the picture is darkened only once, however many areas are added.

Arrows and double arrows: tip shapes (triangle, barbed, open, dot, diamond, bar, none) chosen separately for each end, tip size, tapered line, white outline. Every line, arrow, shape, pen stroke and highlight can have a soft, blurred **shadow** (with distance; the same on screen and in saved pictures) and an **Angle** (rotation of boxes and shapes, direction of lines and arrows, turning of pen strokes). Text boxes and the watermark can use **any font installed in Windows**; the 3 fonts you use most are listed first and as quick buttons at the top.

Photo panel (shutter icon): crop, perspective, white balance, levels, curves, **Sharpness** (edge-aware), **HDR fix** slider (for washed-out or too bright captures from HDR monitors; 0 = off, 50 = standard gentle fix, 100 = strong; it changes only levels, mid-tones and saturation, never clips highlights at 50; the automatic correction of captures has its own strength in Settings › Screenshot; also automatic in Settings › Screenshot), **Upscale** 1.5×/2×/3×/4× with detail recovery (computed on this computer), save. The histogram stays in the bottom-left corner, above the thumbnail strip.

### Watermark
Resize panel and batch processor: text or picture watermark, single (9 positions), tiled grid, diagonal repeat or bottom strip; opacity, size, rotation, spacing.

### More in Edit (1.2)
- Font colour of text notes and callout boxes, and number colour of markers and numbered notes (or Auto).
- Ctrl+X, Ctrl+C, Ctrl+V cut, copy and paste the selected objects (text copied from other programs is pasted as a text note).
- **Vignette** (G): darkens or tints everything around an oval, with strength and softness.
- **Magnifier** (M): a round lens showing a chosen spot enlarged; move and resize the lens, drag its small circle to choose the spot, set the zoom.
- Patch, Brush mode: hold **Alt + left button** to paint the area to repair (several passes), then **left click** (or drag) on a clean spot to copy from it; check the preview and press Enter or *Apply replacement*.
- Font picker: type in *Search system fonts…* to filter; the list stays open while typing.
- **Symbols** in the Shapes menu (also Industry, Packaging & handling, Business & office groups): architecture (north arrow, section/detail/level marks, door, window, stairs, column, grid, dimension…), industrial safety (warning, mandatory and prohibition signs), road signs, emergency and fire signs, and general icons, all recolourable (plate, border, symbol).

### Collage
The composition preview shows the pictures themselves, numbered in order. *As arranged on screen* also includes the picture shown in the centre of the viewer (its visible part, behind the slide windows; a tick in the list leaves it out). Settings › Collage: folder (default Pictures\Collage), file name pattern ({date} {time} {counter} {count} {layout}), save directly or ask every time, open the new collage. Except for *As arranged on screen*, drag a picture onto another to swap them, or use ▲ ▼ in the list.

### New in 1.2.2
- **Image browser for every file choice:** opening pictures and projects, adding files and folders (batch, export, slides, duplicates, watermark picture), choosing folders (settings, output) and *Save as* (projects, copies, exports, screenshots, collages) all use the program's own image browser instead of the Windows dialog, with a file-name bar, *New folder…* and confirmation before replacing a file. The start screen has *Image browser*, *Open project*, *Batch resize* and *Find duplicates*, each with its keyboard shortcut written under it.
- **Clone properties** (Edit, property bar): click an object to copy its look (colours, line, fill, font, border, shadow, opacity…), then click another object to give it to it; hold Ctrl to give it to several objects; Esc stops.
- **Ctrl while rotating** (rotation handle, the end of a line or arrow, or the Angle field): 15° steps.
- The thickness and size values in Edit are measured against the monitor, so showing or hiding toolbars, panels or the whole interface never changes them.
- The Shapes and symbols menu has a light grey background (better contrast for the drawings) and shows the shapes as pictures.
- Settings are ordered General › Viewer › Screenshot › Annotation › Collage › Resizer › Shortcuts › Windows integration.
- About: developed privately by Oneshotblu with A.I. tools, not for profit.

### New in 1.2.7
- **Print window with real previews:** the pages are drawn as the chosen printer will print them (its real paper size and printable area), with page thumbnails below. Choose the printer, copies, paper, portrait/landscape and the printer's own Properties right there; 1, 2, 4, 6, 9, 12 or 20 pictures per page, margins, spacing, file names. Drag a picture onto another frame to swap them (or onto a page thumbnail to move it there); click one to turn it, fit or fill its frame, move it earlier/later or remove it; click an empty frame to add pictures.
- **Carousel:** the picture in the centre frame is now 1.5× (+15 %); 5 seconds after the last change all thumbnails go back to the same height, and grow again at the next picture or when the mouse is over the strip. The strip background is more transparent.

### New in 1.2.6
- **Thumbnail carousel:** the blue frame of the thumbnail strip stays still in the middle of the monitor and the pictures slide through it; the picture in the frame is 30 % larger and its neighbours shrink back smoothly. The mouse wheel over the strip goes to the previous / next picture.
- **Steady top bar:** the View · Edit · Resize · Find switch always stays in the middle of the monitor, whatever side panel (Photo, Objects, Resize) is open; the bottom toolbar and the strip do too whenever there is room.
- **Compact file information** (top left): the same two short lines in View, Edit and Resize (name; size · format · file size · zoom · position).
- **Text notes:** alignment buttons drawn like in word processors; a **No background** button (crossed square, next to the background colour) removes the background in one click and gives it back; the number badge of a numbered note can sit in **8 places** (corners and middles of the sides).
- **Find duplicates** now uses the whole window like the Resize panel: folders and search options on the left, the list of duplicates on the right, and a **comparison grid** in the middle showing 2, 4, 6, 8, 10, 12, 14 or 16 pictures at once (up to 4 × 4). The mouse wheel zooms every picture in the grid together (drag to move), so details can be compared; the tick in each picture's caption selects it for removal; click a row on the right to show its group, double-click to open it in the viewer.
- **Print** (Ctrl+Shift+P, printer button on the toolbar, More menu): this picture or every picture in the folder, with or without notes, whole picture or page filled, turned to match the page, margins and file name; then the Windows print window to choose any installed printer, copies, paper and orientation.
- **Screenshot:** the pointer is a blue crosshair like the selection frame (with a thin dark edge, visible on any background); the start screen reminds the screenshot function and its current shortcut.
- **Fixed:** a crash when a new callout or note was still open and the mouse was dragged over the picture (very flat boxes). Unexpected errors are now written to `crash.log` in the settings folder.

### Monitors and small screens
Move the window to another monitor (monitor button, right-click menu, Ctrl+Shift+M): the monitors are drawn as Windows arranges them (Settings › Display), click one to move there; the last monitor is remembered. On low resolutions the interface shrinks automatically (Settings › General).

## Install

- **Installer:** run `NinjaSightPro-Setup-<version>.exe` (from `dist/`, e.g. `NinjaSightPro-Setup-1.2.2.exe`). Choose **Only me** (`%LOCALAPPDATA%\Programs\NinjaSight Pro`, no administrator rights) or **All users** (`Program Files\NinjaSight Pro`, Windows asks for administrator rights). It lets you choose the Explorer integrations and shortcuts, and registers an uninstaller in *Apps & features*. Settings are always stored per user; if that folder is not writable a fallback folder is used and shown in Settings › General.
- **Update:** run the new `NinjaSightPro-Setup-<version>.exe`. When NinjaSight Pro is already installed it offers a one-click **Update now** that keeps the same place (only you / all users), settings, Explorer integration, shortcuts and screenshot key; open NinjaSight windows reopen on the new version by themselves (windows with unsaved work keep running until you close them). The previous program file is removed at the next start.
- **Portable:** unzip `NinjaSightPro-<version>-Portable.zip` anywhere (the program inside is `NinjaSightPro-<version>.exe`). The installed copy is always called `NinjaSightPro.exe`, so file associations and right-click menus survive updates. The `portable.txt` file next to the program keeps settings in `NinjaSight-data\` beside it.
- Command line: `NinjaSightPro.exe --install` (silent per-user install), `--install-all-users` (asks for administrator rights), `--uninstall`, `--uninstall-all-users`, `--register`, `--unregister`.

## Command line

```
NinjaSightPro.exe [file or folder]        open a picture, project or folder
NinjaSightPro.exe --resize <file>         open in the Resize panel
NinjaSightPro.exe --batch <files/folders> open the batch processor with a queue
NinjaSightPro.exe --selftest <folder>     run the built-in end-to-end tests
NinjaSightPro.exe --screenshot <png> [file]  draw ~120 frames, save the window picture, exit
```

## Build

Requirements: Rust (stable, MSVC toolchain) and the Visual Studio C++ build tools (for libwebp).

```
cargo build            # debug build: target\debug\ninjasight-pro.exe
cargo build --release  # optimised build
.\build-dist.ps1       # release build + self-test + dist\NinjaSightPro-Setup-<version>.exe and NinjaSightPro-<version>-Portable.zip
.\target\debug\ninjasight-pro.exe --make-icon assets  # regenerate the application icon (icon.ico / icon.png)
.\target\debug\ninjasight-pro.exe --selftest .\shots\selftest   # end-to-end tests
```

### Architecture

| Module | Purpose |
|---|---|
| `imaging.rs` | decoding (incl. HEIC), EXIF orientation, encoding, metadata, resizing, sharpening, thumbnails |
| `annot.rs` | annotation data model, geometry, hit-testing, transforms |
| `render.rs`, `fonts.rs` | identical raster rendering for screen sprites and exports (tiny-skia + ab_glyph, Windows fonts) |
| `project.rs`, `doc.rs` | .pvp container, integrity, safe save, document + undo history |
| `batch.rs`, `pdf.rs` | batch engine, PDF writer |
| `services.rs` | background loader with prefetch, thumbnail workers + disk cache, folder listing |
| `explorer.rs` | registry integration, installer/uninstaller |
| `app*.rs`, `canvas.rs`, `chrome.rs`, `annot_ui.rs`, `dialogs.rs`, `batch_ui.rs`, `settings_ui.rs` | user interface (egui) |
| `i18n.rs` | localisation: English built in, Italian for the main strings, more languages via `lang/<code>.json` |

See `SHORTCUTS.md` for the keyboard reference.
