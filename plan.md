# Design plan — RQ Global

Status: **awaiting approval.** No code has been written. Per the brief's step 4,
this is the document to approve or redirect before any scaffold exists.

---

## 1. What is settled, and what is still missing

Confirmed by the client:

| | |
| --- | --- |
| Legal name | RQ Global |
| Jurisdiction | Ontario — regulated by PEO |
| Certificate of Authorization | Held; number to be supplied |
| Publishable projects | **None.** Template built on marked placeholders |

Still outstanding, and each becomes a visible `[PLACEHOLDER: ...]` with a line in
`CONTENT-TODO.md`: the C of A number, named engineers, the contact block
(address, phone, email), and a logo SVG.

**The missing contact address has one design consequence worth stating early.**
Every SEO title, the `LocalBusiness` schema, and the service-area language key
off a city. Until that arrives, those are placeholders, and the site cannot be
launched — not because the design is incomplete, but because a structural
consultancy with no verifiable address fails pre-qualification on sight.

---

## 2. The regulatory content, verified

This is the credibility section's actual text, and every line was verified
against a primary source on 2026-09-13. It is recorded here because getting it
wrong is the single most damaging error this site could make.

- **Ontario Building Code: O. Reg. 163/24**, in force 1 January 2025, replacing
  O. Reg. 332/12. It adopts **NBC 2020** as amended by the Ontario amendments.
  Amended seven times; the operative amendment document is dated 17 July 2026.
- **NBC 2025 was published 22 December 2025** and is the current national model
  code. **Ontario has not adopted it.** Any sentence implying NBC 2020 is
  nationally current is wrong. The correct framing: Ontario's Code adopts NBC
  2020 with Ontario amendments; NBC 2025 feeds Ontario's 2026/27 code.
- **The editions the Code references lag the current CSA editions**, and the
  site must name the right one for the right purpose:

  | Standard | Referenced by the OBC | Current CSA edition |
  | --- | --- | --- |
  | Concrete | A23.3:19 | A23.3:24 |
  | Steel | S16-19 | S16:24 |
  | Wood | O86:19 | O86:24 |
  | Masonry | S304-14 | S304:24 |
  | Cold-formed steel | S136-16 | S136-16 (R2026) — **no :24 edition exists** |

  Bridges (S6:25) are not an OBC standard and are only named if bridges are
  genuinely in scope.
- **Climatic and seismic data: MMAH Supplementary Standard SB-1**, per Div. B
  1.1.3.1 — Ontario's own dataset, not NBC Appendix C.
- **Part 9** — housing and small buildings: 3 storeys or fewer, building area
  not exceeding 600 m², Group C (other than retirement homes), D, E, or F Div.
  2/3. **Part 3** — post-disaster buildings, all Group A, B and F Div. 1, and
  any Group C/D/E/F Div. 2/3 building over 600 m² or over 3 storeys; structural
  design then falls under Part 4.

### Three constraints PEO places on this site

1. **No seal, anywhere.** O. Reg. 941 s.75(d) prohibits reference to or use of
   the professional seal or the Association's seal in advertising. Not in the
   logo, not in the footer, not as a credibility graphic. The permitted device
   is the PEO logo plus: *"Authorized by the Association of Professional
   Engineers of Ontario to offer professional engineering services."*
2. **"Consulting Engineers" is reserved.** Advertising under that phrase
   requires prior PEO Council authorization (s.68) and a designated consulting
   engineer on staff. It does not appear in copy unless RQ Global holds it.
   **This needs a yes or no before launch.**
3. **Project pages are governed too.** s.75 and PEO's practice guidance forbid
   claiming greater responsibility for a project than is fact, failing to
   indicate collaborating firms, and illustrating portions of a project the firm
   was not responsible for without a disclaimer. The project schema below carries
   a scope field for exactly this reason — it is a regulatory requirement wearing
   the clothes of a design decision.

Designation is styled **`P.Eng.`**, comma-separated from the name:
`Jane Smith, P.Eng.` Ontario has **no** structural specialist designation —
`Struct.Eng.` is a British Columbia grade of membership and must not appear.

---

## 3. The visual world

### The artifact, and the one I refused

The obvious artifact for an engineering firm is the blueprint: cyanotype blue,
white linework, graph-paper ground. It is the costume version, it is what any
model produces from the word "engineering", and it describes a reprographic
process abandoned decades ago. Rejected.

The real artifact is **a modern structural drawing set**: black linework on bond
paper, read at a desk under bright light. Its vocabulary is grid bubbles and
grid lines, dimension strings with tick terminators, section marks, hatch
patterns denoting material in section, revision clouds and revision triangles,
member schedules, and — the most characteristic and least-borrowed element — the
**title block**.

