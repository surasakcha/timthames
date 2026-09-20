# Learning Hub

Static, zero-build collection of practice apps. Every push to `main` deploys automatically to Vercel.

## Layout

```
.
├── index.html                  hub landing page (the APPS list lives here,
│                               grouped into one section per school year)
├── manifest.webmanifest        PWA manifest — one installable app for the whole site
├── sw.js                       service worker (offline support)
├── vercel.json                 cache + security headers
├── icons/                      home-screen and favicon art
└── apps/
    ├── number-friends/
    │   └── index.html          Year 1 maths: counting, adding, taking away,
    │                           the missing number, number pairs to 10
    │                           (5 sets of 28)
    ├── english-explorer/
    │   └── index.html          Pronouns, reading, shapes, riddles, rhymes (5 sets)
    ├── inventors-lab/
    │   └── index.html          Year 3–4 English: pronouns, job words, word
    │                           families, to + verb, reading, 2D/3D shapes,
    │                           optical illusions (6 sets of 42)
    ├── science-detectives/
    │   └── index.html          Year 3–4 science: states of matter, particles,
    │                           separating mixtures, dissolving, natural
    │                           resources, fair tests
    │                           (6 core sets + 2 challenge sets)
    └── world-explorers/
        └── index.html          Year 3–4 social studies: climate zones, the
                                compass, map keys and grids, landforms,
                                natural resources, people and travel (4 sets)
```

Each app is one self-contained HTML file: no build step, no dependencies, no
shared runtime. Copying an app folder is a perfectly good way to start a new one.

## The hub is grouped by year

`index.html` renders one labelled section per school year, youngest first, so
a grown-up handing over the tablet can see at a glance which half of the page
belongs to which child. Each entry in `APPS` carries a `year`, and `YEARS`
holds the headings. Everything shipped so far is **Year 1** (maths) or
**Year 3** (English, science, social studies); adding a year means adding one
row to `YEARS` and tagging the apps that belong to it.

## Number Friends

Year 1 maths, built from the P.1 exercise-book pages on finding a missing
number. 5 sets × 28 questions (140 questions, 235 stars).

Six sections per set: **Counting** (to 10, one more and one less, ordering) ·
**Add and Take Away** · **The Missing Number** · **Add or Take Away?** ·
**Number Pairs** (the pairs that make 10) · **Story Sums**.

The exercise book teaches four separate rules — one each for finding the first
number of an addition, the first number of a subtraction, the added number and
the subtracted number. They are really **one** rule, and every explanation in
the app says the same thing:

> If the box is the **biggest** number in the line, **add** the other two.
> If the box is one of the smaller **parts**, **take away**.

A six-year-old cannot be asked to go and find a keyboard, so three question
types are new here:

| type | what the child does | stars |
| --- | --- | --- |
| `keypad` | taps 0–9 to fill the box in a sentence such as `5 + □ = 8` | 1 |
| `counters` | counts pictures laid out in ten-frames, then taps the number | 1 |
| `bond` | fills the missing part of a part-part-whole diagram | 1 |

They join `choice`, `match`, `sort`, `order`, `tfgrid` and `draw`. A `choice`
may carry `big: true`, which shows its number sentence at full size with a real
box — used for every tick-the-right-way question, because that sentence is the
thing the child is actually solving.

Two details that matter more at six than they would at eight: the text size
starts on **Bigger** rather than Normal, and a `counters` question that takes
some away draws them **crossed out** rather than removing them, so the child
can see both the eight that were there and the three that went.

### Every number is checked by arithmetic, not by eye

`nf_check.mjs` re-derives all 140 answers rather than trusting them: it solves
each number sentence for its box (including the reversed form, `10 = 6 + □`),
re-adds every `bond`, re-counts every ten-frame, and confirms that each
tick-the-right-way option really does produce the box. It also rejects a wrong
option that does not itself add up — a child should have to know the rule to
choose, not just spot the line with a mistake in it.

## Inventors Lab

Built from the SG NEXT Year 3 review worksheets. 6 sets × 42 questions (252
questions, 470 stars), each set with its own reading passage about a real
inventor or discovery.

