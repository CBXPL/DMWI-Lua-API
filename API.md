# DreamyWare 2026 - Lua API Documentation
## This is an unfinished preview version of lua api 1.0, More will be avaible with full 1.0 release.
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
- [Vec2 / Vec3 Userdata](#vec2--vec3-userdata)
- [Entities API](#entities-api)
- [Examples](#examples)

---

## Hooks
Functions that you should define in your script to be called by the engine.

| Hook | Description |
| :--- | :--- |
| `OnCreateMove(cmd)` | Called every game tick. Use for Anti-Aim and movement logic. |
| `OnDraw()` | Called every frame. Use for UI and custom ESP. |
| `OnEvent(name)` | Called when a game event occurs. <br> Supported: `OnLocalPlayerDeath`, `OnPlayerKill`, `OnPlayerHit`. |

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

### Advanced Overlay API (`adv_overlay`)
Vec2/Vec3-based drawing widgets for screen and world space.

| Function | Description | Example |
| :--- | :--- | :--- |
| `CreateBoxVec2(vec2, w, h, [r], [g], [b], [a], [thickness])` | Draws a 2D box outline at the Vec2 screen position. | `adv_overlay.CreateBoxVec2(pos, 50, 100, 255, 0, 0, 255)` |
| `CreateBoxVec3(vec3, w, h, [r], [g], [b], [a], [thickness])` | Converts Vec3 world pos to screen then draws a box. | `adv_overlay.CreateBoxVec3(worldPos, 50, 100, 0, 255, 0, 255, 3)` |
| `CreateLineVec2(vec2_a, vec2_b, [r], [g], [b], [a], [thickness])` | Draws a line between two Vec2 screen points. | `adv_overlay.CreateLineVec2(p1, p2, 255, 0, 0)` |
| `CreateLineVec3(vec3_a, vec3_b, [r], [g], [b], [a], [thickness])` | Draws a line between two Vec3 world positions (auto W2S). | `adv_overlay.CreateLineVec3(v1, v2, 0, 255, 0)` |
| `CreateDotVec2(vec2, [radius], [r], [g], [b], [a])` | Draws a filled circle at a Vec2 screen position. | `adv_overlay.CreateDotVec2(pos, 5, 255, 0, 0)` |
| `CreateDotVec3(vec3, [radius], [r], [g], [b], [a])` | Draws a filled circle at a Vec3 world position (auto W2S). | `adv_overlay.CreateDotVec3(world, 4, 0, 255, 0)` |
| `CreateFilledSquareVec2(vec2, [size], [r], [g], [b], [a])` | Draws a filled square centered at a Vec2 screen position. | `adv_overlay.CreateFilledSquareVec2(pos, 8, 255, 255, 0)` |
| `CreateFilledSquareVec3(vec3, [size], [r], [g], [b], [a])` | Draws a filled square centered at a Vec3 world position (auto W2S). | `adv_overlay.CreateFilledSquareVec3(world, 8, 0, 255, 255)` |
| `CreateImageVec2(image, tl, tr, br, bl, [r], [g], [b], [a])` | Draws a textured quad from 4 Vec2 screen corners. | `adv_overlay.CreateImageVec2(img, v2(0,0), v2(100,0), v2(100,100), v2(0,100))` |
| `CreateImageVec3(image, tl, tr, br, bl, [r], [g], [b], [a])` | Draws a textured quad from 4 Vec3 world corners (auto W2S). | `adv_overlay.CreateImageVec3(img, v3a, v3b, v3c, v3d)` |
| `CreateTextVec3(vec3, text, [r], [g], [b], [a])` | Draws text at a Vec3 world position (auto W2S). | `adv_overlay.CreateTextVec3(worldPos, "Hello", 255, 255, 0)` |

## Image Loading

| Function | Description | Example |
| :--- | :--- | :--- |
| `ImageLoad(path)` | Loads a .jpg/.png file. Returns an Image object or nil. | `local img = ImageLoad("dreamyimage/test.png")` or `ImageLoad("test.png")` |

The path is relative to the .lua script or absolute. Images are cached (loaded once).

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
| `GetScreenSize()` | Returns screen width and height. | `local w, h = globals.GetScreenSize()` |
| `GetCursorPos()` | Returns cursor position as `Vec2`. | `local pos = globals.GetCursorPos()` |
| `GetFPS()` | Returns current frames per second. | `local fps = globals.GetFPS()` |
| `GetMapName()` | Returns current map name (e.g. "de_dust2"). | `local map = globals.GetMapName()` |

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
| `ComboBox(label, items, selected_var)` | Dropdown list. `items` is a table of strings, `selected_var` stores 1-based index. | `if dreamyUI.ComboBox("Weapon", {"AWP","AK-47"}, "sel") then end` |
| `InputText(label, var_name)` | Single-line text input bound to a string global. | `dreamyUI.InputText("Name", "my_name")` |
| `ColorEdit4(label, var_r, var_g, var_b, var_a)` | RGBA color picker (each channel 0-1, stored in 4 separate globals). | `dreamyUI.ColorEdit4("Color", "cr","cg","cb","ca")` |
| `ListBox(label, items, selected_var)` | Scrollable list box. Same args as ComboBox. | `dreamyUI.ListBox("Map", maps, "sel_map")` |
| `SameLine()` | Places next widget on the same line. | `dreamyUI.SameLine()` |
| `SetNextItemWidth(w)` | Sets width for the next widget. | `dreamyUI.SetNextItemWidth(100)` |

### DreamyUIColors Enums:
`Background`, `TitleBar`, `Text`, `Button`, `ButtonHovered`, `ButtonActive`, `Checkbox`, `Slider`

---

## Global Functions

| Function | Description | Example |
| :--- | :--- | :--- |
| `getValue(key)` | Returns the current state of a cheat setting. Returns `boolean` or `nil` if key doesn't exist. | `local enabled = getValue("antiaim.enabled")` |
| `IsKeyDown(key)` | Returns `true` if a key is held. Keys: `A-Z`, `ALT`, `TAB`, `MOUSE4`, `MOUSE5`. | `if IsKeyDown("W") then ... end` |
| `IsKeyPressed(key)` | Returns `true` once when key is first pressed (edge detection). Same keys as `IsKeyDown`. | `if IsKeyPressed("F1") then ... end` |

### Supported Keys:
| Key | Type | Description |
| :--- | :--- | :--- |
| `antiaim.enabled` | `bool` | Is Anti-Aim enabled |
| `antiaim.left` | `bool` | Is AA Left direction active (keybind held/toggled) |
| `antiaim.right` | `bool` | Is AA Right direction active (keybind held/toggled) |
| `rage.enabled` | `bool` | Is Ragebot enabled |

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

## Vec2 / Vec3 Userdata

Full 2D/3D vector types with operator overloading and methods.

| Constructor | Description | Example |
| :--- | :--- | :--- |
| `Vec2(x, y)` | Creates a new Vec2. | `local v = Vec2(100, 200)` |
| `Vec3(x, y, z)` | Creates a new Vec3. | `local v = Vec3(0, 0, 0)` |

### Vec2 Methods

| Method | Returns | Description |
| :--- | :--- | :--- |
| `v:add(other)` | `Vec2` | Component-wise addition |
| `v:sub(other)` | `Vec2` | Component-wise subtraction |
| `v:mul(scalar)` | `Vec2` | Scalar multiplication |
| `v:div(scalar)` | `Vec2` | Scalar division |
| `v:length()` | `number` | Euclidean length |
| `v:normalize()` | `Vec2` | Returns a normalized copy |
| `v:dot(other)` | `number` | Dot product |
| `v:unpack()` | `x, y` | Returns components as separate numbers |
| `v:clone()` | `Vec2` | Returns a copy |

**Operators:** `+`, `-`, `*` (scalar), `/` (scalar), `tostring`, field access `.x`, `.y`

### Vec3 Methods

| Method | Returns | Description |
| :--- | :--- | :--- |
| `v:add(other)` | `Vec3` | Component-wise addition |
| `v:sub(other)` | `Vec3` | Component-wise subtraction |
| `v:mul(scalar)` | `Vec3` | Scalar multiplication |
| `v:div(scalar)` | `Vec3` | Scalar division |
| `v:length()` | `number` | Euclidean length |
| `v:distance(other)` | `number` | Euclidean distance to another Vec3 |
| `v:normalize()` | `Vec3` | Returns a normalized copy |
| `v:dot(other)` | `number` | Dot product |
| `v:cross(other)` | `Vec3` | Cross product |
| `v:worldToScreen()` | `Vec2` or `nil` | Projects to screen, returns Vec2 or nil if off-screen |
| `v:unpack()` | `x, y, z` | Returns components as separate numbers |
| `v:clone()` | `Vec3` | Returns a copy |

**Operators:** `+`, `-`, `*` (scalar), `/` (scalar), `tostring`, field access `.x`, `.y`, `.z`

### Example
```lua
local lp = entities.GetLocal()
local enemy = entities.getClosestEnemy()
if lp and enemy then
    local myPos = Vec3(lp.pos.x, lp.pos.y, lp.pos.z)
    local enPos = Vec3(enemy.pos.x, enemy.pos.y, enemy.pos.z)
    local dist = myPos:distance(enPos)
    
    local screenPos = enPos:worldToScreen()
    if screenPos then
        adv_overlay.CreateBoxVec2(screenPos, 100, 200, 255, 0, 0, 255, 2)
    end
end
```

---

## Entities API (`entities`)
Access to cheat's internal entity structures.

| Function | Description | Example |
| :--- | :--- | :--- |
| `GetLocal()` | Returns a table with local player info. | `local lp = entities.GetLocal()` |
| `GetLocalEyePos()` | Returns a `Vec3` with local player eye position (origin + view offset). | `local eye = entities.GetLocalEyePos()` |
| `GetPlayerList()` | Returns a table of all players with fields: `name`, `team`, `weapon`. | `for _, p in ipairs(entities.GetPlayerList()) do ... end` |
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
| `TextBackground(x, y, text, [r, g, b, a], [font], [bg_r, bg_g, bg_b, bg_a])` | Draws text with a solid background rect. | `overlay.TextBackground(100, 100, "Label", 255,255,255,255, nil, 0,0,0,180)` |
| `TextBackgroundGradient(x, y, text, [r, g, b, a], [font], {sr,sg,sb,sa}, {mr,mg,mb,ma}, {er,eg,eb,ea})` | Draws text with a horizontal 3-point gradient background. | `overlay.TextBackgroundGradient(100,100,"Hello",nil,nil, {255,0,0,180}, {0,255,0,180}, {0,0,255,180})` |
| `GetDefaultFont()` | Returns the default cheat font. | `local font = overlay.GetDefaultFont()` |
| `LoadFont(path, size)` | Loads a custom font from disk. | `local myFont = overlay.LoadFont("C:\\Windows\\Fonts\\arial.ttf", 16.0)` |

## Visibility API (`visibility`)
Background visibility checking for enemies. Runs every ~30ms using the same bone checks as ESP (HeadPos + Bone 0). Each triangle has early return.

| Function | Description | Example |
| :--- | :--- | :--- |
| `enabled(bool)` | Toggles the background visibility scanner. Clears cache when disabled. | `visibility.enabled(true)` |
| `IsVisible(base)` | Returns `true` if the entity was visible during the last scan. | `if visibility.IsVisible(enemy.base) then ... end` |

---

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
    
    if name == "OnPlayerHit" then
        engine.ExecuteCMD("say +1 hit")
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

### AA Indicator with Gradient Background
Shows Anti-Aim status on the left side with a sleek transparent-black-transparent gradient background.
```lua
function OnDraw()
    local aa_enabled = getValue("antiaim.enabled")
    local aa_left = getValue("antiaim.left")
    local aa_right = getValue("antiaim.right")
    
    local text = "AA: OFF"
    if aa_enabled and aa_left then text = "AA: <<< LEFT"
    elseif aa_enabled and aa_right then text = "AA: RIGHT >>>"
    elseif aa_enabled then text = "AA: ON"
    end
    
    overlay.TextBackgroundGradient(15, 15, "  " .. text .. "  ",
        255, 255, 255, 255, nil,
        {0, 0, 0, 0}, {0, 0, 0, 200}, {0, 0, 0, 0}
    )
end
```

### Visibility Pitch Flick
Flicks Anti-Aim pitch to -89 for 5ms every 20ms cycle when an enemy is visible using w64.CurrentTime() timing.
```lua
visibility.enabled(true)

function OnCreateMove()
    local lp = entities.GetLocal()
    if not lp then return end
    
    local enemy = entities.getClosestEnemy()
    if enemy and visibility.IsVisible(enemy.base) then
        if (w64.CurrentTime() % 20) < 5 then
            antiaim.override(true)
            antiaim.set_pitch(-89.0)
            return
        end
    end
    
    antiaim.override(false)
end
```

### 3D Box at Map Center
Draws a 40x40x60 box at the center of the map.
```lua
function OnDraw()
    local halfW, halfD, halfH = 20, 20, 30
    local corners = {
        Vec3(-halfW, -halfD, -halfH), Vec3( halfW, -halfD, -halfH),
        Vec3( halfW,  halfD, -halfH), Vec3(-halfW,  halfD, -halfH),
        Vec3(-halfW, -halfD,  halfH), Vec3( halfW, -halfD,  halfH),
        Vec3( halfW,  halfD,  halfH), Vec3(-halfW,  halfD,  halfH),
    }
    local edges = {{0,1},{1,2},{2,3},{3,0},{4,5},{5,6},{6,7},{7,4},{0,4},{1,5},{2,6},{3,7}}
    for _, e in ipairs(edges) do
        adv_overlay.CreateLineVec3(corners[e[1]+1], corners[e[2]+1], 0, 255, 100, 255, 2)
    end
end
```


### 3D Box with Color Picker (72u high)
Customizable 3D box on enemies — color via dreamyUI.
```lua
cr = 1.0; cg = 0.2; cb = 0.2; ca = 1.0

function OnDraw()
    if dreamyUI.CreateWindow("3D Box") then
        dreamyUI.ColorEdit4("Color", "cr", "cg", "cb", "ca")
        dreamyUI.EndWindow()
    end

    local ri = math.floor(cr*255 + 0.5)
    local gi = math.floor(cg*255 + 0.5)
    local bi = math.floor(cb*255 + 0.5)
    local ai = math.floor(ca*255 + 0.5)

    for _, ent in ipairs(entities.GetEnemies()) do
        local x, y, z = ent.pos.x, ent.pos.y, ent.pos.z
        local hw = 20
        local corners = {
            Vec3(x-hw, y-hw, z),     Vec3(x+hw, y-hw, z),
            Vec3(x+hw, y+hw, z),     Vec3(x-hw, y+hw, z),
            Vec3(x-hw, y-hw, z+72), Vec3(x+hw, y-hw, z+72),
            Vec3(x+hw, y+hw, z+72), Vec3(x-hw, y+hw, z+72),
        }
        local edges = {{0,1},{1,2},{2,3},{3,0},{4,5},{5,6},{6,7},{7,4},{0,4},{1,5},{2,6},{3,7}}
        for _, e in ipairs(edges) do
            adv_overlay.CreateLineVec3(corners[e[1]+1], corners[e[2]+1], ri, gi, bi, ai, 2)
        end
    end
end
```

---
*(Copyright) 2026 DreamyWare*
