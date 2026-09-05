---
name: mark-composition
description: Turn photos of a teacher-marked handwritten English composition into a personalised interactive tutoring page — transcribed line by line, every red mark as a clickable annotation, two ordered recommendations for the rewrite, and the whole composition consolidated in corrected form. Use this whenever the user shares scans or photos of a marked school composition, worksheet or essay draft (The Write Connection, Singapore primary/secondary, or any red-pen marking), or says things like "this week's composition", "mark up this draft", "make a tutoring page", "go through her corrections with her", or drops images of ruled paper with handwriting on it — even if they don't say the words "tutoring page". Also use when filing a new week's composition work into this repository.
---

# Marking a composition into a tutoring page

The child brings home a composition covered in red pen. The marks are correct
but terse — `SP`, `(T)`, `pv`, a boxed phrase, an arrow to a margin note. A
seven-year-old cannot learn from a code. This skill turns that paper into a page
that explains every mark, shows what to write instead, and ends with a plan for
the rewrite.

Work through the phases in order. Phase 2 is the one that quietly decides whether
the whole thing is any good.

## 1. File it first

Read the worksheet header — it carries `TERM`, `WEEK` and `DATE`. Create:

```
term-<n>/week-<nn>-<yyyy-mm-dd>/
    <composition-title-slug>.html     the tutoring page, self-contained
    scans/                            the marked draft, rotated upright
    README.md                         what was marked, what to do next
```

The date is the one in the worksheet's DATE field (Singapore convention is
`d/m/yy`), written ISO-first so weeks sort chronologically. Add a row to the root
`README.md` index.

Deciding the folder first means the paths are settled before you start writing,
and the week's work never lands loose at the repo root.

## 2. Transcribe from crops, not from the whole photo

Phone photos of foolscap arrive rotated, and the marking is small red cursive
over grey pencil. Reading the full image and transcribing from that impression
produces a transcript that is *mostly* right — which is worse than one that is
obviously wrong, because nobody catches it.

Do this instead:

1. Rotate upright. `im.rotate(-90, expand=True)` in Pillow is usually right for a
   photo taken in portrait of a landscape-held sheet; check by reading one crop,
   and if the text is upside down use `rotate(90)`.
2. Cut the page into four or five overlapping horizontal strips.
3. Read **each strip as an image**, at full resolution, and transcribe from that.

The difference is not theoretical. On the first composition this skill was built
from, only the crops revealed `stealthily` written over a struck-out word, the
missing `r` in `mumured`, the left-margin note *"the students would not know the
surprise yet"*, and the fact that a four-line passage was bracketed as unclear.
All four were invisible in the full-page view.

Transcribe **by ruled line**, not by sentence — a sentence often runs across two
lines, and line numbers are how the child finds the place on their own paper.

Where a word is genuinely illegible (usually because the teacher struck it
through and wrote over it), say so in the week's README rather than guessing
silently. The correction is still teachable even when the original slip is not
recoverable.

## 3. Catalogue every mark before writing any prose

Go through the pages and list every intervention: underlines, insertions,
strike-throughs, circled letters, boxed phrases, margin remarks, arrows, stars.
Each becomes one annotation. Expect 20–35 on a two-page draft.

Give each one this shape:

| Field | What goes in it |
| --- | --- |
| **code** | The teacher's own shorthand — `SP`, `T`, `PV`, `CI`, `4Ws`, `G` — or a short plain word (`Cut`, `Specify`, `Reveal`) where she used an arrow rather than a code |
| **the mark** | What is physically on the paper: *"`surprice` underlined with `(sp)` and an `s` written above"* |
| **in her hand** | Her exact words, when she wrote a phrase or a model sentence. Reproduce these — they are the highest-authority content on the page |
| **why** | The teaching. Two to four sentences, addressed to the child as *you* |
| **try this** | The original struck through, then the corrected version |
| **go further** | One idea that generalises beyond this sentence — a memory hook for a spelling, a test they can apply to every `said` in the draft |

See `references/marking-codes.md` for what each code means and how to explain it.
Append to that file whenever a new code appears; it is meant to grow week by week.

### Rules that keep the page honest

**Never invent a mark.** Everything red on the page must correspond to something
red on the paper. The child will compare.

**Star the good marks too.** Teachers mark strengths as well as errors — a `✳`,
a "well done", a ticked technique. A page that is 100% correction teaches the
child that their writing is 100% wrong. Two positives in thirty marks changes
how the whole thing reads.

**Where she asked a question, mark your answer as yours.** *"What test? Specify"*
and *"describe the puppy"* are prompts, not corrections. If you supply a word so
the corrected copy reads smoothly, style it distinctly (a dotted underline works)
and say on the page that it is a suggestion and the choice is theirs.

**Gloss codes, don't assert them.** `CI` plausibly means *character's inner view*
but the teacher's classroom wording is authoritative. Give a reading, and say on
the page that hers wins.

## 4. Build the page

Read `references/page-spec.md` before writing any HTML. It carries the palette,
the type pairing, the layout, and the section order, so that this week's page and
last week's are recognisably the same publication. The visual identity is settled
— don't redesign it.

The one thing that changes each week is the data: the transcript, the
annotations, the recommendations, the corrected text.

## 5. Two recommendations, ordered

End the annotated draft with exactly **two** recommendations for the rewrite.

Two, because a child with a list of twenty things fixes none of them. The
surface marks — spellings, tenses, apostrophes — are repairs they can make while
copying the story out. The two recommendations are for the things that change
what the story *is*.

Choose them by asking what the teacher's longest margin remark is really about.
It is almost always structural: whose point of view, what got no space, what the
reader cannot follow.

State the order and why it matters — usually the second only works once the
first is in place. Give each one a concrete before/after on the actual sentences,
not general advice.

## 6. Consolidate the correction

Mark-by-mark annotation shows the trees. Finish with the wood: the whole
composition in one piece, in three switchable views.

- **Clean copy** — their own text with every mark applied and nothing else added.
  This is what Draft 1 should have looked like, and it is what they copy from.
- **What changed** — the same text with deletions struck and insertions coloured,
  so thirty marks can be seen landing at once.
- **Model draft** — how it could read once both recommendations are done.
  Label it plainly as a model to study rather than copy.

Store each paragraph once as a list of segments — plain string for unchanged
text, `{d:…, i:…}` for a replacement — and render both draft views from it. Two
hand-maintained copies of the same paragraph drift apart the first time anything
is edited.

Show a live word count per view. When the middle triples between the clean copy
and the model, that number argues for the recommendations better than any
paragraph of explanation.

## 7. Publish, commit, record

Publish the page as an artifact and put the URL in the week's README.

Write the week's README to cover: the files, what the page contains, the marks
by category, and — importantly — **the judgement calls**. Any word you could not
read, any code you glossed rather than knew, any wording you supplied that the
teacher only gestured at. A parent reading it in six months should be able to
tell your inferences from the teacher's instructions.

Commit to the week's folder. The artifact is tied to its URL rather than to the
file path, so a page that gets moved or rebuilt later must be republished by URL
or it becomes a second, separate artifact.

## Tone

Write to the child, not about them. *"You had already told us the tone twice"*
lands; *"the student repeats the adverb"* does not.

Explain why a rule exists rather than stating it. *"If you need an adverb to prop
up a verb, the verb is too weak"* is worth more than *"use powerful verbs"*,
because it can be applied to sentences this skill has never seen.

Be specific about what is good. A star that says *"strong opening, keep it"*
means less than *"most drafts open with 'One day…'; yours does not."*
