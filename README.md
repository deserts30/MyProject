# RobloxProject

Проект для разработки игры в Roblox Studio с использованием Claude Code и Roblox Skills.

## 🚀 Быстрый старт

### 1. Клонирование и настройка

```bash
git clone https://github.com/deserts30/MyProject.git
cd RobloxProject
```

### 2. Установка инструментов

Для работы с Roblox Skills нужны:

**Aftman + Roblox инструменты:**
```bash
# В PowerShell
aftman add rojo-rbx/rojo@7.4.4
aftman add Kampfkarren/selene@0.27.1
aftman add JohnnyMorganz/StyLua@0.20.0
```

**Roblox Studio MCP сервер** (для взаимодействия Claude с Studio):
```bash
claude mcp add robloxstudio -- npx -y robloxstudio-mcp@latest
```

**GitHub CLI** (для работы с issues):
```bash
gh auth login
```

### 3. Создание labels в GitHub

```bash
gh label create "needs-triage" --color "d73a4a"
gh label create "needs-info" --color "0075ca"
gh label create "ready-to-build" --color "0e8a16"
gh label create "needs-human" --color "fbca04"
gh label create "wontfix" --color "ffffff"
gh label create "bug" --color "d73a4a"
gh label create "enhancement" --color "a2eeef"
```

## 📁 Структура проекта

```
RobloxProject/
├── CLAUDE.md              # Конфигурация Roblox Skills для Claude
├── ROADMAP.md             # Дорожная карта с чекбоксами задач
├── docs/
│   └── agents/
│       ├── tracker.md     # Настройка GitHub Issues
│       ├── triage-labels.md  # Метки для триажа
│       └── design.md      # Правила для UI дизайна
├── design/                # UI дизайн-брифы (создается по мере надобности)
└── roadmap/               # Локальные копии задач (опционально)
```

**⚠️ Важно:** Код Luau и объекты Studio **НЕ хранятся в Git**. Они живут в облаке Roblox и редактируются через Studio MCP. Git используется только для планирования (ROADMAP, PRD, дизайн-брифы).

## 🎮 Начало работы

### Генерация идей игры

```bash
# В Claude Code
roblox-game-ideas
```

Скилл поможет:
- Сгенерировать 3 концепции игры с оценкой по retention, monetization, mobile-fit
- Стресс-тестировать идею (grilling mode)
- Определить core loop

### Создание PRD

```bash
roblox-to-prd
```

Превращает концепцию в короткий buildable PRD.

### Разбивка на задачи (vertical slices)

```bash
roblox-to-issues
```

Разбивает PRD на маленькие, независимо тестируемые вертикальные слайсы.

### Триаж задач

```bash
roblox-triage
```

Управляет состоянием задач: needs-triage → needs-info → ready-to-build → в работу.

## 🔄 Жизненный цикл разработки

1. **Research** → `roblox-game-ideas`, `roblox-genre-patterns`
2. **Concept & core loop** → `roblox-game-ideas`, `roblox-retention-monetization`
3. **Spec & slice** → `roblox-to-prd` → `roblox-to-issues` → `roblox-triage`
4. **Build ONE slice** → `roblox-studio-workflow`, `roblox-luau-architecture`
5. **QA** → `roblox-playtest-qa`, `roblox-mobile-playtest`
6. **Повторить** цикл 3-5 для следующего слайса

**Золотое правило:** Не строить всю игру сразу! Делать по одному вертикальному слайсу, тестировать в Studio, получать approval, и только потом следующий.

## 🤝 Совместная работа

### Пригласить друга

```bash
# Дайте другу ссылку на репозиторий
https://github.com/deserts30/MyProject

# Или добавьте его как коллаборатора
gh repo add-collaborator deserts30/MyProject USERNAME
```

### Рабочий процесс

1. Друг клонирует репозиторий
2. Оба работаете с **одними и теми же GitHub Issues**
3. Код пишется в Roblox Studio (в облаке)
4. Планирование идет через Git (ROADMAP.md + GitHub Issues)

## 📚 Полный список скиллов

См. `CLAUDE.md` — там 40+ скиллов для всех этапов разработки:
- Архитектура и код
- Геймплей системы
- UX/UI
- Экономика и монетизация
- Безопасность
- Публикация и Live Ops
- Производительность и QA

## 🛠️ Полезные команды

```bash
# Создать новую задачу
gh issue create --title "Feature: Player inventory" --body "Description..."

# Посмотреть задачу
gh issue view 1

# Добавить метку
gh issue edit 1 --add-label "ready-to-build"

# Список открытых задач
gh issue list --state open

# Закрыть задачу
gh issue close 1 --comment "Done and tested"
```

## 📖 Документация

- [Roblox Skills Documentation](https://github.com/AshExplained/roblox-skills)
- [Roblox Creator Docs](https://create.roblox.com/docs)
- [Claude Code](https://claude.com/claude-code)

## 📝 Лицензия

MIT
