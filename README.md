# NeoWave Lua 🌊

![Lua](https://img.shields.io/badge/Lua-2C2D72?style=for-the-badge&logo=lua&logoColor=white)

> **Modular terminal customization engine for Neofetch & Fastfetch.**  
> The Lua successor to the legacy Bash NeoWave — built for flexibility, safety, and cross-distro polish.
 
---
 
## Features
 
| Feature | Details |
|---|---|
| **Dual fetch tool support** | Neofetch (Bash config) and Fastfetch (JSONC) with a live toggle switch |
| **Custom ASCII logos** | Drop `.txt` files into `ascii/custom/`; token-based `{cyan}`, `{red}`, `{reset}` colorization |
| **PNG & GIF logos** | Auto-rendered via `chafa → viu → img2txt` fallback chain; gracefully degrades to ASCII |
| **Module toggles** | Enable/disable any info block (CPU, GPU, MEM, OS, DE, etc.) per session |
| **OS & hardware spoofing** | In-memory spoof table injected into config on Apply; zero writes until you confirm |
| **Color presets** | `default`, `neon`, `monochrome`, `ocean` — easily extensible in `engine.lua` |
| **Preview** | Renders the would-be config to stdout with syntax highlighting — no file is touched |
| **Backup & Revert** | Timestamped backups written before every Apply; one-step revert from the menu |
| **Headless mode** | `--apply`, `--revert`, `--preview` flags for scripting/automation |
 
---
 
## Requirements
 
| Dependency | Purpose | Install |
|---|---|---|
| `lua 5.4` | Runtime | `apt install lua5.4` / `pacman -S lua` |
| `luarocks` | Package manager | `apt install luarocks` |
| `luafilesystem` | Directory ops | `luarocks install luafilesystem` |
| `neofetch` **or** `fastfetch` | The actual fetch tool | your distro's package manager |
| `chafa` *(optional)* | Best image renderer | `apt install chafa` |
| `viu` *(optional)* | Good image renderer | `cargo install viu` |
| `img2txt` *(optional)* | Fallback image renderer | `apt install caca-utils` |
 
---
 
## Installation
 
```bash
git clone https://github.com/youruser/neowave-lua.git
cd neowave-lua
bash install.sh
```
 
The installer:
1. Verifies Lua 5.4 and `luafilesystem` are present.
2. Creates `~/.config/neowave-lua/` directory tree.
3. Copies all source and data files.
4. Creates a `~/.local/bin/neowave` symlink (if the directory exists).
---
 
## File Structure
 
```
~/.config/neowave-lua/
├── neowave.lua          ← Entrypoint (bootstrap + CLI flag handling)
├── menu.lua             ← Interactive ANSI terminal menu
├── core/
│   ├── engine.lua       ← State, config generation, apply/backup/revert
│   └── ascii.lua        ← ASCII loader, image renderer, color substitution
├── ascii/
│   ├── custom/          ← Your own .txt logo files go here
│   └── distro/          ← Distro-specific ASCII overrides
├── backup/              ← Auto-created timestamped .bak files
├── colors/              ← (reserved for future per-session color exports)
└── data/
    └── ascii/           ← Bundled ASCII arts shipped with NeoWave
        ├── arch.txt
        ├── ubuntu.txt
        └── generic.txt
```
 
---
 
## Usage
 
### Interactive menu
 
```bash
neowave
# or
lua5.4 ~/.config/neowave-lua/neowave.lua
```
 
```
[1]  Toggle Fetch Tool     (neofetch / fastfetch)
[2]  Logo / ASCII Art
[3]  Info Module Toggles
[4]  OS & Hardware Spoofing
[5]  Color Presets
[p]  Preview Config
[a]  Apply Config          ← APPLY
[r]  Revert to Backup      ← REVERT
[q]  Quit
```
 
### Headless / scripted
 
```bash
# Preview config for the auto-detected tool
neowave --preview
 
# Apply without interactive confirmation
neowave --apply
 
# Revert to the most recent backup
neowave --revert
```
 
---
 
## Custom ASCII Art
 
Create a file in `~/.config/neowave-lua/ascii/custom/mylogo.txt`.
 
Supported color tokens:
 
```
{red}  {green}  {yellow}  {blue}  {magenta}  {cyan}  {white}
{bright_red}  {bright_green}  {bright_yellow}  {bright_blue}
{bright_magenta}  {bright_cyan}  {bright_white}
{bold}  {dim}  {reset}
```
 
Example:
 
```
{cyan}{bold}
  /\_/\   
 ( o.o )  NeoWave Lua
  > ^ <   
{reset}
```
 
Select it from the menu → `[2] Logo / ASCII Art` → enter `mylogo`.
 
---
 
## Spoofing
 
The spoofing system keeps two parallel tables in memory:
 
```
real    = { os="Arch Linux", cpu="...", ... }   ← read from the live system
spoofed = { os="macOS 15.4", cpu="Apple M4" }  ← your overrides
```
 
On **Apply**, the engine injects the spoofed values into the generated config using the tool-appropriate syntax:
 
- **Neofetch:** `info "OS" "macOS 15.4"  # spoofed by NeoWave`  
- **Fastfetch:** `{ "type": "Custom", "format": "OS: macOS 15.4" }`
Nothing is written to disk until you confirm Apply.
 
---
 
## Adding Color Presets
 
In `core/engine.lua`, extend the `M.COLOR_PRESETS` table:
 
```lua
M.COLOR_PRESETS.gruvbox = {
  red     = "\27[31m",
  green   = "\27[32m",
  yellow  = "\27[33m",
  blue    = "\27[34m",
  magenta = "\27[35m",
  cyan    = "\27[36m",
}
```
 
---
 
## Architecture Notes
 
### No global pollution
 
Every module returns a table (`local M = {} … return M`).  
`require()` caches the result; nothing touches `_G`.
 
### engine.lua state model
 
```lua
state = {
  fetch_tool  : string,   -- "neofetch" | "fastfetch"
  installed   : table,    -- { neofetch=bool, fastfetch=bool }
  logo        : string,   -- name or absolute path
  logo_type   : string,   -- "ascii" | "png" | "gif"
  color_table : table,    -- name → ANSI escape
  modules     : table,    -- name → bool
  spoofed     : table,    -- field → override string
  real        : table,    -- field → live system value (lazy cache)
}
```
 
### Config generation
 
`engine.lua` contains two independent generators:
 
- `generate_neofetch_config()` → Bash `config.conf` syntax  
- `generate_fastfetch_config()` → JSONC syntax with `//` comments
The correct one is called based on `state.fetch_tool` at Apply/Preview time.
 
### Image fallback chain
 
```
User picks PNG/GIF
       ↓
  chafa installed? → render with chafa
       ↓ no
  viu installed?   → render with viu
       ↓ no
  img2txt found?   → render with img2txt
       ↓ no
  WARN + fall back to ASCII distro text  (no crash)
```
 
---
 
## License

This project is licensed under the MIT License — see [LICENSE](https://github.com/ethanlabs101/neowave-lua/blob/main/LICENSE) for details.
