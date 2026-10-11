# Pet Quest: Battle & Collect — Design Document

## Core Concept
Action-RPG где питомцы **вселяются** в игрока, даря способности стихий. Фокус на **competitive PvP** с глубокой кастомизацией builds через skill tree систему.

---

## 🎮 Core Mechanics

### Fusion System
- Питомец вселяется в игрока (permanent fusion в бою)
- Игрок получает визуальные эффекты стихии (аура, частицы, trail)
- Доступны способности питомца из skill tree (4 активных слота)
- В PvP: один питомец на весь матч (mastery-focused)

### Combat System

**Базовые атаки игрока (универсальные):**
- **Auto Light Attack**: 8 DMG, авто-атака рядом с врагом каждые 0.8 сек
- **Heavy Attack**: 25 DMG, 1.5 сек windup, 2 сек cooldown, можно прервать
- **Dodge Roll**: 0.3 сек i-frames, 4 сек cooldown

**Combo Chain:**
```
Auto Light (8) → Auto Light (8) → Auto Light (12) = 28 DMG
Heavy Attack (windup) → 25 DMG + knockback
Можно плести: Melee → Ability → Melee
```

**Способности питомца:**
- 4 слота из skill tree (кастомизация)
- 3 loadout'а для быстрой смены вне боя
- Cooldown-based система

---

## 🥚 Rarity & Egg System

### 10 уровней редкости

| Редкость | Abilities в древе | Базовые статы (Lvl 1) | Drop Rate |
|----------|-------------------|------------------------|-----------|
| Standard | 10 | HP:100, ATK:10, DEF:10 | 45% |
| Common | 12 | HP:110, ATK:12, DEF:12 | 25% |
| Uncommon | 14 | HP:120, ATK:14, DEF:14 | 15% |
| Rare | 16 | HP:135, ATK:17, DEF:16 | 8% |
| Epic | 18 | HP:150, ATK:20, DEF:18 | 4% |
| Legendary | 22 | HP:170, ATK:24, DEF:22 | 1.5% |
| Mythic | 24 | HP:190, ATK:28, DEF:26 | 0.7% |
| Divine | 26 | HP:215, ATK:33, DEF:30 | 0.2% |
| Celestial | 28 | HP:240, ATK:38, DEF:35 | 0.08% |
| **Chromatic** | **30** | **HP:280, ATK:45, DEF:42** | **0.02%** |

### Типы яиц

| Яйцо | Стоимость | Rarity Pool |
|------|-----------|-------------|
| Basic Egg | 100 Gold | Standard-Uncommon |
| Silver Egg | 500 Gold | Common-Rare |
| Golden Egg | 2000 Gold | Uncommon-Epic |
| Diamond Egg | 5000 Gold | Rare-Legendary |
| Mystic Egg | 15000 Gold | Epic-Mythic |
| Supreme Egg | 50000 Gold | Legendary-Divine |
| Celestial Egg | 150000 Gold | Mythic-Celestial |
| Chromatic Egg | Event-only | Guaranteed Chromatic |

### Hatching механика
1. Тап на яйцо в инвентаре → "Place in incubator"
2. Яйцо помещается в Incubator (город)
3. Тап = hatching animation (5-10 сек)
4. Reveal редкости по цвету свечения
5. Игрок даёт имя (3-15 символов, Roblox filter)

---

## ⚔️ PvP System

### Структура
- **Quick Play** (Lvl 3+): Casual 1v1, 10 gold за победу
- **Beginner Arena** (Lvl 3-10): Unranked practice, 25 gold за победу
- **Ranked Arena** (Lvl 10+): Seasonal ladder (Bronze → Legend)

### Format
- **1v1, single round**
- **Один питомец в Fusion** весь матч
- **No items в Ranked**
- **First to 0 HP** проигрывает
- **Matchmaking по MMR**

### Rewards
- Gold (50-100 за матч)
- Rank Points (+15-30 за победу)
- Seasonal cosmetics (Legend tier)

---

## 🌳 Skill Tree System

**Прогрессия:**
- Level Up = +1 Skill Point (SP)
- SP тратятся на unlock/upgrade способностей
- Standard: 10 abilities | Chromatic: 30 abilities

**Loadouts:**
- 4 активных слота способностей
- 3 сохранённых loadout'а
- Можно редактировать в любое время вне боя
- Switching между loadout'ами instant

**Пример Fire Tree:**
```
Tier 1: Flame Slash, Fireball, Flame Dash
Tier 2: Inferno, Fire Shield, Burning DoT
Tier 3: Meteor Strike, Phoenix Form
```

---

## 🎯 First Session Flow