**The title block is the organising idea of this site.** Every drawing ever
issued carries one: a fixed field set in a fixed order, tiny labels and large
values, a revision row, and the name of who drew it and who checked it. It is
how the discipline has presented project metadata for a century, and it is
exactly what a procurement officer is trying to extract. The project pages use
its *information structure* — fixed fields, label/value contrast, the revision
row — not a literal drawn box with invented drawing numbers.

**The structural grid is the layout grid.** Content sits on a column grid whose
lines are drawn and whose bubbles are labelled, at section boundaries where they
carry information. This is the rule that keeps it from becoming decoration: if a
grid line is not marking a real boundary, it is not drawn.

### Palette

Light ground, chosen from the use scene rather than by category: a procurement
officer at a desk under office light, an architect on a laptop, both reading
what is effectively a document. Drawing sets are read on white paper. Dark mode
would be a preference imposed on a reading task.

| Token | Value | Role |
| --- | --- | --- |
| `--color-paper` | `#ECEDEF` | The ground. Cool-neutral bond paper, not cream |
| `--color-ink` | `#16191C` | Body text, headings. Graphite, not pure black |
| `--color-secondary` | `#5B6166` | Secondary text, labels, captions |
| `--color-rule` | `#CDD1D5` | Hairlines, borders, table rules |
| `--color-line` | `#8B9299` | Drawing linework and grid lines — **never text** |
| `--color-load` | `#D0341B` | Force, load, active state, revision marks |

A derived `--color-load-deep` (`#A02610`) exists for accent text on paper, where
the fill value will not clear 4.5:1. Contrast ratios get computed with a real
checker during the build, not judged by eye, and the token names encode the rule
that stopped the common failure: a line colour can never be assigned to text.

**The accent means one thing.** Red appears where force is, where a state is
active, and where something was revised. It is the colour of a load arrow and of
a revision mark on a drawing — both of which mean "look here, this is what
changed or what is carrying." It is never a wash, never a gradient, never a
decorative fill on a card. If red appears somewhere that is not about force or
change, that is a bug.

The palette is closed with Tailwind v4's `--color-*: initial`, so no off-system
colour can enter the codebase by accident.

### Typography

**Overpass** (SIL OFL + LGPL 2.1, variable 100–900) for everything, with
**Overpass Mono** (same licence, same designer) for measured values.

Overpass is an interpretation of the US Federal Highway Administration's
*Standard Alphabets for Traffic Control Devices* — letterforms cut to stay
legible at distance, at speed, in bad weather. The near-vertical terminals and
tight-but-open apertures solve the same problem as an 11px dimension string.
That lineage is infrastructure and North American, which is the subject's own
world rather than a borrowed one.

It was chosen over the DIN lineage deliberately. DINish is the better-drawn DIN
descendant and it was the obvious candidate — which is the reason against it. DIN
is what "technical typeface" resolves to for anyone who has looked at the
question for five minutes, and it carries German drafting-stencil associations
this firm has no claim to. Overpass reaches the same precision from the roads
and bridges this firm's audience actually builds.

It also ships **tabular figures and a slashed zero**, and has a true monospace
sibling by the same designer — so `8,400 m²` and `42 m clear span` align in a
column without a second family, and a rate table cannot shimmer.

| Role | Face | Treatment |
| --- | --- | --- |
| Thesis / hero | Overpass | Large, weight 600–700, tracking tightened |
| Section heading | Overpass | Weight 600, sentence case |
| Subhead | Overpass | Weight 500 |
| Body | Overpass | Weight 400, measure under 75ch |
| Label | Overpass | Weight 500, small, `--color-secondary` |
| Measured value | Overpass Mono | Tabular, for spans, loads, areas, code refs |

Approximately 51 KB for both as variable fonts, self-hosted, subset to Latin,
`font-display: swap`.

**The monospace rule, stated because it is the trap.** Mono appears on actual
measurements, code references and drawing identifiers. It never appears as a
decorative label style to signal "technical" — that is a costume, and it is the
single most common way this aesthetic goes wrong.

### What is banned in this build

No ALL-CAPS eyebrow above a heading. No `01 / 02 / 03` markers except on the
engagement sequence, where the content genuinely is a sequence. No middle-dot
meta strings. No `→` appended to link text. No identical rounded cards with the
same soft shadow. No gradient as decoration. No glassmorphism. No stock
photography of hard hats or high-vis. No stat band — RQ Global cannot count
years, projects or offices, and inventing that arithmetic is both dishonest and
a PEO advertising breach.

---

## 4. Layout

### Homepage

