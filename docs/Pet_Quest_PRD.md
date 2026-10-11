# Pet Quest: Battle & Collect — Product Requirements Document

**Version:** 1.0  
**Date:** 2026-10-11  
**Status:** Ready for implementation  
**Target Platform:** Roblox (Mobile-first)

---

## Executive Summary

Pet Quest is a competitive Action-RPG where pets **fuse** with the player, granting elemental abilities. The game focuses on deep build customization (10-30 abilities per pet via skill trees) and skill-based 1v1 PvP combat. Players collect pets through an egg gacha system with 10 rarity tiers, then master one pet rather than grinding many.

**Core Innovation:** Permanent pet fusion combat system — no Pokemon-style switching, pure mastery gameplay.

**Target Audience:** 10-16 years old, mobile-first, competitive players who value skill over collection grind.

**Monetization:** Fair F2P with cosmetics, convenience items, no pay-to-win.

---

## 🎯 Core Pillars

1. **Fusion Combat** — Pet vселяется в игрока, даёт способности стихии
2. **Deep Customization** — 10-30 abilities в skill tree, 4 active slots, 3 loadouts
3. **Skill-based PvP** — 1v1 ranked с фокусом на mastery одного питомца
4. **Fair Progression** — 10 rarity tiers, но Standard pets viable с правильным build
5. **Mobile-first UX** — 6 кнопок, auto light attack, крупный UI

---

## 🎮 Core Mechanics

### 1. Fusion System

**How it works:**
- Player selects **one pet** before combat
- Pet performs **permanent fusion** animation (2-3 sec)
- Player gains visual transformation (aura, particles, elemental effects)
- Pet's abilities from skill tree become available (4 active slots)
- Fusion lasts entire combat session

**Visual Effects by Element:**
- 🔥 Fire: Огненная аура, дымовой trail, огненные руки
- 💧 Water: Водяные крылья, капли воды вокруг, синее свечение  
- 🌿 Grass: Листья кружатся вокруг, лианы на руках, зелёный shimmer

**Technical Notes:**
- Fusion state stored per-player (attribute or value)
- VFX attached to player character model
- Ability UI updates dynamically on fusion
- De-fusion only on combat end or manual exit

---

### 2. Combat System

#### Player Base Attacks (Universal, no pet dependency)

| Attack | Damage | Speed | Cooldown | Notes |
|--------|--------|-------|----------|-------|
| **Auto Light Attack** | 8 DMG | 0.8s | None | Auto-triggers near enemy |
| **Heavy Attack** | 25 DMG | 1.5s (0.5s windup) | 2s | Interruptible, knockback |
| **Dodge Roll** | 0 DMG | 0.8s | 4s | 0.3s i-frames |

**Light Attack Combo Chain:**
- Tap → Tap → Tap = 8 + 8 + 12 DMG (28 total in ~1.5s)
- Final hit in 3-hit combo deals bonus damage

**Heavy Attack Mechanics:**
- 0.5s windup before damage (player vulnerable)
- Can be interrupted by enemy attacks
- Knockback 5 studs on hit
- Risk/reward skill expression

**Dodge Roll Mechanics:**
- 0.3s invincibility frames (can dodge ANY attack including ultimates)
- 4s cooldown prevents spam
- Can be triggered mid-combo to cancel into dodge

#### Pet Abilities (from Skill Tree)

- **4 active slots** assigned from skill tree
- **3 loadouts** for quick switching (out of combat)
- Cooldown-based (no mana/energy system)
- Each ability has unique:
  - Damage value
  - Range (melee/ranged/AoE)
  - Cooldown (1-15 seconds)
  - Visual effects

**Example Fire Pet Abilities:**

| Tier | Ability | Unlock Cost | Damage | Cooldown | Effect |
|------|---------|-------------|--------|----------|--------|
| 1 | Flame Slash | Start | 15 DMG | 1s | Quick melee |
| 1 | Fireball | 3 SP | 30 DMG | 3s | Ranged projectile |
| 2 | Inferno | 5 SP | 80 DMG | 15s | AoE ultimate |
| 3 | Meteor Strike | 8 SP | 150 DMG | 20s | Massive AoE |

