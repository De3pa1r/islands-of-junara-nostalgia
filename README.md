# Islands of Junara: 2012 Nostalgia Setup

A step-by-step guide to playing **Sky Survival: The Islands of Junara** the way it was in 2012. That means an original early map version on **Minecraft 1.2.5**, with an HD texture pack and **MCPatcher**, all running through today's official Minecraft Launcher.

If you watched YouAlwaysWin's (Gunns & Meaty) original Junara series from April 2012, this is the setup for you. That series ran on **map version 0.5** with **Minecraft 1.2.5**.

> All the original download links used adf.ly, which no longer exists. The links below were recovered from old Wayback Machine captures of those adf.ly pages, which still held the real MediaFire link inside. See [How the links were recovered](#how-the-links-were-recovered).

---

## Contents

1. [What you need](#what-you-need)
2. [Set up a Minecraft 1.2.5 installation](#1-set-up-a-minecraft-125-installation)
3. [Install the map](#2-install-the-map)
4. [Install the texture pack](#3-install-the-texture-pack)
5. [Patch the game with MCPatcher](#4-patch-the-game-with-mcpatcher)
6. [Play](#5-play)
7. [Troubleshooting](#troubleshooting)
8. [Download links](#download-links)
9. [How the links were recovered](#how-the-links-were-recovered)
10. [Credits](#credits)

---

## What you need

- Minecraft **Java Edition** and the official **Minecraft Launcher**
- **Java 8**, installed system-wide (only needed to run MCPatcher). For example, [Temurin 8 JRE](https://adoptium.net/temurin/releases/?version=8). During installation, enable **"Set JAVA_HOME"** and **"Associate .jar"**.
- **7-Zip** or WinRAR (some old map versions are `.rar`)
- The [`files/1.2.5-hd.json`](files/1.2.5-hd.json) file from this repo

> **Windows tip:** turn on file extensions first (**File Explorer → View → Show → File name extensions**). Many of the steps involve renaming `.jar` and `.json` files, and Windows hides extensions by default.

---

## 1. Set up a Minecraft 1.2.5 installation

1. In the launcher, go to **Settings** and enable **Show historical versions**.
2. Go to **Installations → New installation**.
   - **Name:** e.g. `Nostalgia`
   - **Version:** `release 1.2.5`
   - **Game directory:** a **new, separate folder**, e.g.
     `%appdata%\.minecraft-1.2.5`
3. Click **Create**, then launch it once and close the game.

**Why a separate game directory?** New Minecraft versions save the language in `options.txt` as `lang:en_us`. Minecraft 1.2.5 expects `en_US`, so with your normal `.minecraft` folder it crashes on startup with a `NullPointerException` at `java.io.Reader.<init>`. A separate folder avoids that, and it also keeps your modern worlds safe from the old version.

---

## 2. Install the map

1. Download the map version you want (see [Download links](#download-links)). For the April 2012 YouAlwaysWin series, use **0.5**.
2. Extract the archive.
3. Put the world folder (the one that contains `level.dat`) into:
   `%appdata%\.minecraft-1.2.5\saves\`

   Make sure `level.dat` is directly inside the world folder, not inside an extra subfolder.

Which map version goes with which Minecraft version:

| Map version | Approx. era | Minecraft |
|---|---|---|
| 0.1 – 0.8.1.1 | late 2011 – 2012 | 1.0 – 1.2.5 (original YouAlwaysWin series: **0.5**) |
| 2.0 – 2.2 | late 2012 – 2013 | 1.4.x (YouAlwaysWin's "Island of Junara 2" series, Ep.1 uploaded Feb 2, 2013) |
| 2.3 | later | 1.7 |
| 2.4 | later | 1.8 |
| 3.0 – 3.2 | later | 1.8 – 1.9 |

---

## 3. Install the texture pack

The pack is **Minecraft Enhanced v1.3.2a (64x)**. It was made for Minecraft 1.3, but it works in 1.2.5 once the game is patched with MCPatcher (step 4).

1. Download it (see [Download links](#download-links)).
2. **Do not extract it.** Put the `.zip` as-is into:
   `%appdata%\.minecraft-1.2.5\texturepacks\`
   Create the `texturepacks` folder if it doesn't exist.
3. Select it in-game under **Mods and Texture Packs**.

> Without MCPatcher, HD packs are broken in 1.2.5. You'll see small fire and water icons on the sides of grass blocks, and flat, unanimated lava. Step 4 fixes that.

---

## 4. Patch the game with MCPatcher

MCPatcher fixes HD textures in old Minecraft versions. It was written for the old launcher, though, so it needs a little trickery to work with the modern one.

### 4.1 Create a custom copy of 1.2.5

You patch a **copy**, not the original. The launcher re-downloads official versions if their jar has been modified, which would undo your patch.

1. Open `%appdata%\.minecraft\versions\`. Note that this is the **main** `.minecraft` folder, not `.minecraft-1.2.5`, because versions are always stored here.
2. Create a new folder: `1.2.5-hd`
3. Copy `versions\1.2.5\1.2.5.jar` into it and rename the copy to **`1.2.5-hd.jar`**.
4. Put **[`1.2.5-hd.json`](files/1.2.5-hd.json)** from this repo into the same folder.

   This is Mojang's official 1.2.5 version file, with two changes: the id is set to `1.2.5-hd`, and the `downloads` section is removed so the launcher never overwrites your patched jar. (The modern launcher often doesn't keep a `.json` in the `1.2.5` folder, which is why this repo ships one.)

You should now have:
```
.minecraft\versions\1.2.5-hd\
├── 1.2.5-hd.jar
└── 1.2.5-hd.json
```

### 4.2 Fake the old launcher folder layout

MCPatcher 2.x only recognizes a Minecraft folder if it contains `bin\lwjgl.jar` **and** a `resources` folder. Otherwise it shows *"Minecraft not found"*. Create the old layout inside your main `.minecraft`:

1. Create `%appdata%\.minecraft\bin\`
2. Copy `versions\1.2.5-hd\1.2.5-hd.jar` into `bin` and rename it to **`minecraft.jar`**.
3. Copy
   `%appdata%\.minecraft\libraries\org\lwjgl\lwjgl\lwjgl\2.9.0\lwjgl-2.9.0.jar`
   into `bin` and rename it to **`lwjgl.jar`**.
4. Create an empty folder `%appdata%\.minecraft\resources\` (skip this if it already exists).

```
.minecraft\
├── bin\
│   ├── minecraft.jar   (copy of 1.2.5-hd.jar)
│   └── lwjgl.jar       (copy of lwjgl-2.9.0.jar)
└── resources\          (empty)
```

### 4.3 Patch

1. Download and run **MCPatcher 2.3.6_02** (see [Download links](#download-links)). It should now find Minecraft automatically.
2. Tick these mods:
   - **HD Textures**: required
   - **HD Font**: recommended
   - **Connected Textures**: recommended (the pack has a `ctm` folder)
   - **Custom Colors**, **Better Grass**, **Random Mobs**, **Better Skies**: optional
3. Click **Patch** and wait for it to finish. Don't click *Test Minecraft*, which won't work with this setup.

MCPatcher leaves two files in `bin\`:
- `minecraft-1.2.5.jar`: a backup of the **original**
- `minecraft.jar`: the **patched** jar, which is the one you need

### 4.4 Copy the patched jar back

1. Copy `%appdata%\.minecraft\bin\minecraft.jar` into `versions\1.2.5-hd\`.
2. Replace the old `1.2.5-hd.jar` with it (rename the copy to `1.2.5-hd.jar`). The patched jar is slightly bigger than the original (~3,938 KB).
3. Optional: delete `.minecraft\bin\` and `.minecraft\resources\`, since only MCPatcher needed them.

---

## 5. Play

1. In the launcher, go to **Installations**, find your 1.2.5 installation, and click **⋯ → Edit**.
2. Set **Version** to **`release 1.2.5-hd`**. Keep the game directory as `.minecraft-1.2.5`.
3. **Save**, then **Play**.
4. Select the texture pack, load the map, and enjoy 2012.

If `1.2.5-hd` doesn't appear in the version list, restart the launcher.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Crash on startup: `NullPointerException` at `java.io.Reader.<init>` | Use a separate game directory ([step 1](#1-set-up-a-minecraft-125-installation)), or change `lang:en_us` to `lang:en_US` in `options.txt` |
| Fire/water icons on grass sides, flat lava | The game isn't patched. Do [step 4](#4-patch-the-game-with-mcpatcher), and check the installation uses `1.2.5-hd` |
| MCPatcher: *"Minecraft not found"* | You need **both** `bin\lwjgl.jar` **and** a `resources` folder ([step 4.2](#42-fake-the-old-launcher-folder-layout)) |
| MCPatcher won't open | Install Java 8 system-wide. The launcher's bundled Java isn't visible to other programs |
| `1.2.5-hd` missing in the launcher | Restart the launcher. Check that the folder, jar and json are all named exactly `1.2.5-hd` (watch for `1.2.5-hd.json.json` or `1.2.5-hd (1).json`) |
| No `.json` in `versions\1.2.5` | Normal with the modern launcher. Use the one from this repo |
| Patch disappears after launching | You patched the official `1.2.5` instead of the `1.2.5-hd` copy, or the json still has a `downloads` section |

---

## Download links

These are the original files from the creators, recovered via the Wayback Machine. Nothing is rehosted in this repo. If a link dies, check the [original forum thread](https://www.minecraftforum.net/forums/mapping-and-modding-java-edition/maps/1476857-surv-adv-sky-survival-the-islands-of-junara-3-2).

### The Islands of Junara (by cowmonkey)

| Version | File | Link |
|---|---|---|
| 0.8.1 | Sky_Survival_0.8.1.zip | http://www.mediafire.com/?lyrquv82842ey9m |
| 0.8.0 | Sky_Survival_0.8.0.zip | http://www.mediafire.com/?5dcck4s4p7vs99o |
| 0.7.2 | Sky_Survival_0.7.2.rar | http://www.mediafire.com/download.php?5ahoeu917er8r2c |
| 0.7 | Sky_Survival_0.7.rar | http://www.mediafire.com/download.php?4dk3iz1mkwi6ki5 |
| 0.6 | — | http://www.mediafire.com/download.php?9w61dum7idwm8ia |
| **0.5** | Sky_Survival_0.5.zip | http://www.mediafire.com/download.php?yd4j1u9j5ewougt |
| 0.3 | Sky_Survival_0.3.zip | http://www.mediafire.com/download.php?5oarin3qmzdn3su |

Not recovered (no archived copy found): 0.8.1.1, 0.7.3, 0.7.1, 0.6b, 0.4.

### Minecraft Enhanced texture pack

| Version | File | Link |
|---|---|---|
| v1.3.2a, 64x | Minecraft Enhanced v1.3.2a 64x.zip | http://www.mediafire.com/download.php?34dmxmd526zsqxs |

The 128x and 256x versions for 1.3 could not be recovered. ([forum thread](https://www.minecraftforum.net/forums/mapping-and-modding-java-edition/resource-packs/1234744-32x-64x-96x-128x-256x-v1-7-9-minecraft-enhanced))

### MCPatcher

| Version | Link |
|---|---|
| 2.3.6_02 (Windows .exe) | https://web.archive.org/web/20130606064102/http://cloud.github.com/downloads/pclewis/mcpatcher/mcpatcher-2.3.6_02.exe |

This is the archived copy of the original download from the author's GitHub ([pclewis/mcpatcher](https://github.com/pclewis/mcpatcher)).

> **Safety:** MediaFire files from this era are old uploads, so check what you download. Maps and texture packs should be `.zip` or `.rar`. Never run an `.exe` from a map or texture pack link. The only `.exe` in this guide is MCPatcher, from the Wayback Machine link above.

---

## How the links were recovered

adf.ly shut down and joined Linkvertise, so every old `adf.ly/XXXX` link is dead. But the Wayback Machine archived many adf.ly pages back in 2013–2014, and those pages contained the real destination, base64-encoded in the page script.

To recover a link yourself:

1. List the captures of a link:
   `https://web.archive.org/cdx/search/cdx?url=adf.ly/XXXX&output=json`
2. Pick a capture with status `200` and open it raw:
   `https://web.archive.org/web/<timestamp>id_/http://adf.ly/XXXX`
3. In the page source, find either:
   - `https://adf.ly/go/<token>/<base64>`, where you base64-decode the last part, or
   - `var ysmm = '...'`, which is adf.ly's newer obfuscation. Take the characters at even positions, then the characters at odd positions **reversed**, join the two, base64-decode, drop the first 2 characters, and base64-decode the `u=` parameter.

Captures from 2019 onward are just redirects to a dead page, so only the early captures are useful. If a forum post's current links have no captures, check an **older archived version of the forum thread**, which may have used different adf.ly links that were archived.

---

## Credits

- **The Islands of Junara**: cowmonkey (madgoblin), with SkarT3 and MRDGaming
- **Minecraft Enhanced** texture pack: its original author ([forum thread](https://www.minecraftforum.net/forums/mapping-and-modding-java-edition/resource-packs/1234744-32x-64x-96x-128x-256x-v1-7-9-minecraft-enhanced))
- **MCPatcher**: pclewis and Prupe
- **YouAlwaysWin** (Gunns & Meaty), for the series that made this map a childhood memory for so many people
- The **Internet Archive / Wayback Machine**, without which none of these files would still be reachable

Minecraft is a trademark of Mojang/Microsoft. This guide is unofficial and not affiliated with Mojang, Microsoft, or any of the creators above.

---

## License

The guide text in this repository is licensed under the MIT License.

`files/1.2.5-hd.json` is derived from Mojang's official version metadata for Minecraft 1.2.5 and is **not** covered by the MIT License. It contains only download URLs, checksums and library names, and is provided solely so the official launcher can run Minecraft 1.2.5 that you already own. All rights belong to Mojang/Microsoft.

The map, texture pack and MCPatcher are not included in this repository. They are only linked, and they remain the property of their respective authors.
