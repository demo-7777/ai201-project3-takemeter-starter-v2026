# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

TakeMeter sorts posts from **HFBoards** (forums.hfboards.com, a public hockey
discussion forum readable without a login) into four labels: `analysis`,
`hot_take`, `reaction`, and `question`. Each individual forum post gets exactly
one label, based on its dominant purpose: arguing from evidence, asserting
without support, venting about a game, or asking the community.

**Milestone 1 — candidate distinctions noticed while reading HFBoards:**

1. Some posts describe a specific play or moment from a game (a goal, save,
   hit, overtime winner). Others talk about players or teams in general.
2. Some posts cite a specific number (points, records, milestones, "first
   since 19XX"). Most don't.
3. Some posts report a transaction (trade, signing, injury) or relay a
   reporter's information. Others report nothing new.
4. Some posts ask the community for an answer, like "who should be in the
   Hall of Fame?" or "will anyone pass X?"



---

## Label Taxonomy

Each example includes a direct excerpt from the author's actual words, along with the thread title, post number, and source link. The label applies to the individual post's content, not the entire discussion thread.

### `analysis`

**Definition:** A post that evaluates a player, team, or hockey related situation using supporting statistics, comparisons, evidence, or detailed reasoning.

**Example 1:**

Thread: 2025/26 Reflections and Ramblings

Author: Brian39  
Post: #1

**Actual post text (excerpt):**

> He was one of the worst goalies in the league this year. Dead last in goals saved above expected

Link: https://forums.hfboards.com/threads/2025-26-reflections-and-ramblings.3028450/

**Example 2:**

Thread: Rumors & Proposals Thread | Deadline Day Edition

Author: McDoused  
Post: #2,730  
Date: March 29, 2026

**Actual post text (excerpt):**

> A 47.27 xGF% for a shutdown pair is excellent. Asking for anymore would be unrealistic.

Link: https://forums.hfboards.com/threads/rumors-proposals-thread-deadline-day-edition.3024438/page-110

### `hot_take`

**Definition:** A post making a confident hockey claim or prediction offered with little or no supporting reasoning.

**Example 1:**

Thread: Your 2025-2026 NHL hot takes

Author: Kent Hughes  
Post: #6

**Actual post text:**

> Tampa Bay won't make the playoffs.  
> Colorado won't make the playoffs.

Link: https://forums.hfboards.com/threads/your-2025-2026-nhl-hot-takes.3011442/

**Example 2:**

Thread: 2025-2026 NHL Predictions

Author: Aashir  
Post: #8

**Actual post text (excerpt):**

> A: Auston Matthews. I think he returns in a big way and scores 55-65G and 105+ points

Link: https://forums.hfboards.com/threads/2025-2026-nhl-predictions.3011343/

### `reaction`

**Definition:** A post primarily expressing an emotional response, such as excitement, frustration, anger, disappointment, or surprise, to a game, performance, or recent hockey event.

**Example 1:**

Thread: Fire the team

Author: Cloned  
Post: #1

**Actual post text (excerpt):**

> This is a circus shitshow.  
> Bouchard playing like Tom Gilbert, Ingram bringing back memories of Ty Conklin

Link: https://forums.hfboards.com/threads/fire-the-team.3028945/

**Example 2:**

Thread: Another Loss

Author: ACC1224  
Post: #1

**Actual post text:**

> BOOOOOOOOOO!!!!!!!

Link: https://forums.hfboards.com/threads/another-loss.3023969/

### `question`

**Definition:** A post whose primary purpose is asking the community for information, opinions, predictions, or answers rather than presenting a personal argument or reaction.

**Example 1:**

Thread: Will any dman surpass Erik Karlssons 101p in the coming seasons?

Author: Janvonpobben  
Post: #1

**Actual post text (excerpt):**

> Will any dman surpass Erik Karlssons 101points in the coming seasons?  
> All-time list btw

Link: https://forums.hfboards.com/threads/will-any-dman-surpass-erik-karlssons-101p-in-the-coming-seasons.3024056/

**Example 2:**

Thread: Which of the follow goalies should be in the hall of fame?

Author: Mitch nylander  
Post: #1

**Actual post text (excerpt):**

> When all is said and done which of this list should be in the hockey hall of fame??

Link: https://forums.hfboards.com/threads/which-of-the-follow-goalies-should-be-in-the-hall-of-fame.3029497/

### Hardest Boundary: `analysis` vs. `hot_take`

**Decision Rule:** When a post contains both an opinion and supporting information, the label depends on whether evidence and reasoning are central to its message. If the author develops an argument using statistics, comparisons, performance evaluations, or other evidence, classify the post as `analysis`, even when its conclusion is controversial. If the post mainly presents a bold prediction or strong opinion with little supporting reasoning, classify it as `hot_take`.

For example, predicting that Colorado will miss the playoffs without providing supporting evidence is a `hot_take`. Arguing that Colorado will miss the playoffs by examining roster depth, defensive statistics, injuries, and player performance is `analysis`.

For other overlapping categories, classify according to the primary communicative purpose of the post. An emotional response to a recent game is `reaction`, even if it includes criticism. A post primarily requesting an answer is `question`, even if it provides statistics or background information. A question mark alone is not sufficient to determine the label.

**General Classification Rule:** Assign exactly one label to each individual post based on its dominant purpose, rather than on the title of the thread in which it appears.



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** All examples were collected from [HFBoards](https://forums.hfboards.com/), a hockey discussion forum. The unit of text used for classification is **one individual forum post**. This can be either the opening post of a thread (#1) or a single reply within an existing thread.

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->
I labeled the first 20 posts cold, by myself, using only my definitions (marked `cold` in the note column). The remaining 181 posts were parsed from HFBoards threads and pre-labeled by AI (marked `AI-assisted`). I then reviewed those labels against my definitions and decision rule and changed some of them, mainly `question` labels. Posts that were borderline or depended on missing context are flagged in the note column.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `reaction` | 62 | 30.8% |
| `analysis` | 53 | 26.4% |
| `hot_take` | 52 | 25.9% |
| `question` | 34 | 16.9% |
| **Total** | 201 | 100% |

No label is above the 70% cap. `question` is the thinnest label.

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* "He's had knee problems dating back to before his draft where he cracked his kneecap horsing around with a teammate in the locker room. I assume kneecaps heal like bones and there is minimal to no post recovery complications but for someone who's effectiveness relies on his skating and edgework this blows. Ritchie 2C?"
>
> *Could have been:* `analysis` or `question`
>
> *I chose, because:* `question`. Most of the post is injury history and reasoning, which reads like `analysis`, but it ends by asking the community a lineup question ("Ritchie 2C?"). Under my rule, a post that asks the community for an answer is `question` even if it includes background. At first I read only the first part and thought it was `analysis`, which shows how much the last line decides this one.

**2.**
> *The post:* "Did anyone watch the film? I'm watching it now and, I'm kinda disappointed in it. It's very quick, there's next to no mention of the regular season, and very little about the first 3 rounds. It mentions the ECF struggles, and Rods time as captain and 2006, but it's not what the 2006 film was or others that were released in that time frame. It basically gets right into the Final, and even then there's some recycled material from "Quest for the Cup". I feel there's a lot that is missed. Particularly the years after the Cup, the transfer of ownership from Karmanos to Dundon and the hiring of Rod shortly after, and the build-up to this season."
>
> *Could have been:* `question` or `reaction`
>
> *I chose, because:* `analysis`. It opens with a question ("Did anyone watch the film?") and expresses disappointment, so it could be read as `question` or `reaction`. But the opening question only introduces the writer's own review. It doesn't ask the community for an answer the way "Ritchie 2C?" does in hard case #1. Most of the post supports the disappointment with specific evidence (what the film covers, what it skips, the comparison to the 2006 film), so under my dominant-purpose rule it's `analysis`.

**3.**
> *The post:* "Panarin? Leadership? WTF are these clowns even talking about?"
>
> *Could have been:* `question` or `reaction`
>
> *I chose, because:* `reaction`. Every sentence ends in a question mark, but the writer isn't asking the community for an answer. The questions express disbelief and frustration at what commentators said. Under my rule, a question mark alone doesn't make a post `question`. The dominant purpose here is venting, so it's `reaction`.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** <!-- epochs, learning rate, batch size, seed -->
3 epochs · learning rate 2e-5 · batch size 16 · max length 128 tokens · seed 42

**Device:** cpu (torch 2.14.1+cu130), as recorded in `results.json`. Results can differ slightly on other hardware with the same seed.

**Anything I changed from the defaults, and why:** Nothing. All settings are the notebook defaults. The only edit was setting `LABELS` to my four labels.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->

train 139 · val 31 · test 31

| Label | Train | Val | Test |
|---|---|---|---|
| `analysis` | 37 | 8 | 8 |
| `hot_take` | 36 | 8 | 8 |
| `reaction` | 42 | 10 | 10 |
| `question` | 24 | 5 | 5 |

- `question` has only **5 examples in the test split** (below the ~8 threshold). Its F1, and criterion #5, will move a lot between seeds.


---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* I used AI to parse posts from HFBoards threads and write them into `labels.csv` with a suggested label and a note.
- *What came back:* Rows in `text,label,note` format. Quoted replies were stripped out, and borderline or context-dependent posts were flagged in the note column (e.g. "borderline analysis/hot_take", "context-dependent").
- *What I changed:* I reviewed the suggested labels against my definitions and decision rule, and changed some of the `question` labels the AI assigned.

**Moment 2**

- *What I asked for:* I used AI to review errors I hit while running cells in `takemeter.ipynb`.
- *What came back:* For example, section 5 failed with `NameError: name 'DEVICE' is not defined`. The explanation was that section 1's first cell hadn't run in the current kernel session.
- *What I changed:* I reran sections 1–5 in order, and section 5 then wrote `results.json` and `test_split.csv`.

**Pre-labelling disclosure:** 20 rows were labeled cold by me with no tools (marked `cold` in the note column). The other 181 rows were pre-labeled by AI (marked `AI-assisted` in the note column) and then reviewed by me.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.290 | 0.516 | +0.226 |
| Macro F1 | 0.201 | 0.322 | +0.121 |
| F1 — `analysis` | 0.471 | 0.600 | +0.129 |
| F1 — `hot_take` | 0.000 | 0.000 | +0.000 |
| F1 — `question` | 0.333 | 0.000 | -0.333 |
| F1 — `reaction` | 0.000 | 0.690 | +0.690 |

Baseline: `facebook/bart-large-mnli` zero-shot, using my definitions from `label_definitions.txt`, scored on the same 31 posts in `test_split.csv` (seed 42), on cpu.

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->
Looking at the baseline's performance, I predict the trained model will be able to catch `hot_take` and `reaction` posts (where the baseline scored 0.000 F1), and will be most accurate at finding `question` posts.
     

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->
The trained model had mixed results across catching multiple labels, in some cases improving accuracy, decreasing accuracy, or continuing to not catch a label. It improved on `reaction` (0.000 → 0.690) and `analysis` (0.471 → 0.600), got worse on `question` (0.333 → 0.000), and still caught no `hot_take` posts (0.000 in both). Overall accuracy rose from 0.290 to 0.516, but almost all of that gain came from `reaction`.



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
