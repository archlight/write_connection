# Page specification

The visual identity is settled. Reproduce it — a parent should recognise week 12
as the same publication as week 7. Deviate only where a week's marking genuinely
needs something this spec has no slot for, and then extend the spec.

The reference implementation is the most recent week's page in `term-*/week-*/`.
Read it before writing a new one; copying its CSS wholesale is the intended
shortcut, and faster than rebuilding from this description.

## Format

One self-contained HTML file, published as an artifact. Scans embedded as
base64 `data:` URIs so the file works offline and survives being emailed. Keep
the whole page under 16 MB — downscale scans to ~1100px on the long edge at
quality 72 before encoding, which lands around 350 KB each.

## Colour

Neutrals carry a slight blue bias — the feint ruling of exercise paper, not a
warm cream. Red is functional: it means *the teacher marked this* and appears
nowhere decorative. Green means *this is the improvement*.

```css
:root{
  --paper:#eef1f6;  --sheet:#ffffff;  --sheet-2:#f7f9fc;
  --ink:#1b2027;    --ink-soft:#3c4757; --muted:#5b6779; --faint:#8894a6;
  --rule:#c9d8e6;   --edge:#d9e2ec;
  --red:#be2530;    --red-soft:#fdeff0; --red-line:#e8adb2;
  --green:#0d6b55;  --green-soft:#e7f4f0;
  --radius:3px;
}
```

Dark theme (`--paper:#111419; --sheet:#191d24; --ink:#e7ebf0; --rule:#2c3540;
--edge:#2b333d; --red:#ff7b80; --red-soft:#2a1b1d; --red-line:#6d3237;
--green:#5fd0ae; --green-soft:#132621;`) must be declared in **both**
`@media (prefers-color-scheme: dark){:root:not([data-theme="light"])}` and
`:root[data-theme="dark"]`, with every token first defined on bare `:root`.
`body` needs an explicit token background or it borrows the host's ground.

## Type

Four faces, each with a job. Loaded from Google Fonts with real fallback stacks.

| Role | Face | Used for |
| --- | --- | --- |
| Reading | **Literata** | headings, the transcript, the corrected copy. A face designed for reading instruction |
| Interface | **Public Sans** | explanations, buttons, notes |
| Data | **IBM Plex Mono** | code badges, line numbers, counts, eyebrows |
| Hand | **Caveat** | the teacher's red marks and margin remarks, in short doses only |

Caveat is the detail that makes the page feel like the paper. It is also
unreadable in bulk — margin remarks and inline insertions only, never body text.

## Layout

A marking sheet, not a document.

- **Line gutter** — 58px, mono line numbers, right-aligned, with a `--red-line`
  vertical rule down its edge. That rule is the red margin line of foolscap and
  is why the page reads as a marked script.
- **Ruled lines** — each transcript line is a grid row with a `--rule` bottom
  border. One row per ruled line on the paper.
- **Remarks strip** — the teacher's margin notes sit in a band at the head of
  each paragraph, in Caveat, dash-separated, each one clickable. This is the
  worksheet's REMARKS column, folded above the text it refers to.
- **Cards open in place** — clicking a mark expands its explanation directly
  beneath its own line, indented to the text column. Never a modal, never a jump
  to a footnote; the child must not lose their place on the line.

Mobile collapses the gutter to 36px and stacks the card's label/value rows.

## Section order

1. **Masthead** — programme, term, week, marking date as a mono eyebrow;
   composition title as the headline with *Draft Two* in red italic; a short
   note on how to use the page; a right-hand stamp block (draft, title, length,
   number of marks)
2. **Tally** — a row of code badges with counts. Compact chips, not big-number
   tiles; the codes are the content, and the figures are not the point of the page
3. **Legend** — what each red code is asking for, in a bordered grid, with the
   note that the teacher's own wording takes precedence
4. **Toolbar** — sticky. Progress count, *Show corrections* toggle, open all,
   close all
5. **The marked draft** — by paragraph, each with a remarks strip then its ruled
   lines
6. **Two recommendations** — numbered *Do first* / *Do next*, each with the
   diagnosis, a plan or beat list, and a before/after on the real sentences
7. **The whole composition, put right** — three-view switch, word count, copy
   button
8. **The scans** — the original photos, upright
9. **Footer** — how line numbers map to the paper, and where the ticks are stored

## Components

**Code badge** — mono, uppercase, letterspaced, red on `--red-soft` with a
`--red-line` border. Green variant for positive marks. Appears in the tally, the
legend, the margin chips, and as a small superscript on every marked span.

**Marked span** — a button, red 2px underline with 4px offset. Struck through
where the mark is a deletion, green where the mark is a compliment. Hover tints
`--red-soft`; open state doubles the underline.

**Inline correction** — the teacher's insertion in Caveat red, hidden until the
*Show corrections* toggle. Needs `display:inline-block` so it does not inherit
the strike-through or underline of the span beside it, and `del + ins` needs
`margin-left:.3em` or the two words run together.

**Coaching card** — `--sheet-2` ground, label/value rows on an 88px label
column, labels in mono small caps. Ends with a *Fixed in Draft 2* checkbox.

**Word bank** — pill-shaped green chips. Include one wherever advice would
otherwise be unactionable ("use a stronger verb" needs the alternatives).

**Teacher quote** — Caveat red with a `--red-line` left rule, used wherever she
wrote an actual phrase or model sentence.

## Interaction

Clicking any marked span or margin remark opens its card. Where the same
annotation appears twice (a margin chip and the line it points at), both toggle
together — keep the slot-clearing keyed on the annotation id, not the button.

*Show corrections* reveals every red insertion inline at once, turning the page
into a completed correction without opening a single card.

Per-annotation *Fixed in Draft 2* ticks persist in `localStorage`, keyed per
composition. Wrap reads and writes in try/catch and render correctly with no
stored value — private windows throw.

The page must be fully readable at rest. Nothing important behind a click; the
annotations add depth, they don't hide the content.

## Accessibility and build

`aria-expanded` on every toggle, `aria-selected` on the view switch, visible
focus rings (`outline:2px solid var(--red); outline-offset:2px`), and a
`prefers-reduced-motion` block. Wide content scrolls in its own container; the
body never scrolls sideways.

Look at the rendered page once before publishing — Chromium is at
`/opt/pw-browsers/chromium` — then make one pass of fixes. The things that
consistently show up in that look are inline red corrections inheriting the
wrong text-decoration, and struck/inserted words running together.
