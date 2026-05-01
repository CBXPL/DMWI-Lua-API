# DreamyWare 2026 - Lua API Documentation
This document describes the available Lua functions for creating scripts in DreamyWare.

## Table of Contents
- [Hooks](#hooks)
- [Engine API](#engine-api)
- [Globals API](#globals-api)
- [Windows 64-bit API](#windows-64-bit-api)
- [Anti-Aim API](#anti-aim-api)
- [DreamyUI API](#dreamyui-api)
- [Memory API](#memory-api)
- [Entities API](#entities-api)

---

## Hooks
Functions that you should define in your script to be called by the engine.

| Hook | Description |
| :--- | :--- |
| `OnCreateMove()` | Called every game tick. Use for Anti-Aim and movement logic. |
| `OnDraw()` | Called every frame. Use for UI and custom ESP. |
| `OnEvent(name)` | Called when a game event occurs (e.g., "OnLocalPlayerDeath"). |

**Example:**
```lua
function OnDraw()
    -- Your UI code here
end
```

---

## Engine API (`engine`)
Interaction with the game engine.

| Function | Description | Example |
| :--- | :--- | :--- |
| `ExecuteCMD(command)` | Executes a console command. | `engine.ExecuteCMD("say Hello")` |
| `IsInGame()` | Returns true if connected to a server. | `if engine.IsInGame() then ... end` |

---

## Globals API (`globals`)
Access to global game variables.

| Function | Description | Example |
| :--- | :--- | :--- |
| `RealTime()` | Returns the current unix timestamp. | `local t = globals.RealTime()` |

---

## Windows 64-bit API (`w64`)
System-level timing.

| Function | Description | Example |
| :--- | :--- | :--- |
| `CurrentTime()` | Returns system uptime in milliseconds (GetTickCount64). | `local now = w64.CurrentTime()` |

---

## Anti-Aim API (`antiaim`)
Control over player rotations.

| Function | Description | Example |
| :--- | :--- | :--- |
| `override(bool)` | Enables/disables Lua angle override. | `antiaim.override(true)` |
| `set_yaw(float)` | Sets the custom absolute Yaw angle. | `antiaim.set_yaw(180.0)` |
| `set_pitch(float)` | Sets the custom absolute Pitch angle. | `antiaim.set_pitch(89.0)` |
| `get_original_yaw()` | Gets the original mouse Yaw angle. | `local y = antiaim.get_original_yaw()` |
| `get_original_pitch()` | Gets the original mouse Pitch angle. | `local p = antiaim.get_original_pitch()` |
| `get_menu_yaw()` | Gets the Yaw angle calculated by the cheat menu. | `local y = antiaim.get_menu_yaw()` |
| `get_menu_pitch()` | Gets the Pitch angle calculated by the cheat menu. | `local p = antiaim.get_menu_pitch()` |

---

## DreamyUI API (`dreamyUI`)
Custom menu and floating window creation.

| Function | Description | Example |
| :--- | :--- | :--- |
| `CreateWindow(title)` | Starts a new window. Returns true if visible. | `if dreamyUI.CreateWindow("Test") then ... end` |
| `EndWindow()` | Closes the current window. | `dreamyUI.EndWindow()` |
| `Checkbox(label, var_name)` | Creates a checkbox linked to a global variable. | `dreamyUI.Checkbox("Enable", "b_enabled")` |
| `SliderInt(label, var, min, max)` | Creates an integer slider. | `dreamyUI.SliderInt("Speed", "i_speed", 0, 100)` |
| `SliderFloat(label, var, min, max)`| Creates a float slider. | `dreamyUI.SliderFloat("FOV", "f_fov", 0, 180)` |
| `Button(label)` | Returns true if the button was clicked. | `if dreamyUI.Button("Reset") then ... end` |
| `Separator()` | Adds a horizontal line. | `dreamyUI.Separator()` |
| `Text(text)` | Displays simple text. | `dreamyUI.Text("Hello World")` |
| `SetColor(color_idx, r, g, b, a)` | Changes the style color of UI elements. | `dreamyUI.SetColor(dreamyUIColors.Button, 148, 0, 255, 255)` |

### DreamyUIColors Enums:
`Background`, `TitleBar`, `Text`, `Button`, `ButtonHovered`, `ButtonActive`, `Checkbox`, `Slider`

---

## Memory API (`memory`)
Direct access to game memory (Handle with care!).

| Function | Description | Example |
| :--- | :--- | :--- |
| `GetModule(name)` | Returns the base address of a DLL. | `local base = memory.GetModule("client.dll")` |
| `ReadInt(address)` | Reads a 4-byte integer from memory. | `local hp = memory.ReadInt(addr + 0x34C)` |
| `ReadFloat(address)` | Reads a float from memory. | `local f = memory.ReadFloat(addr + 0x10)` |
| `ReadPtr(address)` | Reads a 64-bit pointer address. | `local ptr = memory.ReadPtr(base + 0x123)` |

---

## Entities API (`entities`)
Access to cheat's internal entity structures.

| Function | Description | Example |
| :--- | :--- | :--- |
| `GetLocal()` | Returns a table with local player info (base, name, team, pos). | `local lp = entities.GetLocal()` |
| `GetEnemies()` | Returns a list of all visible enemies. | `local enemies = entities.GetEnemies()` |
| `GetTeammates()` | Returns a list of all teammates. | `local team = entities.GetTeammates()` |

**Entity Table Structure:**
- `.base` (Number/Pointer)
- `.name` (String)
- `.team` (Number)
- `.pos` (Table: `{x, y, z}`)

---
*(Copyright) 2026 DreamyWare*
