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

**Definition:** A post that evaluates a player, team, or hockey-related situation using supporting statistics, comparisons, evidence, or detailed reasoning.

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

**Counts per label:**

| Label | Count | Share |
|---|---|---|
|  |  |  |
|  |  |  |
|  |  |  |
| **Total** |  | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

**2.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

**3.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:**

**Settings:** <!-- epochs, learning rate, batch size, seed -->

**Anything I changed from the defaults, and why:**

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Moment 2**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Pre-labelling disclosure:**

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `label_one` |  |  |  |
| F1 — `label_two` |  |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



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
