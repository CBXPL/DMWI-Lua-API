# DreamyWare 2026 - Lua API Documentation

This document describes the available Lua functions for creating scripts in DreamyWare.

## Table of Contents
- [Hooks](#hooks)
- [Engine API](#engine-api)
- [ESP API](#esp-api)
- [User Input API](#user-input-api)
- [Globals API](#globals-api)
- [Windows 64-bit API](#windows-64-bit-api)
- [Anti-Aim API](#anti-aim-api)
- [DreamyUI API](#dreamyui-api)
- [Memory API](#memory-api)
- [Entities API](#entities-api)
- [Examples](#examples)

---

## Hooks
Functions that you should define in your script to be called by the engine.

| Hook | Description |
| :--- | :--- |
| `OnCreateMove(cmd)` | Called every game tick. Use for Anti-Aim and movement logic. |
| `OnDraw()` | Called every frame. Use for UI and custom ESP. |
| `OnEvent(name)` | Called when a game event occurs. <br> Supported: `OnLocalPlayerDeath`, `OnPlayerKill`. |

---

## Engine API (`engine`)
Interaction with the game engine.

| Function | Description | Example |
| :--- | :--- | :--- |
| `ExecuteCMD(command)` | Executes a console command. | `engine.ExecuteCMD("say Hello")` |
| `IsInGame()` | Returns true if connected to a server. | `if engine.IsInGame() then ... end` |

---

## ESP API (`esp`)
Custom rendering and visual utilities.

| Function | Description | Example |
| :--- | :--- | :--- |
| `AddFlag(base, pos, text, r, g, b, a)` | Adds a stacked flag to an entity. <br> Pos: `top`, `bottom`, `left`, `right`. | `esp.AddFlag(e.base, "right", "LETHAL", 255, 0, 0, 255)` |
| `Text(x, y, text, r, g, b, a, font)` | Draws simple text on screen. | `esp.Text(100, 100, "DreamyWare", 255, 255, 255, 255)` |
| `GetDefaultFont()` | Returns the internal UI font. | `local f = esp.GetDefaultFont()` |
| `LoadFont(path, size)` | Loads a .ttf font from disk. | `local f = esp.LoadFont("C:\\Windows\\Fonts\\Arial.ttf", 18)` |
| `GetBoundingBox(base)` | Returns screen coords for entity. | `local box = esp.GetBoundingBox(e.base)` |

**Box Structure:** `{left, top, right, bottom, visible}`

---

## User Input API (`usercmd`)
Direct manipulation of game input.

| Function | Description | Example |
| :--- | :--- | :--- |
| `sendInput(flag)` | Holds a button for 1 tick (15.6ms). | `usercmd.sendInput(IN_JUMP)` |

### Supported Input Flags:
`IN_ATTACK`, `IN_ATTACK2`, `IN_JUMP`

---

## Globals API (`globals`)
...
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
| `GetLocal()` | Returns a table with local player info. | `local lp = entities.GetLocal()` |
| `GetEnemies()` | Returns a list of all visible enemies. | `local enemies = entities.GetEnemies()` |
| `GetTeammates()` | Returns a list of all teammates. | `local team = entities.GetTeammates()` |
| `getClosestEnemy()` | Returns the single closest enemy based on 3D distance. | `local target = entities.getClosestEnemy()` |
| `getClosestTeammate()` | Returns the single closest teammate. | `local friend = entities.getClosestTeammate()` |
| `getClosestToCrosshairEnemy()` | Returns the enemy closest to screen center (FOV-based, best for aimbot). | `local target = entities.getClosestToCrosshairEnemy()` |
| `getClosestToCrosshairTeammate()` | Returns the teammate closest to screen center. | `local friend = entities.getClosestToCrosshairTeammate()` |
| `getUnderCrosshairEnemy()` | Returns the **enemy** the engine crosshair is directly on (ray-cast). Returns `nil` if aiming at teammate or empty space. | `local t = entities.getUnderCrosshairEnemy()` |
| `getUnderCrosshairTeammate()` | Returns the **teammate** the engine crosshair is directly on (ray-cast). Returns `nil` if aiming at enemy or empty space. | `local t = entities.getUnderCrosshairTeammate()` |
| `getHeadPos(base)` | Returns head position XYZ (Bone ID 7). | `local hx, hy, hz = entities.getHeadPos(enemy.base)` |
| `getBonePos(base, id)` | Returns position of a specific bone ID. | `local bx, by, bz = entities.getBonePos(enemy.base, 8)` |

**Entity Table Structure:**
- `.base` (Number/Pointer)
- `.team` (Number)
- `.index` (Number)
- `.name` (String)
- `.isUnderCrosshair` (Boolean) — Engine ray-cast hit. True only when the CS2 engine places the crosshair directly on the model. Works in all functions.
- `.isClosestToCrosshair` (Boolean) — True only when returned by `getClosestToCrosshairEnemy/Teammate`. FOV-based, does not require direct ray-cast hit.
- `.pos` (Table: `{x, y, z}`)
- `.velocity` (Table: `{x, y, z, length2d}`)
---

## Overlay API (`overlay`)
Render custom graphics and text directly on the screen (above the game).

| Function | Description | Example |
| :--- | :--- | :--- |
| `Text(x, y, text, [r, g, b, a], [font])` | Draws text at absolute X, Y screen coordinates. | `overlay.Text(100, 100, "Hello!", 255, 0, 0)` |
| `Graph(x, y, length, value, [min, max])` | Draws a scrolling line graph (like a wave) tracking `value`. Optional `min` and `max` limits prevent auto-scaling "bouncing". | `overlay.Graph(50, 50, 200, lp.velocity.length2d, 0, 256)` |
| `SetColor(target, r, g, b, a)` | Changes the default color of overlay components globally. | `overlay.SetColor(overlay.colors.Graph, 255, 0, 0, 255)` |
| `GetDefaultFont()` | Returns the default cheat font. | `local font = overlay.GetDefaultFont()` |
| `LoadFont(path, size)` | Loads a custom font from disk. | `local myFont = overlay.LoadFont("C:\\Windows\\Fonts\\arial.ttf", 16.0)` |

**`overlay.colors` Enum:**
- `Font`
- `Graph`

---

## Windows 64-bit API (`w64`)
System-level timing and input.

| Function | Description | Example |
| :--- | :--- | :--- |
| `CurrentTime()` | Returns system uptime in milliseconds (GetTickCount64). | `local now = w64.CurrentTime()` |
| `input.sendInput(key)` | Simulates Windows mouse click. Supported: `"(LMB)"`, `"(RMB)"`. | `w64.input.sendInput("(LMB)")` |

---

## UserCmd API (`usercmd`)
Manipulate the current user command (CUserCmd).

| Function | Description | Example |
| :--- | :--- | :--- |
| `sendInput(IN_FLAG)` | Injects a button press into the user command. <br>**Supported flags:** `IN_ATTACK`, `IN_ATTACK2`, `IN_JUMP` | `usercmd.sendInput(IN_JUMP)` |
| `GetViewAngles()` | Gets current view angles from memory (returns x, y, z). | `local x, y, z = usercmd.GetViewAngles()` |
| `SetViewAngles(x, y)` | Sets engine view angles (x = pitch, y = yaw). | `usercmd.SetViewAngles(89, 180)` |

---

## Examples

### Kill/Death Say
Automatically sends messages to chat upon specific events.
```lua
function OnEvent(name)
    if name == "OnPlayerKill" then
        engine.ExecuteCMD("say ez ez")
    end
    
    if name == "OnLocalPlayerDeath" then
       engine.ExecuteCMD("say bruh")
    end
end
```

### Simple Speedometer
Displays your current horizontal movement speed in a custom window.
```lua
function OnDraw()
    local lp = entities.GetLocal()
    if lp and dreamyUI.CreateWindow("Stats") then
        local speed = math.floor(lp.velocity.length2d)
        dreamyUI.Text("Current Speed: " .. speed .. " u/s")
        dreamyUI.EndWindow()
    end
end
```

### Simple Speed Graph
Displays your current horizontal movement speed in an overlay graph.
```lua
function OnDraw()
    local lp = entities.GetLocal()
    
    if lp then
        overlay.SetColor(overlay.colors.Font, 255, 255, 255, 255)
        overlay.SetColor(overlay.colors.Graph, 255, 0, 0, 255)
        
        local speed = math.floor(lp.velocity.length2d)
        overlay.Text(100, 100, "Speed: " .. speed)
        overlay.Graph(100, 120, 200, speed, 0, 260) 
    end
end
```

### Custom Head Aimbot
Automatically calculates angles and aims at the closest enemy's head (Bone 7).
```lua
local function CalcAngle(srcX, srcY, srcZ, dstX, dstY, dstZ)
    local deltaX = dstX - srcX
    local deltaY = dstY - srcY
    local deltaZ = dstZ - srcZ

    local hyp = math.sqrt(deltaX * deltaX + deltaY * deltaY)
    
    local pitch = -(math.atan(deltaZ, hyp) * 180.0 / math.pi)
    local yaw = (math.atan(deltaY, deltaX) * 180.0 / math.pi)
    
    return pitch, yaw
end

function OnCreateMove(cmd)
    local lp = entities.GetLocal()
    local enemy = entities.getClosestEnemy()
    
    if lp and enemy then
        local myX, myY, myZ = entities.getBonePos(lp.base, 7)
        local eX, eY, eZ = entities.getHeadPos(enemy.base)
        
        if myX and myY and myZ and eX and eY and eZ then
            local pitch, yaw = CalcAngle(myX, myY, myZ, eX, eY, eZ)
            usercmd.SetViewAngles(pitch, yaw)
        end
    end
end
```

### Custom Triggerbot
Automatically shoots when the engine crosshair is directly on an enemy.
```lua
function OnCreateMove()
    local target = entities.getUnderCrosshairEnemy()
    if target then
        usercmd.sendInput(IN_ATTACK)
    end
end
```

---
*(Copyright) 2026 DreamyWare*
