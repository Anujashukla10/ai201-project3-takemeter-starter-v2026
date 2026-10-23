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

## 1. Overall accuracy

Overall accuracy on the held-out test split is at least 0.65 on all three seeds (42, 7, 2024).

**Why this target:** There are three labels, so random guessing has an expected accuracy of about 0.33. I chose 0.65 because the model should perform substantially better than guessing. Gaming posts can mix arguments with strong opinions, so distinguishing `analysis` from `hot_take` may be difficult. I also want a target that is challenging without expecting near-perfect predictions from only about 200 posts.

---

## 2. Per-label performance

Every label (`analysis`, `hot_take`, `reaction`) has an F1 score of at least 0.50 on the held-out test split, on all three seeds (42, 7, 2024).

**Why this target:** `analysis` may be the least common label because fewer gaming comments provide a specific, checkable fact (a price, date, number, or patch note) than simply express an opinion or reaction. With only around 30 test posts, a few classification errors can significantly affect a label's F1 score. A minimum of 0.50 requires the model to recognize every label reasonably well rather than succeeding mainly on the easier `reaction` posts.

---

## 3. Balance

In `labels.csv`, no label accounts for more than 45% of the rows, and no label accounts for less than 20%.

**Why this target:** Gaming discussions often contain many short reactions and confident opinions, while detailed analysis may be less common. If one label dominates the dataset, the model could learn to favor that label instead of learning meaningful differences between all three. These limits aim to provide enough examples of `analysis`, `hot_take`, and `reaction` to train and evaluate the classifier.

---

## 4. Consistency

My agreement rate with the staff labels on the 30 staff posts, calculated using `agreement.py`, is at least 75%.

**Why this target:** The distinction between `analysis` and `hot_take` can be difficult because a confident gaming opinion may include a statistic, a specific game mechanic, or a reference to a match without actually supporting its argument. I chose 75% because I want to apply the labeling rules consistently while allowing for genuinely ambiguous posts. The staff posts come from a running forum, but staff rule 1 (does the fact carry the argument?) is the same boundary as my `analysis` vs `hot_take` rule, so this checks whether I apply that kind of rule consistently. If my agreement is lower, my own 200 labels are probably inconsistent on that boundary too.

---

## 5. Confidence

`accuracy_most_confident_third` minus `accuracy_least_confident_third` in `results.json` is at least 0.15 on all three seeds (42, 7, 2024).

**Why this target:** I want the model's confidence scores to provide useful information about the reliability of its predictions. Short `reaction` posts may contain obvious emotional language, while `analysis` and `hot_take` can overlap in gaming discussions. A difference of at least 0.15 would indicate that the model performs noticeably better on its most-confident predictions than on its least-confident predictions. 


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
