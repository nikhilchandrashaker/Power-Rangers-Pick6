# ⚡ Power Rangers Pick 6

A browser draft game. Spin a wheel, get a random Ranger, **draft** them or **re-spin**, and build a lineup of six. Your team is scored out of 100, and you can battle a CPU lineup or play the daily challenge.

## Quick start

1. Open `power_rangers_pick6.html` in any modern browser (double-click it). It is a single self-contained file: the data, styles and code are all inside, and it works offline.
2. Press **SPIN**. The wheel is made of Ranger colors (Red, Blue, Yellow, Pink, Green, Black, White, Gold, Silver and Special). Whichever color it lands on, you see **every Ranger of that color** in a scrollable list (sortable by Overall, name, year or any key stat). Anyone whose character is already in your lineup is greyed out. Click one to select them, then **click any empty silhouette to place them** where you want (click the card again to unselect; the final empty slot fills automatically). Or use a **RE-SPIN**. Repeat until all 6 silhouettes in the lineup row are filled in. Each one takes on the Ranger's color and shows their name, season and Overall.
3. Read your score, then optionally **⚔️ BATTLE THE CPU**.

## Game modes

| Mode | What it does |
|---|---|
| **Free play** | Unlimited games with a fresh random wheel each time. |
| **📅 Daily Challenge** | Everyone gets the same wheel on the same date (resets 00:00 UTC). One scored attempt per day, with a streak counter and a shareable result. |
| **⚔️ Battle the CPU** | After drafting, fight a CPU lineup. The CPU spins the same color wheel. Easy scouts 4 random Rangers of the color, Normal scouts 12, and Hard sees the whole color, weighs chemistry and role coverage, and uses 2 re-spins. |
| **📖 Browse all Rangers** | Searchable, filterable, sortable pool of all 154 Rangers with full stat cards. |
| **🔄 Redo draft** | Restarts the current draft from scratch. In the Daily Challenge it replays the same wheel; it is disabled once you've finished today's scored attempt. |

## Rules

- **6 slots, no color requirements.** Six Reds is allowed.
- **No duplicate characters.** Tommy can appear once per lineup, in whichever form the wheel gives you.
- **2 re-spins per game.** A re-spin throws away the current color and lets you spin again.
- **The color is the luck.** The wheel lands on each color equally often. The skill is in which Ranger you take, where you place them, when to re-spin, and balancing chemistry and role coverage across the six.

## How scoring works (0-100)

| Part | Max | How |
|---|---|---|
| Strength | 62 | Blend of average Overall (40%) and average Offense, Defense, Support and Clutch (60%), scaled between two calibration constants (`LO=76`, `HI=92`). |
| Chemistry | 12 | Same-team pairs: +4 each. Same-era (different team) pairs: +0.75 each. |
| Role coverage | 21 | +3.5 each for having an elite (top 7%) Leadership, Intelligence, Speed, Power/Durability, Clutch and Support player. |
| Variety | 5 | 5+ different primary roles = 5, 4 roles = 2. |

Rank: **S** 90+, **A** 80+, **B** 70+, **C** 60+, **D** below 60. Because you can see every Ranger in the landed color, strong drafts are common. In simulation the median score for a play-the-highest-Overall draft is about 74, a draft that also weighs chemistry and role coverage scores about 79, and an S (90+) shows up in only a few percent of games.

**CPU battle:** 9 team-average categories (Overall, Offense, Defense, Support, Clutch, Leadership, Intelligence, Teamwork, Speed) are worth 1 point each; the Final Score is worth 3. Highest total of 12 wins.

## Team grades

Your team gets a letter grade based on the **average Overall rating** of your six Rangers, shown live above the lineup as it fills and in the results next to your score. Each stat bar in the results (Offense, Defense, Support, Clutch, Leadership, Intelligence, Teamwork) gets its own letter grade from its team average too.

| Avg rating | Grade | Avg rating | Grade |
|---|---|---|---|
| 92.5+ | A+ | 86.5+ | C+ |
| 91.5+ | A | 85.5+ | C |
| 90.5+ | A- | 84+ | C- |
| 89.5+ | B+ | 82+ | D+ |
| 88.5+ | B | 78+ | D |
| 87.5+ | B- | below 78 | F |

For reference, in simulation a typical draft averages about 88.5 (a B), a strong one 91+ (an A), and a purely random six well below that. The category grades use small offsets so a median team lands around a B in each category (for example Teamwork averages run lower than Overall). The separate **Rank** (S to D) and the 0-100 **Score** also include chemistry and role coverage.

## Ratings and the dataset

- Ratings are **game-design values**, not official Power Rangers statistics.
- The shipped ratings are a *stretched* version of dataset V2 (each stat is rescaled to a mean of about 76 and spread of about 9) so there are clear stars and clear duds. Rarity is assigned by Overall rank: top 5% Legendary, next 15% Elite, next 30% Rare, next 30% Uncommon, bottom 20% Common.
- `power_rangers_pick6_dataset_v3_stretched.csv` is the exact data embedded in the game.
- Your original V2 CSV is unchanged.

## Using your own Ranger photos

Helmet icons in each Ranger's color are shown by default. To show real images:

1. Create a folder named `images` next to `power_rangers_pick6.html`.
2. Add one JPG per Ranger named by its ID from the CSV `id` column, e.g. `images/MIG-001.jpg`.
3. Reload. Any Ranger without a photo keeps the helmet.

Photos work only when the file is opened locally (or hosted with the images folder). A page published on a platform that blocks outside files will always show helmets.

## Saved data (browser only)

Everything is stored in your browser's `localStorage`: best score (`pr6best`), CPU record (`pr6rec`) and daily result and streak (`pr6daily`). Clearing site data resets it. There is no server and no leaderboard, so the daily challenge is an honor system.

## Tweaking the game

Open the HTML file in a text editor:

- **Scoring constants:** search for `LO=` and `HI=`; the chemistry, coverage and grade logic is in `scoreLineup`.
- **Rarity:** Rangers still carry a rarity tier (shown on cards and in the pool browser), but it no longer changes what the wheel offers.
- **Wheel colors:** the `SEGS` array. **Re-spins:** `skips=2` in `newGame`.
- **Ranger data:** the `RAW` array. Each row follows the column order in `H` (id, character, season, year, color, form, role, rarity, overall, power ... special, team, era, offense, defense, support, clutch, tags).

If you change the ratings a lot, re-run a few hundred simulated drafts and re-pick `LO` and `HI` so the grades stay balanced.

## Notes

- The two JS files from the earlier dataset step (`power_rangers_pick6_rangers_v2.js`, `power_rangers_pick6_lineup_scoring.js`) arrived empty, so the scoring in this game was written fresh from the CSV columns.
- Power Rangers is a trademark of its respective owners. This is an unofficial fan project and uses no official artwork.
