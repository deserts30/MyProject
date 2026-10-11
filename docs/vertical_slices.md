# Pet Quest: Vertical Slices (Development Issues)

**Based on:** Pet_Quest_PRD.md  
**Created:** 2026-10-11  
**Methodology:** Golden rule — build in small playtestable slices, never "whole game at once"

---

## 🎯 Slicing Principles

Each slice must be:
- ✅ **Independently playtestable** — can launch in Studio and verify it works
- ✅ **Builds on previous slices** — incremental, not parallel
- ✅ **Server-authoritative** — valuable state protected from exploits from Day 1
- ✅ **1-3 days of work** — small enough to complete quickly

---

## Phase 1: Prototype (Prove Core Combat is Fun)

### Slice 1.1: Basic Player Movement & Camera
**Priority:** P0 (Foundation)  
**Estimate:** 1 day  
**Dependencies:** None

**Scope:**
- Empty baseplate world
- Player spawns at 0,10,0
- WASD keyboard movement (PC testing)
- Mobile virtual joystick (left side of screen)
- Camera follows player (third-person, over-the-shoulder)

**Acceptance Criteria:**
- [ ] Player can move with WASD on PC
- [ ] Player can move with virtual joystick on mobile (test in Device Emulator)
- [ ] Camera is smooth, no clipping
- [ ] No server errors on join

**Technical:**
- Use Roblox Humanoid for movement
- ControlModule for mobile joystick
- Default camera type: Custom (follow player)

---

### Slice 1.2: Player Base Attacks (Light + Heavy + Dodge)
**Priority:** P0  
**Estimate:** 2 days  
**Dependencies:** 1.1

**Scope:**
- **Auto Light Attack**: Triggers automatically when near dummy target (3 studs)
  - 8 DMG, 0.8s cooldown, 3-hit combo (8→8→12)
  - Simple punch animation
- **Heavy Attack**: Button on screen (mobile) or Mouse2 (PC)
  - Hold 0.5s windup, release = 25 DMG + knockback
  - Can be interrupted
- **Dodge Roll**: Button on screen (mobile) or Shift (PC)
  - 0.8s roll animation, 0.3s i-frames, 4s cooldown

**Test Setup:**
- Spawn 3 training dummies with 100 HP each
- Visual HP bar above dummy
- Dummy takes damage, doesn't attack back

**Acceptance Criteria:**
- [ ] Auto light attack works near dummy (no button press)
- [ ] 3-hit combo plays correctly (8→8→12 DMG)
- [ ] Heavy attack charges and releases on button hold
- [ ] Dodge roll has i-frames (invincible during roll)
- [ ] All attacks work on mobile + PC
- [ ] Server validates damage (no client-side HP manipulation)

**Technical:**
- RemoteEvent for damage (server validates)
- Cooldowns tracked server-side
- Animations: default Roblox punch + custom dodge roll
- Mobile UI: 2 buttons (Heavy, Dodge) bottom of screen

---

### Slice 1.3: Single Starter Pet (Fire) — Stats Only
**Priority:** P0  
**Estimate:** 1 day  
**Dependencies:** 1.2

**Scope:**
- Flameling pet model (simple fire-themed mesh, <500 tris)
- Pet has stats: HP:80, ATK:20, DEF:8, Level:1
- Pet stored in DataStore (ProfileService)
- Pet appears in simple inventory UI (shows name + stats)
- NO fusion yet, NO abilities yet — just data structure

**Acceptance Criteria:**
- [ ] Player spawns with Flameling in inventory
- [ ] Inventory UI shows pet name, stats, level
- [ ] Pet data persists on rejoin (DataStore)
- [ ] Pet model visible in UI preview (ViewportFrame)

**Technical:**
```lua
Profile.Data = {
    Pets = {
        {
            PetId = "Flameling_Standard_001",
            Name = "Flameling",
            Element = "Fire",
            Rarity = "Standard",
            Level = 1,
            Stats = {HP = 80, ATK = 20, DEF = 8}
        }
    }
}
```

---

### Slice 1.4: Fusion System (Visual Only)
**Priority:** P0  
**Estimate:** 2 days  
**Dependencies:** 1.3

**Scope:**
- Button in inventory: "Equip Pet"
- Tap = Fusion animation (2-3 seconds):
  - Pet model flies toward player
  - Flash of light
  - Pet disappears
  - Player gains fire aura (ParticleEmitter + orange glow)
