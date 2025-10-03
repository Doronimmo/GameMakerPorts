# Porting GameMaker Studio Games with GMLoader

> This tutorial was made for dummies. Don’t worry if you’ve never done this before, we’ll walk you through step by step.  

This guide explains how to port **GameMaker Studio (GMS)** games to ARM-based Linux handheld devices using [GMLoader](https://github.com/JohnnyonFlame/droidports) and [GMLoader-Next](https://github.com/JohnnyonFlame/gmloader-next).  
It includes example templates, packaging instructions, and troubleshooting advice.

---

## 1. What is GameMaker Studio?

[GameMaker Studio](https://gamemaker.io/en) is a 2D game development engine. It supports drag-and-drop tools for beginners and scripting for advanced users.  
Games can be exported to many platforms (Windows, Linux, macOS, Android, iOS, consoles, etc.), but **ARM Linux handhelds** are not supported natively.  

That’s where **GMLoader** comes in.

---

## 2. How Porting Works

GMLoader (and its successor **GMLoader-Next**) act as **compatibility layers** for GameMaker’s Android runner (`libyoyo.so`).  
They create a lightweight Android-like environment, allowing ARM-based Linux devices to run GMS games.  

For more background, see [GMLoader Research Docs](https://github.com/JohnnyonFlame/yyg_fix/blob/master/RESEARCH.md).

---

## 3. Compatibility

| Platform | Bytecode | YYC (YoYo Compiler) |
|----------|----------|----------------------|
| Android  | ✅        | ✅                   |
| Windows  | ✅        | ❌                   |
| Linux    | ✅        | ❌                   |
| macOS    | ✅        | ❌                   |

- **gmloader** – GMS ≤ 2022.x, ARMv7/armhf  
- **gmloader-next.armhf** – All GMS versions, ARMv7/armhf  
- **gmloader-next.aarch64** – GMS ≥ 2.2.1, ARMv8/aarch64  

> 💡 **YYC Games** (compiled with YoYo Compiler) run faster, but are less portable. UndertaleModTool will warn you if a game uses YYC.

---

## 4. Step-by-Step Porting Tutorial

### Step 1: Pick a Game
Find a GMS game:
- [Itch.io – Made with GameMaker](https://itch.io/games/made-with-gamemaker)  
- [SteamDB – GameMaker titles](https://steamdb.info/tech/Engine/GameMaker/)  

Start with free games before attempting commercial ones.

---

### Step 2: Extract the Game Data
1. Download [**UndertaleModTool**](https://github.com/UnderminersTeam/UndertaleModTool/releases).  
2. Extract game files (APK or install folder). Look for:  
   - `data.win`  
   - `game.unx`  
   - `game.ios`  
   - `game.droid`  
3. Open them in UndertaleModTool.

---

### Step 3: Check Version & Compiler
Inside UndertaleModTool:
- Look for a **YYC warning** (means game is compiled with yyc).  
- Check **Data → General Info** for the GMS version.  

---

### Step 4: Choose the Correct Wrapper
Download the right wrapper APK:  
👉 [Wrappers repo](https://github.com/Fraxinus88/GMloader-ports/tree/main/gmloader%20wrappers%20(APK))

- **gmloader-next.aarch64** → GMS 2.2.1+ (ARMv8)  
- **gmloader-next.armhf** → GMS 2.2.1- (ARMv7)  

---

### Step 5: Rework the Example Package
Modify the [template package](https://github.com/Doronimmo/GameMakerPorts/raw/refs/heads/main/Test/portname.zip) for your chosen game. (only gmloader-next.aarch64 for now)  

Add:
- Game assets inside `/assets`  
- Your wrapper renamed to `game.port`

Extra, costume port name and folder: 
Change `portname` in line 20 of `Portname.sh` to your folder name. Example: `GAMEDIR="/$directory/ports/applerabbledazzle"`
Change `Portname` in `Portname.sh` to actual port name. Example: `Apple Rabble Dazzle.sh`

---

## 5. Packaging

A port folder usually looks like this:

```
Portname.sh
portname/
├── lib/ (we don't really touch this folder, unless we know what we are doing)
│ ├── armv8a/
│ ├── armv7a/
│ ├── libopenal.so.1
│ ├── libzip.so.5
│ └── libcrypto.so.1
├── assets/
│ └── (game files here)
├── gmloader.json
├── tools/
│ └── patchscript
├── portname.gptk
└── game.port
```


Youv'e reached your limit on chetGepT 4.12312, please try again later.
