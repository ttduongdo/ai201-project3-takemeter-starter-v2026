# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1. Overall accuracy is at least 0.65 on the held-out set.

<!-- Your criterion. It must name a number. -->



**Why this target:** There are 4 labels (`tip`, `complaint`, `milestone`, `question`) so randomly guessing gets ~25%. I would only consider a post sorter useful if it labels right 6.5 times out of 10.



---

## 2. Every label has an F1 of at least 0.60 on the held-out set.

<!-- Your criterion. -->



**Why this target:** A 20% test set split of 200 posts is only about 40 posts (around 10 per label) so one wrong prediction moves a label's F1 a lot. 0.60 is a fair F1 threshold to hold across seeds.



---

## 3. No label is more than 50% of the dataset, and none is less than 10% (at least 20 posts).

<!-- Your criterion. -->



**Why this target:** I think 50% is a tighter bar than 70% and more realistic for this sub. The 10% floor is so the smallest label has enough examples to learn and to appear in the held-out set at all.



---

## 4. Accuracy beats the share of the largest label by at least 15 points.

<!-- Your criterion. -->



**Why this target:** If a certain label is a large share of the dataset, the model needs to perform substantially better than just guessing the most common label.



---

## 5. At most 25% of true `question` posts are predicted as `complaint`, and at most 25% of true `complaint` posts are predicted as `question`.

<!-- Your criterion. -->



**Why this target:** `complaint` vs. `question` is the hardest boundary in my taxonomy, and there is a specific rule for it. If the model confuses them more than 1/4 of the time, either the rule isn't learnable from this data or my labels aren't consistent. With only about 10 posts per label in the held-out set, 25% is 2–3 posts.



---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
