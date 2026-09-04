# Lull

A sleep-native learning app for high-stakes college students.

**Learn around sleep, not while asleep.**

## The thesis

You cannot teach a sleeping brain new complex material. What is real:

1. Protecting sleep improves learning capacity.
2. A focused review right before bed feeds the overnight consolidation window (and can steady sleep).
3. Audio cues during sleep can reinforce already-studied material (TMR) — but not teach it.
4. Morning retrieval practice (the testing effect) is what builds durable memory.

Every feature and every word respects this. No "learn while you sleep" claims. The app never costs the user sleep.

## The daily loop

| Phase | Duration | What happens |
|---|---|---|
| **Evening encode** | ~10 min | Wren teaches one slice, ends with active recall. Timed for the pre-sleep window. |
| **Overnight** | — | Optional, honest audio reinforcement of tonight's material. Protect + measure sleep. Labeled "reinforcement, not new learning." |
| **Morning retrieve** | ~5 min | Interactive quiz. Right answers get a why; wrong answers get a correction *and* a why. Misses re-queue on a spaced schedule. This is the hero moment. |
| **Wren** | always | Sidebar tutor, source-grounded on the user's own materials. |

Curriculum is a sleep-anchored spaced-repetition schedule: items resurface based on morning performance **and** sleep signal.

## Audience

Memorization-heavy, high-stakes tracks first — pre-med/bio, nursing, law, MCAT/USMLE/bar.

JTBD: *"help me actually retain this in 6 weeks without wrecking myself."* Then widen.

## Running the prototype

Open `lull-prototype.html` in any browser. No build, no server, no dependencies.

Append `#selftest` to the URL to run the built-in assertions (scheduler, content integrity, and a guardrail check that no copy anywhere claims sleep-learning). Results print to the console.

## Design system

**Signature = two faces.** Same app, two circadian states: deep candlelit night ↔ bright morning. Auto-selected by time of day; a preview toggle lives in settings.

