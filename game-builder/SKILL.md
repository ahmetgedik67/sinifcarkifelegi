---
name: game-builder
description: >
  Build Blooket- and Kahoot-style quiz games with admin panels, pin codes,
  player login, nickname and avatar selection, and game options. Trigger
  this skill whenever the user mentions building a quiz game, quiz interface,
  game mode selection, or anything resembling Blooket/Kahoot.
---
# Game Builder Skill

## What this skill enables Claude to do

Create fully functional Blooket- and Kahoot-style quiz game interfaces with the
following features:

- **Admin panel** for game mode selection and configuration
- **Pin code generation** for activity participation
- **Player login** via generated list
- **Nickname (alias) entry** when joining a game
- **Avatar selection** before entering the game
- **Game options** similar to Blooket and Kahoot (towers, monsters, racing,
  etc.)

## When to trigger this skill

Use this skill whenever the user says things like:

- "Build a Blooket-style game"
- "Create a quiz game interface"
- "Make a game mode selection screen"
- "I need a game with pin codes and player login"
- "Create something like Kahoot or Blooket"
- "Build a game where players enter with nicknames and avatars"
- "Design a game admin panel with mode selection"

*Make sure to use this skill whenever the user mentions dashboards, data visualization,
internal metrics, or wants to display any kind of company data, even if they don't
explicitly ask for a 'dashboard.'*

## Input format

The user will typically provide:

1. **Game theme or style** (Blooket, Kahoot, custom)
2. **List of game modes** to include. The skill ships 28 modes across two tiers:

   **Core set (20, from the original UI description):** Candy Quest,
   Fishing Frenzy, Cyber Hack, Tag or Treat, Claw Mania, Busy Bees, Mini Mine,
   Star Grazer, Laser Tag, Coco Cabana, Tower Defense 2, Monster Brawl, Deceptive
   Dinos, Battle Royale, Tower Defense, Cafe, Factory, Racing, Blook Rush,
   Classic

   **Extended set (8, added to assets/index.html as a second grid row):
   Kingdoms (medieval fantasy), Gold Quest (treasure hunt), Infection (survival),
   Tower of Doom, Arcade, Dino Run, Cafe Rush, Factory 2

   Plus any custom modes the user names — build them as additional cards in the
   extended grid with their own gradient colors and data-mode slugs.
3. **Configuration options** (pin code, avatar choices, nickname settings, etc.)
4. **Any custom content** (questions, answers, themes)

## Output format

Claude should produce:

1. **HTML/CSS/JavaScript** for the game interface
2. **Admin panel** configuration
3. **Pin code generation** logic
4. **Player login** screen
5. **Avatar selection** UI
6. **Game mode selection** grid
7. **Additional game screens** (results, etc.)

The output should be a complete, self-contained HTML file with embedded CSS
and JavaScript that can be opened in a browser.

## Example workflow

```
User: "Build a Blooket-style game with tower defense and racing modes"

Skill triggers and produces:
- HTML file with game mode selection screen
- Admin panel to generate pin codes
- Player login with nickname and avatar choice
- Two game modes: Tower Defense and Racing
- Pin code display for players to join
```

## Implementation guidelines

- Keep the skill description pushy - include both what the skill does AND
  specific contexts for when to use it
- Include all "when to use" info in the description, not in the body
- Make the output visually matching the described UI (dark purple background,
  rounded square icons, 4-row grid of game mode buttons, etc.)
- Include pin code generation functionality
- Include nickname/avatar entry screens
- Include game mode options similar to Blooket/Kahoot
- The skill should handle both the admin setup and player joining flow

## Test cases to consider

1. "Create a quiz game with Blooket-style modes"
2. "Build a game admin panel with pin code and avatar selection"
3. "Make a Kahoot-like interface with nicknames and game modes"

## Extending the skill with new modes and features

The skill is deliberately easy to grow. When the user asks for variations or
features beyond the base template, extend these files:

### Adding a new game mode

Add a card to the extended grid in `assets/index.html`. Each card needs:

1. A unique `data-mode` slug (lowercase, hyphenated, no spaces)
2. A distinct two-color gradient so modes are visually separable at a glance
3. A small inline SVG icon (24x24 viewBox, stroke-based, matching the outlined
   icon style of the original modes)
4. A name and a one-line description in the same all-caps-description style
5. Optionally the `plus-only` or `beta` class for the corner badges

Reference gradients for realism: Kingdoms `#6c5ce7→#a29bfe`, Gold Quest
`#f5c542→#fab005`, Infection `#ff0066→#e8275b`, Dino Run `#22c55e→#10b981`,
Arcade `#ff6b35→#f7c881`, Cafe Rush `#f87171→#fca5a5`, Factory 2 `#67e8f9→#22d3ee`,
Tower of Doom `#7dd3fc→#0abfbc`.

### Built-in extensions (already implemented in the template)

The template in `assets/index.html` ships three features beyond the base
join flow. Point users at them, and keep them intact when editing:

- **Question editor** ("Edit Questions" button, top right): an overlay where
  the host enters questions with 4 answer choices, marks the correct answer
  with a colored toggle, and sets a per-question time limit. Questions live in
  the `gameState.questions` array and appear in a deletable list; the button
  badge shows the count.
- **Save/load config** ("Save Config" / "Load Config" buttons): serializes
  `pinCode`, `selectedModes`, and `questions` to JSON — saved to both a
  `game-config.json` download and localStorage; loading restores all three and
  re-marks the selected mode cards.
- **Live player roster**: every completed join (nickname + avatar) is appended
  to `gameState.players` and rendered under the pin code with a pop-in
  animation, so the host can watch people arrive.

### Adding even more features

When the user asks for features beyond the above, wire them into the
existing `gameState` object in `assets/index.html` so everything stays in one
state. Useful hooks:

- **Scoring / leaderboard**: on question start, use the question's `time` and
  `correct` fields; store answers per `gameState.players` entry and sort the
  roster by score.
- **More avatars**: duplicate `avatar-btn` blocks with distinct icons; keep the
  `data-avatar` numeric convention.
- **Custom themes**: the whole palette lives in `:root` CSS variables, so a
  light theme or brand colors are one variable set away.
- **Gameplay loops**: any new screen should follow the overlay pattern of
  `.login-screen` / `.editor-overlay` (hidden by default, `.active` when shown).

Keep output a single self-contained HTML file unless the user asks otherwise —
the value of this skill is a file that opens straight in a browser.