```
┌──────────────────────────────────────────────────────────────┐
│ RQ GLOBAL                        Work  Practice  Codes  Contact│  ← rule below
├──────────────────────────────────────────────────────────────┤
│  A          B            C            D          E            │  ← grid bubbles
│  ·          ·            ·            ·          ·            │
│                                                               │
│   The structure is the first decision                         │
│   and the most expensive one to change.                       │
│                                                               │
│   We are engaged before the drawings harden, when the         │
│   structural system is still a choice.                        │
│                                                               │
│   [ Send project details ]                                    │
│                                                               │
│        ┌─────────────────────────────┐                        │
│        │   frame at rest, linework   │   ← the same SVG that  │
│        │                             │     later carries load │
│        └─────────────────────────────┘                        │
└──────────────────────────────────────────────────────────────┘
```

Then the load path, pinned, as specified in the brief's section 3.

```
┌──────────────────────────────────────────────────────────────┐
│                                                               │
│                    ↓  ↓  ↓  ↓  ↓  ↓        1.4 kPa            │  ← accent
│              ┌───────────────────────┐                        │
│              │                       │                        │
│              │                       │     ← members          │
│              ├───────────────────────┤       illuminate in    │
│              │                       │       structural       │
│              │                       │       sequence         │
│             ═╧═                     ═╧═                       │
│                                                               │
│   [ scroll drives the load case — pinned, scrubbed ]          │
└──────────────────────────────────────────────────────────────┘
```

Section order, and each earns its place:

1. **Hero** — the thesis as type, frame at rest beside it.
2. **The load path** — the signature moment.
3. **What we do** — the engagement *sequence*, not a card grid. Concept and
   system selection → analysis and modelling → member and connection design →
   drawings and specifications → tender support → construction-phase review.
   Each stage names what is produced. This is the one place numbered markers are
   legitimate.
4. **Materials and systems** — the same structural bay re-rendering in concrete,
   steel, timber, masonry and composite, member proportions actually changing to
   suit. Steel slender, concrete stocky, timber between. The second, quieter
   scroll moment.
5. **Codes and jurisdictions** — the credibility block, set as a dense data
   table. Density is the aesthetic here, not a problem to hide. Content per
   section 2 above, with the referenced-edition/current-edition distinction
   visible rather than smoothed over. This section is what a procurement officer
   screenshots.
6. **Projects** — see the decision below.
7. **The people** — named engineers, `Name, P.Eng.`, designation, specialism,
   years in practice. Two or three named and licensed reads better than a vague
   team claim.
8. **Pre-qualification** — capability statement download, insurance, licensure,
   C of A. One click from the homepage. The PEO authorization statement lives
   here, and no seal appears.
9. **Contact** — form with project-type selector, optional drawing upload, a
   response time the firm can actually hold, plus address and phone in plain
   text.

### Project page — the title block

```
┌──────────────────────────────────────────────────────────────┐
│  [PLACEHOLDER: Project name]                                  │
├───────────────────────┬───────────────────────┬───────────────┤
│ LOCATION              │ CLIENT                │ YEAR          │
│ [PLACEHOLDER]         │ [PLACEHOLDER]         │ [PLACEHOLDER] │
├───────────────────────┼───────────────────────┼───────────────┤
│ BUILDING TYPE         │ ARCHITECT             │ GFA           │
│ [PLACEHOLDER]         │ [PLACEHOLDER]         │ 0,000 m²      │
├───────────────────────┼───────────────────────┼───────────────┤
│ STOREYS ABOVE / BELOW │ GRAVITY SYSTEM        │ LATERAL SYSTEM│
│ [PLACEHOLDER]         │ [PLACEHOLDER]         │ [PLACEHOLDER] │
├───────────────────────┼───────────────────────┼───────────────┤
│ FOUNDATION            │ CODE FRAMEWORK        │ SEISMIC CAT.  │
│ [PLACEHOLDER]         │ O. Reg. 163/24        │ [PLACEHOLDER] │
├───────────────────────┴───────────────────────┴───────────────┤
│ RQ GLOBAL'S SCOPE     [PLACEHOLDER]                           │
│ ENGINEER OF RECORD    [PLACEHOLDER], P.Eng.                   │
└──────────────────────────────────────────────────────────────┘

  Then: the engineering problem, and how it was solved.
```

Labels in `--color-secondary` at label size; values in ink, measured values in
mono. Fields are validated by the content-collection schema, so a missing
required field fails the build rather than shipping a half-filled page.

**Scope is a primary field, not a footnote.** The reference study found that
MKA's single most procurement-useful field is their own role on the project, and
it is the only facet on their 222-project archive. PEO independently requires
that a firm not claim greater responsibility than is fact. The same field
satisfies both.

### The projects decision