#### Combo System

Players can chain: **Melee → Ability → Melee → Dodge → Ability**

Example optimal Fire combo:
```
Flame Dash (gap close) → Heavy Attack (stun) → 
Fireball (guaranteed hit) → Auto Light x3 → 
Inferno (finish) = ~180 DMG in 5 seconds
```

**Technical Implementation:**
- Animation canceling allowed between abilities
- Dodge can cancel any animation (escape tool)
- Hitstun on Heavy Attack = 0.5s (guarantees follow-up)
- Server validates damage and cooldowns

---

### 3. Rarity & Progression System

#### 10 Rarity Tiers

| Rarity | Abilities in Tree | Base Stats (Lvl 1) | Drop Rate | Visual |
|--------|-------------------|-------------------|-----------|--------|
| Standard | 10 | HP:100, ATK:10, DEF:10 | 45% | Grey |
| Common | 12 | HP:110, ATK:12, DEF:12 | 25% | White |
| Uncommon | 14 | HP:120, ATK:14, DEF:14 | 15% | Green |
| Rare | 16 | HP:135, ATK:17, DEF:16 | 8% | Blue |
| Epic | 18 | HP:150, ATK:20, DEF:18 | 4% | Purple |
| Legendary | 22 | HP:170, ATK:24, DEF:22 | 1.5% | Orange + aura |
| Mythic | 24 | HP:190, ATK:28, DEF:26 | 0.7% | Red + particles |
| Divine | 26 | HP:215, ATK:33, DEF:30 | 0.2% | Gold + glow |
| Celestial | 28 | HP:240, ATK:38, DEF:35 | 0.08% | Rainbow + sparkles |
| **Chromatic** | **30** | **HP:280, ATK:45, DEF:42** | **0.02%** | Shifting colors + trail |

**Design Philosophy:**
- Standard pets viable with good builds (10 abilities enough for skilled players)
- Rarity = more build options, NOT pure stat advantage
- Chromatic pets best, but not 10x stronger (skill matters more)

#### Skill Tree Progression

**How it works:**
1. Pet gains XP from battles
2. Level up = +1 Skill Point (SP) + stat increase
3. SP spent to unlock/upgrade abilities
4. Max level depends on rarity:
   - Standard-Uncommon: Level 25
   - Rare-Epic: Level 30
   - Legendary-Mythic: Level 35
   - Divine-Chromatic: Level 40

**Skill Tree Structure (per element):**
```
Tier 1 (Lvl 1-5): 3 basic abilities
├─ Starter ability (free)
├─ Ranged ability (3 SP)
└─ Mobility ability (3 SP)

Tier 2 (Lvl 6-10): 4 advanced abilities
├─ Ultimate (5 SP, requires Tier 1 maxed)
├─ Defensive ability (5 SP)
└─ 2 specialized abilities (4 SP each)

Tier 3 (Lvl 11+): 3-5 master abilities
└─ Endgame abilities (8 SP each, high requirements)
```

**Loadout System:**
- 4 active ability slots (assigned from unlocked abilities)
- 3 saved loadouts (can name them: "Burst", "Tank", "Hit & Run")
- Switch loadouts anytime outside combat (instant)
- Loadout editing via drag-and-drop UI

---

### 4. Egg & Hatching System

#### Egg Types

| Egg Type | Cost | Rarity Pool | Source |
|----------|------|-------------|--------|
| Basic Egg | 100 Gold | Standard (90%) / Common (9%) / Uncommon (1%) | Shop |
| Silver Egg | 500 Gold | Common (50%) / Uncommon (35%) / Rare (13%) / Epic (2%) | Boss reward |
| Golden Egg | 2000 Gold | Uncommon (40%) / Rare (35%) / Epic (20%) / Legendary (5%) | 7-day login |
| Diamond Egg | 5000 Gold | Rare (50%) / Epic (35%) / Legendary (13%) / Mythic (2%) | Ranked Gold+ |
| Mystic Egg | 15000 Gold | Epic (60%) / Legendary (30%) / Mythic (9%) / Divine (1%) | Monthly event |
| Supreme Egg | 50000 Gold | Legendary (65%) / Mythic (25%) / Divine (9%) / Celestial (1%) | Legend rank |
| Celestial Egg | 150000 Gold | Mythic (70%) / Divine (25%) / Celestial (4.5%) / Chromatic (0.5%) | Championship |
| Chromatic Egg | Event-only | Chromatic (100%) | Rare seasonal events |

