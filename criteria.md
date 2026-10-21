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

Accuracy and F1 are decimals from 0 to 1. Balance and agreement are
percentages. Unless a criterion says otherwise, "held-out test set" means the
15% test split the notebook makes, and every model criterion must hold on
**all three seeds** (42, 7, 2024) to count as met.

## 1. Overall accuracy

On the held-out test set, the classifier's accuracy (correct predictions ÷
test posts, the `accuracy` value in `results.json`) will be **at least 0.70**.

**Why this target:** My labels are uneven. `resource_promotion` is 58% of the
data, so a model that always guesses it scores about 0.57 on my 30-post test
split (17/30). Random guessing would score about 0.33. To be worth using, the
model has to beat the "always resource_promotion" guesser by a clear margin.
0.70 is 21 of 30 correct, about four more posts than that guesser. I didn't go
higher because the test set is only 30 posts and the target has to hold on all
three seeds.

---

## 2. Per-label performance

On the held-out test set, **each** of `technical_explanation`,
`resource_promotion`, and `discussion_opinion` will have an F1 of **at least
0.60** (the `per_label` → `f1` values in `results.json`).

**Why this target:** A high overall accuracy can hide a model that mostly
predicts `resource_promotion` and almost never predicts `discussion_opinion`.
A per-label floor catches that. I kept the same 0.60 for every label, including
`discussion_opinion`, even though it has only 24 examples and about 4 in the
test split, where one mistake moves its recall by 0.25. That makes this the
criterion most likely to miss, and I'm keeping it strict on purpose: opinion
posts are the most distinct kind of post in this community, and a model that
can't find them isn't doing the job.

---

## 3. Dataset balance

In the final `labels.csv`, every label will make up **at least 10%** and **no
more than 60%** of the rows (label count ÷ total rows).

**Why this target:** Most posts in this LinkedIn group are link shares, so
`resource_promotion` naturally dominates (currently 58%). A 60% cap, stricter
than the assignment's 70%, stops it from growing to the point where the model
learns to guess it. A 10% floor keeps `discussion_opinion` (currently 12%) big
enough to show up in the test split at all. If I add posts and break either
bound, I have to collect more of the thin label.

---

## 4. Labeling consistency

On the 30 staff posts in `data/staff_posts.csv`, which I label myself using the
staff taxonomy in `data/staff_taxonomy.md`, my labels will exactly match the
staff labels on **at least 80%** of posts (24 of 30). This is measured by
`agreement.py` as matching labels ÷ 30.

**Why this target:** My own 200 labels have no second labeller, so this is the
only check on whether I apply written definitions consistently. Most of my
data was pre-labeled by AI and then reviewed by me, which makes that check
matter more. Missing 1 in 5 posts would mean I bend rules I didn't write, and
I'd probably bend my own boundary (technical_explanation vs.
resource_promotion) the same way. I chose 80% rather than something higher
because some staff posts were picked to be ambiguous.

---

## 5. Confidence

On the held-out test set, the most-confident third of predictions (10 of 30)
will have an accuracy of **at least 0.80**, and that accuracy will be **at
least 0.15 higher** than the accuracy of the least-confident third. Measured
with `accuracy_most_confident_third` and `accuracy_least_confident_third` in
the `confidence` section of `results.json`.

**Why this target:** In this community, the clear cases look different from
the hard ones. Bare "title + link" posts should be easy, while blog teasers
that explain a little and then link are my hardest boundary. If the model's
confidence means anything, it should be right on at least 8 of its 10 surest
calls, and noticeably less accurate on its 10 least-sure calls. That way a
reader could trust a confident label and send the unsure ones for a human
check. A confidence score is not a guarantee, so if there's no gap between the
two thirds, I'll report that the model's confidence tells us nothing.


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