Every part of the two review sheets is covered several times over in every
set, using the sheets' own sentences and definitions: all thirteen
circle-the-pronoun sentences, the write-a-sentence-with-both-pronouns task
(marked on whole words, so *he* is never found hiding inside *the*), all six
job titles with the sheet's definitions, the four word-family pairs plus more,
all six *to + verb* sentences, both reading passages with every question asked
of them, and the eight-shape word bank — circle, square, triangle, rectangle,
star, diamond, heart, oval — with star, heart and diamond drawn as their own
shapes. A child who can do every set can do the sheet.

Seven sections per set: **Pronouns** (subject/object) · **Job Words** ·
**Word Families** (invent → inventor → invention) · **To + Verb** (want/hope/
plan/try + to) · **Reading** · **Shapes** (17 flat and solid shapes) ·
**Look and Think** (optical illusions and design).

Eleven ways to answer, so no single skill gates a child's score:

| type | what the child does | stars |
| --- | --- | --- |
| `choice` | tap an option (text or shape picture) | 1 |
| `type` | type a short answer, with an optional hint button | 1 |
| `write` | write a sentence, graded on the ideas it contains | 1 per idea |
| `spell` | build a word from letter tiles, or type it | 1 |
| `build` | tap words into sentence order | 1 |
| `sort` | drag words into two boxes | 1 per word |
| `fill` | drag cards into gaps (with decoys) | 1 per gap |
| `match` | draw joining lines between two columns | 1 per pair |
| `order` | drag or arrow cards into a sequence | 1 per position |
| `draw` | draw an invention on a canvas | saved, not marked |
| `free` | write a short paragraph | saved, not marked |

Drawings and writing are kept in **My Workshop** inside My Results. Ten badges
reward breadth (spelling, illusions, writing, drawing, persistence, streaks)
rather than speed.

### Marking is deliberately forgiving

Typed answers ignore case, punctuation and a leading *a/an/the*, and an
edit-distance check forgives one or two slips of the pen — so `inventer`
passes for `inventor`, and the feedback still shows the correct spelling. The
tolerance scales with word length, so very short answers (`to fly`, `to see`)
still need exact spelling — those verbs are printed in the question itself.
Written answers look for any word in each idea-group, so several phrasings
earn the same stars.

### Each learner gets their own set order

A brand-new learner is given a random seed on first visit, and the sets are
shuffled with it. That order is saved immediately and is **never reshuffled** —
a returning learner always sees the same journey, on every visit. A backup file
carries the seed and the order, so restoring on a new device keeps the same
journey. A grown-up can deliberately draw a new order in Settings (for a second
child sharing a device); nothing else changes it.

The order decides how the sets are *presented*, not what may be played. **Every
set is open from the first visit**, in every app. Sets used to unlock one after
another at 60%, which meant a child who wanted more practice on one topic had
to score their way to it — and, when a save failed, could be shut out of work
they had already finished. Because the sets are parallel rather than
progressive (every set covers every topic), nothing is lost by letting a child
pick whichever one they like.

## Science Detectives

Built from the Y3 final science review packet and the "separating materials
from natural gas" handout. 8 sets × 42 questions (336 questions, 940 stars),
in two tiers.

Every section of every set has six questions, and between them the sets ask
every question on the review packet in several forms: the seven-property
solid/liquid/gas table (split across two tick-and-cross grids so it fits a
phone), the soluble-or-not list with all eight of its items, the four
method-equipment-mixture matches, the *why can't a sieve / why can't a filter*
explanations, the materials table with its magnet-sieve-or-hands decision, the
filter prediction, all ten dissolving true-or-false statements, and the sugar
chart with its most / least / in-between questions.

The **six core sets** are parallel, not a progression — every one covers all six
topics, which is what makes the shuffled set order safe. The **two challenge
sets** use exactly the same plain English but push the reasoning a layer
deeper, and they always come last (see below).

Seven sections per set: **Solid, Liquid, Gas** · **Tiny Particles** (the
particle model) · **Separating** (sieving, filtering, magnets, hand picking) ·
**Dissolving** (soluble and insoluble, what speeds it up) · **Materials and
Fuels** (where materials come from, ores and smelting, oil and natural gas,
burning fuels and carbon dioxide) · **Think It Out** (clue tables,
odd-one-out, sequencing) · **Test It** (fair tests, predictions, reading
charts).

