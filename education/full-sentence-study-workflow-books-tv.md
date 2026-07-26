# Full-Sentence English Study Workflow — Books and TV

> Goal: turn a book chapter or TV episode into a complete, source-verified study system that improves reading/listening, vocabulary, grammar, and speaking.

## Core division of work

| Work | Tool / model | Rule |
|---|---|---|
| Source extraction, cleaning, sentence IDs, batching, source matching, file generation, Structure, Chunk, Drill, navigation, and QA | **Codex** | Codex does the general processing and English-learning analysis. |
| Chinese `Meaning` translation | **SiliconFlow HY — `tencent/Hunyuan-MT-7B`** | HY is the translation source. Codex must not silently overwrite HY Meaning fields. |

The SiliconFlow credential must remain in secure local credential storage. Do not put API keys in Markdown notes, logs, prompts, Git, or chat messages.

---

## 1. Prepare the source

### For a book / EPUB

1. Locate the EPUB and inspect the navigation/TOC and spine.
2. Identify the exact chapter boundaries; do not rely only on split-file names.
3. Extract the chapter while preserving paragraph order.
4. Remove navigation, duplicate page headers, image-only labels, and licence text.
5. Keep the personal-study source under `_source text/`.

### For a TV show / film

1. Use an English subtitle/transcript file and verify that the dialogue is actually English.
2. Preserve time codes in the source SRT, but create a clean text stream for analysis.
3. Keep the original file under `_source subtitles/`.
4. Keep speaker names when they help with reference words, pronouns, tone, or plot context.

---

## 2. Build a stable sentence corpus — Codex

1. Split into complete sentence units without breaking long book arguments or merging unrelated subtitle lines.
2. Give every unit a permanent ID, such as `0001`, `0002`, …
3. Save resumable batches of about 15–50 units.
4. Preserve the exact original sentence for every ID.

5. Use the exact English sentence as a translation-cache key, so repeated lines are translated only once.

**Coverage rule:** every sentence requested by the learner appears exactly once. A title, ingredient-list fragment, or image label should be marked as a fragment rather than falsely treated as a complete sentence.

---

## 3. Translate `Meaning` with HY only

1. Run a one-sentence API smoke test before bulk work.
2. Call SiliconFlow’s OpenAI-compatible API with model `tencent/Hunyuan-MT-7B` through secure local credentials.
3. Send numbered batches and require an exact JSON mapping:

```text
0001 -> Chinese Meaning
0002 -> Chinese Meaning
```

4. Use a small, safe concurrency level (default: **3 workers**), with retries and exponential backoff for malformed output or rate limits.
5. Validate that every returned ID is present exactly once before writing anything into notes.
6. Add a domain prompt/glossary. Examples for cooking:
   - `stock` → 高汤
   - `carcass` → 熬汤用的禽畜骨架 / 胴体, never “尸体”
   - `simmer` → 微沸 / 小火煨煮
   - `baked` → 烘烤 / 烤制, never “蒸”
7. Save a translation-progress manifest and cache so interrupted work can resume safely.

**HY output rule:** `Meaning` is a study gloss, not a published translation. Keep the meaning faithful, natural, and context-aware.

---

## 4. Generate English analysis with Codex

For every sentence, Codex writes exactly four learner fields:

```markdown
## 0001
**Original:** Exact source sentence.
**Meaning:** HY Chinese translation.
**Structure:** Grammar, tense, clause relationships, and a warning about likely learner errors.
**Chunk:** Natural speaking / reading chunks, collocations, phrasal verbs, connectors, and useful frames.

**Drill:** A short shadowing, substitution, retelling, or campus-life / daily-life production task.
```

### What Codex should emphasize

- **TV dialogue:** phrasal verbs, indirect tone, relationship language, quick everyday chunks, and past-tense retelling.
- **Books:** long-sentence logic, definitions, connectors, academic verbs, arguments, and seminar-style speaking frames.
- **Speaking transfer:** turn each important sentence into something the learner can say about class, work, grocery shopping, friends, or personal experience.

---

## 5. Create two learning layers

### A. High-value chapter / episode note

Use this first for immediate study:

- plot / argument map
- essential vocabulary and chunks
- selected high-value sentence table
- 45–60 second retelling task
- error checklist
- active-recall questions

### B. Full Sentence-by-Sentence Analysis

Use this for slow, complete study. It contains every source sentence with `Meaning`, `Structure`, `Chunk`, and `Drill`.

The high-value note helps you start quickly; the full note ensures that no useful sentence is lost.

---

## 6. Verify before calling a unit complete — Codex

For each completed batch, verify:

1. Sentence IDs are complete, unique, and in source order.
2. Every `Original` exactly matches the extracted source.
3. Every item has nonempty `Meaning`, `Structure`, `Chunk`, and `Drill`.
4. Every `Meaning` has HY provenance, not a silent Codex replacement.

5. No batch is left with placeholders such as `[[HY_TRANSLATION_PENDING]]`.
6. Obsidian wikilinks and index navigation work.

Document translation provenance in the index: provider, model, coverage, date, and a few spot-checks.

---

## 7. Obsidian structure

```text
English Learning/note/<Book or Show>/
├── 00 - <Title> Study Index.md
├── _source text/                  # book source
│   └── or _source subtitles/      # TV source
├── Ch01 / S01E01 - High-Value Study Note.md
├── Ch01 / S01E01 - Full Sentence Analysis.md
├── Speaking Routine - Shadowing and Retelling.md
├── Vocabulary and Chunk Deck.md
├── Grammar Watchlist - Tense for Speaking.md
└── translation progress / provenance note
```

The index should link all chapter / episode notes and show whether full-sentence coverage is complete.

---

## 8. How to study a book chapter

1. **Preview — 5 minutes:** read the high-value map and predict the main idea.
2. **Read — 15–25 minutes:** read the original text; mark sentences you cannot explain.
3. **Deep study — 15–20 minutes:** study 10–20 full-sentence entries, not all at once.
4. **Speak — 5 minutes:** retell the chapter in your own words using 3–5 chunks.
5. **Review — 5 minutes:** revisit yesterday’s Drill sentences and record yourself.

For nonfiction books, use present tense for claims and definitions:

- `The author argues that...`
- `The key point is that...`
- `What makes this important is that...`


---

## 9. How to study a TV episode

1. **Watch once without pausing:** follow the plot and emotions.
2. **Replay a short scene:** use English subtitles and notice 5–10 useful lines.
3. **Study the full-sentence entries:** focus on chunks, tone, and references.
4. **Shadow:** repeat one line 5 times: normal speed → slow → normal speed.
5. **Retell:** explain the scene for 45–60 seconds in the past tense.
6. **Transfer:** replace the characters and plot with your own daily-life situation.

Useful TV retelling frames:

- `At first, ... but then ...`
- `He found out that ...`
- `She ended up ...`
- `The problem was that ...`
- `What surprised me was ...`

---

## Non-negotiable quality rules

- Do not label selected examples as full-sentence coverage.
- Do not call a chapter complete until the exact sentence count and verified source-match count are known.
- Do not use ordinary free machine translation as the final Meaning.
- Use Codex for general processing and learning analysis; use HY for the final Chinese Meaning translation.
- Keep copyrighted sources only as local personal-study material and watch video through legitimate sources.

