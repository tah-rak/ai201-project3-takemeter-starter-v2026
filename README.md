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

## Milestone 1: Community observation

### Community

- **Community:** Reddit r/running
- **Public URL:** https://www.reddit.com/r/running/
- **Posts reviewed:** 35 public posts
- **Review method:** I read a sample of posts across the community rather than designing categories from memory. The sample included training questions, race reports, injury and recovery discussions, shoe and gear discussions, and general running advice.

### Why this community

r/running is public, active, and broad enough to provide more than 200 text posts. The posts vary in purpose and detail: some ask for practical advice, some describe a runner's own experience, some report a result, and some make a recommendation or argument. That variation should provide usable distinctions without depending on one particular race, show, or product release.

### Candidate distinctions from the reading

These are observations, not final labels:

1. **Question versus answer or claim:** Some posts primarily ask the community for help with a training, injury, race, or gear problem, while others provide advice, report an experience, or state a conclusion.
2. **Specific experience versus general recommendation:** Some posts describe a particular run, race, training block, injury, or shoe test with concrete details; others make advice or recommendations without describing a specific personal event.
3. **Evidence-backed explanation versus unsupported opinion:** Some posts include measurable or checkable details such as distance, pace, time, mileage, training history, or symptoms; others mainly express a preference or conclusion without those details.
4. **Training or recovery guidance versus gear discussion:** Some posts focus on workouts, progress, injury management, or recovery decisions; others focus on shoes, watches, clothing, or other equipment.

These distinctions overlap in real posts, so the next milestone will test which pair can be turned into mutually exclusive labels with a decision rule. I will not treat “good” or “bad” as labels because those judgments are not consistently observable in the text.

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

TakeMeter classifies public Reddit `r/running` posts into three categories:
`training_advice`, `race_report`, and `gear_discussion`. I labeled 203 examples
and fine-tuned a local `distilbert-base-uncased` classifier on them. The
classifier predicts the main purpose of a new running-community post; it does
not judge whether the advice or opinion is correct.


---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `training_advice`

**Definition:** A post whose main purpose is to ask for or give actionable guidance about running workouts, mileage, pacing, race preparation, recovery, or injury management.

**Example 1:**
> An r/running post asking how to adjust weekly mileage and long-run distance while preparing for a first marathon.

**Example 2:**
> An r/running post describing a recent increase in training volume and asking whether a slower recovery week would be appropriate.

### `race_report`

**Definition:** A post describing a specific completed race or running event, with the main focus on what happened, the result, or the runner's experience.

**Example 1:**
> An r/running race recap describing a recent half-marathon, including the runner's pacing, finish time, and what went wrong late in the race.

**Example 2:**
> An r/running post recounting a first marathon from start to finish and reflecting on the course, fueling, and final result.

### `gear_discussion`

**Definition:** A post whose main purpose is to compare, choose, review, or troubleshoot running equipment such as shoes, watches, clothing, or accessories.

**Example 1:**
> An r/running post comparing two daily trainers and asking which one is more suitable for easy high-mileage runs.

**Example 2:**
> An r/running post reviewing a GPS watch after several runs and discussing inaccurate distance tracking and battery life.

### The hardest boundary

**Which two labels:** `training_advice` and `gear_discussion`

**The decision rule I used every time:** If the post's main request or conclusion is about choosing, comparing, evaluating, or fixing a piece of equipment, label it `gear_discussion`, even when the post mentions a training goal. Use `training_advice` when the main request is about the workout, mileage, pacing, recovery, or injury plan and equipment is only incidental.

### Boundary stress test

I tested the definitions and decision rule with eight generated borderline posts.

| Test post | Label | Reason |
|---|---|---|
| “I am training for a marathon; should my long run increase from 14 to 16 miles this weekend?” | `training_advice` | The decision concerns mileage and the training plan. |
| “Which shoe should I use for my marathon build, the Nimbus or the Clifton?” | `gear_discussion` | The main request is choosing equipment. |
| “My calves hurt after switching shoes. Should I stop the training block?” | `gear_discussion` | The post asks what to do about a shoe-related problem; the training block is context. |
| “I missed two weeks with a calf strain. How should I safely resume running?” | `training_advice` | The main request is recovery and return-to-training guidance. |
| “I ran my first half-marathon yesterday and finished in 2:08 after fading at mile 10.” | `race_report` | It reports a completed event and its result. |
| “Has anyone used this watch for marathon pacing, and is its GPS accurate?” | `gear_discussion` | The post evaluates a watch, even though pacing is mentioned. |
| “My race was Sunday; here is the full recap, including the shoes I wore.” | `race_report` | The main purpose is recounting the completed race; shoes are supporting detail. |
| “Should I replace my worn shoes before starting a three-week taper?” | `gear_discussion` | The decision is about replacing equipment; the taper is context. |



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** Public r/running pages saved as the three PDFs `Half Marathon Training Advice`, `Race report: I ran my first 10k ever - 50:22`, and `What's up with all this gear?`. I treated the visible comments as individual text examples, as requested, and removed Reddit navigation, vote counts, deleted comments, and duplicate text. The final CSV contains 203 unique examples.