- Player character shows fusion state (visual effects only)
- De-fuse button to remove effects

**Acceptance Criteria:**
- [ ] Fusion animation plays smoothly
- [ ] Player has fire aura + glow when fused
- [ ] Fusion state persists on rejoin
- [ ] Mobile + PC both work
- [ ] No abilities yet — purely visual test

**Technical:**
- Fusion state: Player.Attributes.FusedPetId
- VFX attached to player HumanoidRootPart
- Tween pet model toward player during animation

---

### Slice 1.5: First Ability (Flame Slash)
**Priority:** P0  
**Estimate:** 2 days  
**Dependencies:** 1.4

**Scope:**
- Add 1 ability to Flameling: **Flame Slash**
  - 15 DMG, 1s cooldown, melee range (10 studs)
  - Button appears on screen when fused
  - Fires projectile (fast fireball) toward cursor/nearest enemy
- Ability damages training dummy
- Cooldown visual (button dims, shows timer)

**Acceptance Criteria:**
- [ ] Ability button appears after fusion
- [ ] Flame Slash fires correctly on tap
- [ ] Deals 15 DMG to dummy (server validates)
- [ ] 1s cooldown prevents spam
- [ ] VFX: fire slash particle trail
- [ ] Works on mobile + PC

**Technical:**
- RemoteEvent: Client → Server (FireAbility)
- Server validates cooldown + range
- Server applies damage to dummy
- Client plays VFX locally (responsiveness)

---

### Slice 1.6: Skill Tree UI (1 Ability Unlockable)
**Priority:** P0  
**Estimate:** 2 days  
**Dependencies:** 1.5

**Scope:**
- Skill Tree UI opens from button
- Shows Fire element tree (simplified):
  - Tier 1: Flame Slash (unlocked by default)
  - Tier 1: Fireball (costs 3 SP, locked)
- Player starts at Level 1 (0 SP)
- Manual level-up button (testing only) → +1 SP
- Unlock Fireball with 3 SP → adds to ability bar

**Acceptance Criteria:**
- [ ] Skill tree UI displays correctly
- [ ] Flame Slash shown as unlocked
- [ ] Fireball shown as locked (greyed out)
- [ ] Manual level-up grants 1 SP
- [ ] Spending 3 SP unlocks Fireball
- [ ] Unlocked abilities appear on ability bar
- [ ] DataStore saves skill tree state

**Technical:**
```lua
SkillTree = {
    ["FlameSlash"] = {Unlocked = true, Level = 1},
    ["Fireball"] = {Unlocked = false, Level = 0}
}
SkillPoints = 0
```

---

### Slice 1.7: Second Ability (Fireball)
**Priority:** P0  
**Estimate:** 1 day  
**Dependencies:** 1.6

**Scope:**
- Fireball ability: 30 DMG, 3s cooldown, ranged (50 studs)
- Fires projectile in straight line
- Explodes on hit (VFX + damage)
- Added to ability bar when unlocked

**Acceptance Criteria:**
- [ ] Fireball unlocks after spending 3 SP
- [ ] Fires correctly on button press
- [ ] Deals 30 DMG to dummy
- [ ] 3s cooldown enforced
- [ ] Explosion VFX on impact

---

### Slice 1.8: First Enemy AI (Fire Slime)
**Priority:** P0  
**Estimate:** 2 days  
**Dependencies:** 1.7

**Scope:**
- Fire Slime enemy spawns in world (replaces dummy)
- Stats: HP:30, ATK:5, DEF:3
- AI behavior:
  - Pathfind toward player
  - Melee attack when in range (3 studs)
  - 10 DMG every 2 seconds
- Player can kill Slime with abilities
- Slime drops 10 Gold on defeat

**Acceptance Criteria:**
- [ ] Fire Slime spawns automatically
- [ ] Slime pathfinds to player
- [ ] Slime attacks player (damages HP)
- [ ] Player can kill Slime with abilities
- [ ] Slime drops 10 Gold (appears on screen)
- [ ] Gold saved to DataStore

**Technical:**
- PathfindingService for AI
- RemoteEvent for Slime damage to player (server-authoritative)
- Gold stored in Profile.Data.Inventory.Gold

---

### Slice 1.9: Player HP & Death
**Priority:** P0  
**Estimate:** 1 day  
**Dependencies:** 1.8

**Scope:**
- Player has HP bar (based on fused pet stats)
- Flameling fusion = 80 HP
- Taking damage reduces HP
- HP reaches 0 = death:
  - De-fuse automatically
  - Respawn at spawn point
  - HP restored to full

