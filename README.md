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
Sorts text posts from r/airplanechefs (a subreddit for the mobile game Airplane Chefs) by 4 categories: 
- sharing something others can use, 
- complaints about the game, 
- celebrating progress,
- asking for help. 



---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `tip`

**Definition:** Shares specific and helpful information about the game that other players can use, such as level costs and priority upgrades.

**Example 1:**
> Hit and trial findings for daily challenge - Serve 100 Bowls of Beans in London — Did a first Serve 100 daily challenge today and thought this might help someone. You end up serving these many bowls on the following levels - 46 - 10, 47 - 12, 48 - 19, 49 - 13, 50 - 9. Obviously Lvl 48 is the best so far.

**Example 2:**
> Serve 100 Creme Brulees in Paris — Just thought to share. I did level 49 twice so i can confirm if it's really 19

### `complaint`

**Definition:** Complains about the game/game developers/ads/price/difficulty.

**Example 1:**
> I wish the game wasn't so money dependent — it seems like to do good in this game u gotta spend money on everything, from needing gems to upgrade and needing fuel because it takes so long to replenish. and this isn't really a problem for me but I think it's unfair for ppl who enjoy the game but don't necessarily have that ability to spend a lot of money

**Example 2:**
> Does anybody else feel that the daily tasks are getting worse day by day? — Why would I play 7 levels just to get coins?


### `milestone`

**Definition:** Annouces personal achievement on the game.

**Example 1:**
> finished every airport — i've 3-starred every airport, now I don't know what to do with my life. lmao just kidding. but i really hope they make a new airport soon. [...] i'm proud i finished this game without buying anything

**Example 2:**
> All airports done ✅ What to do now? — I'm done with all the airports. I don't know what to do with my life now 🥲

### `question`

**Definition:** Needs an answer from other players to proceed, such as why something is broken or how to do something.

**Example 1:**
> Why isn't my bacon fryer working? — I can do a first round but not a second it's so annoying and I don't have enough gems to upgrade it was fine before upgrading is this a glitch?

**Example 2:**
> What's wrong with the game today? — Everytime i select an airport my game crashes. Does anyone experience this at the moment?

### The hardest boundary

**Which two labels:** `complaint` vs. `question`, since complaints are often phrased as questions ("?")

**The decision rule I used every time:** 
- If the post would still make sense without the question then it's `complaint`. 
- If it needs a reply to be complete, it's `question`. 
- Example: "Do they really think we have money like that??! - What's up with these prices on airplane chefs?? I dont mind spending $5 bucks [...] but this is ridiculous!" These are framed as questions but OP doesn't actually need an answer. "Why isn't my bacon fryer working?" can't be resolved without an answer, so it's a `question`.
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** r/airplanechefs, a public subreddit for the mobile cooking game Airplane Chefs. I downloaded Reddit's public JSON listing for the sub (300 posts) and kept the only text posts which trims the total down to 212.

**How I labelled them:** I labelled 22 posts cold first and pre-labelled the other 190 with Claude, given my definitions and boundary rules. I then review all the pre-labelled posts and have the AI flag hard cases to tighten the rules.
<!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `complaint` | 69 | 32.5% |
| `question` | 63 | 29.7% |
| `tip` | 58 | 27.4% |
| `milestone` | 22 | 10.4% |
| **Total** | 212 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* "I really enjoy this game but…" — "I enjoy playing this game a lot.. when I bought Los Angeles map I liked it at first but then it became too much to do for a while so I stopped playing and moved on to another world.. does anyone else feel like this or is is just only me"
>
> *Could have been:* `question` or `complaint`
>
> *I chose `complaint`, because:* it ends with a question mark, but "does anyone else feel like this" only invites agreement. The post makes its point with the question removed, so it's a `complaint`.

**2.**
> *The post:* "We need to report them to the App Store!!" — "Everyone that isn't getting rewards after watching ads should report them on the App Store. Nordcurrent obviously doesn't care but the App Store might. Go to the App Store app look up airplane chef, scroll down and press report a problem."
>
> *Could have been:* `tip` or `complaint`
>
> *I chose `tip`, because:* it gives concrete steps another player can follow, even though it's driven by a complaint about the ads.

**3.**
> *The post:* "Finally reached 150 smiles on a level!" — "Location: Singapore (Level 50) I have the first class seats here btw, though they aren't fully upgraded"
>
> *Could have been:* `milestone` or `tip`
>
> *I chose `tip`, because:* the title is an achievement, but the level and the seat upgrades are details another player could use to repeat it.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** 3 epochs, learning rate 2e-5, batch size 16, seed 42, max length 128 tokens<!-- epochs, learning rate, batch size, seed -->

**Anything I changed from the defaults, and why:** None

**Split sizes:** train 148, val 32, test 32 (70/15/15). `milestone` has only 3 examples in the test split.
<!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->

| Label | Train | Val | Test |
|---|---|---|---|
| `tip` | 40 | 9 | 9 |
| `complaint` | 48 | 11 | 10 |
| `milestone` | 16 | 3 | 3 |
| `question` | 44 | 9 | 10 |


---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1: acceptance criteria**

- *What I asked for:* Help with choosing criteria 4 and 5.
- *What came back:* A draft with numbers and reasons for each. Criterion 4 was a self-consistency check (re-label 30 posts a week later), and criterion 5 was a test of whether the model's high-confidence predictions are right more often.
- *What I changed:* I thought both were too complicated for 200 posts so I replaced them. Criterion 4 became "beats the always-guess-the-biggest-label baseline by 15 points", and criterion 5 became a cap on `question` vs. `complaint` confusion (hardest boundary).

**Moment 2: checking my cold labels**

- *What I asked for:* I sent my 22 cold-labelled rows and asked Claude to pre-label the rest with my existing definitions and rules.
- *What came back:* 190 rows labelled and marked with "ai-label".
- *What I changed:* I reviewed the pre-labelled rows and corrected its decision on about 15 posts + tightened the rules.


**Pre-labelling disclosure:** I also asked Claude to flag posts that genuinely don't fall into any of the 4 labels as "hard case". They were either official game announcements, jokes, discussion prompts, or can fit 2 labels equally. I'm not sure if I should drop these from the dataset or allow for some weaker labelling.

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