Fonts: **Fraunces** (display — headlines and Wren's voice, used sparingly), **Hanken Grotesk** (UI/body).

| Token | Night | Morning |
|---|---|---|
| sky | `#14111f` → `#1c1830` | `#fbf6ee` → `#fceede` |
| surface | `#221d35` | `#ffffff` |
| accent | `#e7a94e` / `#f0c578` | `#e39a34` (text `#b5771e`) |
| ink | `#ece4d6` | `#2a2333` |
| mute | `#9c94ac` | `#7a7286` |

Feedback uses a good green and a soft coral — never alarm red. All theming flows through `data-theme` + CSS custom properties.

## Difficulty changes kind, not just spacing

A rung further up the ladder is not the same question asked later. Each of the six
rungs asks a different *kind* of question about the same material:

| Rung | Interval | The ask |
|---|---|---|
| 0 | 1 day | first principles, plain language, no jargon |
| 1 | 2 days | the mechanism, and the vocabulary that names it |
| 2 | 4 days | the working method — applying it deliberately |
| 3 | 8 days | failure modes — how this goes wrong and how you catch it |
| 4 | 16 days | structure — how the parts constrain each other |
| 5 | 32 days | synthesis and transfer — explaining it to someone who does not know it |

Without this, an item you can recite reads as mastered when you have never once had to
apply it. The register is derived from the scheduler's step rather than stored beside it
— the scheduler already knows how well an item is held, and a second field tracking the
same thing would be free to disagree with it.

Six rungs rather than eight because the ladder tops out at 32 days for a 6-week course.
The last rung therefore carries both synthesis and teaching, which are the two registers
that actually prove mastery.

## Courses — name anything, learn it to mastery

Alongside the daily loop over the learner's own materials, Lull composes a course from a
named subject: a skill, book, essay, speech, film, documentary or textbook. Eight
competency tiers from first principles to teaching it back, a lesson and quiz per tier,
and a level that moves on XP earned by answering rather than by hours logged.

**Two ladders, deliberately.** The spacing ladder above is *when* an item returns. The
competency ladder is *how hard* it is asked. They are different axes — an item can be due
tomorrow and still be asked at mastery level, and one held for a month can still need
first principles if it was memorised rather than understood.

Eight rungs here against spacing's six, because competency runs past the length of one
6-week course. You can keep getting better at renal physiology after the exam.

| Level | Tier | The ask |
|---|---|---|
| 1 | Novice | plain first principles, zero jargon |
| 2 | Apprentice | core vocabulary and the mechanism behind it |
| 3 | Practitioner | the working method — applying it deliberately |
| 4 | Journeyman | failure modes and how to avoid them |
| 5 | Adept | structure — how the parts constrain each other |
| 6 | Specialist | nuance, tension, contested readings |
| 7 | Authority | synthesis and transfer to new situations |
| 8 | Master | original judgment, and teaching it to others |

A wrong answer still earns XP. The attempt is the learning, and zero for a miss turns
retrieval back into a score — the one thing this app will not put in front of someone at
6am.

**`grounded` is not optional.** A course built from the learner's own uploads and one
built from the model's general knowledge are different things, and the learner is entitled
to know which they are reading. Every composed course carries the flag and every surface
that shows a course shows it. Guardrail 3 still holds inside a course: where the tutor
answers from the learner's materials it says so, and where it has nothing it says that
instead of inventing.

`composeCourse()` is stubbed in the prototype exactly as `CONTENT` and `WREN_REPLIES` are
— no build, no server, no network — and the real implementation replaces that one
function.

## The explanation contract

Every question carries two pieces of copy, and the tutor must produce both:

- **`why`** — why the correct answer is correct.
- **`miss`** — why the *tempting* wrong answer fails.

The second is the one that does the work. A learner who picks a plausible distractor has
a specific wrong model, and naming only the right answer leaves that model intact. The
self-check enforces that both exist on every authored question; the LLM tutor is held to
the same contract when it generates them.

## UX principles (non-negotiable)

- One primary action per screen. Single hero button in the bottom thumb zone. Settings in the top corner, out of the easy path.
- The quiz is full-screen and single-focus with **no live score** — score anxiety kills recall. Reasoning appears as a calm reveal after answering.
- Gamify consistency and sleep, never raw hours. A rested, consistent student wins.
- Copy in Wren's voice: plain verbs, sentence case, active, never selling. Errors give direction, not apology.

## Wren

Warm, gender-neutral human name (dawn songbird → morning). Calm mentor at night, crisp coach in the morning — one identity, two energies.

Voice: warm, lower-pitched, gender-ambiguous by default, user-selectable across masc/fem/neutral. Softer and slower at night, brighter in the morning. A/B test on trust and 30-day retention.

## What's stubbed vs. real

**Stubbed:** the sample content object (one lesson + four questions), Wren's canned replies, the sleep signal. No auth, no storage, no network.

**Real:** navigation, two-face theming, the encode flow, quiz logic, the spaced-return scheduler and its copy, and the sidebar. All of it is built to extend.

## Roadmap

1. **Tutor** — LLM API for teach/quiz/sidebar, source-grounded on uploaded materials (RAG). Socratic "guide, don't dump" by default. Generated questions honour the explanation contract above and the register for the item's current rung.
2. **Voice, both directions** — Wren narrates the evening slice (TTS) and takes spoken answers in the morning (STT), as two thin server functions: `narrate(text) -> audio` and `listen(audio, mime) -> text`. Voice is what makes the pre-sleep slice usable with the lights already off, which is exactly when it should be used.
3. **Curriculum engine** — any book or subject → a spaced schedule over N days; generates the evening slice and the morning questions.
4. **Spaced repetition** — scheduler keyed to morning performance and sleep. Misses resurface.
5. **Personalization** — import NotebookLM / LLM history / connected apps to calibrate level and tone.
6. **Sleep integration** — HealthKit and wearables for sleep signal. Honest overnight audio reinforcement (TMR-style cueing of that night's material only). Protect-sleep guardrails.
7. **Accounts + storage**, then privacy as a marketed feature. Data-hungry, student, health-adjacent — trust is the moat.

## Guardrails

- No "learn while asleep" claims, anywhere.
- Never reduce the user's sleep.
- Source-ground the tutor. If it isn't in the user's materials, Wren says so rather than inventing.
- Privacy-first.
- Non-punitive tone throughout.