**Acceptance Criteria:**
- [ ] HP bar shows current/max HP
- [ ] Taking damage from Slime reduces HP
- [ ] Death at 0 HP triggers respawn
- [ ] Respawn restores HP to full
- [ ] Death de-fuses pet (remove aura)

---

### Slice 1.10: Zone 1 — Meadow Plains (Basic World)
**Priority:** P1  
**Estimate:** 2 days  
**Dependencies:** 1.9

**Scope:**
- Replace baseplate with grassy terrain (Meadow Plains theme)
- Spawn point (safe zone, no enemies)
- Enemy spawn zones (3 locations, 2 Slimes each)
- Invisible walls (boundaries)
- Simple lighting (daytime, bright)

**Acceptance Criteria:**
- [ ] Terrain looks like grassy meadow
- [ ] 6 Fire Slimes spawn in zone
- [ ] Slimes respawn 30s after death
- [ ] Player cannot fall off map (boundaries)
- [ ] Zone feels open but contained

---

## Phase 2: Vertical Slice (Complete Loop)

### Slice 2.1: Starting Town (Hub)
**Priority:** P1  
**Estimate:** 2 days  
**Dependencies:** 1.10

**Scope:**
- Town area with buildings (low-poly models)
- NPCs:
  - Professor NPC (tutorial, starter selection)
  - Egg Shop NPC (buy Basic Eggs)
- Spawn point in town center
- Portal to Zone 1 (teleports player to Meadow Plains)

**Acceptance Criteria:**
- [ ] Town has 3-4 buildings (visual only)
- [ ] NPCs have dialogue UI (tap to talk)
- [ ] Portal to Zone 1 works (bidirectional)
- [ ] Town is safe zone (no enemies)

---

### Slice 2.2: Egg System (Basic Egg)
**Priority:** P1  
**Estimate:** 2 days  
**Dependencies:** 2.1

**Scope:**
- Egg Shop NPC sells Basic Egg (100 Gold)
- Buy egg → added to inventory
- Inventory shows eggs (icon + count)
- Tap egg in inventory → "Hatch" button
- Hatch animation (5 seconds):
  - Egg shakes, cracks, pet appears
  - Rarity reveal (Standard 90% / Common 9% / Uncommon 1%)
- Name input screen (3-15 characters, Roblox filter)
- Pet added to collection

**Acceptance Criteria:**
- [ ] Can buy Basic Egg with 100 Gold
- [ ] Egg appears in inventory
- [ ] Hatch animation plays
- [ ] Rarity rolled server-side (no exploits)
- [ ] Name filter works (blocks profanity)
- [ ] New pet saved to DataStore

**Technical:**
- Server-side rarity roll:
```lua
local roll = math.random(1, 100)
if roll <= 90 then rarity = "Standard"
elseif roll <= 99 then rarity = "Common"
else rarity = "Uncommon" end
```

---

### Slice 2.3: Pet Collection UI
**Priority:** P1  
**Estimate:** 1 day  
**Dependencies:** 2.2

**Scope:**
- "Pets" menu button (open pet collection)
- Grid view of all owned pets
- Each pet shows:
  - Icon
  - Name
  - Rarity (color-coded)
  - Level
  - Element
- Tap pet → detail view:
  - Stats (HP/ATK/DEF)
  - "Equip" button (fusion)
  - "Release" button (delete pet, confirm popup)

**Acceptance Criteria:**
- [ ] Collection UI shows all pets
- [ ] Can equip different pet (de-fuse current, fuse new)
- [ ] Release button deletes pet (with confirmation)
- [ ] Sorting options (by level, rarity, element)

---

### Slice 2.4: Water & Grass Starters
**Priority:** P1  
**Estimate:** 3 days  
**Dependencies:** 2.3

**Scope:**
- Add 2 more starter pets:
  - **Aqualing** (Water): HP:110, ATK:12, DEF:15, Tank archetype
  - **Leafling** (Grass): HP:95, ATK:15, DEF:12, Balanced archetype
- Each has 2 starting abilities:
  - Aqualing: Water Slash (15 DMG), Bubble Beam (30 DMG)
  - Leafling: Leaf Slash (15 DMG), Vine Whip (30 DMG)
- Fusion VFX unique per element:
  - Water: Blue aura + water droplets
  - Grass: Green shimmer + floating leaves