The **Materials and Fuels** section runs through all eight sets and builds a
chain rather than a list of facts: paper ← wood ← trees, plastic and petrol ←
crude oil, metal ← ores ← smelting. It ends on consequences — burning a fuel
releases carbon dioxide, and too much carbon dioxide warms the Earth — and in
the challenge sets on what follows from that: a forest with no replanting runs
out, oil took millions of years so it cannot be replaced in a lifetime, and
heating crude oil to separate it is the same idea as evaporating salty water.

The English is deliberately plainer than the other apps: short sentences,
common words, and no long written answers. The thinking is meant to be the
hard part, not the reading. Two question types come straight off the
worksheet:

- **`grid`** — the tick-and-cross property table. Tap a cell to cycle it
  empty → ✓ → ✗ → empty. One star per cell, so a 3×4 table is worth 12.
- **`tfgrid`** — a list of statements, each marked True or False, one star per
  row.

A third type carries most of the extra difficulty in the challenge sets:

- **`multi`** — "tick every one that is true", scored **one star per option**.
  Because every line is marked separately, a child cannot pick the single best
  answer and move on; each statement has to be judged on its own, and leaving a
  false one unticked earns just as much as ticking a true one.

Alongside them: `choice`, `sort` (two or three boxes), `fill`, `type`, `match`,
`order` and `draw`. There is no spelling-from-tiles, no sentence building and
no paragraph writing anywhere in this app.

### The two challenge sets

`🧾 The Evidence Room` and `⚗️ The Method Lab` keep the reading level identical
and raise only the thinking. They add:

- **Conservation of mass** — a sealed jar of ice that melts, and salt water that
  weighs exactly the salt more.
- **Working backwards** — nothing came through the sieve, so what do you know?
- **Counter-examples** — one case that breaks a rule is enough to sink it.
- **Explaining an odd result** — same water, same sugar, three times longer.
- **Method order** — *why* the filtering has to come after the dissolving.
- **Judging evidence** — which tests would actually tell you something new, and
  why "it vanished, so it must be salt" is not safe.
- **Elimination across methods** — a sieve cannot help, a magnet cannot help,
  so what is left?
- **Clue tables where no single column identifies anything**, so two clues must
  be combined for every row.

They are marked `challenge: true`, and `setOrder()` shuffles core sets among
themselves and challenge sets among themselves, never letting a challenge set
land before a core one. A learner who already has a saved six-set order keeps
it untouched; the new sets are simply appended.

Diagrams, clue tables and bar charts are all generated inline as SVG from two
small builders (`tableFig` and `barFig`), so the numbers a question asks about
and the numbers drawn in the chart cannot drift apart.

### Clue tables must have one answer

Each "which is A, B and C?" puzzle is backed by a property table. The content
audit checks that **every row of every clue table is unique** — if two rows had
the same pattern of ticks, the puzzle would have no single answer.

The same check runs over the tick-and-cross `grid` questions, where repeated
rows usually mean a careless column. One grid breaks that rule deliberately:
in set 8, water and a pile of dry sand score identically on all three tests,
which is the whole point of the question. That item carries `sameRowsOk: true`
so the intent is recorded in the content rather than argued about later.

## World Explorers

Built from the Y3 social studies final exam review. 4 sets × 27 questions (108
questions, 306 stars). Like the science sets these are **parallel, not a
progression** — every set covers all six topics, so any set is a safe place to
start and the shuffled set order stays safe.

Six sections per set: **Climate Zones** (polar, temperate, tropical, arid,
Mediterranean, and how climate changes what people grow and wear) · **The
Compass** (the four and eight points, clockwise order, turning and reversing a
direction) · **Reading Maps** (keys, relief colours, grid coordinates) ·
**Landforms** (mountain, plateau, plain, valley, island, inlet, oasis, cave) ·
**Natural Resources** (ores and metals, wood, stone, water, and using them so
they last) · **People and Travel** (land/water/air/rail transport, public
versus private, age groups and what a town needs).

The English is kept as plain as Science Detectives. Two question types were
added for this subject, because a map and a compass are things you point at,
not things you describe:

- **`compass`** — drag N, E, S and W (or all eight points) onto a drawn compass
  rose, or tap a label and then tap a point. **One star per point**, so a
  half-remembered rose earns half the marks instead of nothing.