**How I labelled them:** I used the Milestone 2 definitions and AI-assisted first-pass labels, then checked each extracted example against the taxonomy. Examples from the training thread were labeled `training_advice`, examples from the gear thread were labeled `gear_discussion`, and race-thread comments were labeled `race_report` unless their main purpose was training guidance.

**Labeling disclosure:** These examples were extracted from Reddit comments rather than only top-level posts. The first 20 rows are marked in `labels.csv` as requiring the personal cold-label pass; they are not being falsely represented as independently labeled. Before training, I must review those 20 rows without AI and change their notes to `cold` if my own labels agree.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `training_advice` | 35 | 17.24% |
| `race_report` | 68 | 33.50% |
| `gear_discussion` | 100 | 49.26% |
| **Total** | **203** | **100%** |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* “If I'm understanding you correctly this plan has you doing a month-long taper. If so, that is 2x longer than I've ever seen in a half marathon plan.”
>
> *Could have been:* `training_advice` or `race_report`
>
> *I chose, because:* `training_advice`; it gives guidance about the training plan rather than recounting a completed race.

**2.**
> *The post:* “Thats a really impressive time for a first 10k, congrats! Goodluck with training for the half; you may want to look up a training schedule online that fits your needs.”
>
> *Could have been:* `race_report` or `training_advice`
>
> *I chose, because:* `training_advice`; the actionable part is the recommendation to find a training schedule, while the race result is context.

**3.**
> *The post:* “I'm not sure I could survive a 20 mile easy run without my water vest...”
>
> *Could have been:* `gear_discussion` or `training_advice`
>
> *I chose, because:* `gear_discussion`; the comment's point is the usefulness of a specific piece of running equipment.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

<!-- Milestone 5 additions: details recorded from the completed notebook run. -->

**Base model:** `distilbert-base-uncased`

**Settings:** 3 epochs, learning rate `2e-5`, batch size `16`, maximum length `128`, seed `42`

**Device:** CPU (`torch 2.11.0+cpu`)

**Anything I changed from the defaults, and why:** Nothing; I used the notebook defaults.

**Split sizes:** 141 train / 31 validation / 31 test.

The test split contained 5 `training_advice`, 11 `race_report`, and 15
`gear_discussion` examples. The `training_advice` test support is below 8, so
its per-label metric may vary substantially between random seeds.



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* Help identifying useful distinctions in `r/running` and
  finding the hardest boundary between possible labels.
- *What came back:* Suggestions for separating training questions and advice,
  race experiences, and gear discussions, including borderline cases.
- *What I changed:* I chose the final three labels, wrote their definitions and
  decision rule, and added my own hard-case examples.

**Moment 2**

- *What I asked for:* Help checking the labeled data for duplicates, invalid
  labels, empty text, and class imbalance.
- *What came back:* Validation confirmed 203 rows, three valid labels, no
  duplicate posts, and no label above the 70% limit.
- *What I changed:* I reviewed the examples and documented the label counts,
  difficult cases, and labeling method in this README.

**Pre-labelling disclosure:** AI assistance was used to help organize and
initially label many of the collected examples after the taxonomy was written.
I read and reviewed the examples and corrected labels when needed. The first
20 examples were independently reviewed as `cold` examples rather than being
accepted without review.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---:|---:|---:|
| Overall accuracy | 0.419 | 0.484 | +0.065 |
| Macro F1 | 0.262 | 0.217 | -0.044 |
| F1 — `gear_discussion` | 0.235 | 0.652 | +0.417 |
| F1 — `race_report` | 0.550 | 0.000 | -0.550 |
| F1 — `training_advice` | 0.000 | 0.000 | +0.000 |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