**Acceptance Criteria:**
- [ ] All 3 starters available in collection
- [ ] Each element has unique fusion effects
- [ ] Abilities work correctly for each type
- [ ] Stats reflect archetype (Tank, Balanced, Attacker)

---

### Slice 2.5: XP & Leveling System
**Priority:** P1  
**Estimate:** 2 days  
**Dependencies:** 2.4

**Scope:**
- Killing enemies grants XP (Fire Slime = 10 XP)
- XP bar shown on screen (current/max)
- Level up at XP thresholds:
  - Lvl 1→2: 50 XP
  - Lvl 2→3: 100 XP
  - Lvl 3→4: 200 XP (exponential curve)
- Level up grants:
  - +1 Skill Point (SP)
  - +5 HP, +2 ATK, +2 DEF
- Level up animation (flash, +SP popup)

**Acceptance Criteria:**
- [ ] Killing Slime grants 10 XP (shown as popup)
- [ ] XP bar fills correctly
- [ ] Level up triggers at correct XP
- [ ] Stats increase on level up
- [ ] SP granted for skill tree
- [ ] Level persists on rejoin (DataStore)

**Technical:**
- XP formula: `NextLevelXP = 50 * (CurrentLevel ^ 1.5)`
- Stats formula: `NewStat = BaseStat + (Level * Multiplier)`

---

### Slice 2.6: Loadout System (4 Slots)
**Priority:** P1  
**Estimate:** 2 days  
**Dependencies:** 2.5

**Scope:**
- Skill tree unlocks 10 abilities (Standard rarity pet)
- Player can only equip 4 at a time (active slots)
- Loadout UI (drag-and-drop):
  - Left: All unlocked abilities (pool)
  - Right: 4 active slots
  - Drag ability from pool → slot to equip
- Save loadout (DataStore)
- Ability bar updates when loadout changes

**Acceptance Criteria:**
- [ ] Can unlock multiple abilities via skill tree
- [ ] Only 4 abilities shown on ability bar
- [ ] Drag-and-drop works (mobile + PC)
- [ ] Loadout saves on change
- [ ] Abilities not in slots are inactive

---

### Slice 2.7: Zone Boss (Giant Fire Wolf)
**Priority:** P1  
**Estimate:** 3 days  
**Dependencies:** 2.6

**Scope:**
- Giant Fire Wolf boss spawns in special arena (Zone 1 end)
- Stats: HP:500, ATK:20, DEF:10
- Boss AI:
  - Phase 1 (100%-50% HP): Melee attacks + Fireball every 5s
  - Phase 2 (50%-0% HP): Adds Fire Spin (AoE, 40 DMG, 10s CD)
- Boss arena has barrier (prevents leaving mid-fight)
- Victory rewards:
  - +100 Gold
  - +100 XP
  - Silver Egg (guaranteed drop)
  - Unlock Zone 2 portal

**Acceptance Criteria:**
- [ ] Boss spawns in arena
- [ ] Boss has 2 phases with different attacks
- [ ] Player can defeat boss with abilities
- [ ] Death in boss fight respawns outside arena
- [ ] Victory grants rewards (Gold, XP, Silver Egg)
- [ ] Zone 2 portal unlocks after victory (one-time)

**Technical:**
- Boss HP tracked server-side
- Phase change at 250 HP (50% threshold)
- Barrier uses invisible ForceField parts

---

### Slice 2.8: PvP Quick Play (Basic 1v1)
**Priority:** P1  
**Estimate:** 3 days  
**Dependencies:** 2.7

**Scope:**
- PvP Arena building in town
- Tap → "Quick Play" button
- Matchmaking:
  - Simple queue (no MMR yet)
  - Pairs 2 players (or bot if no opponent)
- Teleport both to PvP arena (separate instance)
- 1v1 combat (same rules as PvE)
- Win condition: First to 0 HP
- Victory screen:
  - Winner: +10 Gold
  - Loser: 0 rewards
  - Return to town button

**Acceptance Criteria:**
- [ ] Quick Play queue works
- [ ] 2 players matched together
- [ ] PvP arena loads (clean arena, no clutter)
- [ ] Combat works (damage, abilities, HP)
- [ ] Winner/loser determined correctly
- [ ] Rewards granted to winner
- [ ] Both players return to town after match

**Technical:**
- Use TeleportService to separate instance
- Reserved server for PvP arena (2 players max)
- Server validates all damage (anti-cheat)
- 5-minute timeout if match takes too long