With no publishable projects, a filter over placeholder cards is theatre. The
schema is built completely and the filter is written but not rendered; projects
list plainly until there are enough real ones to need filtering. Arup's own
services axis — a directory-less section with an empty intro block — is what a
taxonomy outgrowing its content looks like, and it is visible from outside.

---

## 5. Motion

One orchestrated moment (the load path), one quieter second (the material
transition), stillness elsewhere. No fade-and-slide-up on every section.

**The load path is built with GSAP ScrollTrigger, not CSS scroll-driven
animation.** Firefox stable does not yet ship `animation-timeline` — Baseline has
been blocked since September 2025, support lands in Firefox 158, and stable is
155 as of 1 September 2026. iOS Safari 18.7 and earlier have none either. For
reveals, where the failure mode is "no animation", CSS scroll-driven animation
with an `IntersectionObserver` fallback is correct and carries most of the
motion. For the section that *is* the site's argument, silent non-execution in
Firefox is not an acceptable failure mode.

Animation is restricted to `transform`, `opacity`, `stroke-dashoffset` and
`stroke-opacity`. No geometry attributes, no layout properties.

**Reduced motion gets a designed artifact, not a disabled one.** The
accessible-animation guidance classifies scroll-jacked pinned scenes as
remove-entirely, so the reduced-motion path is not a degraded load path — it is a
properly set static structural diagram with every label, load value and
deflection callout visible at once. It must carry the same information
standalone. It is drawn and reviewed as its own deliverable.

---

## 6. Interrogation — critiquing this plan against the generic

**What would I produce for a generic "professional engineering firm" brief?**
Navy or steel-blue palette. Inter or IBM Plex. A hero photograph of a building
under a gradient overlay. Three icon cards reading Design / Analysis /
Consulting. Animated stat counters. A blue accent used decoratively. A
cream-and-terracotta variant if the brief said "premium".

**What in this plan was uncomfortably close, and what changed:**

- **The grid was going to be a background pattern.** That is graph-paper
  wallpaper and it is the cliché. Changed to: a grid line is drawn only where it
  marks a real content boundary, and bubbles are labelled only where the label
  means something. If it is not load-bearing, it is not drawn.
- **The typeface was going to be DIN.** DIN is the correct-sounding answer that
  every model reaches for, and this firm has no connection to German drafting
  standards. Changed to Overpass, whose FHWA signage lineage belongs to North
  American infrastructure.
- **Mono was going to be the label style throughout.** That is monospace as a
  costume for "technical". Restricted to actual measured values and code
  references.
- **The title block was going to be drawn literally**, with a fake drawing number
  and a scale that means nothing. Changed to its information structure only.
- **The accent was nearly a warm orange**, which is one hex step from the
  terracotta tell the brief bans. Changed to a signal red with a stated meaning.

**What could this break?** The plan's risk is concentrated in one place: the
load path is both the most technically demanding element and the one that must be
structurally *correct*, since an engineer will recognise a wrong mode shape
instantly. A beautiful animation showing a column bending under pure axial load
would do more damage than no animation at all.

**Riskiest step?** The deflection stage. Exaggerated deflection in a correct mode
shape is a narrow target: too little and it reads as a bug, too much and it reads
as a cartoon. This gets built first, before any other section, and the mode
shapes get confirmed rather than guessed.

**What is deliberately not done:** No hero photography — there is no photo
library, and full-bleed imagery magnifies weak photography rather than hiding it.
No stat band, because the arithmetic works against a new firm and inventing it
breaches s.75. No project filter until there are projects. No dual
markets/services taxonomy. No dark mode. No blog. No React.

**What I chose that is not in the brief:** the title block as the organising
device; Overpass over DIN; the specific palette values; deferring the project
filter. Each is called out above rather than buried.

---

## 7. Build order, once approved

1. Scaffold — Astro 7.3.2, Tailwind v4 via `@tailwindcss/vite`, content
   collections in `src/content.config.ts`, fonts self-hosted and subset, tokens
   in an `@theme` block with the palette closed.
2. **The load path first**, desktop and mobile, with its reduced-motion twin.
   It is the highest-risk element; if it does not land, the rest of the plan
   changes.
3. Remaining sections in page order.
4. Verification loop — screenshots at 320 / 768 / 1440, Lighthouse mobile to ≥95
   on all four, keyboard-only pass, reduced-motion pass.
5. `CONTENT-TODO.md` and `README.md` updated to match what was built.

**One limit, stated now rather than at the end.** The brief's definition of done
requires 60fps on a mid-range Android. There is no physical device here. CPU
throttling at 4–6× in Chrome DevTools catches most of it and is what will be
measured, but it is not the same test and will not be reported as if it were.
That check needs a real phone before launch.
