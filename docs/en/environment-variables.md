# Environment variables and command-line arguments

Almost none of this is for everyday use: the `UOFIDDLER_*` variables exist for screenshots and automated tests. The ones that matter to someone using the program are `UOFIDDLER_UO_DIR`, `--experimental`, the language ones and `HOME`.

## Command-line arguments

| Argument | What it does |
|---|---|
| `--experimental` | Shows the tabs that are still being finished (today, **Paperdoll**). Without it they stay hidden. The updater passes the arguments on when it restarts, so the flag survives an update. `./run.sh --experimental` works too. |
| `--version` | Prints the build number (`0` on a development build) and exits, without opening a window. |
| `--self-update <folder>` | Runs the update code against a local folder laid out like a release (zip, tar.gz or AppImage + `SHA256SUMS.txt`) and exits, without a window. Without the folder it uses `UOF_UPDATE_SOURCE`. It serves the updater test. |

## For people who use the program

| Variable | What it does |
|---|---|
| `UOFIDDLER_UO_DIR` | Ultima Online client folder **for this run only**. It beats the folder saved in the options and the automatic detection. |
| `UOFIDDLER_EXPERIMENTAL` | Any value is the same as `--experimental`. It exists for scripts and screenshots. |
| `LC_ALL`, `LC_MESSAGES`, `LANG` | Pick the interface language on the first run (`pt_...` -> Portuguese, anything else -> English). The first one that is set and not empty wins, in that order. After that, the choice made in the options prevails. |
| `HOME` (and `USERPROFILE`) | Base of the settings folder `~/.config/uofiddler` (`settings.json`, `gumpstudio_resizepics.txt`, GumpStudio favorites, the map editor's borders) and of the folders where the program looks for the client by itself. The map editor and GumpStudio use `USERPROFILE` when `HOME` is missing (Windows). |
| `APPIMAGE` | Set by the AppImage itself with the file's path. The updater uses it to know **which file to replace**. When the program restarts, it removes `APPDIR`, `APPIMAGE`, `ARGV0` and `OWD` from the new process's environment, so the new AppImage mounts itself instead of reusing the old one's mount. |

## Screenshots and automated tests

They open the program already in a given state so it can be photographed with nobody at the screen. Combine them with `UOFIDDLER_UO_DIR`.

| Variable | Format | What it does |
|---|---|---|
| `UOFIDDLER_START_TAB` | `<tab name>` (`Items`, `Map`, `Paperdoll`, `GumpStudio`...) | Initial tab. Case-insensitive; an experimental tab only counts together with `--experimental`. |
| `UOFIDDLER_MAP` | `<radar\|live\|overview>,<facet>,<x>,<y>[,nodiff][,zoom=<f>][,maxz=<z>][,noroofs][,grid]` | State of the Map tab: mode (`live` is the isometric view), facet 0 to 5, coordinates, mapdif off (`nodiff`), zoom, height cut, no roofs and grid. |
| `UOFIDDLER_EDITOR` | `<tool>[/x,y][/options...]` | Opens the map editor on the tool, with the cursor on the tile and optional actions (palette, help, height, brush...). Only counts together with `UOFIDDLER_MAP`. The full list is in the comment of `MapEditor::focus` (`crates/uo_gui/src/map/editor/state.rs`). Example: `select/1480,1620/palette`. |
| `UOFIDDLER_PICKER` | `<kind>[/<id>[/<search>]]` | Opens the map's art picker at startup. Examples: `static/0x0F51/dagger`; for hues, `hue/1153/0x1F03` shows hue 1153 on a robe. |
| `UOFIDDLER_OPEN` | `<file name>` | Files tab: opens that file at startup. |
| `UOFIDDLER_GS_SELECT` | `<element index>` | GumpStudio: selects that element of the sample gump at startup. |
| `UOFIDDLER_PAPERDOLL` | `<plate\|common\|robe>[,<body 0-5>]` | Paperdoll: puts on a ready set (plate armor, common clothes, robe) and optionally changes the body (0 human ♂ ... 5 gargoyle ♀). |
| `UOFIDDLER_PAPERDOLL_ITEMS` | `0x2662,0x1539,...` | Paperdoll: wears exactly these items. |
| `UOFIDDLER_SCREENSHOT` | `<file.png>[,<seconds>]` | Saves a capture of the window after the delay (5 s if omitted) and **closes the program**. |

## Build, CI and updater

| Variable | What it does |
|---|---|
| `UOF_BUILD` | Read by `build.rs`: the build number embedded in the executable (CI uses `github.run_number`). Without it the build is 0, shown as "dev build". It is what the updater compares with the latest release. |
| `UOF_UPDATE_SOURCE` | Release folder used by `--self-update` when the argument is not given. |
| `UOF_UPDATE_TEST_ARCH` | Only counts with `--self-update`: fakes the architecture (`x86_64`) to rehearse `packaging/test-updater.sh` on machines that are not x86_64 (the releases only have x86_64 files). The graphical interface never reads it. |
| `UOF_UPDATE_ACK` | **Internal.** The old process sets it to a file path when it starts the new one; the new one writes to it when its window is up, and only then does the old one quit. If that does not happen in time, the old executable is put back. |
| `SKIP_APPIMAGE` | Used by `packaging/test-updater.sh`: skips the AppImage part. |
| `GH_TOKEN`, `REPO`, `OLD_TAG` | Set in the `updater-live.yml` workflow (manual): the GitHub token, the repository and the tag of the old build to update (default `build-29`). |

## Tests and development tools

| Variable | What it does |
|---|---|
| `UPDATE_I18N_BASELINE` | When running `cargo test -p uo_gui`, rewrites `crates/uo_gui/i18n_baseline.txt` with the current counts of hard-coded texts, as long as none of them went up (the i18n test is a ratchet: a count can never rise). |
| `UO_DIR` | Client folder read by the examples in `crates/uo_core/examples` (`anim_probe`, `equip_audit`, `mapdif_check`...). Default: `/uo`. |
| `UOFIDDLER_UO_DIR` | In the tests that use real files (map editor, picker), it is the client folder; without it those tests are skipped. |