#### Hatching Flow

1. Player buys/receives egg → added to inventory
2. Tap egg → "Place in Incubator?" prompt
3. Egg placed in Incubator building (in town)
4. Tap incubator → Hatching animation starts:
   - Egg shakes (2-3 sec)
   - Cracks appear
   - Light flashes (color = rarity reveal)
   - Pet bursts out
5. "You got [Fire Type] - [Rare Rarity]!" popup
6. Name input screen (3-15 characters)
   - Roblox TextService filter applied
   - Default: "[Type][Rarity] #[ID]" if skipped
7. Pet added to collection

**Technical:**
- Server-side rarity roll (prevent exploits)
- Animation can be skipped with Robux item
- Naming uses FilterStringAsync
- Duplicate pets stored for Ascension system

---

### 5. PvP Arena System

#### Three PvP Modes

**1. Quick Play (Lvl 3+)**
- Casual 1v1 matchmaking
- No rank, no stakes
- Rewards: 10 gold per win
- Purpose: Practice builds

**2. Beginner Arena (Lvl 3-10)**
- Unranked matchmaking
- Only players Lvl 1-10
- Rewards: 25 gold + XP per win
- Purpose: Learn PvP without getting crushed

**3. Ranked Arena (Lvl 10+)**
- Seasonal ladder system
- Ranks: Bronze → Silver → Gold → Platinum → Legend
- MMR-based matchmaking
- Rewards:
  - 50-100 gold per win
  - +15-30 Rank Points
  - Seasonal cosmetics at Legend tier
  - Diamond Egg at Gold rank
  - Supreme Egg at Platinum+

#### Combat Rules

**Format:**
- 1v1, single round
- One pet in Fusion entire match
- First to 0 HP loses
- No items allowed in Ranked
- Items allowed in Quick Play (potions)

**Match Flow:**
1. Queue → Matchmaking (30s max wait)
2. Loading screen (shows both players + pets)
3. Arena spawn (opposite sides, 50 studs apart)
4. 3-second countdown
5. Fight! (no time limit)
6. Victory/Defeat screen + rewards
7. Option to rematch or return to lobby

**Technical:**
- Server-authoritative damage validation
- Client prediction for responsiveness
- Remotes for abilities (cooldown checked server-side)
- Anti-cheat: speed/damage/cooldown validation
- Replay system (optional, for content creators)

---

## 🗺️ World Structure

### Hub: Starting Town

Central hub with NPCs and buildings:
- **Professor NPC** (tutorial, starter selection)
- **Egg Shop** (buy Basic/Silver/Golden eggs)
- **Incubator Building** (hatch eggs)
- **PvP Arena Entrance** (matchmaking portal)
- **Skill Respec Trainer** (reset skill points for gold)
- **Cosmetic Shop** (hats, outfits)

### 5 Combat Zones (Level-gated)

| Zone | Level Req | Wild Pets | Boss | Unlock Reward |
|------|-----------|-----------|------|---------------|
| **1. Meadow Plains** | 1-5 | Fire Slimes, Grass Rabbits, Water Frogs | Giant Fire Wolf | Silver Egg |
| **2. Volcanic Peaks** | 6-10 | Fire Golems, Lava Lizards | Inferno Dragon | Beginner Arena access |
| **3. Crystal Caves** | 11-15 | Crystal Bats, Rock Elementals | Crystal Titan | Ascension system unlock |
| **4. Frozen Tundra** | 16-20 | Ice Wolves, Snow Spirits | Frost Giant | Golden Egg shop access |
| **5. Sky Islands** | 21-25 | Wind Eagles, Thunder Serpents | Sky Emperor | Diamond Egg + Prestige |