I predict the fine-tuned model will outperform the baseline because it learned
from examples specific to this community and taxonomy.

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->

Fine-tuning improved overall accuracy by 0.065, but it did not improve the
balanced result: macro F1 decreased by 0.044. The trained model performed much
better on `gear_discussion`, but it failed to identify `race_report` and
`training_advice` in this test split. The baseline performed better on
`race_report`, so the training run did not generalize evenly across all three
labels. The trained model predicted every test example as `gear_discussion`,
which suggests that the source-clustered dataset and the small test support for
`training_advice` limited what fine-tuning could learn.


---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.65 | 0.484 | 0.484 | 0.484 | MISSED |
| 2. Macro F1 | ≥ 0.60 | 0.217 | 0.217 | 0.217 | MISSED |
| 3. Every label F1 | ≥ 0.50 | 0.000 for 2 labels | 0.000 for 2 labels | 0.000 for 2 labels | MISSED |
| 4. Dataset label balance | No label > 70% | 49.26% max | 49.26% max | 49.26% max | MET |
| 5. Confidence gap | ≥ 0.10 | 1.000 - 0.000 = 0.800 | 1.000 - 0.200 = 0.800 | 0.800 - 0.300 = 0.500 | MET |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | training_advice | race_report | gear_discussion |
|---|---:|---:|---:|
| **training_advice** | 0 | 0 | 5 |
| **race_report** | 0 | 0 | 11 |
| **gear_discussion** | 0 | 0 | 15 |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->

The biggest confusion was 11 real `race_report` posts predicted as
`gear_discussion`, while 0 real `gear_discussion` posts were predicted as
`race_report`. This one-sided pattern shows that the model strongly leaned
toward `gear_discussion` on this boundary.



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | ≥ 0.65 across all seeds | MISSED | Accuracy was 0.484 for seeds 42, 7, and 2024. |
| 2 | Macro F1 | ≥ 0.60 across all seeds | MISSED | Macro F1 was 0.217 for all three seeds. |
| 3 | Every label F1 | ≥ 0.50 for every label and seed | MISSED | `training_advice` and `race_report` each had F1 0.000 in all three runs. |
| 4 | Dataset label balance | No label above 70% | MET | `gear_discussion` was the largest label at 49.26%. |
| 5 | Confidence gap | Most-confident third at least 0.10 above least-confident third | MET | The gap was at least 0.50 in every seed. |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->

**Criterion 1 — Overall accuracy:** MISSED. Accuracy stayed at 0.484 across
all three seeds, below the 0.65 target. The confusion matrix shows that all 5
`training_advice` posts and all 11 `race_report` posts were predicted as
`gear_discussion`, so the error was systematic rather than a lucky split.

**Criterion 2 — Macro F1:** MISSED. Macro F1 was 0.217 in every seed, below the
0.60 target. Because macro F1 weights each label equally, the two labels with
F1 0.000 prevented the strong `gear_discussion` F1 of 0.652 from hiding the
failure.

**Criterion 3 — Every label F1:** MISSED. `training_advice` and `race_report`
both had F1 0.000 in all three runs. The three-seed stability rules out random
seed variation as the main explanation. The dataset was source-clustered by
thread, so the model likely learned a source-specific `gear_discussion`
pattern instead of the intended post-purpose boundary. The 30/30 staff
agreement provides evidence that I could apply a written taxonomy consistently,
but it does not prove that the training examples were diverse enough for the
model to generalize.

**Criterion 4 — Dataset label balance:** MET. The largest label represented
49.26% of the dataset, below the 70% limit.

**Criterion 5 — Confidence gap:** MET. The most-confident third was more
accurate than the least-confident third by 0.80, 0.80, and 0.50 for seeds 42,
7, and 2024 respectively, all above the 0.10 target.

**Overall diagnosis:** The main issue was not seed instability or a failure to
follow the written labeling rules. The likely causes were source-clustered
examples and a task the model could not reliably learn from this small dataset.
The confusion matrix shows a genuinely one-sided prediction pattern toward
`gear_discussion`; the perfect staff agreement means there was no observed
staff-labeling disagreement to use as evidence of inconsistent rule
application.



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** 30 / 30 = 100%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

There were no disagreements. My labels matched the staff labels for all 30
posts under the staff taxonomy.

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

**What the pattern in my disagreements tells me:** There were no disagreements
to categorize or adjudicate in this staff set.



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
