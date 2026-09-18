# Learning Hub

Static, zero-build collection of practice apps. Every push to `main` deploys automatically to Vercel.

## Layout

```
.
├── index.html                  hub landing page (the APPS list lives here)
├── manifest.webmanifest        PWA manifest — one installable app for the whole site
├── sw.js                       service worker (offline support)
├── vercel.json                 cache + security headers
├── icons/                      home-screen and favicon art
└── apps/
    ├── english-explorer/
    │   └── index.html          Pronouns, reading, shapes, riddles, rhymes (5 sets)
    ├── inventors-lab/
    │   └── index.html          Year 3–4 English: pronouns, job words, word
    │                           families, to + verb, reading, 2D/3D shapes,
    │                           optical illusions (6 sets)
    └── science-detectives/
        └── index.html          Year 3–4 science: states of matter, particles,
                                separating mixtures, dissolving, fair tests
                                (6 core sets + 2 challenge sets)
```

Each app is one self-contained HTML file: no build step, no dependencies, no
shared runtime. Copying an app folder is a perfectly good way to start a new one.

## Inventors Lab

Built from the SG NEXT Year 3 review worksheets. 6 sets × 25 questions (150
questions, 273 stars), each set with its own reading passage about a real
inventor or discovery.

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

A brand-new learner is given a random seed on first visit, and the six sets are
shuffled with it. That order is saved immediately and is **never reshuffled** —
a returning learner always sees the same journey, on every visit, and sets
unlock along their own order rather than by set number. A backup file carries
the seed and the order, so restoring on a new device keeps the same journey.
A grown-up can deliberately draw a new order in Settings (for a second child
sharing a device); nothing else changes it.

## Science Detectives

Built from the Y3 final science review packet. 8 sets × 24 questions (192
questions, 504 stars), in two tiers.

The **six core sets** are parallel, not a progression — every one covers all six
topics, which is what makes the shuffled set order safe. The **two challenge
sets** use exactly the same plain English but push the reasoning a layer
deeper, and they always come last (see below).

Six sections per set: **Solid, Liquid, Gas** · **Tiny Particles** (the particle
model) · **Separating** (sieving, filtering, magnets, hand picking) ·
**Dissolving** (soluble and insoluble, what speeds it up) · **Think It Out**
(clue tables, odd-one-out, sequencing) · **Test It** (fair tests, predictions,
reading charts).

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

All apps share one browser origin, so **`localStorage` keys must be namespaced per app**.
English Explorer uses `englishExplorer.v1`, Inventors Lab uses
`inventorsLab.v1` and Science Detectives uses `scienceDetectives.v1`. Use
`<appName>.v1` for anything new — never a bare key like `progress`.

Both newer apps store drawings as PNG data URLs. Their `save()` drops the
oldest drawings rather than failing if the browser's storage quota is reached,
so a full gallery can never cost a child their stars.

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
