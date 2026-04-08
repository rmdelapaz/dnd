# D&D 5e Course Site — Continue Notes

## Project Location
- **WSL path:** `~/projects/dnd/` (aka `\\wsl$\Ubuntu\home\practicalace\projects\dnd\`)
- **Filesystem access roots:** `\\wsl$\Ubuntu\home\practicalace\projects`, `D:\mydata`, `G:\My Drive`
- **CSS:** `styles/main.css` (global dark theme), `styles/main copy.css` (backup of original CSS)

## Page Order (from index.html)
1. `dnd_intro.html` – Introduction
2. `dnd_char.html` – Character Creation
3. `dnd_character_examples.html` – Character Examples
4. `dnd_100.html` – 100 Characters
5. `dnd_names.html` – Character Names
6. `dnd_groups.html` – Character Group Dynamics
7. `dnd_core.html` – Core Mechanics
8. `dnd_dice_roller.html` – Dice Roller
9. `dnd_combat.html` – Combat
10. `dnd_magic.html` – Magic
11. `dnd_rp_and_char_dev.html` – Roleplay & Character Dev
12. `dnd_dm.html` – Dungeon Mastering

Other files: `index.html` (hub page), `dnd_100inc.html` (HTML fragment, unclear purpose, not modified), `favicon.png`, `favicon.ico`, `readme.md`

---

## What's Been Done

### 1. Global CSS Modernization — `styles/main.css` ✅
Complete rewrite of the stylesheet:
- Dark theme: `#0f1117` bg, `#1a1d27` surfaces, `#2e3347` borders, `#3b82f6` blue accents, `#c9cdd8` text
- Mobile-first with `clamp()` typography, responsive grids, `overflow-x` scroll on tables/code/mermaid
- Breakpoints at 768px and 480px
- CSS custom properties throughout
- Print styles included
- All existing HTML selectors/class names preserved (no HTML changes needed for base styling)

### 2. d20 Illustration — `dnd_intro.html` ✅
- Replaced original flat green canvas hexagon with a canvas-drawn d20: flat-top hexagon in dark red (`#b91c1c`), inner point-up triangle (`#dc2626`), 9 symmetric edge lines, bold white "20", shadow, caption

### 3. Prev/Next Navigation — All 12 pages ✅
- Consistent nav bar added before `</body>` on each page
- Format: `← Prev | Home | Next →` (first/last pages omit one link)
- Inline styles matching the dark theme

### 4. `dnd_core.html` Fixes ✅
- `drawDie20()`: replaced diamond with simple flat-top red hexagon + number (no triangle/lines)
- `probabilityChart` canvas: axes/labels/legend changed from `#000` to `#c9cdd8`
- `damageCanvas`: dice text → `#fff`, weapon names and title → `#c9cdd8`

### 5. `dnd_dice_roller.html` — Full Rewrite ✅
- **Self-contained** — does NOT link to `main.css` (avoids specificity conflicts); all styles inline
- Dark/blue theme matching the rest of the site
- Centered layout, `max-width: 960px`
- **Quick Actions section** with 6 preset buttons (Attack Roll, Saving Throw, Skill Check, Initiative, Ability Scores, Percentile)
- **Inline Advantage/Disadvantage buttons** flank each d20 quick action: `[DISADV] [Roll] [ADV]` button groups
  - DISADV button (red, left) and ADV button (green, right) directly trigger rolls with that mode
  - Center button rolls normally
  - Ability Scores and Percentile remain standalone (no adv/disadv)
- **Advantage/Disadvantage toggle** lives inside Custom Roll section
  - Disabled (grayed out, `pointer-events:none`) by default
  - Enables automatically when d20 count > 0 in custom roll
  - Helper label tells user when it's available
- **Advantage/Disadvantage results show both dice** — kept die in full color, dropped die dimmed with strikethrough (same visual treatment as ability score dropped die); breakdown text shows "kept / dropped" format
- **Custom Roll section** with +/− buttons and number inputs for d4, d6, d8, d10, d12, d20, d100
  - `changeDie()` function handles increment/decrement
  - `onDiceChange()` monitors d20 count to toggle advantage row
- **Modifier** input field
- **Roll Dice / Clear All** buttons
- **Results display as a modal popup** (fixed overlay, centered, dark backdrop)
  - Close via: × button, clicking backdrop, or pressing Escape
  - Shows total, breakdown, individual colored dice, critical/fumble animations
  - Ability score rolls: 4d6 drop lowest with visual strikethrough on dropped die
- `rollDice()` returns early if no dice selected (prevents empty modal)

### 6. `dnd_names.html` — Dark Theme Conversion ✅
- Removed entire inline `<style>` block (was purple gradient light theme)
- All styling now handled by `main.css` — name-grid, name-card, tips-section, example-box, analogy-box, canvas-container rules added/updated in main.css
- Added h3 overrides inside `.analogy-box`, `.example-box`, `.tips-section` (removes blue heading bg, uses plain text-bright color)
- `.name-card` hover effect and `.race-suggestion` blue color added to main.css
- Canvas name wheel: text → `#fff`, segment strokes → `#1a1d27`, pointer → `#e8ebf2`
- Mermaid diagrams: switched to `theme: 'dark'`

---

## What Still Needs Work

### Pages with potential dark-theme readability issues
These pages have inline `<canvas>` JS that likely draws text in `#000` (black on dark background), same issue that was fixed in `dnd_core.html`:
- `dnd_char.html`
- `dnd_character_examples.html`
- `dnd_combat.html`
- `dnd_magic.html`
- `dnd_rp_and_char_dev.html`
- `dnd_dm.html`
- `dnd_groups.html` — also has a commented-out `<style>` block, needs review

### `dnd_100inc.html`
- Appears to be an HTML fragment (partial content from `dnd_100.html`)
- Purpose unclear, not modified

### General
- No page has been checked for mobile layout beyond what `main.css` provides
- Content is all 5th Edition — keep it that way, do not update to newer editions
- The goal is modernization and mobile-friendliness WITHOUT changing content

---

## Design Tokens (for reference)
```
--color-bg:      #0f1117
--color-surface: #1a1d27
--color-border:  #2e3347
--color-accent:  #3b82f6
--color-text:    #c9cdd8
--color-heading: #e8ebf2
--color-muted:   #7c8298
--color-link:    #60a5fa
```

## Key Decisions
- `dnd_dice_roller.html` intentionally does NOT link `main.css` to avoid CSS specificity conflicts — all styles are self-contained inline
- Canvas illustrations use canvas API (not SVG) after SVG attempts produced distorted results for the d20
- `drawDie20()` in `dnd_core.html` is a simplified flat-top hexagon + number (user rejected triangle/lines version)
