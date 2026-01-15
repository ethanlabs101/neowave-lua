# NeoWave Lua 🌊

![Lua](https://img.shields.io/badge/Lua-2C2D72?style=for-the-badge&logo=lua&logoColor=white)

[![License: MIT](https://img.shields.io/badge/License-MIT-green)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-blue)](https://github.com/ethanlabs101/neowave-lua)
[![Stars](https://img.shields.io/github/stars/ethanlabs101/neowave-lua?style=social)](https://github.com/ethanlabs101/neowave-lua/stargazers)

---

## 🔹 Overview

NeoWave Lua is a modular terminal system tool for Neofetch/Fastfetch customization. It is the successor to the Bash legacy version, designed for maximum flexibility:

- Custom ASCII logos (distro-based or your own)  
- Multi-color terminal output with full override support  
- Module-based info display (CPU, GPU, MEM, OS, etc.)  
- Preview, Apply, and Revert actions  
- Cross-platform modularity (Linux-based terminals)  

> NeoWave Lua is designed for power users, terminal enthusiasts, and sysadmins who want precise control over Neofetch/Fastfetch output, without modifying source files manually.

---

## 💎 Features

- Module overrides: Control which info modules are displayed and their formatting.    
- Legacy support: Fully documents Bash version differences; Lua version adds flexibility and polish.  
- Fastfetch ready: Designed to integrate or adapt with Fastfetch.
- Custom ASCII, PNG, and GIF logos – fully displayable in the terminal with live resizing and color adaptation.
- OS & system component spoofing – fake your OS, CPU, GPU, and memory stats for testing or demos, with the ability to revert to real values.
- Dynamic color tables – define your own palettes for ASCII, PNG/GIF logos, and info modules directly in Lua; no external color preset files required.
- Modular info modules – toggle which system info to display (CPU, memory, disk, network, etc.) per session.
- Preview, Apply, and Revert – safely test changes, apply your configuration, or revert to the original Neofetch config.
- Backup system – automatically saves the original Neofetch config and previous NeoWave configs for full safety.
- Custom logo support – easily add your own logos to ~/.config/neowave/ascii/custom and select them on demand.
- Flexible alignment & right-side display – control info width, alignment, and spacing for a polished terminal layout.
- Cross-distro compatible – works on any Linux distro running Neofetch or Fastfetch.
- Lightweight & scriptable – fully written in Lua for speed, readability, and easy customization.


---

## 📦 Dependencies
*Image Rendering support requires one of the following tools to be installed:*
- chafa
- viu
- img2txt

*Arch / Manjaro*
- Update your system first
- ```sudo pacman -Syu```

- Install chafa, viu, and caca-utils (img2txt)
- ```sudo pacman -S chafa viu caca-utils```

*Ubuntu / Debian*
- Update your package list
- ```sudo apt update```

- Install chafa, viu, and caca-utils (img2txt)
- ```sudo apt install chafa viu caca-utils -y```


> If none are installed NeoWave automatically falls back to standard ASCII logos.

---

## 📂 File Structure

- ~/.config/neowave-lua/
- ├── ascii/                     # ASCII logos
- │   ├── custom/                # User-added custom ASCII logos
- │   └── distro/                # Live previews for distro logos
- ├── backup/                    # Automatically saved previous configs
- ├── colors/                    # Optional Lua tables for color palettes
- ├── core/                      # Core engine scripts (Lua modules)
- │   ├── engine.lua             # Everything: apply, backup, colors, logos, state
- │   └── menu.lua               # CLI menu interface
- ├── data/                      
- │   └── distros.db             # Table of applicable stock distros
- ├── profiles/                  
- ├── stock_configs/             
- ├── theme/                     
- ├── neowave.lua                # Entrypoint script; run neowave from terminal
- └── README.md                  # You are here!
> Note: Unlike the Bash version, Lua does not require a temporary config file. All tables and memory-based settings are handled dynamically.

---

## ⚡ Installation

- i. Clone This repo
```bash git clone https://github.com/ethanlabs101/neowave-lua.git ~/.config/neowave-lua```
