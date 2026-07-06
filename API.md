# DreamyWare 2026 - Lua API Documentation

## Table of Contents
- [Hooks](#hooks)
- [Engine API](#engine-api)
- [ESP API](#esp-api)
- [Image Loading](#image-loading)
- [User Input API](#user-input-api)
- [Globals API](#globals-api)
- [Global Functions](#global-functions)
- [Windows 64-bit API](#windows-64-bit-api)
- [Anti-Aim API](#anti-aim-api)
- [SetModel API](#setmodel-api-setmodel)
- [DreamyUI API](#dreamyui-api)
- [Chams API](#chams-api)
- [Memory API](#memory-api)
- [Offset Access API](#offset-access-api)
- [Standard Library (`std`)](#standard-library-std)
- [Hook API (`Hook`)](#hook-api-hook)
- [Vec2 / Vec3 Userdata](#vec2--vec3-userdata)
- [Entities API](#entities-api)
- [Weapon VData API (`weapon`)](#weapon-vdata-api-weapon)
- [Spread Manipulation API (`spreadmanipulation`)](#spread-manipulation-api-spreadmanipulation)
- [ConVar API (`convar`)](#convar-api-convar)
- [Sound API (`vsnd`)](#sound-api-vsnd)
- [Overlay API (`overlay`)](#overlay-api-overlay)
- [Visibility API (`visibility`)](#visibility-api-visibility)
- [UserCmd API (`usercmd`)](#usercmd-api-usercmd)
- [Examples](#examples)

---

## Hooks
Functions that you should define in your script to be called by the engine.

| Hook | Description |
| :--- | :--- |
| `OnCreateMove(cmd)` | Called every game tick. Use for Anti-Aim and movement logic. |
| `OnDraw()` | Called every frame. Use for UI and custom ESP. |
| `OnEvent(name)` | Called when a game event occurs. <br> Supported: `OnLocalPlayerDeath`, `OnPlayerKill`, `OnPlayerHit`. |
| `OnFrameStage(stage)` | Called every frame stage. Use `FRAME_RENDER_START`, `FRAME_RENDER_END`, `FRAME_NET_UPDATE_START`, `FRAME_NET_UPDATE_END` constants to filter. |

---

## Engine API (`engine`)
Interaction with the game engine.

| Function | Description | Example |
| :--- | :--- | :--- |
| `ExecuteCMD(command)` | Executes a console command. | `engine.ExecuteCMD("say Hello")` |
| `IsInGame()` | Returns true if connected to a server. | `if engine.IsInGame() then ... end` |
| `WorldToScreen(x, y, z)` | Projects 3D world coordinates to 2D screen. Returns `sx, sy` or `nil` if off-screen. | `local sx, sy = engine.WorldToScreen(pos.x, pos.y, pos.z)` |

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
| `CreateQuadVec2(a, b, c, d, [r], [g], [b], [a])` | Draws a filled convex quad from 4 Vec2 screen points. | `adv_overlay.CreateQuadVec2(p1, p2, p3, p4, 255, 0, 0)` |
| `CreateQuadVec3(a, b, c, d, [r], [g], [b], [a])` | Draws a filled convex quad from 4 Vec3 world points (auto W2S). | `adv_overlay.CreateQuadVec3(w1, w2, w3, w4, 0, 255, 0)` |
| `CreateVerticeVec2(a, b, c, [r], [g], [b], [a])` | Draws a filled triangle from 3 Vec2 screen points. | `adv_overlay.CreateVerticeVec2(p1, p2, p3, 0, 0, 255)` |
| `CreateVerticeVec3(a, b, c, [r], [g], [b], [a])` | Draws a filled triangle from 3 Vec3 world points (auto W2S). | `adv_overlay.CreateVerticeVec3(w1, w2, w3, 255, 255, 0)` |
| `CreateTriangleVec2(a, b, c, [r], [g], [b], [a], [thickness])` | Draws a hollow triangle outline from 3 Vec2 screen points. | `adv_overlay.CreateTriangleVec2(p1, p2, p3, 255, 0, 0, 255, 2)` |
| `CreateTriangleVec3(a, b, c, [r], [g], [b], [a], [thickness])` | Draws a hollow triangle outline from 3 Vec3 world points (auto W2S). | `adv_overlay.CreateTriangleVec3(w1, w2, w3, 0, 255, 0, 255, 3)` |
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

## SetModel API (`SetModel`)
Set agent or weapon models via Lua.

| Function | Description | Example |
| :--- | :--- | :--- |
| `agent(path)` | Sets the local player's agent model to the given path. Uses ResourceSystem PreCache + BlockingLoadResourceByName + CModelState fallback. Only works for official game models (custom .vmdl files not in manifest cannot be loaded). | `SetModel.agent("agents/models/tm_professional/tm_professional_vari.vmdl")` |

### Example – Number K Agent
```lua
-- Number K agent (T-side)
SetModel.agent("agents/models/tm_professional/tm_professional_vari.vmdl")
```

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
| `SetCursorPos(x, y)` | Sets the cursor position for the next widget (relative to window). | `dreamyUI.SetCursorPos(50, 100)` |

### DreamyUI Flags (`dreamyUI.flags`)
Global UI appearance settings applied to every subsequent window.

| Function | Description | Example |
| :--- | :--- | :--- |
| `WindowSize(w, h)` | Sets default size for new windows (0,0 = auto). | `dreamyUI.flags.WindowSize(400, 300)` |
| `RoundedCorners(bool)` | Enables/disables rounded corners (default true). | `dreamyUI.flags.RoundedCorners(false)` |
| `NoResize(bool)` | Enables/disables the resize grip corner (default false). | `dreamyUI.flags.NoResize(true)` |
| `InputTextSize(w)` | Sets a custom width in pixels for subsequent InputText widgets (default 80). | `dreamyUI.flags.InputTextSize(150)` |

### DreamyUIColors Enums:
`Background`, `TitleBar`, `Text`, `Button`, `ButtonHovered`, `ButtonActive`, `Checkbox`, `Slider`

---

## Global Functions

| Function | Description | Example |
| :--- | :--- | :--- |
| `getValue(key)` | Returns the current state of a cheat setting. Returns `boolean` or `nil` if key doesn't exist. | `local enabled = getValue("antiaim.enabled")` |
| `setValue(key, value)` | Sets a cheat setting. Pass `"true"` or `"false"` (strings) to toggle booleans. Useful for custom toggle scripts. | `setValue("ragebot.autofire", "true")` |
| `IsKeyDown(key)` | Returns `true` if a key is held. Keys: `A-Z`, `ALT`, `TAB`, `MOUSE4`, `MOUSE5`. | `if IsKeyDown("W") then ... end` |
| `IsKeyPressed(key)` | Returns `true` once when key is first pressed (edge detection). Same keys as `IsKeyDown`. | `if IsKeyPressed("F1") then ... end` |

### Supported Keys:
| Key | Type | Description |
| :--- | :--- | :--- |
| `antiaim.enabled` | `bool` | Is Anti-Aim enabled |
| `antiaim.left` | `bool` | Is AA Left direction active (keybind held/toggled) |
| `antiaim.right` | `bool` | Is AA Right direction active (keybind held/toggled) |
| `antiaim.spin` | `bool` | Is AA Spin active |
| `rage.enabled` | `bool` | Is Ragebot enabled |
| `rage.doubletap` | `bool` | Is doubletap active |
| `ragebot.doubletap` | `bool` | Is Double Tap enabled |
| `ragebot.autofire` | `bool` | Is Auto Fire enabled |
| `ragebot.hideshots` | `bool` | Is Hide Shots enabled |
| `ragebot.silent` | `bool` | Is Silent Aim enabled |
| `legitbot.enabled` | `bool` | Is Legitbot enabled |
| `legitbot.silent` | `bool` | Is Legitbot Silent Aim enabled |
| `misc.bhop` | `bool` | Is bhop enabled |
| `misc.subtickstrafe` | `bool` | Is autostrafe enabled |

### Entity Flags (`FL_` constants)
Use with `entities.GetFlags()` and `bit.band()`.

| Flag | Value | Description |
| :--- | :--- | :--- |
| `FL_ONGROUND` | `1` | Entity is on the ground |
| `FL_DUCKING` | `2` | Entity is ducking |

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

## Offset Access API (`offset`)
Returns raw offset values from `Offsets.hpp` so you don't have to remember hex. Combine with `memory` API.

| Function | Description | Example |
| :--- | :--- | :--- |
| `Get(name)` | Returns the offset value (ptrdiff_t) for the given name, or `nil` if unknown. | `local hpOff = offset.Get("Health")` |

### Usage with memory API
```lua
local lp = entities.GetLocal()
if lp then
    -- Read defuser status via item services
    local itemSvc = memory.ReadPtr(lp.base + offset.Get("m_pItemServices"))
    local hasDefuser = memory.ReadBool(itemSvc + offset.Get("CCSPlayer_ItemServices.m_bHasDefuser"))

    -- Read weapon price
    local weaponBase = memory.ReadPtr(memory.ReadPtr(lp.base + 0x60) + 0x10)
    local price = memory.ReadInt(weaponBase + offset.Get("CCSWeaponBaseVData.m_nPrice"))
end
```

### Supported Offset Names

| Name | Offset | Type | Read | Write | Description |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `CCSWeaponBaseVData.m_nKillAward` | 0x710 | int32 | Yes | Yes | Kill reward money |
| `CCSWeaponBaseVData.m_nPrice` | 0x70C | int32 | Yes | Yes | Weapon buy price |
| `CCSWeaponBaseVData.m_bIsRevolver` | 0x71E | bool | Yes | Yes | Is revolver weapon |
| `m_pItemServices` | 0x11E8 | ptr | Yes | No | CPlayer_ItemServices pointer |
| `CCSPlayer_ItemServices.m_bHasDefuser` | 0x48 | bool | Yes | Yes | Has defuser kit |
| `CCSPlayer_ItemServices.m_bHasHelmet` | 0x49 | bool | Yes | Yes | Has helmet |
| `m_pAimPunchServices` | 0x1490 | ptr | Yes | No | CCSPlayer_AimPunchServices pointer |
| `C_SmokeGrenadeProjectile.m_nVoxelUpdate` | 0x1294 | int32 | Yes | Yes | Voxel update counter |
| `C_SmokeGrenadeProjectile.m_vSmokeColor` | 0x125C | Vector(12B) | Yes | Yes | Smoke color RGB |
| `C_SmokeGrenadeProjectile.m_bDidSmokeEffect` | 0x1254 | bool | Yes | Yes | Did smoke effect fire |
| `CEntityInstance.m_pEntity` | 0x10 | ptr | Yes | No | CEntityIdentity pointer |
| `CEntityIdentity.m_nameStringableIndex` | 0x14 | int32 | Yes | Yes | String table index |
| `CEntityIdentity.m_name` | 0x18 | ptr | Yes | No | Entity name symbol |
| `CEntityIdentity.m_designerName` | 0x20 | ptr | Yes | No | Designer class name symbol |

---

## Standard Library (`std`)
Useful math and utility functions.

| Function | Description | Example |
| :--- | :--- | :--- |
| `clamp(val, min, max)` | Clamps value between min and max. | `std.clamp(15, 0, 10)` → `10` |
| `min(a, b)` | Returns the smaller of two numbers. | `std.min(3, 7)` → `3` |
| `max(a, b)` | Returns the larger of two numbers. | `std.max(3, 7)` → `7` |
| `lerp(a, b, t)` | Linear interpolation (`a + (b-a)*t`). | `std.lerp(0, 100, 0.5)` → `50` |
| `sign(x)` | Returns `1` for positive, `-1` for negative, `0` for zero. | `std.sign(-5)` → `-1` |
| `round(x)` | Rounds to nearest integer. | `std.round(3.7)` → `4` |
| `sqrt(x)` | Square root. | `std.sqrt(25)` → `5` |

---

## Hook API (`Hook`)
Direct game function hooking and calling from Lua using MinHook.

| Function | Description | Example |
| :--- | :--- | :--- |
| `FindPattern(module, pattern)` | Scans a module for a byte pattern. Returns address or `0`. | `Hook.FindPattern("client.dll", "48 8B C4")` |
| `CreateHook(module, pattern, name)` | Hooks a function by pattern. | `Hook.CreateHook("client.dll", "...", "MyFunc")` |
| `CreateHookDirect(addr, name)` | Same as `CreateHook` but takes a direct address instead of scanning. | `Hook.CreateHookDirect(addr, "MyFunc")` |
| `HookVTable(vtablePtr, index, name)` | Hooks a vtable entry by patching the vtable. | `Hook.HookVTable(vtableAddr, 5, "MyVFunc")` |
| `Call(addr, a1, ..., a10)` | Calls a function at `addr` with up to 10 arguments (all passed as uint64). Returns the uint64 result. | `Hook.Call(fn, thisPtr, arg1, 0xFFFFFFFF, 0)` |

### Hook Handler
When a hooked function is called, your Lua function receives up to 10 arguments as Lua numbers. If it returns a number, that value is used as the return value. If it returns nothing, the original function is called.

### Example — Remove Scope Overlay
```lua
Hook.CreateHook("client.dll", "48 8B C4 53 57 48 83 EC ? 48 8B FA 44 0F 29 40 ? 48 8B 51 ? 48 8B", "DrawScopeOverlay")

function DrawScopeOverlay(a1, a2)
    return 0 -- null result → scope overlay not drawn
end
```

### Example — Call native function by pattern
```lua
local fn = Hook.FindPattern("client.dll", "48 89 5C 24 08 57")
-- call with args (this, pPassEntity, mask, layer, unkNum)
local result = Hook.Call(fn, thisPtr, entPtr, 0xFFFFFFFFFFFFFFFF, 0, 0)
```

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
| `v:toVec3([z])` | `Vec3` | Converts to Vec3 (optional z, default 0) |

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
| `v:toVec2()` | `Vec2` | Converts to Vec2 (drops Z) |

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
| `ComputeRandomSeed(pitch, yaw, roll, tick)` | Calculates engine's random seed for given angles and tick. | `local seed = entities.ComputeRandomSeed(89, 180, 0, lp.TickBase + 1)` |
| `GetCurrentSpread(base, seed, inaccuracy, spread)` | Calculates spread using the engine function, returns table with `{x, y}`. | `local spread = entities.GetCurrentSpread(localPlayer.base, seed, inaccuracy, basespread)` |
| `GetBaseSpread(base)` | Returns base weapon spread from weapon data. | `local spread = entities.GetBaseSpread(localPlayer.base)` |
| `GetInaccuracy(base)` | Returns current inaccuracy from weapon. | `local inaccuracy = entities.GetInaccuracy(localPlayer.base)` |
| `GetBaseSpreadSeed(base)` | Returns base weapon spread seed from weapon data. | `local seed = entities.GetBaseSpreadSeed(localPlayer.base)` |
| `GetBaseRecoilSeed(base)` | Returns base weapon recoil seed from weapon data. | `local seed = entities.GetBaseRecoilSeed(localPlayer.base)` |
| `GetBaseRecoilMagnitude(base)` | Returns base weapon recoil magnitude from weapon data. | `local mag = entities.GetBaseRecoilMagnitude(localPlayer.base)` |
| `GetFlags(base)` | Returns entity flags (FL_ONGROUND, FL_DUCKING, etc.). | `local flags = entities.GetFlags(lp.base)` |

**Entity Table Structure:**
- `.base` (Number/Pointer)
- `.health` (Number) — HP 0-100
- `.money` (Number) — Current money (from m_iAccount)
- `.team` (Number)
- `.index` (Number)
- `.name` (String)
- `.isScoped` (Boolean) — True if entity is scoped
- `.currentClip` (Number) — Current ammo in clip
- `.maxClip` (Number) — Maximum ammo capacity of clip
- `.TickBase` (Number) — The entity's m_nTickBase
- `.isUnderCrosshair` (Boolean) — Engine ray-cast hit. True only when the CS2 engine places the crosshair directly on the model. Works in all functions.
- `.isClosestToCrosshair` (Boolean) — True only when returned by `getClosestToCrosshairEnemy/Teammate`. FOV-based, does not require direct ray-cast hit.
- `.pos` (Table: `{x, y, z}`)
- `.velocity` (Table: `{x, y, z, length2d}`)

---

## Weapon VData API (`weapon`)
Read-only access to weapon data (CCSWeaponBaseVData) from the currently held weapon.

| Function | Description | Example |
| :--- | :--- | :--- |
| `GetDamage(base)` | Returns weapon damage per shot. | `local dmg = weapon.GetDamage(lp.base)` |
| `GetMaxSpeed(base)` | Returns max movement speed with this weapon (units/sec). | `local spd = weapon.GetMaxSpeed(lp.base)` |
| `IsFullAuto(base)` | Returns true if weapon is fully automatic. | `if weapon.IsFullAuto(lp.base) then ... end` |

---

## Spread Manipulation API (`spreadmanipulation`)
Calculate spread offsets for accuracy prediction.

| Function | Description | Example |
| :--- | :--- | :--- |
| `CalculateSpread(base, seed, inaccuracy, spread)` | Calculates spread X/Y for given parameters. | `local sx, sy = spreadmanipulation.CalculateSpread(base, seed, inacc, spr)` |
| `GetSeed(base)` | Returns current weapon random seed. | `local seed = spreadmanipulation.GetSeed(lp.base)` |
| `GetInaccuracy(base)` | Returns current weapon inaccuracy. | `local inacc = spreadmanipulation.GetInaccuracy(lp.base)` |
| `GetSpread(base)` | Returns current weapon spread. | `local spr = spreadmanipulation.GetSpread(lp.base)` |
| `GetRecoilIndex(base)` | Returns current recoil index. | `local ri = spreadmanipulation.GetRecoilIndex(lp.base)` |

---

## ConVar API (`convar`)
Read console variables by name.

| Function | Description | Example |
| :--- | :--- | :--- |
| `Get(name)` | Returns the CConvar pointer (or nil). | `local cv = convar.Get("sv_cheats")` |
| `GetFloat(name)` | Returns the float value of a convar. | `local fov = convar.GetFloat("fov_cs_debug")` |
| `GetInt(name)` | Returns the integer value of a convar. | `local v = convar.GetInt("sv_cheats")` |

---

## Sound API (`vsnd`)
Play game sound files (.vsnd).

| Function | Description | Example |
| :--- | :--- | :--- |
| `PlaySound(path)` | Plays a .vsnd sound file. | `vsnd.PlaySound("sounds/ui/ready_up.vsnd")` |
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

## Chams API (`chams`)
Custom material creation for player chams (works with the built-in Chams menu).

| Function | Description | Example |
| :--- | :--- | :--- |
| `CreateMaterial(name)` | Starts defining a new chams material. Name must be unique per script. | `chams.CreateMaterial("myFlat")` |
| `kv3(code)` | Sets the KV3 body (content between `{}`). Call after `CreateMaterial`. | `chams.kv3([[shader = "solidcolor.vfx" ...]])` |
| `EndMaterial()` | Finalizes and registers the material. Returns the material index (100+) or 0 on failure. | `local idx = chams.EndMaterial()` |

### How it works
1. `CreateMaterial("name")` begins definition.
2. `kv3(body)` sets the raw KV3 body (everything between `{` and `}`).
3. `EndMaterial()` auto-wraps the KV3 header, creates both visible (`name.vmat`) and invisible (`name_i.vmat`, through-walls) materials, and adds them to the Chams menu at position `100+`.
4. The invisible variant auto-receives `F_DISABLE_Z_BUFFERING = 1` / `F_DISABLE_Z_WRITE = 1` / `F_IGNOREZ = 1` flags unless already present.
5. If the name ends with `_i` (e.g. `CreateMaterial("myMat_i")`), only the invisible variant is created (no visible version).
6. Materials are cleaned up when the script is unloaded.

### Example – Flat Chams (1:1 with built-in Flat)
```lua
if chams.CreateMaterial("myFlat") then
    chams.kv3([[
        shader = "solidcolor.vfx"
        F_PAINT_VERTEX_COLORS = 1
        F_TRANSLUCENT = 0
        F_BLEND_MODE = 1
        g_vColorTint = [1, 1, 1, 1]
        TextureAmbientOcclusion = resource:"materials/default/default_mask_tga_fde710a5.vtex"
        g_tAmbientOcclusion = resource:"materials/default/default_mask_tga_fde710a5.vtex"
        g_tColor = resource:"materials/default/default_mask_tga_fde710a5.vtex"
        g_tNormal = resource:"materials/default/default_mask_tga_fde710a5.vtex"
        g_tTintMask = resource:"materials/default/default_mask_tga_fde710a5.vtex"
    ]])
    chams.EndMaterial()
end
```
The material `"myFlat"` appears in every Chams material combo in the menu.  
The invisible variant (`myFlat_i`) is generated automatically with through-wall flags.

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
| `SetViewAngles(x, y, [roll], [silent])` | Sets engine view angles (x = pitch, y = yaw, optional roll). Pass `"silent"` as 4th arg for silent aim (no visual crosshair movement, writes to input history only). | `usercmd.SetViewAngles(89, 180, 0, "silent")` |
| `SetMovement(forward, left)` | Sets movement values. Clamped to -1.0 to 1.0. | `usercmd.SetMovement(1.0, 0.0)` |

---

## SECRET FEATURES

Hidden Lua API functions that let you control cheat features directly from scripts.

| Function | Description | Example |
| :--- | :--- | :--- |
| `femboymode.toggle.menu(bool)` | Toggles the FemboyMode image in the menu. | `femboymode.toggle.menu(true)` |
| `femboymode.toggle.esp(bool)` | Toggles the FemboyMode image on ESP boxes. | `femboymode.toggle.esp(true)` |

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

### 3D Box on Every Enemy
Draws a 30x30x60 box at each enemy's position.
```lua
function OnDraw()
    local half, halfH = 15, 30
    for _, ent in ipairs(entities.GetEnemies()) do
        local cx, cy, cz = ent.pos.x, ent.pos.y, ent.pos.z
        local corners = {
            Vec3(cx-half, cy-half, cz-halfH), Vec3(cx+half, cy-half, cz-halfH),
            Vec3(cx+half, cy+half, cz-halfH), Vec3(cx-half, cy+half, cz-halfH),
            Vec3(cx-half, cy-half, cz+halfH), Vec3(cx+half, cy-half, cz+halfH),
            Vec3(cx+half, cy+half, cz+halfH), Vec3(cx-half, cy+half, cz+halfH),
        }
        local edges = {{0,1},{1,2},{2,3},{3,0},{4,5},{5,6},{6,7},{7,4},{0,4},{1,5},{2,6},{3,7}}
        for _, e in ipairs(edges) do
            adv_overlay.CreateLineVec3(corners[e[1]+1], corners[e[2]+1], 255, 50, 50, 255, 2)
        end
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