**Zone Unlock Condition:**
- Reach required level **AND** defeat previous zone's boss
- Boss can be attempted multiple times (no penalty for losing)
- Zones are instanced (avoid griefing)

---

## 💰 Economy & Monetization

### Gold Sources (F2P Currency)

| Source | Gold Amount | Frequency |
|--------|-------------|-----------|
| Wild pet battle | 5-10 | Unlimited |
| Zone boss defeat | 100 | Once per boss |
| Daily Login | 50 | Daily |
| Quick Play win | 10 | Unlimited |
| Ranked win | 50-100 | Unlimited |
| Daily Quest completion | 100 | 3 quests/day |
| Weekly Quest | 500 | 1/week |
| First-time pet catch | 20 | Per unique |

**Expected Grind Times (F2P):**
- Basic Egg (100g): 20 minutes
- Silver Egg (500g): 1.5 hours
- Golden Egg (2000g): 4-5 hours with Daily Quests
- Diamond Egg (5000g): 10-12 hours

### Gold Sinks

| Sink | Cost | Purpose |
|------|------|---------|
| Basic Egg | 100g | Main progression |
| Skill Respec | 100g | Build experimentation |
| Healing Potions (10x) | 50g | PvE convenience |
| Pet Rename | 50g | Personalization |
| Cosmetic Hat | 200-500g | Cosmetics |
| Player Outfit | 500-1000g | Cosmetics |

### Robux Monetization (Premium Currency)

| Product | Price | Description | Pay-to-Win? |
|---------|-------|-------------|-------------|
| **VIP Pass** | 399 Robux ($4.99) | 2x gold + early access to new pets | ❌ No (convenience) |
| **Instant Hatch Scroll** | 99 Robux ($0.99) | Skip egg animation | ❌ No (convenience) |
| **Golden Egg Bundle** | 299 Robux | 1x Golden Egg | ❌ No (time skip) |
| **Chromatic Chance Boost** | 499 Robux | +30% Chromatic chance for 7 days | ❌ No (still RNG) |
| **Extra Loadout Slot** | 199 Robux | 4th loadout slot | ❌ No (convenience) |
| **Pet Cosmetics** | 149-499 Robux | Wings, trails, hats | ❌ No (cosmetic) |
| **Player Outfits** | 299-699 Robux | Costumes | ❌ No (cosmetic) |
| **Private Server** | 99 Robux/month | PvP practice with friends | ❌ No (QoL) |
| **Name Change Token** | 49 Robux | Rename pet (vs 50 Gold) | ❌ No (convenience) |

**Anti-Pay-to-Win Guarantee:**
- ❌ NO Prestige Points for Robux
- ❌ NO guaranteed Chromatic pets
- ❌ NO stat boosts
- ❌ NO exclusive abilities
- ❌ NO XP boosters

**Revenue Strategy:**
- Whale targeting: Cosmetics + convenience
- Dolphin targeting: VIP Pass + occasional eggs
- F2P respect: Everything earnable, Robux = time skip only

---

## 🔄 Endgame: Ascension System

### Concept

When a pet reaches max level, it can **Ascend** to gain:
- +10 levels to cap
- Stat boost (+10-20% depending on rarity)
- New visual effects (glow, particles, trail)
- +1 new ability tier in skill tree

**Max Ascension Tiers:** 3 (Ascension I → II → III)

### Ascension Requirements

**Two paths (player choice):**

**Path 1: Duplicate Sacrifice**
- Max level pet
- 3x duplicate pets (same type & rarity, any level)
- 2000 Gold
→ Fast path for lucky/whale players

**Path 2: Prestige Points**
- Max level pet
- 100 Prestige Points (earned from PvP/PvE)
- 3000 Gold
→ F2P friendly, skill-based

**Prestige Point Sources:**
- Ranked win: +5 PP
- Boss defeat: +10 PP
- Daily Quest: +3 PP
- Weekly Quest: +15 PP

### Ascension Stat Boosts

| Rarity | Ascension I | Ascension II | Ascension III | Total |
|--------|-------------|--------------|---------------|-------|
| Standard | +10% | +8% | +7% | +25% |
| Rare | +12% | +10% | +8% | +30% |
| Legendary | +15% | +12% | +10% | +37% |
| Chromatic | +20% | +15% | +12% | +47% |