- **`gridmap`** — tap the square on a lettered-and-numbered map grid. The
  places are drawn into the cells, so the question is read off the map rather
  than off the text. One star. Some questions ask for a square that is
  *empty* ("which square is north of the hospital?"), which is the point:
  coordinates have to be worked out, not spotted.

Both live alongside the ten types Science Detectives already had: `choice`,
`match`, `type`, `tfgrid`, `multi`, `sort`, `order`, `fill`, `grid` and `draw`
— twelve in all, and every one of them is used in every set.

Figures are generated inline as SVG (`climatebands`, `rose4`, `reliefkey`,
`reliefmap`, `landprofile`, `cavecut`, and two bar charts), so a question about
"the tallest bar" and the bar actually drawn come from the same array.

### Property tables have to be true of every example

The tick-and-cross `grid` questions are the easiest place to write something
that is *usually* true and key it as always true. Three were rewritten during
review for exactly that: "found in a desert" had no honest answer for a cave or
a river (the Nile runs through one), "a desert is flat land" is wrong for the
dunes of the Sahara and the high Gobi, and "a cave is made of land" is odd for
something that is a space *inside* land. The replacements — "made mostly of
sand", a beach instead of a desert, and "you can stand on dry ground there" —
are true of every row they are asked about. Worth remembering when adding rows:
the audit can prove the rows are *distinct*, but only reading them proves they
are *true*.

## Adding a new quiz

1. Create `apps/<folder-name>/index.html` — one self-contained HTML file.
2. Add one entry to the `APPS` array in `index.html`:

   ```js
   { slug: 'folder-name', emoji: '🔢', title: 'Math Explorer',
     desc: 'Times tables and number bonds', ready: true }
   ```

3. Add `'/apps/<folder-name>/'` to the `CORE` array in `sw.js` and bump `CACHE`
   (e.g. `learning-hub-v1` → `learning-hub-v2`) so devices pick up the change.
4. Commit and push. Vercel builds and publishes in under a minute.

### Storage rule for new apps

All apps share one browser origin **and one storage allowance** — about 5 MB
between the five of them — so keys must be namespaced per app, and a new app
has to assume the others have already used most of the room. In use today:
`englishExplorer.v1`, `inventorsLab.v1`, `scienceDetectives.v1`,
`worldExplorers.v1` and `numberFriends.v1`.

Each app keeps **two** keys:

| key | holds | may be lost |
| --- | --- | --- |
| `<appName>.v1` | stars, badges, set order, settings, paused set | never |
| `<appName>.art.v1` | drawings, as JPEG data URLs | freely |

That split is the whole point. Stars and drawings used to share one key, and a
single drawing is far bigger than a whole year of progress — so a browser with
no room left could not save the picture, the write failed, and **the child's
stars went down with it**. Now progress is written first and on its own; art is
written afterwards and is thrown away, oldest first, until the rest fits.
Nothing about a picture can cost a star.

Three rules follow, for any new app here:

1. **Write progress before art, in separate keys.** `save()` must succeed on a
   full browser.
2. **Read every write back.** `setItem` throws when the browser is full, and in
   some private modes it fails silently, so `writeKey()` compares what came back
   before believing it.
3. **Art is the shared throwaway tier.** When progress will not fit, an app
   deletes its own `*.art.v1` first, then *any other app's* — those are
   replaceable, a child's record of their own work is not. Name the key with the
   `.art.v` suffix so the other apps recognise it.

Drawings are saved as JPEG rather than PNG: a child's drawing on a white
background costs roughly a tenth as much that way (about 30 KB instead of
360 KB), and the gallery keeps the 8 most recent.

Progress made while the browser refuses to save is not silently dropped — the
home and results screens both say so, and point at **Save backup** in
Settings.

## Why the Home Screen matters on iPad / iPhone

Safari's Intelligent Tracking Prevention deletes script-writable storage
(`localStorage` included) after roughly **seven days without visiting the site**.
A week of school holidays is enough to wipe a child's stars.

Web apps launched from the **Home Screen** get their own storage container and are
exempt from that eviction. So the hub shows an "Add to Home Screen" card on iOS until
the site is installed, and English Explorer has a **Backup progress** panel in Settings
that saves a `.json` file as belt-and-braces insurance.

Private Browsing disables persistent storage entirely — progress will not survive there.

## Local preview

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

Service workers need `localhost` or HTTPS; they will not register from `file://`.
