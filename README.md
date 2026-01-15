# NeoWave Lua 🌊

![NeoWave Banner](https://via.placeholder.com/800x150/000000/ffffff?text=NeoWave+Lua+Terminal+Vibes)

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

- Custom ASCII art support: Add your own .ascii files to the ascii/custom/ folder.
- Dynamic distro previews: Previews are live when selecting a distro from ascii/distros/.  
- Color presets: Fully configurable via colors/ folder (`red.sh`, blue.sh, etc.)  
- Module overrides: Control which info modules are displayed and their formatting.  
- Backup & restore: Original Neofetch configs saved in stock_configs/.  
- Legacy support: Fully documents Bash version differences; Lua version adds flexibility and polish.  
- Fastfetch ready: Designed to integrate or adapt with Fastfetch later.
- PNG & GIF logo support: via optional external renderers (chafa,viu), allowing images to be converted into high-quality ANSI/Unicode output for terminal display with auto fallback to ASCII.
- OS & Component Spoofing: Optional OS/Hardware display overrides for screenshots, demos, theming, and privacy. Fully reversible and applied only at config level.

---

## 📦 Dependencies
*Image Rendering support requires one of the following tools to be installed:*
- chafa
- viu
- img2txt
  > If none are installed NeoWave automatically falls back to standard ASCII logos.

---

## 📂 File Structure
