<div align="center">
  <h1>Warcraft Survivors</h1>
  <p><b>A Vampire Survivors-style roguelite set in the world of World of Warcraft 1.12.1</b></p>
  <p>A personal fork by <a href="https://github.com/mlemors">mlemors</a>, based on <a href="https://github.com/AdamMcWilliam/warcraft-survivors">the original Warcraft Survivors project</a> and powered by <a href="https://github.com/samwhosung/benilla">benilla</a>.</p>
</div>

Choose a class and battleground, then survive for fifteen minutes against an ever-growing horde. Your spells cast automatically; you focus on moving, collecting experience and choosing upgrades. Bosses arrive throughout the run, with the final encounter at 13:30.

The game reads terrain, creatures, animations, spell effects, sounds and interface art from your own World of Warcraft client at runtime. This repository does not include those game assets.

## Requirements

- Your own World of Warcraft 1.12.1 client, build 5875. The game reads the client files and does not modify them.
- [Rust](https://rustup.rs). `rust-toolchain.toml` selects the required toolchain through rustup.
- A C compiler and platform dependencies:
  - **Windows:** MSVC build tools.
  - **macOS:** Xcode Command Line Tools (`xcode-select --install`).
  - **Linux:** ALSA and udev development packages, plus `pkg-config`. On Debian or Ubuntu:
    `sudo apt install build-essential pkg-config libasound2-dev libudev-dev`.

No game server or account is needed. Survivors mode runs offline.

## Install and run

Clone this fork:

```sh
git clone https://github.com/mlemors/warcraft-survivors.git
cd warcraft-survivors
```

Give the game access to your WoW data in either of these ways:

1. Create a `WoW` link in the repository root that points to your client installation directory (the directory containing `Data`). On Windows, use PowerShell:

   ```powershell
   New-Item -ItemType Junction -Path WoW -Target "C:\path\to\WoW"
   ```

   On macOS or Linux:

   ```sh
   ln -s /path/to/WoW WoW
   ```

2. Or set `WOW_DATA` to the client's `Data` directory before launching. In PowerShell:

   ```powershell
   $env:WOW_DATA = "C:\path\to\WoW\Data"
   ```

   On macOS or Linux:

   ```sh
   export WOW_DATA=/path/to/WoW/Data
   ```

The `WoW` link is ignored by git. Start the game with:

```sh
cargo survivors
```

The first run compiles the project and may take several minutes. Later launches are faster. The command builds and runs `warcraft-survivors`; the executable is written to `target/play/` (`warcraft-survivors.exe` on Windows).

## How to play

| Input | Action |
|---|---|
| `W` `A` `S` `D` or arrow keys | Move |
| Mouse wheel | Zoom the camera |
| Hover over a spell or passive icon | Read its tooltip and upgrade details |
| `1`, `2`, `3` or click | Choose a level-up card |
| `Esc` | Pause or resume |
| `Enter` | Start from the menu or return to it after a run |

- **Classes:** Warrior, Paladin, Hunter, Rogue, Priest, Shaman, Mage, Warlock and Druid. Each has its own spell pool and class-themed heroes.
- **Level-ups:** Enemies drop experience wisps. Collect them to choose a new spell, improve a spell or gain a passive blessing.
- **Healing:** Some enemies drop turkey legs; bosses always drop one that fully heals you.
- **Difficulty:** The horde grows stronger over time. Between boss encounters, an enemy ring closes in from around the arena.
- **Goal:** Survive until 15:00.

## Battlegrounds

Each battleground has its own creature roster and six boss encounter slots, drawn from the zone and related dungeons or raids.

| Battleground | Zone | Creatures | Final boss |
|---|---|---|---|
| The Barrens | Kalimdor | Plains beasts, quilboar, centaur and harpies | Hezrul Bloodmark |
| The Dark Portal | Blasted Lands | Hyenas, scorpids and the Burning Legion | Lord Kazzak |
| Gates of Ahn'Qiraj | Silithus | Silithids and the Qiraji | Ossirian the Unscarred |
| Fire Plume Ridge | Un'Goro Crater | Dinosaurs, oozes and fire elementals | King Mosh |
| Gurubashi Arena | Stranglethorn Vale | Jungle beasts and Gurubashi trolls | Hakkar |
| Blackrock Mountain | Burning Steppes | Blackrock orcs, worgs and the black dragonflight | Nefarian |
| Kodo Graveyard | Desolace | Scorpashi, basilisks, demons and kodo | Princess Theradras |
| Winterspring | Winterspring | Owls, chimaeras, Highborne and blue dragons | Azuregos |
| Mount Hyjal | Kalimdor | The Legion, the Scourge and dragons of Nightmare | Ysondre |
| Naxxramas | Eastern Plaguelands | The Scourge of Plaguewood | Kel'Thuzad |

Mount Hyjal has no creature roster of its own in the 1.12.1 client, so its enemies are themed for the location. The Barrens uses the shared fallback boss roster for encounters without a map-specific boss.

## Developer options

Survivors mode is implemented in [`crates/benilla-app/src/survivors/`](crates/benilla-app/src/survivors/), with its launcher in [`crates/warcraft-survivors/`](crates/warcraft-survivors/). It runs locally on the benilla client without connecting to a server.

An autopilot can run a hands-off session:

```powershell
$env:WOW_SURVIVORS_AUTO = "Mage"          # Add :all, :late or :idle to change the run
$env:WOW_SURVIVORS_MAP = "Naxxramas"      # Map name or index from 0 to 9
$env:WOW_SURVIVORS_SHOTS = "C:\shots"     # Optional screenshot output directory
cargo survivors
```

The autopilot chooses level-up cards, moves to collect experience and logs its progress every ten seconds. See the [benilla README](https://github.com/samwhosung/benilla#running-it) and [`docs/`](docs/) for the underlying client and development documentation.

## Credits and licensing

This repository is a personal fork of [AdamMcWilliam/warcraft-survivors](https://github.com/AdamMcWilliam/warcraft-survivors). The game is built on [benilla](https://github.com/samwhosung/benilla), which provides the 1.12.1 client, file readers, renderer, animation and spell systems, and interface engine.

This is an independent fan project and is not affiliated with or endorsed by Blizzard Entertainment. It contains no Blizzard game assets or client data. You must provide your own legally obtained client. World of Warcraft and Warcraft are trademarks of Blizzard Entertainment, Inc.; Vampire Survivors is a trademark of poncle.

The code is available under the [MIT License](LICENSE-MIT) or [Apache License 2.0](LICENSE-APACHE), at your option. Vendored components in `third_party/` retain their own upstream licenses.