**Visual Changes:**
- Ascension I: Glowing eyes + small aura + ★ badge
- Ascension II: Larger model (+10%) + particles + ★★ badge
- Ascension III: Dramatic new appearance + trail + ★★★ badge + "Transcendent" title

**Max Level Progression:**
- Standard: 25 → 35 → 45 → 55
- Chromatic: 40 → 50 → 60 → 70

**Endgame Cap:**
- Ascension III = completion (no infinite scaling)
- Encourages trying different pets
- Keeps PvP balanced

---

## 📱 Mobile-First UX

### UI Layout (6 buttons)

```
┌──────────────────────────────┐
│ HP: ████████░░ 80/100        │ ← Top
│ Energy: ███████░░░ 70/100    │
├──────────────────────────────┤
│                              │
│       [GAME FIELD]           │
│                              │
│   👤 Player    🐉 Enemy      │
│                              │
├──────────────────────────────┤
│ [🕹️]          [Ability 1]   │ ← Right
│  ↑            [Ability 2]   │   (4 abilities)
│ ←→ Joystick   [Ability 3]   │
│  ↓            [Ultimate ]   │
│                              │
│ [💥 Heavy] [🌀 Dodge]       │ ← Bottom
└──────────────────────────────┘
```

**Controls:**
- **Left Joystick**: Movement (always visible)
- **4 Ability Buttons**: Glow when ready, dim on cooldown
- **Heavy Attack Button**: Hold 0.5s to charge
- **Dodge Button**: Double-tap to roll
- **Auto Light Attack**: Automatic near enemy (no button)

**Design Principles:**
- Thumb zones: All buttons within comfortable reach
- Large tap targets (60x60 px minimum)
- No obscured gameplay (UI on edges)
- Safe area insets respected
- Vertical orientation (one-handed playable)

### Accessibility

- Text scaling: 16px minimum
- High contrast mode toggle
- Colorblind friendly indicators (icons + colors)
- Auto-pause on app switch (mobile)
- Tutorial skip for returning players

---

## 🎯 First Session Flow (Tutorial)

**Goal:** Player understands fusion, basic combat, and gacha loop in <5 minutes.

### Flow Chart

1. **Spawn in Starting Town** (3 sec)
   - See other players, NPCs, buildings
   - Arrow points to Professor

2. **Talk to Professor** (20 sec)
   - Tap Professor NPC
   - Dialogue (4 lines, 1 tap each):
     - "Welcome to Pet Quest!"
     - "Trainers catch and battle with Pets!"
     - "Wild Pets roam... will you catch them all?"
     - "Choose your partner:"
   - Skip button top-right (for repeat players)

3. **Choose Starter** (15 sec)
   - 3 options: Fire/Water/Grass
   - Each shows: Icon + Name + Type + Stats (HP/ATK/DEF) + Archetype
   - Examples:
     - 🔥 Flameling (Fire) | HP:80 ATK:20 DEF:8 | ⚔️ Attacker
     - 💧 Aqualing (Water) | HP:110 ATK:12 DEF:15 | 🛡️ Tank
     - 🌿 Leafling (Grass) | HP:95 ATK:15 DEF:12 | ⚖️ Balanced
   - Tap to preview → Confirm button

4. **Tutorial Battle vs Professor** (60 sec)
   - Professor: "Let's test your skills!"
   - Pet auto-fuses with player (2s animation)
   - Tutorial prompts:
     - "Tap [ABILITY] to attack!" → Player uses starter ability
     - "Try [HEAVY ATTACK]!" → Player charges attack
     - "Press [DODGE] to roll!" → Player dodges Professor's attack
     - "Use [ULTIMATE]!" → Player fires ultimate
   - Professor HP scripted to 0 after tutorial completes
   - Impossible to lose (Professor does 1 DMG max)

5. **Victory + Rewards** (10 sec)
   - "You won! +50 XP, +100 Gold"
   - Pet levels up to 2 (teach progression)
   - "You unlocked Quick Play!"

