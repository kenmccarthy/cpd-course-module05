# Building Your Module — SETU CPD

**Module 5** of the SETU Continuing Professional Development series, *Building Your
Module Without Making a Mess of the LMS*. A clean, modern, self-paced SCORM 1.2
course built as a **standalone static site** — no build step, no server framework,
no external dependencies (brand fonts are self-hosted, JS is vanilla).

Built from the *Building Your Module Without Making a Mess of the LMS — On-demand
learning script* (Unit 5), and styled to the **SETU Online Course Brand Guide** and
the *Lifelong Learning at SETU Design Toolkit*.

Derived from [`kenmccarthy/cpd-course-template`](https://github.com/kenmccarthy/cpd-course-template),
which carries the reusable shell (navigation, Rise layer, SCORM plumbing,
animations, accessibility conventions). Fixes to that shell are worth porting
back to the template so later modules inherit them.

**Audience:** Academic staff new to curriculum design.
**Pitch level:** NFQ Level 8 (honours bachelor degree standard), with Level 7 and
Level 9 contrasts.
**Platform:** LMS-agnostic, with Moodle and Blackboard examples.
**Runtime:** ~30 minutes, 7 sections.

## Sibling modules

| Module | Course | Repository |
|---|---|---|
| 1 | Understanding Module Descriptors | [`cpd-course-module01`](https://github.com/kenmccarthy/cpd-course-module01) |
| 2 | Teaching Online | [`cpd-course-module02`](https://github.com/kenmccarthy/cpd-course-module02) |
| 3 | From Module Descriptor to Teaching Plan | [`cpd-course-module03`](https://github.com/kenmccarthy/cpd-course-module03) |
| 4 | Assessment that Works | [`cpd-course-module04`](https://github.com/kenmccarthy/cpd-course-module04) |
| 5 | Building Your Module | this repo |

The shell is shared; each module keeps its own `storeKey`, so a learner's progress in
one never collides with another even when both are served from the same origin.

## Course outline

| Section | Topic | Time |
|--------:|-------|-----:|
| 1 | Welcome and learning outcomes | 1.5 min |
| 2 | A clear, accessible and engaging online learning space | 5.5 min |
| 3 | The module page, announcements and content organisation | 6 min |
| 4 | Discussion forums, live links and communication | 5.5 min |
| 5 | LMS tools for peer and lecturer–student interaction | 5 min |
| 6 | Using Turnitin links for assessments | 4.5 min |
| 7 | Summary and final reflection | 2 min |

Sections are numbered **1–7** everywhere the learner sees them. Internally each
`<section class="lesson">` keeps a 0-based `data-lesson` id (`0`–`6`) — that id is what
`localStorage` progress and the SCORM bookmark use, so renumbering the display never
disturbs saved progress.

### Theory covered

Sweller's cognitive load theory · Mayer's multimedia principles · Biggs and Tang's
constructive alignment · the EU Web Accessibility Directive and WCAG · CAST's
Universal Design for Learning · the Community of Inquiry (Garrison, Anderson &
Archer) · the National Forum's digital roadmap · Salmon's five-stage e-moderating
model · Moore's three types of interaction · Laurillard's conversational framework ·
the National Forum on feedback as dialogue · NAIN and QQI guidance on generative AI.

## Checks, reflections and completion

The script has **two knowledge checks** — one single-answer (a Level 8 forum prompt),
one **multiple-response** (good practice in setting up Turnitin) — and **eight
reflection fields**: five "your experience" prompts and the three-part module action
plan. It asks for neither a graded quiz nor a pass mark. So:

- `finalQuiz` in `js/course.config.js` is **empty**; no check is graded.
- The course reports SCORM **`completed`**, never `passed`/`failed`, and **sends no
  score**. `<adlcp:masteryscore>` is omitted from `imsmanifest.xml`.
- Both checks are written as `cmi.interactions.n` entries, so per-question results
  appear in the LMS. The multiple-response check records its answer as `A,C,D`.
- The script gives one feedback paragraph for the multiple-response check, for right
  and wrong alike, so that is what both outcomes show; a short line says how many
  options were right, wrong or missed.

## Interactions (all native, no libraries)

| Activity | Section | Type |
|---|---|---|
| **Spot the problems** (`messy-hotspots`) | 2 | Six hotspots on a mock-up of a messy module page |
| Start Here / structure / announcements (`module-accordion`) | 3 | Three-panel accordion |
| **Where does each item belong?** (`page-match`) | 3 | Drag, tap or keyboard sort; six items, four boxes |
| Kinds of forum (`forums-tabs`) | 4 | Tabs: Q&A, learning discussion, social |
| Knowledge check: forum prompt | 4 | Single answer with retry |
| Types of interaction (`interaction-tabs`) | 5 | Tabs, each with a Moodle and Blackboard comparison |
| **Build your interaction map** (`interaction-mapper`) | 5 | Three dropdowns, feedback on each, then a model answer |
| Setting up Turnitin well (`turnitin-accordion`) | 6 | Five-panel accordion |
| Turnitin at Levels 7/8/9 (`turnitin-tabs`) | 6 | Tabs, defaults to Level 8 |
| **Knowledge check: Turnitin set-up** | 6 | Multiple response, with retry |
| Reflections | 2–7 | Eight ungraded prompts, exportable |

### Figures and mock-ups

Inline SVG or CSS, theme-aware: the three-qualities process (§2), the messy module
page (§2), the module page wireframe (§3), a threaded discussion (§4), Moore's three
interactions (§5), Salmon's five stages (§4) and a clearly set-up submission page (§6).
There is also a **downloadable one-page audit checklist**, a self-contained printable
page in `assets/` that ships inside the SCORM package.

## Widgets available to any course

Carried in the shell and used only when a course provides their markup. Each one's
markup contract is documented at the top of its block in `js/app.js` or
`js/interactions.js`.

- **Multiple-response knowledge check** (`.kc[data-type="multi"]`) — select every
  option that applies, then check. Correct only when the selected set is exact. The
  chosen letters are recorded comma-separated (`A,C,D`), which is how a SCORM 1.2
  `choice` interaction expresses several responses.
- **Hotspots** (`[data-hotspots]`) — a mock-up with problems hidden in it.
- **Interaction map** (`[data-mapper]`) — pick a tool per column, get feedback on
  each pick, then compare with a suggested answer. Unscored.
- **Accordion** (`[data-accordion="KEY"]`) — keyed like the other widgets, so a page
  can carry several; it emits `<KEY>-accordion`. Panels can be as long as they like.
- **Matching** (`[data-match]`) — takes distractors via a catch-all box, an optional
  `data-perfect` message, and an optional `data-feedback` note shown after every
  check (with a `[data-matchnote]` element).
- **Checklist with a progress ring**, **scenario chooser** and **side-tab explorer**
  — not used in this course.

`[data-match]`, `[data-tabs]`, `[data-accordion]`, `[data-hotspots]`, `[data-mapper]`,
`[data-checklist]` and `[data-scenario]` are all keyed by their attribute value, which
becomes the `interaction.complete` id, so a page can carry several of each. Add a
widget's id to `activities` in `js/course.config.js` if it should count toward
*activities explored*.

## Rise-style learner experience (Lifelong Learning at SETU look)

The template follows the Articulate Rise pattern and the *Lifelong Learning at SETU
Design Toolkit* (see `docs/design-toolkit.md`):

- **Course overview page** — the front door: a Slate Grey hero with a gradient
  **U-shape** panel (title, subtitle, tagline "LEARN MORE. GO FURTHER."), a large
  cropped **gradient crest**, a *Start / Continue* button, and **lesson cards**
  showing duration and live status (not started / in progress / completed) with a
  progress bar. Also reachable from the sidebar's "Course overview" item.
- **Hero banner per lesson** — every lesson opens with the *same treatment as the
  overview*: a Slate Grey block with a gradient **U-panel** (eyebrow "Section N of 7",
  title, read time), the large cropped **gradient crest**, and a progress strip on
  the right (one segment per section, completed ones in Grass Green) with a
  "N of 7 sections completed" line. The eyebrow and strip are rendered by
  `js/rise.js` from `Course.SECTIONS`, so the counts are never hand-typed. To add a
  cover photo, place `<img class="hero__bg" src="assets/…" alt="">` as the first
  child of the `.hero` element — it sits behind the crest at reduced opacity.
- **Continue-button reveal** — lesson content is paced in chunks separated by
  `<div class="gate" data-gate></div>` markers. Each *Continue* reveals the next chunk
  with a staggered entrance and scrolls to it; the lesson's Next/Previous bar appears
  only after the last gate. Progress through the gates persists and feeds the lesson
  cards. `Course.rise.revealAll(lessonEl)` opens a lesson fully (testing/accessibility).
- **Horizontal slide transitions** — lessons slide in from the right (forward) or the
  left (back) with a crossfade; a plain swap under `prefers-reduced-motion`. The
  outgoing lesson is pinned at the scroll position the learner was reading (so that
  part is what slides away), the scroll reset is instant rather than smooth, and
  blocks already in view settle at once instead of staggering in on top of the slide —
  one motion per page change, no judder.
- **Rise block layout** — a wider 900px content column for full-bleed blocks, with prose
  held to a ~70-character measure for readability.

### Programme colour year

`programmeYear` in `js/course.config.js` selects the Lifelong Learning accent trio
(always on Slate Grey + White). It drives section accents, tints, mesh gradients,
heroes and cards:

| Year | Accent trio |
|---|---|
| `"2026/27"` (default) | Heather Purple · Sunset Red · Clover Pink |
| `"2027/28"` | Barrow Blue · Suir Blue · Grass Green |
| `"2028/29"` | Sea Green · Sunrise Yellow · Suir Blue |

Section accents cycle through the trio via `data-accent="1|2|3"` on each lesson.
Semantic colours stay fixed (Grass Green = correct, Sunset Red = incorrect).

### Tagline font

The campaign tagline uses **Compressa Condensed Black**, a licensed face. Drop
`assets/fonts/compressa.woff2` into the repo to enable it; until then it falls back to
DM Sans Bold. Set `showTagline: false` in the config to hide the tagline entirely.

### Icons

All UI icons come from a single **inline SVG sprite** at the top of `index.html`
(24px line icons adapted from Lucide, ISC licence) — no emoji, no icon font, no
external requests, so they render identically on every OS and LMS. Use one with

```html
<svg class="icon" aria-hidden="true" focusable="false"><use href="#i-check"></use></svg>
```

or from JavaScript with `Course.icon("check")`. Icons are decorative (the adjacent
text carries the meaning); add new `<symbol id="i-…">` entries to the sprite as
needed. Sizing follows the surrounding font size (`.icon` = 1.05em) with a few
context overrides in `css/styles.css` under *Icons + accessibility utilities*.

## Accessibility

The template is built to WCAG 2.1 AA and audited with axe-core (no violations
across the overview, lessons and the dark theme). Conventions to keep when
authoring new courses:

- **Landmarks & skip link** — `<main id="scroll">`, `<nav aria-label="Course
  sections">`, and a "Skip to course content" link that appears on keyboard focus.
- **Page changes are announced** — a visually hidden live region (`#a11yStatus`)
  receives "Section N of 7: …", "Course overview", "More of this section revealed"
  etc. via `Course.announce(text)`; focus moves to the new lesson's `<h1>` (or the
  newly revealed chunk after *Continue*) so screen-reader and keyboard users land on
  the new content. The active navigation item carries `aria-current="page"`.
- **Real widget semantics** — the tab explorers are ARIA tab lists (roving tabindex,
  ←/→/Home/End); the accordion buttons expose `aria-expanded`/`aria-controls`;
  knowledge-check options are grouped under their question, and the
  multiple-response options are `role="checkbox"` with `aria-checked`; feedback and
  scores are `role="status"`; reflection textareas are labelled by their prompt;
  matching tokens are keyboard-operable buttons and every placement/removal is
  announced.
- **Hotspots by keyboard** — each hotspot is a real `<button>` wrapping the part of
  the mock-up it marks, with `aria-pressed`; every find is announced and collected
  into a list, and "Show me the rest" is there for anyone who would rather read
  than hunt.
- **Results never rest on colour** — after a multiple-response check every option
  says what it was ("Good practice — correct choice", "Not good practice", "Good
  practice — you missed this one"), and the interaction map labels each verdict
  ("Strong fit", "Partial fit", "Weak fit") with an icon and text.
- **Native form controls** — the interaction map uses `<select>` elements, each with
  a visible `<label>`.
- **Label in Name** (WCAG 2.5.3) — each lesson card on the overview takes its
  accessible name from its visible content rather than a separate `aria-label`.
- **Keyboard** — everything works without a mouse: `Esc` closes the mobile menu
  (focus returns to the menu button), the global ←/→ page shortcuts stay out of the
  way of controls, and `:focus-visible` rings are visible on every colour (white on
  slate/gradients, accent elsewhere).
- **Contrast** — text tokens and the sidebar's white-on-slate opacities meet 4.5:1;
  colour is never the only signal (correct/incorrect also show a tick/cross and text).
- **Motion** — every animation and transition, including the slide between lessons,
  is disabled under `prefers-reduced-motion`.
- **Structure** — one `<h1>` per lesson, headings step by one level, figures are
  `role="img"` with a text alternative, decorative art is `aria-hidden`.

## SCORM 1.2 packaging

The course ships ready to package for an LMS. A single **SCO** (`index.html`) and
an `imsmanifest.xml` are included; `js/scorm.js` maps the course onto the SCORM
1.2 data model. It is a **no-op when no LMS is present**, so the same files still
run standalone from `file://`.

**Build the uploadable zip:**

```bash
python3 scripts/build_scorm.py
# -> dist/<course-slug>-scorm12.zip  (manifest at the root)
```

Upload that zip to your LMS, or to <https://cloud.scorm.com> to validate.

**What it reports**

| SCORM (CMI) field | Meaning |
|---|---|
| `cmi.core.lesson_status` | **`completed`** for this course (reflection-led, no graded quiz). With a `finalQuiz` configured it reports `passed` / `failed` instead |
| `cmi.core.score.raw` | Not sent for this course. With a graded quiz: final-quiz percentage (min 0, max 100) |
| `cmi.core.lesson_location` | Bookmark — the section the learner was on |
| `cmi.suspend_data` | Compact resume state (visited sections, Continue-gate progress, answers, activities). Kept **well under the SCORM 1.2 ~4 KB cap**; reflections are **excluded** (private, and local only) |
| `cmi.core.session_time` | Time on task |
| `cmi.interactions.n` | One entry per knowledge-check answer — see *Analytics* |

**Test the adapter offline:** append `?scorm=mock` to the URL to inject a mock
LMS that logs every SCORM call to an in-memory store (used by the automated tests).

## Analytics

Per the chosen design, the **LMS is the analytics store**: every knowledge-check
answer is written as a `cmi.interactions.n` entry, so completion
and per-question results appear in the LMS's own reporting — no third-party
scripts, no external calls, and nothing that breaks the standalone/offline
property. Reflection prompts report only that a note was saved and how long it
was, never its text.

- **Event bus** — all interactions emit typed events through `js/core.js`
  (`section.view`, `knowledge_check.answer`, `interaction.complete`,
  `quiz.complete`, `course.complete`, …). SCORM and analytics subscribe to these.
- **Dev event inspector** — append `?debug=1` to see a live on-screen log of
  every event (and whether an LMS is connected).
- **Optional self-hosted sink** — for non-LMS hosting, set
  `Course.analytics.endpoint = "https://…"` to POST events via `sendBeacon`.
  Disabled by default.

## Running it

It's static, so any of these work:

```bash
# Simplest: just open the file
open index.html            # macOS   (or double-click it)

# Or serve it (recommended for a shared/hosted copy)
python3 -m http.server 8000
# then visit http://localhost:8000
```

Deploy by copying the whole folder to any static host (GitHub Pages, Netlify,
an LMS file area, a shared drive, etc.).

## File structure

```
.
├── index.html                 # all course content + interactive markup (+ inline icon sprite)
├── imsmanifest.xml            # SCORM 1.2 package manifest (single SCO)
├── css/styles.css             # SETU design tokens, layout, components, animations
├── js/
│   ├── course.config.js       # per-course identity (title, slug, institution, storeKey, pass mark, activities)
│   ├── core.js                # state store and the event bus (foundation)
│   ├── scorm.js               # SCORM 1.2 adapter (+ ?scorm=mock test harness)
│   ├── analytics.js           # dev event inspector + optional endpoint sink
│   ├── animations.js          # scroll-reveal, count-ups, self-building diagrams
│   ├── confetti.js            # brand-coloured canvas confetti
│   ├── interactions.js        # hotspots, interaction map (+ shell checklist / scenario chooser)
│   ├── dashboard.js           # results dashboard + export-my-notes
│   ├── rise.js                # overview page, lesson cards, Continue-reveal
│   └── app.js                 # navigation, progress, theme, core widgets
├── scripts/
│   ├── build_scorm.py         # builds dist/<slug>-scorm12.zip
│   └── init_course.py         # interactive setup for a new course's identity
└── assets/
    ├── MONO_WHITE.png         # official SETU reversed (white) logo — used in the UI
    ├── MONO_BLACK.png         # official SETU mono-black logo (for light backgrounds)
    ├── RGB.png                # official SETU full-colour logo
    ├── module-page-audit-checklist.html   # the printable one-page checklist learners download
    └── fonts/                 # self-hosted DM Sans + Inter (variable WOFF2)
```

Load order in `index.html`: `course.config → core → scorm → analytics →
animations → confetti → interactions → dashboard → rise → app`. `course.config.js`
must load before `core.js` (which reads `window.CourseConfig`), and
`scorm.js` runs early so it can restore `suspend_data` into the shared state
before the widgets read it.

## Logos

The official SETU logo files are used directly. The interface shows
`assets/MONO_WHITE.png` (the reversed/white lockup) in the sidebar and mobile
top bar, both of which sit on Slate Grey. `MONO_BLACK.png` and `RGB.png` are
kept in `assets/` for use on light backgrounds (e.g. print, certificates, or a
light header if you add one). Clear space is preserved around the logo and it is
displayed well above the 60px minimum.

`assets/favicon.png` is used as the browser-tab icon. `assets/setu-symbol.png`
(the official SETU "U" symbol) is used everywhere the course shows its "U"
motif — the cover graphic, the small eyebrow icon on each section, and the
watermark behind statement panels — via a CSS `mask-image` on the `.cover__u`,
`.u-mark`, and `.u-watermark` classes in `css/styles.css`, so it's still tinted
with `currentColor` (white on Slate, accent colours, dark-mode-aware) rather
than baked in as a fixed-colour image.

## Placeholders / items needing your input

Marked in-course with a *[Placeholder …]* note where a learner would see the gap.

- **Links** — the script gives only domain names, so those are linked at the root
  (`teachingandlearning.ie`, `docs.moodle.org`, `help.blackboard.com`,
  `help.turnitin.com`) and nothing deeper has been guessed. Add your institution's
  own LMS and Turnitin guides before publishing.
- **References carry no DOIs.** The script supplied none, and inventing them would
  be worse than leaving the citations plain.
- **SETU and Turnitin AI detection** (Section 6) is reproduced as the script states
  it — that SETU has turned the AI detection tool off. Confirm that is still true on
  the day you publish.
- **Hero banner** — the script's cover calls for an image of a tidy module page next
  to a cluttered one. The overview and section heroes use the gradient crest, as in
  the other modules; add approved imagery via `<img class="hero__bg">` if you want
  it. The cluttered page itself appears in Section 2 as the hotspot exercise.

### Written during the build because the script didn't supply it

The script gives build instructions for several things without their content. Each of
these is derived from the script's own guidance, but the wording and some of the
choices were made during the build rather than taken from it, so please read them.

- **The interaction-map exercise** (Section 5). The script names only the case
  ("Level 8 Project Management") and says to pick from a dropdown, then compare with
  a model answer. The two sample outcomes, the four options in each column, every
  piece of per-option feedback and the model answer were added during the build. Several
  options are deliberately defensible ("there's no single right answer").
- **The sort's answer key** (Section 3). The script gives six items and four boxes
  and a closing sentence, not a key. The key follows that sentence: the deadline
  reminder and the room change are announcements; the full brief and the contact
  details are Start Here; this week's reading is the weekly section; the case-study
  discussion is a forum.
- **The module page audit checklist.** The script says "one page… ten minutes" and
  doesn't supply it. Its 35 items are drawn from the guidance in Sections 2–6,
  grouped as Start Here, Structure and content, Announcements, Forums and
  communication, Accessibility, and Assessment submission.
- **The hotspot explanations** (Section 2) restate the script's own advice for each
  of its six listed problems. The clause about screen readers listing links is an addition.
- **Small connective text** — the *Social* forum tab (the script says only "perhaps a
  social space"), the one-line bridge in the *Announcements* accordion panel, and the
  wrong-answer feedback for the first check.

### Deliberately not built

The script's closing block asks for a downloadable *Module action plan template*. The
course offers **Download action plan** instead, which saves everything the learner
has written as a text file and needs no server. Add a formatted template in your LMS
if you want one.

## Brand fonts

DM Sans and Inter (both open-source, and both named in the SETU guide) are
self-hosted as variable WOFF2 files under `assets/fonts/`, so the course renders
with the correct brand typography **offline and with no external requests**. If
you ever need additional weights/styles, add the WOFF2s and extend the
`@font-face` block at the top of `css/styles.css`.

All diagrams called for in the script (the wireframe, the threaded discussion, the
three types of interaction, the five stages) are rendered as inline SVG or CSS in
the SETU palette, so they stay crisp and theme-aware without external images.

## A note on dark mode

The SETU guide describes a white-background identity. An optional, restrained
**slate-based dark theme** is included as a learner accessibility feature (toggle
in the sidebar). The default is the on-brand light theme; the dark theme keeps
Slate Grey and the approved accents.