1. **Spawn в Starting Town** (видно масштаб игры)
2. **Тап на Professor NPC**
3. **Диалог (3-4 строки):**
   - "Welcome to Pet Quest!"
   - "Trainers catch and battle with Pets!"
   - "Wild Pets roam... will you catch them all?"
   - "Choose your partner:"
4. **Выбор стартера** (Fire/Water/Grass с статами + архетип)
5. **Tutorial бой против Professor** (невозможно проиграть)
6. **Обучение Fusion + базовым атакам**
7. **Первая victory** → XP, Gold, переход в открытый мир

---

## 🤖 AI Враги

**Поведение:**
- **Simple melee** (бегут к игроку, кусают фиксированный DMG)
- **1 ability** по cooldown (зависит от типа врага)
- **Предсказуемые паттерны** для PvE

**Примеры:**
- **Fire Slime**: Melee каждые 2 сек (10 DMG) + Fireball на дистанции
- **Wolf**: Melee combo + Flame Dash (gap closer)
- **Boss Dragon**: 3 abilities по паттерну, держит дистанцию

---

## 🎨 Визуальная идентичность

### Fusion эффекты по стихиям
- 🔥 **Fire**: Огненная аура, дым, огненные руки
- 💧 **Water**: Водяные крылья, капли вокруг, синее свечение
- 🌿 **Grass**: Листья кружатся, лианы на руках, зелёный shimmer

### Rarity визуалы
- Standard-Uncommon: базовые цвета
- Rare-Epic: aura effects
- Legendary+: particles, glow, sparkles
- Chromatic: shifting colors + trail

---

## 📱 Mobile UI (6 кнопок)

```
Левый джойстик: Movement
Правая сторона (4 кнопки): Abilities
Низ (2 кнопки): Heavy Attack + Dodge
Auto Light Attack: автоматически рядом с врагом
```

---

## 🎯 Core Loop

**PvE Loop:**
Battle Wild Pets → Earn Gold/XP → Buy Eggs → Hatch → Level Up → Unlock Abilities → Build Loadout → Repeat

**PvP Loop:**
Build Loadout → Ranked Match → Win/Lose → Earn Rank Points → Climb Ladder → Unlock Cosmetics → Refine Build → Repeat

---

## 📊 Progression Milestones

- **Lvl 1-3**: Tutorial, first catches, Quick Play unlock
- **Lvl 3-10**: Beginner Arena, skill tree basics, first egg purchases
- **Lvl 10+**: Ranked Arena unlock, serious builds, seasonal climb
- **Lvl 15+**: Advanced builds, rare eggs farming, competitive meta

---

## 🎁 Retention Hooks

- **Day 1**: Daily Login Bonus (50 gold + random pet)
- **Day 3**: PvP Arena unlock
- **Day 7**: Legendary Chest (guaranteed Legendary egg)
- **Weekly**: Double Gold events
- **Monthly**: Seasonal Championship
- **Live Ops**: New pets, limited eggs, ranked seasons

---

## 💰 Monetization (без pay-to-win)

### Робуксовые покупки

| Покупка | Цена | Описание |
|---------|------|----------|
| **VIP Pass** | 399 Robux ($4.99) | 2x gold + ранний доступ к новым pets |
| **Instant Hatch Scroll** | 99 Robux ($0.99) | Пропуск анимации hatching |
| **Golden Egg Bundle** | 299 Robux | 1× Golden Egg (экономия ~6 часов фарма) |
| **Chromatic Chance Boost** | 499 Robux | +30% шанс Chromatic из Celestial Egg (7 дней) |
| **Extra Loadout Slot** | 199 Robux | 4-й loadout вместо 3 |
| **Cosmetic Pet Skins** | 149-499 Robux | Крылья, trail эффекты, шляпы |
| **Player Outfits** | 299-699 Robux | Уникальные костюмы игрока |
| **Private Server** | 99 Robux/мес | PvP training с друзьями |
| **Name Change Token** | 49 Robux | Переименовать питомца |

### ⚠️ НЕТ pay-to-win
- ❌ НЕ продаём Prestige Points
- ❌ НЕ продаём гарантированных Chromatic pets
- ❌ НЕ продаём stat boosts
- ❌ НЕ продаём exclusive abilities

---

## 🔑 Unique Selling Points

1. **Fusion Combat** — уникальная механика вселения
2. **Deep Customization** — 10-30 abilities, 4 слота, 3 loadouts
3. **Skill-based PvP** — один питомец, mastery важнее collection
4. **10 Rarities** — от Standard до Chromatic
5. **Combo System** — melee + abilities chains
6. **Mobile-first** — 6 кнопок, авто light attack

---

**Status:** Concept approved, ready for PRD
**Next Step:** `roblox-to-prd` → `roblox-to-issues` → Start building first vertical slice