6. **First Wild Encounter** (30 sec)
   - Professor: "Now catch your first wild pet!"
   - Fire Slime spawns nearby
   - Player defeats it (easy enemy)
   - "Congratulations! Wild pet caught!"
   - Free Basic Egg added to inventory

7. **First Hatch** (20 sec)
   - "Tap your egg to hatch it!"
   - Hatching animation
   - Random Common/Uncommon revealed
   - Name input
   - "You can battle with this pet too!"

8. **Tutorial Complete** (5 sec)
   - "Explore zones, collect pets, battle in PvP!"
   - Free roam unlocked

**Total Time:** ~3 minutes (skippable after first playthrough)

---

## 🤖 AI Enemy Behavior

### Enemy Types

**Low-Tier (Slimes, Frogs):**
- **Melee AI**: Run to player, basic attack every 2s (10 DMG)
- **1 Ability**: Use ranged attack if player >15 studs away
- **No dodge**, **no combos**

**Mid-Tier (Wolves, Golems):**
- **Melee AI**: Run to player, 3-hit combo (8/8/12 DMG)
- **1 Ability**: Gap closer (dash) or defensive (shield)
- **Simple pattern**: Melee → Ability → Melee

**Boss-Tier (Dragons, Titans):**
- **Ranged AI**: Keep distance, circle player
- **3 Abilities**: Rotation pattern (predictable but requires dodging)
- **Phase changes**: At 50% HP, unlock stronger ability
- **Telegraphed attacks**: 1s windup before big damage (teach dodge timing)

### Technical Implementation

- **Pathfinding**: Roblox PathfindingService
- **Attack triggers**: Raycasts + hitboxes (server validates)
- **Ability cooldowns**: Server-tracked
- **Difficulty scaling**: Stats increase per zone (+20% HP/DMG per zone)

**Example Boss Pattern (Inferno Dragon):**
```
Phase 1 (100%-50% HP):
- Fireball (ranged, 30 DMG, 3s CD)
- Tail Swipe (melee, 40 DMG, 5s CD)
- Fly + Dive Bomb (AoE, 60 DMG, 10s CD)

Phase 2 (50%-0% HP):
- All above + Meteor Storm (screen-wide AoE, 80 DMG, 15s CD)
- Enrage: +50% attack speed
```

---

## 📊 Retention & Engagement Hooks

### Daily Systems

- **Daily Login Bonus**: 50 gold + random pet (Day 7 = Legendary Egg)
- **Daily Quests** (3/day, reset 12:00 UTC):
  - Win 5 battles → 100 gold
  - Catch 3 wild pets → 50 gold
  - Win 1 PvP match → 75 gold
- **Daily Spin** (free gacha spin, low-tier eggs only)

### Weekly Systems

- **Weekly Quest**: Defeat 2 Zone Bosses → 500 gold
- **Weekly Challenge**: Reach Gold rank in PvP → 500 gold + exclusive cosmetic
- **Double Gold Weekend** (Saturday-Sunday)

### Monthly/Seasonal

- **Ranked Season** (4 weeks):
  - Season rewards: Cosmetics, eggs, titles
  - Rank resets each season (soft reset, keep 50% MMR)
- **Limited-Time Events**:
  - Halloween: Event-only Dark element pets
  - Christmas: Snow element pets + Chromatic Egg drop boost
- **Championship Tournament** (end of season):
  - Top 100 players compete bracket-style
  - Prize: Guaranteed Chromatic Egg

### Progression Milestones

- Lvl 3: Quick Play unlock
- Lvl 5: Zone 2 unlock
- Lvl 10: Ranked Arena unlock
- Lvl 15: Ascension system unlock
- Lvl 20: Diamond Egg shop unlock
- Lvl 25: Prestige system unlock

---

## 🔧 Technical Requirements

### Server Architecture

- **Client-Server Model**: All valuable state server-authoritative
- **DataStore**: ProfileService for data persistence
- **Anti-Exploit**:
  - Server validates damage, cooldowns, movement speed
  - Combat remotes limited by cooldown (prevent spam)
  - Egg rolls server-side (prevent manipulation)
