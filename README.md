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

This project classifies anime and manga recommendation comments from an online anime discussion community. It sorts each comment into one of three categories: title_only, personal_experience, or reasoned_recommendation. The goal is to distinguish comments that only name a title from comments that explain why a person recommends it or describe how it affected them.



---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `title_only`

**Definition:** A comment that recommends one or more anime or manga titles without explaining why.

**Example 1:**
> A Silent Voice

**Example 2:**
> Hikaru no Go for me

### `personal_experience`

**Definition:** A comment that recommends anime or manga by explaining how it personally affected, helped, or motivated the commenter.

**Example 1:**
> Haikyuu was the anime that motivated me the most. The story is a great way to reflect on hard work vs talent and the motivation to be the best no matter your starting point.

**Example 2:**
> Attack on Titan literally changed me as a person and my outlook on life.

### `reasoned_recommendation`

**Definition:** A comment that recommends anime or manga by explaining its themes, tone, story, characters, or why it fits the request.

**Example 1:**
> Berserk is all about the endless struggle against internal and external darkness. It is famously dark and brutal though, but that does elevate its message of hope even higher.

**Example 2:**
> Vinland Saga and Attack on Titan both cover the complexity of human emotions, growth, community, and the battle against yourself while navigating different challenges.


### The hardest boundary

**Which two labels:** `personal_experience` and `reasoned_recommendation`

**The decision rule I used every time:** If a comment includes both personal experience and details about the anime, I label it `personal_experience` when the writer's own life, feelings, or motivation are the main reason for recommending it. Otherwise, I label it `reasoned_recommendation`.


---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** Anime and manga recommendation comments collected from online discussion posts.

**How I labelled them:** I applied the three label definitions and the decision rule for the hardest boundary. I labelled 20 comments cold (without AI assistance). For the remaining comments, AI-assisted pre-labeling or review was used where noted in the `note` column; I reviewed the final labels and corrected them when needed.


**Counts per label:**

| Label | Count | Share |
|---|---:|---:|
| `reasoned_recommendation` | 81 | 40.5% |
| `title_only` | 75 | 37.5% |
| `personal_experience` | 44 | 22.0% |
| **Total** | **200** | **100%** |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* My Hero Academia taught me that hard work can lead to greatness.
>
> *Could have been:* `personal_experience` or `reasoned_recommendation`
>
> *I chose:* `personal_experience`, because the main point is what the commenter personally learned.

**2.**
> *The post:* Spy x Family
>
> *Could have been:* `title_only` or `reasoned_recommendation`
>
> *I chose:* `title_only`, because it gives only a title and no explanation.

**3.**
> *The post:* Watch Frieren because its slow pacing and emotional story make the characters feel realistic.
>
> *Could have been:* `reasoned_recommendation` or `personal_experience`
>
> *I chose:* `reasoned_recommendation`, because it recommends the title using specific qualities of the show rather than the writer's personal experience.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** 3 epochs, learning rate `2e-05`, batch size 16, maximum length 128, and seed 42.

**Device used for training:** CPU (`torch 2.14.1+cpu`)

**Anything I changed from the defaults, and why:** I did not change the default hyperparameters.

**Split sizes:** 139 training examples, 31 validation examples, and 30 test examples. The test split had 12 `reasoned_recommendation` examples, 11 `title_only` examples, and 7 `personal_experience` examples. The small `personal_experience` test group means its F1 score may vary more between seeds.



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* Help checking whether borderline anime comments fit `personal_experience` or `reasoned_recommendation`.
- *What came back:* Explanations of the difference between a comment focused on the writer's own experience and one focused on qualities of an anime.
- *What I changed:* I used that guidance to review the comments myself and applied the decision rule in my taxonomy.

**Moment 2**

- *What I asked for:* Help troubleshooting the CSV validation, Git commands, VS Code Jupyter kernel, and notebook errors.
- *What came back:* Step-by-step troubleshooting guidance.
- *What I changed:* I ran the commands and notebook cells myself, checked the output, and committed the generated project files.

**Pre-labelling disclosure:** Some comments were AI-prelabeled or AI-reviewed, as indicated in the `note` column. I reviewed the labels and corrected them when needed before using the final dataset.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---:|---:|---:|
| Overall accuracy | 0.500 | 0.467 | -0.033 |
| Macro F1 | 0.478 | 0.303 | -0.175 |
| F1 — `personal_experience` | 0.400 | 0.000 | -0.400 |
| F1 — `reasoned_recommendation` | 0.500 | 0.600 | +0.100 |
| F1 — `title_only` | 0.533 | 0.308 | -0.226 |

**What I predicted before I looked:**
I predict that my fine-tuned model will perform better overall than the zero-shot baseline because it trained on 200 anime recommendation comments using my three specific labels. I expect the biggest advantage to be on `reasoned_recommendation`, while `personal_experience` may remain difficult because it had the fewest examples.

**What the gap actually means:** My prediction was partly correct: fine-tuning improved `reasoned_recommendation` from 0.500 to 0.600 F1, but it did not improve the model overall. The zero-shot baseline had higher overall accuracy and macro F1 and performed better on `personal_experience` and `title_only`; the trained model completely missed `personal_experience` on this split, so my 200 labels and/or that label boundary need investigation.



---

## Run Log — Before



| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---:|---:|---:|---|
| 1. Overall accuracy | ≥ 0.70 | 0.467 | 0.733 | 0.733 | MISSED |
| 2. Macro F1 | ≥ 0.67 | 0.303 | 0.567 | 0.567 | MISSED |
| 3. `title_only` F1 | ≥ 0.60 | 0.308 | 0.952 | 0.952 | MISSED |
| 4. Largest label share in `labels.csv` | ≤ 65% | 40.5% | 40.5% | 40.5% | MET |
| 5. Staff-label agreement | ≥ 80% | Pending | Pending | Pending | PENDING |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | reasoned_recommendation | title_only | personal_experience |
|---|---:|---:|---:|
| **reasoned_recommendation** | 12 | 0 | 0 |
| **title_only** | 9 | 2 | 0 |
| **personal_experience** | 7 | 0 | 0 |

**My biggest off-diagonal number, and what it means:** Nine real `title_only` posts were predicted as `reasoned_recommendation`, while zero real `reasoned_recommendation` posts were predicted as `title_only`. This lopsided pattern means the model is over-predicting `reasoned_recommendation` when it should identify a title-only recommendation; the agreement report will help determine whether inconsistent labeling contributed to that boundary problem.



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