---

### Slice 2.9: Daily Login Bonus
**Priority:** P2  
**Estimate:** 1 day  
**Dependencies:** 2.8

**Scope:**
- On join, check last login timestamp
- If >24 hours since last login:
  - Show "Daily Bonus!" popup
  - Grant 50 Gold
  - Increment streak (Day 1, 2, 3...)
  - Day 7 streak = Legendary Egg bonus
- Popup shows streak progress (Day X/7)

**Acceptance Criteria:**
- [ ] Daily bonus triggers on first join of the day
- [ ] Does NOT trigger multiple times per day
- [ ] Streak tracked correctly (Day 1→7)
- [ ] Day 7 grants Legendary Egg
- [ ] Streak resets if >48 hours gap

**Technical:**
```lua
Profile.Data.DailyLogin = {
    LastLogin = os.time(),
    Streak = 1
}
```

---

### Slice 2.10: Mobile UX Polish
**Priority:** P2  
**Estimate:** 2 days  
**Dependencies:** 2.9

**Scope:**
- Test on Device Emulator (iPhone 12, Android)
- UI adjustments:
  - Button sizes (60x60 px minimum)
  - Safe area insets (notch/home bar)
  - Text scaling (16px minimum)
- Performance:
  - 30 FPS minimum on low-end devices
  - Reduce particles on mobile (lower quality)
- Touch controls:
  - Joystick deadzone tuning
  - Button tap responsiveness

**Acceptance Criteria:**
- [ ] All UI readable on mobile
- [ ] Buttons easy to tap (no mis-taps)
- [ ] Game runs at 30+ FPS on mobile emulator
- [ ] No UI clipping (safe areas respected)

---

## Phase 3: Content Expansion

### Slice 3.1: Zone 2 — Volcanic Peaks
**Priority:** P2  
**Estimate:** 3 days

**Scope:**
- New zone (volcano theme, lava terrain)
- New enemies: Fire Golems (HP:50, ATK:10)
- Boss: Inferno Dragon (HP:800, 3 abilities)
- Unlock condition: Level 6 + Zone 1 boss defeated

---

### Slice 3.2: Ranked Arena (MMR System)
**Priority:** P2  
**Estimate:** 4 days

**Scope:**
- MMR system (ELO-based, hidden)
- Rank tiers: Bronze → Silver → Gold → Platinum → Legend
- Seasonal resets (every 4 weeks)
- Rank UI (shows current rank + progress bar)
- Ranked rewards (Gold, cosmetics at Legend)

---

### Slice 3.3: Rarity Tiers 6-10 (Mythic → Chromatic)
**Priority:** P2  
**Estimate:** 3 days

**Scope:**
- Add 5 higher rarities (Mythic, Divine, Celestial, Chromatic)
- Each has more abilities (24-30)
- Unique visual effects per rarity
- Premium eggs (Diamond, Mystic, Supreme)

---

### Slice 3.4: Ascension System
**Priority:** P2  
**Estimate:** 4 days

**Scope:**
- Max level pets can Ascend
- Two paths: Duplicate Sacrifice or Prestige Points
- 3 Ascension tiers
- Visual changes (glow, particles, ★ badges)
- Stat boosts (+10-20%)

---

### Slice 3.5: Trading System
**Priority:** P3  
**Estimate:** 3 days

**Scope:**
- Trade UI (offer/accept flow)
- Server-validates trades (anti-dupe)
- Trade history log
- Cooldown (24 hours between trades, anti-scam)

---

## Summary: Phase 1 Checklist

**MVP Goal:** Prove fusion combat is fun in 1-hour playtest

- [ ] 1.1: Basic movement + camera
- [ ] 1.2: Player base attacks (Light, Heavy, Dodge)
- [ ] 1.3: Single starter pet (Fire, stats only)
- [ ] 1.4: Fusion system (visual effects)
- [ ] 1.5: First ability (Flame Slash)
- [ ] 1.6: Skill tree UI (1 unlockable)
- [ ] 1.7: Second ability (Fireball)
- [ ] 1.8: First enemy AI (Fire Slime)
- [ ] 1.9: Player HP & death
- [ ] 1.10: Zone 1 world (Meadow Plains)

**Phase 1 Complete = Combat prototype ready for playtest!** 🎉

---

**Next Steps:**
1. Create GitHub issues from slices above
2. Assign to team members
3. Start building Slice 1.1 in Studio
4. Playtest after each slice completion