- **Matchmaking**: Custom MMR system (ELO-based)
- **Instancing**: Each combat zone = separate Place instance

### Performance Targets

- **Mobile FPS**: 30 FPS minimum (target 60 FPS)
- **Load Time**: <10 seconds from join to gameplay
- **Memory**: <500 MB RAM on low-end devices
- **Instance Count**: <2000 per zone (use LOD, streaming)

### Asset Requirements

- **Models**: Low-poly pets (<500 triangles each)
- **VFX**: Particle emitters (limit 5 per player)
- **Audio**: Compressed (64 kbps max)
- **UI**: Scalable vector icons (no raster where possible)

### Data Schema (ProfileService)

```lua
Profile.Data = {
    Pets = {
        {
            PetId = "Fire_Rare_001",
            Name = "Blaze",
            Rarity = "Rare",
            Element = "Fire",
            Level = 15,
            XP = 450,
            Stats = {HP = 135, ATK = 17, DEF = 16},
            SkillTree = {["FlameSlash"] = 3, ["Fireball"] = 2},
            AscensionTier = 0
        }
    },
    Inventory = {
        Gold = 2500,
        Eggs = {"Basic", "Silver"},
        Items = {["HealthPotion"] = 10}
    },
    Progress = {
        Level = 15,
        CurrentZone = 3,
        BossesDefeated = {1, 2},
        TutorialComplete = true
    },
    PVP = {
        Rank = "Gold",
        MMR = 1450,
        Wins = 42,
        Losses = 38,
        PrestigePoints = 65
    },
    Cosmetics = {
        Equipped = {PetSkin = "FireWings", Outfit = "Knight"},
        Owned = {"FireWings", "Knight", "SummerHat"}
    },
    Settings = {
        MusicVolume = 0.8,
        SFXVolume = 1.0
    }
}
```

---

## ✅ Acceptance Criteria

### MVP Features (Must-Have for Launch)

- [ ] Tutorial flow (starter selection → first battle → first catch)
- [ ] 3 starter pets (Fire/Water/Grass) with 10 abilities each
- [ ] Fusion combat system (visual effects + ability system)
- [ ] Player base attacks (Auto Light, Heavy, Dodge)
- [ ] 2 zones (Meadow Plains + Volcanic Peaks) with wild pets + bosses
- [ ] Egg system (Basic, Silver, Golden) with hatching
- [ ] 5 rarity tiers (Standard → Legendary)
- [ ] Skill tree UI (unlock/upgrade abilities)
- [ ] Loadout system (4 slots, 3 presets)
- [ ] PvP Quick Play (casual 1v1)
- [ ] DataStore persistence (pets, progress, gold)
- [ ] Mobile-optimized UI (6-button layout)
- [ ] Gold economy (earning + spending)
- [ ] Daily Login Bonus

### Post-Launch (Phase 2)

- [ ] Ranked Arena (seasonal ladder)
- [ ] Remaining 3 zones (Crystal Caves, Frozen Tundra, Sky Islands)
- [ ] 5 additional rarities (Mythic → Chromatic)
- [ ] Ascension system (3 tiers)
- [ ] Daily/Weekly Quests
- [ ] Premium eggs (Diamond, Mystic, Supreme)
- [ ] Cosmetic shop (pet skins, outfits)
- [ ] Trading system (pet trading between players)
- [ ] Guild/Clan system (optional)

### Polish (Phase 3)

- [ ] Animation polish (ability VFX, fusion effects)
- [ ] Audio (combat SFX, music, voice lines)
- [ ] Localization (Spanish, Portuguese)
- [ ] Replay system (PvP match recordings)
- [ ] Leaderboards (global, friends)
- [ ] Admin tools (moderation, analytics dashboard)

---

## 📅 Development Roadmap

### Phase 1: Prototype (4-6 weeks)

**Goal:** Prove fusion combat is fun, mobile controls feel good

- Week 1-2: Fusion system + player attacks + 1 starter pet
- Week 3: Basic enemy AI + Zone 1
- Week 4: Egg system + hatching + pet collection
- Week 5: Skill tree UI + loadout system
- Week 6: Tutorial + polish

