# UOFiddler 🗡️

> 🌐 **Language / Idioma**: **English** | [Português (README_pt-br.md)](README_pt-br.md)  
> 👤 **Author**: Mauricio Nunes + DeepSeek & Gemini · [Contributors](#-contributors)  
> 📥 **Download**: [latest release](https://github.com/mbnunes/UOFiddler-Rust/releases/latest) (Windows, Linux `.AppImage` / `.deb` / `.tar.gz`)  
> 🔗 **GitHub**: [https://github.com/mbnunes/UOFiddler-Rust](https://github.com/mbnunes/UOFiddler-Rust)

**UOFiddler** is a complete, modern, high-performance **100% native Rust** reimplementation of the classic **UOFiddler** for Ultima Online. Built on **egui / eframe**, it runs smoothly, stably, and elegantly on **Linux, Windows, and macOS**, completely eliminating dependencies on the **.NET runtime**, **Mono**, **Wine**, or **Python**.

---

## 🤝 Contributing

Every Pull Request must meet the [acceptance criteria](CONTRIBUTING.md). The first one: **all user-visible text must be localized**; a PR without it is refused. Agents: see [AGENTS.md](AGENTS.md).

---

## ⚡ Highlights & Why UOFiddler?

Traditional UOFiddler on Linux via Wine or Mono is prone to graphical glitches, crashes, and path casing errors. **UOFiddler** provides:

1. **100% Native Rust**: Zero runtime dependencies. Memory-safe, instant startup (< 50 ms), and microscopic memory footprint.
2. **Dual-Client Engine (Classic & Enhanced UOP)**: Seamless live toggling between **Classic 2D** (`.mul`/`.idx`) and **Enhanced Client 3D** (`.uop` Mythic MYP v5 / DDS textures).
3. **Hardware-Accelerated UI (egui/eframe)**: Smooth 60+ FPS navigation across 65,536 static items, 16,384 land tiles, 79 GameData tables, and huge multis.
4. **Intelligent Path Detection**: Automatically discovers Ultima Online Classic and Enhanced installations across standard locations (`/opt/hd_jogos/`, `~/.wine/`, Lutris, Steam, local directories) with case-insensitive file resolution.
5. **Universal Keyboard Navigation**: Full keyboard control (Arrow Keys, PageUp, PageDown) with smooth automatic scrolling across lists and grids in all tabs.
6. **Native Audio Engine**: Direct PCM WAV playback using `rodio` over PipeWire, PulseAudio, and ALSA without external audio drivers.
7. **Vertical Zoom Slider Tool**: 1:1 true-size rendering with vertical sliders (0.5x to 5.0x) on previews for Items, Land Tiles, TileData, Textures, and Animations.
8. **Isometric Map Viewer & Map Editor**: The world drawn like the game client (GPU, real art, sloped land), from `mapN.mul` or `mapNLegacyMUL.uop`, and a full map editor that **saves straight into the client files** (with `.bak` backups), for shard admins.
9. **Multilingual interface**: English and Brazilian Portuguese (system language detected on first run, switchable in the options); every text goes through the translation tables in `crates/uo_gui/locales`, so a new language is one more file.

---

## 📑 Comprehensive Overview of all 21 Tabs (20, plus 1 experimental)

Tabs are grouped in three categories at the top of the window: **Arte** (art), **Dados** (data) and **Mundo** (world).

| Tab | Description |
|---|---|
| **⚔️ Items** | Static items viewer with **Grid** and **List** modes. Live toggle between Classic 2D Art and Enhanced Client DDS textures (`LegacyTexture.uop`). Hue tester (3,000 hues), the item's **TileData** inspector (classic 32-bit and High Seas 64-bit flags), and vertical zoom tool. |
| **🌿 Land Tiles** | 16,384 terrain tiles with diamond geometry (44x44 rhombus). Dual-engine switch for Classic vs Enhanced 3D terrain textures (`Texture.uop`), the land's **TileData** inspector, and zoom slider. |
| **🖼️ Gumps** | Full browser for classic `gumpart.mul` and modern `gumpartLegacyMUL.uop`. Container bounding box overlays (71 containers), search, and vertical zoom tool. |
| **🎨 Hues** | Complete palette of all 3,000 Ultima Online hues. Color bar gradient inspector with individual 32-color RGB555 swatches and table view. |
| **🧱 Textures** | Raw 64x64 and 128x128 surface textures from `texmaps.mul`. Grid and list view with 1:1 preview and vertical zoom tool. |
| **🎯 RadarCol** | All 81,920 minimap radar colors mapped to items and terrain. Real-time RGB color preview and 1:1 tile art inspection. |
| **🎭 Animations** | Complete creature animation player supporting `anim1..6.mul` and `AnimationFrame1..6.uop` (46,000+ entries). Full category filtering (Monstros, Marinhos, Animais, Humanos/Equip, UOP Modernos), dynamic action selection with `✓` indicators, directional playback (8 directions), and sprite sheet exporter. |
| **💬 Cliloc** | High-speed string localization browser (`cliloc.enu`). Search across 100,000+ entries by ID or localized text. |
| **🔊 Sounds** | PCM sound browser and player for `sound.mul`. Direct audio playback with wave information. |
| **🗺️ Map** | All six facets (Felucca, Trammel, Ilshenar, Malas, Tokuno, Ter Mur) from `mapN.mul` or `mapNLegacyMUL.uop`. **Isometric** view drawn on the GPU like the client (real art, textured slopes, roofs and height cut, radar inset), **Radar** and whole-facet **Overview** (`facet0N.mul` / `Multimap.rle`), mapdif/stadif toggle, presets and go-to. The **Edit map** button opens the [map editor](#-map-editor). |
| **🏰 Multis** | Multi-tile structures browser (houses, castles, ships). Supports classic `multi.mul` and Enhanced Client `MultiCollection.uop`. Real-time isometric software ray-assembler with multi-height Z slicing and mouse wheel zoom. |
| **💡 Light** | Circular and directional light patterns (`light.mul`) with high-contrast preview. |
| **🔤 Fonts** | Bitmap ASCII font browser (`fonts.mul`) with interactive text typing preview. |
| **⭐ Skills** | Skills definition table (`skills.mul`) with action button flags and text entries. |
| **🗣️ Speech** | Speech keywords and command triggers (`speech.mul`). |
| **🎬 AnimData** | Frame animation timing and delay tables (`animdata.mul`) with live static art preview. |
| **🎮 GameData** | Enhanced Client 3D configuration inspector (`GameData.uop`). Decodes all 79 configuration tables with **Structured Grid** and **Raw Monospace** views, row filtering, and batch CSV export. |
| **🧍 Paperdoll** *(experimental)* | Paperdoll builder: the body animation and the paperdoll gump side by side, backpack-style item slots and hues (`Equipconv.def`, `gump.def`), any item on any layer, and the character on a mount. Still being finished, so it is **hidden unless you start the program with `--experimental`** (see [Experimental features](#-experimental-features)). |
| **🧩 GumpStudio** | Visual gump designer: backgrounds, images, items, text, HTML/cliloc, buttons, checkboxes, radios, text entries and alpha regions, drawn with the client's art and **UO fonts** (`unifont*.mul`, `fonts.mul`). Saves and opens JSON and exports to **RunUO (C#)**, **Sphere** and **POL**. Visual pickers for images, items, buttons, checkboxes and radios, a resizepic browser (with a manual valid/invalid list), HTML text, repeat loops, favorites and groups. |
| **📡 Comunicação** | Static reference of the client's packets (server to client and client to server) and gump layout commands, with search and filters. |
| **📁 Arquivos** | Browser for every client file: text/def files as tables, IFF localization, UOP browser with entries and preview, hex view, `Multimap.rle` and `facet0N.mul` as images, and the **UOP ↔ MUL converter** (art, gumps, maps). |

---

## 🗺️ Map Editor

Open the **Map** tab and press **Edit map** (or F11). The editor takes the place of the viewer, in the same tab:

- **Tools**: Select (inspect and edit a tile and its statics), Stamp, Terrain, Height, Eraser and Region, with brush size and shape, a preview of the stroke and a see-through **ghost** of the static where it will land (it stacks on tables, floors, counters…).
- **Terrain with automatic borders**, read from the art or from **border sets** you edit in the *Borders…* dialog (saved in `~/.config/uofiddler/map_borders.json`); floor statics get their borders inside the stroke.
- **Height band** to work inside caves and on upper floors, and a fixed working Z.
- **Region**: copy, cut, paste, move, delete, fill, and saved templates.
- **Palette** with the visual art picker (categories, search, favorites, recents), undo/redo and keyboard shortcuts (press `?`).

**Saving**: changes stay pending until you press **Save**, which writes them into the facet's files in the client folder: land into `mapN.mul` / `mapNLegacyMUL.uop` (patched in place), statics into `staidxN.mul` / `staticsN.mul` (rewritten). A `.bak` copy of each file is made before its first save. **Save as…** writes a full copy of the facet's files into another folder instead, and pending changes can be exported to and imported from JSON. The editor edits the base map: `mapdif`/`stadif` patches, if the facet has them, still apply on top in the client.

> Keep a copy of your client folder before saving, and close the game client while you save.

---

## 🧪 Experimental features

Tabs that are still being finished stay hidden, so the published tool shows only what is ready. Start the program with `--experimental` to show them:

```bash
uo_gui --experimental                  # Windows: uo_gui.exe --experimental
./UOFiddler-<version>-x86_64.AppImage --experimental
cargo run --release -p uo_gui -- --experimental
```

- Today that is the **Paperdoll** tab. It appears marked *(experimental)*, and the tab counters (20 tabs, 21 with the flag) include it only with the flag.
- The flag is kept when the [auto updater](#updates) restarts the program. For scripts and automated screenshots, `UOFIDDLER_EXPERIMENTAL=1` does the same. All environment variables are listed in [`docs/en/environment-variables.md`](docs/en/environment-variables.md).

---

## 📥 Download

Ready-to-run builds of every push to `master` are published on the [Releases page](https://github.com/mbnunes/UOFiddler-Rust/releases/latest):

| System | File |
|---|---|
| Windows 10/11 (x64) | `uofiddler-<version>-windows-x86_64.zip`: unzip and run `uo_gui.exe` |
| Linux (any distro) | `UOFiddler-<version>-x86_64.AppImage`: `chmod +x` and run it |
| Debian / Ubuntu / Mint | `uofiddler_<version>_amd64.deb`: `sudo apt install ./uofiddler_<version>_amd64.deb`, then open *UOFiddler* from the menu |
| Linux (plain archive) | `uofiddler-<version>-linux-x86_64.tar.gz`: extract and run `./uofiddler` |

Linux builds need glibc 2.30 or newer (Ubuntu 20.04+, Debian 11+, Fedora 32+ and anything newer).

### Updates

UOFiddler checks, in the background, whether the [latest release](https://github.com/mbnunes/UOFiddler-Rust/releases/latest) has a newer build than yours (the build number is embedded by the CI; a local `cargo build` is build 0, shown as "dev build"). If GitHub cannot be reached, nothing happens and the app opens normally.

- When there is a newer build, a dialog shows the new version and the release notes with **Update now**, **Remind later** and **Ignore this version**. Nothing is downloaded or installed without your click.
- **Update now** downloads the file for your system, checks its **SHA-256** against the release's `SHA256SUMS.txt` (a file that does not match is never installed), keeps the old executable as `<name>.old`, replaces the executable and restarts. If the new version does not start, the old one is put back. Settings are kept (they live in `~/.config/uofiddler`).
- It updates the Windows `.exe` (from the `.zip`), the Linux `.AppImage` and the `.tar.gz` binary. For the `.deb` (system install) it only opens the release page.
- **Options** has the toggle *Check for updates automatically at startup* (turn it off to disable the check) and the **Check for updates** button. *Ignore this version* is remembered in the same `settings.json` (`auto_update_check`, `ignored_update_build`).
- Command line (no window): `uo_gui --version` prints the build number; `uo_gui --self-update <dir>` runs the same update code against a local folder laid out like a release (zip/tar.gz/AppImage + `SHA256SUMS.txt`). It is used by the **Updater test** workflow (GitHub: Actions > Updater test > Run workflow, manual only), which builds build 1 and build 2 on Windows and Linux, updates one into the other and checks that a tampered checksum is refused.

---

## 🛠️ Build and Run

### Prerequisites
- [Rust toolchain](https://rustup.rs/) (version 1.75+ recommended)
- On Linux, only the usual desktop libraries at run time (OpenGL plus X11 or Wayland); nothing extra is needed to build.

### Building and Running
```bash
# Clone the repository
git clone https://github.com/mbnunes/UOFiddler-Rust.git
cd UOFiddler-Rust

# Run directly using the launch script
./run.sh

# Or compile and run with cargo
cargo run --release -p uo_gui
```

### Running Tests
```bash
cargo test -p uo_core --lib
cargo test -p uo_gui

# Tests that read real client files run when UOFIDDLER_UO_DIR points at a client folder
UOFIDDLER_UO_DIR=/path/to/client cargo test -p uo_gui
```

`cargo test --workspace` also runs the `benchmark_test` integration test, which needs an Enhanced Client installation (`GameData.uop`).

---

## 🏗️ Architecture

```
UOFiddler-Rust/
├── Cargo.toml                  # Workspace manifest
├── crates/
│   ├── uo_core/                # High-performance UO format decoders (100% safe Rust)
│   │   ├── src/
│   │   │   ├── art.rs          # Classic Art & EC DDS decoding (image-rs dds)
│   │   │   ├── animations.rs   # MUL & UOP animation sequence decoder
│   │   │   ├── gamedata.rs     # GameData.uop parser (79 tables & containers)
│   │   │   ├── multis.rs       # Isometric multi renderer & uop collection
│   │   │   ├── maps.rs         # Map reader (mapN.mul / mapNLegacyMUL.uop, statics, mapdif), radar
│   │   │   ├── map_edit.rs     # Map changes and saving them into the client files
│   │   │   ├── uop_convert.rs  # UOP <-> MUL converter
│   │   │   ├── gump_studio.rs  # GumpStudio model and RunUO / Sphere / POL exporters
│   │   │   ├── tiledata.rs     # 32-bit & 64-bit TileData parser
│   │   │   ├── gumps.rs        # Classic & UOP Gump decoder
│   │   │   └── ...
│   └── uo_gui/                 # Native hardware-accelerated GUI (egui / eframe)
│       └── src/
│           ├── main.rs         # Tabs, virtualization, keyboard navigation, theme
│           ├── map/            # Isometric map view (GPU) and the map editor
│           └── ...             # Files, Packets, Paperdoll and GumpStudio tabs
├── docs/
│   ├── formats/                # Every client file format, documented
│   └── protocol/               # Packets and gump layout reference
├── packaging/linux/            # .deb / AppImage / .tar.gz packaging (used by CI)
├── Animationlist.xml           # Creature animation definitions
├── Multilist.xml               # Multi house names and metadata
├── run.sh                      # Universal launcher
└── README.md
```

---

## 📚 Documentation

- [`docs/en/formats`](docs/en/formats/README.md): one document per family of client files (MUL/IDX, maps, tables, animations, UOP containers, `.def`, localization…), covering every file of a 7.x client folder.
- [`docs/en/environment-variables.md`](docs/en/environment-variables.md): every environment variable and command-line argument (`--experimental`, `UOFIDDLER_UO_DIR`, the screenshot variables, the updater ones). The Portuguese original is [`docs/variaveis-de-ambiente.md`](docs/variaveis-de-ambiente.md).
- [`docs/en/protocol`](docs/en/protocol/README.md): packets, gump layout commands and flows (login, gumps, house customization, boats). The Portuguese originals are in [`docs/formats`](docs/formats/README.md) and [`docs/protocol`](docs/protocol/README.md).

---

## 👥 Contributors

| | |
|---|---|
| [**Mauricio Nunes**](https://github.com/mbnunes) | Author and maintainer: the Rust rewrite, the core decoders and the whole original app. |
| [**Felipe Tuffani**](https://github.com/fetuffani) | Animations via `Body.def`/`Bodyconv.def`, the Files and Communication tabs, the Paperdoll builder, the UOP ↔ MUL converter, GumpStudio and UO fonts, the file format and protocol documentation, and the CI. |
| [**João Escribano**](https://github.com/joaoescribano) | UOP map reading, the isometric map viewer, the map editor that saves into the client files, and the release packages. |

Contributions and pull requests are welcome!

---

## 📜 License
Open-source under the MIT License.