**Playtest Goal:** Players understand fusion, enjoy combat, want more pets

---

### Phase 2: Vertical Slice (6-8 weeks)

**Goal:** Complete 2 zones, 3 starters, PvP Quick Play, DataStore persistence

- Week 1-2: Zone 2 (Volcanic Peaks) + boss fight
- Week 3: Water & Grass starter pets (10 abilities each)
- Week 4: PvP Quick Play matchmaking
- Week 5-6: DataStore (ProfileService integration)
- Week 7: Economy balancing + daily login
- Week 8: Mobile UX optimization

**Playtest Goal:** 1-hour session loop works, retention Day 1 > 40%

---

### Phase 3: Soft Launch (8-10 weeks)

**Goal:** Full content, Ranked Arena, monetization, analytics

- Week 1-2: Zones 3-5 + all bosses
- Week 3-4: Ranked Arena + seasonal system
- Week 5: Ascension system + endgame
- Week 6: Rarity tiers 6-10 (Mythic → Chromatic)
- Week 7: Monetization (Robux products, VIP Pass)
- Week 8: Analytics instrumentation
- Week 9: Soft launch (limited audience)
- Week 10: Balance pass + bug fixes

**Playtest Goal:** Day 7 retention > 20%, ARPU > $0.50

---

### Phase 4: Launch (4 weeks)

**Goal:** Public launch, marketing, community

- Week 1: Thumbnail, icon, store page optimization
- Week 2: Trailer, influencer outreach
- Week 3: Launch + sponsored ads
- Week 4: First live event (Double Gold Weekend)

**Success Metrics:**
- 100k visits in first week
- Day 1 retention > 45%
- Day 7 retention > 25%
- Conversion rate > 5%

---

## 🎯 Success Metrics (KPIs)

### Retention

- **Day 1:** 40%+ (tutorial completion)
- **Day 7:** 20%+ (endgame hook)
- **Day 30:** 10%+ (seasonal content)

### Monetization

- **Conversion Rate:** 5%+ (% of players who spend)
- **ARPU:** $0.50+ (average revenue per user)
- **ARPPU:** $10+ (average revenue per paying user)

### Engagement

- **Session Length:** 30+ minutes average
- **Sessions/Week:** 5+ (daily habit)
- **PvP Participation:** 40%+ of active players

### Virality

- **K-Factor:** 0.3+ (viral coefficient)
- **Social Share Rate:** 5%+ of sessions

---

## 🚨 Risks & Mitigations

| Risk | Impact | Mitigation |
|------|--------|------------|
| Combat too complex for kids | High | Extensive tutorial, auto light attack |
| Mobile performance poor | High | Low-poly assets, LOD, streaming enabled |
| Gacha too grindy | Medium | Generous F2P rates, Prestige Point path |
| PvP imbalanced | Medium | Weekly balance patches, community feedback |
| Pay-to-win perception | High | No stat boosts for Robux, transparent odds |
| Content drought post-launch | Medium | Live ops roadmap, monthly events |

---

## 📝 Appendix

### Glossary

- **Fusion**: Pet vселяется в игрока, даёт способности
- **Skill Tree**: Древо прокачки способностей питомца
- **Loadout**: Набор из 4 способностей в активных слотах
- **Ascension**: Эндгейм система улучшения питомца
- **Prestige Points**: Валюта для Ascension (PvP/PvE rewards)
- **MMR**: Matchmaking Rating (скрытый рейтинг для PvP)
- **Vertical Slice**: Маленькая завершённая фича (playtestable)

### References

- [Roblox Pet Simulator X](https://www.roblox.com/games/6284583030) — gacha economy
- [Pokemon Showdown](https://pokemonshowdown.com) — competitive build meta
- [Brawlhalla](https://www.brawlhalla.com) — skill-based combat
- [Roblox Mobile Best Practices](https://create.roblox.com/docs/production/publishing/mobile)

---

**Document Status:** ✅ Approved for implementation  
**Next Step:** Break into vertical slices → Create GitHub issues → Begin Prototype Phase 1

