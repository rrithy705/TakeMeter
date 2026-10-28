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

TakeMeter classifies posts from a LinkedIn group about Java and software
engineering by their main purpose. Each post gets one of three labels:
`technical_explanation` (it teaches a concept in the post itself),
`resource_promotion` (it points readers to an external article, video, course,
or tool), or `discussion_opinion` (it shares a viewpoint or experience, or
invites discussion).

### Community

I am focusing on Java and software engineering discussions in the LinkedIn
group Java Developers Community (group 25827):
https://www.linkedin.com/groups/25827/

**Group name:** Java Developers Community (LinkedIn)

**Public access:** _TODO: confirm that this community is public and does not
need an invitation or membership approval to view its posts. When I checked,
the group link redirected to a sign-in page, so I have not yet confirmed that
it qualifies._

### Initial observations

The posts I collected suggest three distinctions:

1. Some posts explain technical concepts in the post itself, such as
   object-oriented programming and Spring Boot.
2. Some posts mainly send readers to an external tutorial, article, video,
   course, or tool.
3. Some posts mainly share a viewpoint or experience, or invite discussion
   about software development.

These distinctions fit this collection better than the categories I started
with (personal life, informational, achievements). Many posts share
information, while personal-life updates and achievement announcements are
less common.

**Independent reading:** _Draft from a read of all 200 collected posts.
TODO: check it against my own 30–40 posts and change what doesn't match._

- **Most posts are links.** Many posts are just an article title and a
  `lnkd.in` link ("How Volatile Actually Works in Java", "6 Ways to Create Java
  Singleton"). The title often sounds like a lesson, but the lesson is behind
  the link.
- **"Article teaser" posts are the most common gray area.** An author
  summarizes their own blog post in a few bullet points, then links to it.
  Some of these summaries teach the idea and some only list topics.
- **Teaching posts follow a pattern.** "Hello Everyone👋👋 What is X?" posts,
  emoji checklists, and interview-question posts explain a concept in a few
  lines.
- **Opinion posts are the smallest group.** They are career advice, "AI vs.
  developers" takes, and short questions to the group ("Which Java feature has
  improved your day-to-day coding the most?").
- **News clusters.** The Java 27 release produced about ten posts in a row.
  Some of them explain the changes and others link to articles about them.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

| Label | Main purpose |
|---|---|
| `technical_explanation` | Teaches a technical concept within the post |
| `resource_promotion` | Directs readers to an article, video, course, or tool |
| `discussion_opinion` | Shares a viewpoint or experience, or invites discussion |

### `technical_explanation`

**Definition:** Primarily explains a technical concept, mechanism, or
procedure directly in the post.

**Example 1:** (original post 40)
> Hello Everyone👋👋
>
> What is object-oriented paradigm?
>
> Object-oriented paradigm is a programming paradigm based on objects having
> data and methods defined in the class to which it belongs. It aims to
> incorporate modularity and reusability, focusing on data with methods to
> operate upon the objects data.

**Example 2:** (original post 90)
> Do you know that you can use Spring Boot without an HTTP server?
>
> The Spring framework and Spring Boot can be used completely independent of
> an HTTP server.
>
> At its core, Spring 🍃 is simply an Inversion of Control (IoC) and
> Dependency Injection container designed to manage Java objects (beans 🫘)
> and their lifecycles.
>
> Here are the most common ways Spring is used without an HTTP server:
>
> ✅ Command Line Applications (CLI): To wire up dependencies for a standalone
> terminal application, script, or utility.
>
> ✅ Batch Processing: To process large volumes of records, generate overnight
> reports, or migrate data between databases without any web interface.
>
> ✅ Message-Driven Workers: To consume and process messages from brokers like
> Kafka, RabbitMQ, or AWS SQS.
>
> ✅ Scheduled Tasks: Background workers that execute periodic jobs using
> Spring's @Scheduled annotation.
>
> Let me know if you're using Spring for something else!

### `resource_promotion`

**Definition:** Primarily introduces or directs readers to an external
article, video, course, or tool.

**Example 1:** (original post 21)
> REST API Tutorial: Resources, HTTP Methods, Status Codes and a Spring Boot
> Example
> https://lnkd.in/ebRbRY7M

**Example 2:** (original post 28)
> SOLID Principles in Java Explained with Example
> https://lnkd.in/gHcMWwJC

### `discussion_opinion`

**Definition:** Primarily expresses a viewpoint, reflects on an experience, or
invites discussion without mainly explaining a technical concept or promoting
a resource.

**Example 1:** (original post 48)
> Java is not dead.
>
> Your bank still runs on it.
>
> Every year someone says Java is finished.
> Every year it is still in the job posts.
>
> New languages win the headlines.
> Java keeps the systems that cannot go down.
>
> → Banks - payments and core systems
> → Big tech - backend services at huge scale
> → Android - years of apps still in production
> → Spring Boot - the default choice for enterprise backends
> → Modern Java - records, virtual threads and a new release every six months
> → The ecosystem - a library and an answer for almost every problem
>
> Companies do not rewrite working systems because a new language is
> trending. They hire people who can keep them running and make them better.
>
> Hype moves fast.
> Production moves slowly.
> That gap is where the jobs are.
>
> Takeaway: Learn the language companies cannot afford to replace. Then learn
> it properly, not just enough to finish a tutorial.

**Example 2:** (original post 50)
> One thing I find interesting about working on an existing Java application:
>
> Sometimes understanding why the code was written a certain way takes longer
> than understanding what the code does.
>
> There may be business rules, dependencies or old decisions that aren't
> obvious at first.
>
> And changing one thing without understanding the context can create three
> new problems. 😅
>
> Java developers, what usually takes you longer in an existing project —
> understanding the code or understanding the business logic behind it?

### The hardest boundary

**Which two labels:** `technical_explanation` vs. `resource_promotion`. A post
can explain a topic and also link to an article about it.

**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->

If the post teaches a concrete concept or procedure that a reader can
understand without opening the link, it is `technical_explanation`. If it
mainly announces a resource, lists what the resource covers, or requires
opening the resource to learn the explanation, it is `resource_promotion`. A
link alone does not decide the label.

**Secondary rule (`discussion_opinion`):** A question at the end of a teaching
post does not change its label. Choose `discussion_opinion` when expressing a
viewpoint or inviting discussion is the main purpose. When teaching and
opinion overlap, choose `technical_explanation` only if most of the post
explains a concrete mechanism or procedure.

**Borderline example:** Original post 90 explains how Spring Boot can run
without an HTTP server and ends by asking readers how they use Spring. Under
this rule it is `technical_explanation`, because most of the post teaches the
concept. I agree: the closing question is one line, and everything before it
teaches.

### Stress test and boundary review

<!-- Fictional stress-test posts stay out of labels.csv. -->

**Stress-test prompt** (to run before labeling): give an AI tool the three
definitions and decision rules, then ask: "Here are my three label definitions and decision rules.
Write eight fictional software-engineering posts that sit near the boundaries
between the labels. Do not label them. Include posts combining a technical
explanation with a resource link, and posts combining an explanation with an
opinion or discussion question." Then label the eight posts myself and
tighten any unclear boundaries.

**The eight stress-test posts** (fictional, AI-written, not in `labels.csv`):

| # | Post (shortened) | Boundary it tests | My label |
|---|---|---|---|
| 1 | Explains in three lines why `HashMap` isn't thread-safe, then: "Full deep dive with benchmarks here 👉 link" | technical vs. resource | _TODO_ |
| 2 | "My new article covers: ✅ G1 vs ZGC ✅ pause times ✅ tuning flags. Read it here 👉 link" | technical vs. resource | _TODO_ |
| 3 | Walks through how `@Transactional` proxies work, then asks "Have you been bitten by self-invocation?" | technical vs. discussion | _TODO_ |
| 4 | "Hot take: microservices were a mistake for 90% of teams. Monoliths are easier to test, deploy, and reason about. Agree?" | discussion vs. technical | _TODO_ |
| 5 | A quiz: shows a code snippet and asks "What does this print? A, B, C or D" with no answer | discussion vs. technical | _TODO_ |
| 6 | "I built an open-source CLI that generates DTOs from SQL. Here's why I think code generators beat Lombok:" plus a link | resource vs. discussion | _TODO_ |
| 7 | A personal story about a production outage caused by a missing index, with the fix explained step by step | discussion vs. technical | _TODO_ |
| 8 | A list of 10 must-read books for Java developers, with one sentence on each | resource vs. discussion | _TODO_ |

**Stress-test findings:** _Draft. TODO: confirm after labeling the eight
posts above._ Pre-labeling the real posts showed two gaps in my rules:

1. **Quiz and poll posts** (like original post 3 and fictional post 5) ask a
   technical question but teach nothing and share no opinion. None of my
   definitions covers them. Proposed rule: a post whose main content is a
   question to readers, with no explanation given, is `discussion_opinion`.
2. **Blog teasers with bullet points** (like fictional posts 1 and 2). My rule
   says "lists what the resource covers" means `resource_promotion`, but some
   bullet lists actually teach. Proposed tightening: if the bullets state facts
   or steps a reader could use without the link, it is
   `technical_explanation`. If the bullets are only topic names, it is
   `resource_promotion`.

**Group or independent boundary review:** I worked on my own. I gave Claude my
definitions and borderline post 90 (Spring Boot without an HTTP server), and
asked for the strongest case for each label, without a verdict.

- *Case for `technical_explanation`:* Almost all of the post teaches. It
  defines Spring as an IoC/DI container, then explains four concrete ways to
  use it without a web server, each with a purpose. A reader learns the idea
  without replying or clicking anything.
- *Case for `discussion_opinion`:* It opens with a question ("Do you know…?")
  and closes with an invitation ("Let me know if you're using Spring for
  something else!"). The list is short and general, more of a conversation
  starter than a tutorial, and its aim is engagement.

Both cases are plausible, so the boundary needs a rule. I kept the secondary
rule: a closing question doesn't change the label when most of the post
teaches a concrete mechanism. Post 90 stays `technical_explanation`.

---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** Public posts in LinkedIn group 25827 (Java and
software engineering), copied into `labels.csv` as whole posts. I removed
interface text such as poll buttons. I collected 200 posts. Four were exact
duplicates (the same post shared twice), so I removed the second copy of each
to keep duplicates from leaking across the train/test split.
The final file has **196 rows**: 200 collected minus the 4 duplicates. That
is above the 150-row floor, and I chose not to pad it with posts I hadn't
read the same way.

**How I labelled them:** I labelled the first 20 posts cold, by myself, with
no tools (marked `cold` in the note column). An AI tool (Claude) then
pre-labelled the remaining posts using my three definitions and decision
rules (marked `pre-labeled by AI` in the note column). It flagged 32 posts it
was unsure about. _TODO: after reading and correcting every pre-labelled row,
say so here, and say how many labels I changed._

**Counts per label:** _(update after review)_

| Label | Count | Share |
|---|---|---|
| `resource_promotion` | 114 | 58% |
| `technical_explanation` | 58 | 30% |
| `discussion_opinion` | 24 | 12% |
| **Total** | 196 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* Original post 3, a PostgreSQL quiz: a 100M-row users table with
> soft deletes, ending "Which index would you choose?" with four options.
>
> *Could have been:* `discussion_opinion` or `technical_explanation`
>
> *I chose, because:* `discussion_opinion`. The post sets up a technical
> problem but never explains the answer. Its main purpose is to get readers to
> reply with their reasoning, which is inviting discussion.

**2.**
> *The post:* Original post 20, on authentication vs. authorization. It
> summarizes an article on API security mistakes and ends with a question.
>
> *Could have been:* `technical_explanation` or `resource_promotion`
>
> *I chose, because:* `technical_explanation`. The post itself defines both
> terms and states the takeaway, so a reader learns the distinction without
> opening the article. Under my decision rule, the link doesn't decide it.

**3.**
> *The post:* Original post 18, on maintaining Java 8 systems versus building
> new projects with Java 21.
>
> *Could have been:* `technical_explanation` or `discussion_opinion`
>
> *I chose, because:* `technical_explanation`. It is framed as a reflection on
> working in both worlds, but most of the post explains what concretely differs
> (boilerplate and platform threads vs. Records, Text Blocks and Virtual
> Threads). Under my rule, when teaching and opinion overlap, the teaching
> wins.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased` (the notebook default)

**Settings:** 3 epochs, learning rate 2e-5, batch size 16, max length 128
tokens, seed 42

**Anything I changed from the defaults, and why:** Nothing. With 196 posts, I
want the first run to show what the default pipeline does with my labels, so
that next unit's diagnosis can point at the data rather than at settings I
tuned.

**Device:** `mps` (Apple Silicon GPU), as reported by `test.py`. `results.json`
records it too.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->

Seed 42, 70/15/15 stratified split of 196 rows: 136 train / 30 val / 30
test. _(This comes from the notebook's split code on the current file. Rerun
section 3 after the label review and paste its table if the numbers change.)_

| Label | Train | Val | Test |
|---|---|---|---|
| `technical_explanation` | 40 | 9 | 9 |
| `resource_promotion` | 80 | 17 | 17 |
| `discussion_opinion` | 16 | 4 | 4 |

⚠️ `discussion_opinion` has only **4 test examples**, below the ~8 the notebook
warns about. One mistake on that label moves its recall by 0.25, so its F1
will swing a lot between seeds.

---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* I asked Claude to pre-label posts 21–200 using my three
  label definitions and decision rules.
- *What came back:* A label for every post, with 32 marked "unsure" and the
  two labels each one could be. It also found that four posts appeared twice
  in my file.
- *What I changed:* _TODO: which labels I corrected after reviewing every row,
  and why._

**Moment 2**

- *What I asked for:* Help writing my five acceptance criteria. I asked
  Claude to lay out target options for each one, based on facts from my data:
  the 0.567 accuracy of always guessing `resource_promotion`, the 4
  `discussion_opinion` posts in the test split, and my 12/30/58% balance.
- *What came back:* Three options per criterion, each with its trade-off
  (safe, middle, ambitious), plus a warning that a confidence threshold can't
  be measured because `results.json` doesn't save per-prediction
  probabilities.
- *What I changed:* I chose every number myself, and on two criteria I took
  the stricter option rather than the suggested safer one: F1 ≥ 0.60 instead
  of 0.50, and agreement ≥ 80% instead of 70%. I switched criterion 5 to the
  most- vs. least-confident-third measure the notebook actually reports.
  Claude wrote the wording of the criteria and reasons from my choices.

**Moment 3 (unit 6)**

- *What I asked for:* Help running the unit 6 testing pipeline with Claude
  Code: the baseline (with and without definitions), the three-seed run, the
  confusion matrices, and a first draft of the unit 6 README sections.
- *What came back:* It ran `baseline.py` twice and ran sections 6 and 7 of the
  notebook headless across seeds 42 / 7 / 2024. It pointed out that every
  seed predicted `resource_promotion` for every post, and that validation loss
  was still falling at epoch 3, so it suggested undertraining as the cause. It
  also spotted that my `discussion_opinion` definition mentions both other
  labels, which is why the baseline did worse with my definitions than with
  bare label names. It drafted the run logs, verdicts and diagnoses from those
  numbers.
- *What I changed:* _TODO(you): what you checked, rewrote or disagreed with._
  The prediction, the staff labels and every adjudication are my own.

**Pre-labelling disclosure:** Posts 1–20 were labelled cold, by me alone.
The rest were pre-labelled by Claude and are marked in the note column.
_TODO: confirm "I read and corrected every one" once that is true._


<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

Both models scored on the same 30 held-out posts in `test_split.csv` (seed 42
split). Baseline: `facebook/bart-large-mnli` zero-shot, prompted with my three
definitions from `label_definitions.txt`, no training. Trained: the unit 5
`distilbert-base-uncased` run (`results.json`, 3 epochs, `mps`).

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.300 | 0.567 | +0.267 |
| Macro F1 | 0.296 | 0.241 | -0.054 |
| F1 — `discussion_opinion` | 0.320 | 0.000 | -0.320 |
| F1 — `resource_promotion` | 0.300 | 0.723 | +0.423 |
| F1 — `technical_explanation` | 0.267 | 0.000 | -0.267 |

**Definitions vs. bare label names.** I also ran the baseline once without
`label_definitions.txt`, so it saw only the label names
(`baseline_results_bare_names.json`). The table above uses the definitions
run, which is the official baseline.

| Baseline prompt | Accuracy | Macro F1 | F1 tech | F1 resource | F1 discussion |
|---|---|---|---|---|---|
| My definitions | 0.300 | 0.296 | 0.267 | 0.300 | 0.320 |
| Bare label names | 0.467 | 0.407 | 0.154 | 0.667 | 0.400 |

My definitions made the baseline **worse by 0.167 accuracy**. With the
definitions, it called 21 of 30 posts `discussion_opinion`, including 10 of the
17 real `resource_promotion` posts. The `discussion_opinion` definition ends
"…without mainly explaining a technical concept or promoting a resource", so
it mentions both of the other labels. An entailment model reads that sentence
as matching almost any post. The definitions are written for a human who
reads the "without", not for a model that matches words.

**What I predicted before I looked:** I predicted that the trained model's
labels would be at least 80 percent correct.

**What the gap actually means:** The unit 5 trained model beats the baseline
on accuracy (+0.267), but it has no real skill. It predicted
`resource_promotion` for all 30 posts, so its 0.567 equals the share of that
label in the test split. On macro F1 it **loses** to the baseline (0.241 vs.
0.296). The baseline is weak but at least tries all three labels. As trained
in unit 5, fine-tuning added nothing except "guess the majority". After the
improvement below, the trained model on this same split scores 0.833 accuracy
and 0.818 macro F1. That gap of +0.533 over the baseline is real.

**Runtime:** the baseline took a few seconds per post on `mps` (a couple of
minutes for 30). The trained model scores all 30 posts in under a second.

---

## Run Log — Before

Unit 5 settings (3 epochs, lr 2e-5, batch 16), seeds 42 / 7 / 2024, device
`mps` (Apple Silicon GPU), torch 2.14.1. Source:
`results_three_seeds_before.json`.

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.70 | 0.567 | 0.567 | 0.567 | MISSED |
| 2. Every label F1 (tech / resource / discussion) | ≥ 0.60 each | 0.00 / 0.72 / 0.00 | 0.00 / 0.72 / 0.00 | 0.00 / 0.72 / 0.00 | MISSED |
| 3. Dataset balance (every label 10–60%) | 10% ≤ share ≤ 60% | 30% / 58% / 12% | same file | same file | MET |
| 4. Agreement with staff on 30 posts | ≥ 80% | _TODO_ (one number, not per seed) | — | — | _TODO_ |
| 5. Most-confident third acc / gap to least-confident third | ≥ 0.80 and gap ≥ 0.15 | 1.00 / +1.00 | 1.00 / +0.80 | 1.00 / +0.90 | MET (vacuous, see below) |

Spread across seeds: **0.000** on every measure. All three seeds produced the
same predictions: `resource_promotion` for every post.

### Confusion matrix

Seed 42. Seeds 7 and 2024 are identical, because every split is stratified to
9 / 17 / 4 and every post was predicted `resource_promotion`.

| true \ predicted | technical_explanation | resource_promotion | discussion_opinion |
|---|---|---|---|
| **technical_explanation** | 0 | 9 | 0 |
| **resource_promotion** | 0 | 17 | 0 |
| **discussion_opinion** | 0 | 4 | 0 |

**My biggest off-diagonal number, and what it means:** 9 real
`technical_explanation` posts were called `resource_promotion`, and none went
the other way. That is not one boundary going wrong; the model has learned no
boundary at all, so the whole matrix sits in the majority column. The notebook
says a lopsided direction "usually points at your labelling". Here it points at
training instead, because the `discussion_opinion` posts collapse into the same
column, and that pair isn't my hard boundary.

---

## Verdicts and Diagnoses

Every model criterion must hold on all three seeds.

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | ≥ 0.70 | MISSED | 0.567 (17/30) on all three seeds, 4 posts short of the 21/30 target on each. |
| 2 | Every label F1 | ≥ 0.60 | MISSED | `technical_explanation` and `discussion_opinion` are 0.00 on all three seeds. |
| 3 | Dataset balance | 10%–60% per label | MET | 114 / 58 / 24 of 196 = 58% / 30% / 12%. Both bounds hold, but the 60% cap holds by only 2 points. |
| 4 | Labeling consistency vs. staff | ≥ 80% (24/30) | _TODO_ | _Run `agreement.py` once you have the staff labels._ |
| 5 | Confidence | top third ≥ 0.80, gap ≥ 0.15 | MET, but vacuously | Top third 1.00 on every seed. The gap is 1.00 / 0.80 / 0.90. The letter of the criterion is met. |

**Diagnoses**

**Criteria 1 and 2: a training problem, not a labeling problem.**
- *Cause:* undertraining. 136 training posts at batch 16 is 9 steps per epoch,
  so 3 epochs is only 27 gradient steps at lr 2e-5. That isn't enough to move
  off the majority prior.
- *Evidence:*
  1. Validation loss was still falling at the last epoch on every seed
     (seed 42: 0.935 → 0.827 → 0.787; seed 7: 0.940 → 0.857 → 0.830;
     seed 2024: 0.938 → 0.844 → 0.810), and average training loss was
     0.88–0.90. The model hadn't finished learning when training stopped.
  2. The matrix is a single column. A labeling problem on my hard boundary
     would show `technical_explanation` ↔ `resource_promotion` confusion while
     `discussion_opinion` survived. Here every label collapses, including the
     one with the most distinct posts.
  3. The confidence scores already rank posts well: the most-confident third
     is 100% correct on every seed. The model has signal; it just never pushes
     a non-majority class past 50%.
  4. _TODO(you): the agreement report is the instrument that rules a labeling
     problem in or out. Add one sentence here once you have it, for example
     "my agreement was X/30, and Y of my disagreements were pile (a)…"._

**Criterion 5: met, but the criterion has a hole.** All predictions were
`resource_promotion`, so "confidence" here only measures how much a post looks
like a bare link share. The 10 most link-like posts really were links, so the
top third scored 1.00. A model with zero skill on two of three labels passed
this criterion. The criterion is measurable, so I'm leaving it as written, but
a verdict of MET here says nothing about whether the model is useful. Criteria
1 and 2 are the ones that catch that.

---

## Agreement Report

<!-- TODO(you): this whole section is yours.
     1. Label all 30 posts in data/staff_posts.csv under data/staff_taxonomy.md
        and save my_staff_labels.csv (text,label — text pasted exactly).
     2. ONLY THEN get data/staff_labels.csv from your TF.
     3. python agreement.py   → commit agreement_results.json
     4. Adjudicate every disagreement below and sort into piles (a)/(b)/(c). -->

**Agreement rate:** ___ / 30 = ___%

**Disagreements**

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**

_TODO_

---

## The Improvement

**What I changed:** One training setting: `EPOCHS` 3 → 10. Everything else is
unchanged: same `labels.csv`, same taxonomy, same criteria, lr 2e-5, batch 16,
max length 128, seeds 42 / 7 / 2024, device `mps`. Source:
`results_three_seeds_after.json`.

**Which diagnosis pointed at it:** The diagnosis for criteria 1 and 2. Loss was
still falling after epoch 3, and the model predicted only the majority class
while its confidence ranking already carried signal. The brief says a training
setting is usually the wrong fix, but this is the case where the evidence
points at training rather than at my labels.

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.70 | 0.833 | 0.867 | 0.767 | MET |
| 2. Every label F1 (tech / resource / discussion) | ≥ 0.60 each | 0.78 / 0.88 / 0.80 | 0.89 / 0.89 / 0.67 | 0.71 / 0.84 / **0.40** | MISSED |
| 3. Dataset balance (every label 10–60%) | 10% ≤ share ≤ 60% | 30% / 58% / 12% | same file | same file | MET |
| 4. Agreement with staff on 30 posts | ≥ 80% | _TODO_ (unchanged by a training setting) | — | — | _TODO_ |
| 5. Most-confident third acc / gap to least-confident third | ≥ 0.80 and gap ≥ 0.15 | 1.00 / +0.50 | 1.00 / +0.20 | 1.00 / +0.50 | MET |

Spread table from section 6:

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.833 | 0.867 | 0.767 | 0.100 |
| Macro F1 | 0.818 | 0.815 | 0.649 | 0.168 |
| F1 — `technical_explanation` | 0.778 | 0.889 | 0.706 | 0.183 |
| F1 — `resource_promotion` | 0.875 | 0.889 | 0.842 | 0.047 |
| F1 — `discussion_opinion` | 0.800 | 0.667 | 0.400 | 0.400 |

Confusion matrix after, **summed across all three seeds** (90 test
predictions):

| true \ predicted | technical_explanation | resource_promotion | discussion_opinion |
|---|---|---|---|
| **technical_explanation** | 21 | 5 | 1 |
| **resource_promotion** | 4 | 46 | 1 |
| **discussion_opinion** | 1 | 4 | 7 |

**Did it help, and how do I know:** Yes, a lot.
- **Accuracy:** went from 0.567 on every seed to 0.767–0.867, so criterion 1
  moves from MISSED to MET with its lowest seed 0.067 above target.
- **Matrix:** now has a real diagonal. The model learned all three labels.
- **Errors that remain:** 9 of the 16 summed errors still fall into the
  `resource_promotion` column. The remaining leaning is toward the majority
  label.
- **My hard boundary:** `technical_explanation` ↔ `resource_promotion` is now
  roughly symmetric, 5 one way and 4 the other. That looks like a genuinely
  hard pair, not a one-sided labeling habit.
- **Criterion 2:** still MISSED, on one seed and one label. `discussion_opinion`
  recall on seed 2024 is 1 of 4.

The 0.100 accuracy spread means these numbers are less stable than the flat
"before" run, which was stable only because it was degenerate.

---

## What's Still Broken

**Criterion 2: `discussion_opinion` F1 (0.40 on seed 2024).**
- *Cause:* too few examples. There are 24 in total, so about 16 in training and
  4 in each test split. One missed post moves recall by 0.25. Its F1 swings
  from 0.40 to 0.80 across seeds, while `resource_promotion` (17 per split)
  moves only 0.047.
- *Where they go:* the summed matrix sends 4 of its 5 misses to
  `resource_promotion`.
- *What I'd do:* collect about 30 more `discussion_opinion` posts, the
  "add examples to your thinnest label" fix. That would also loosen the 60% cap
  in criterion 3, where `resource_promotion` sits at 58%.
- *Why I stopped:* the brief allows one improvement in this unit, and I spent
  it on the epochs, which my diagnosis named first.
- _TODO(you): if your agreement report shows you split opinion posts
  inconsistently, say so here, because that would change the fix from "collect
  more" to "relabel first"._

**Criterion 4:** _TODO, depends on the agreement report._

**The gap between what I meant my labels to capture and what the model
learned:** I meant the labels to capture a post's *purpose*: teach, point
elsewhere, or start a conversation. In unit 5 the model learned only "most
posts are links", the single-column matrix. Even after 10 epochs, most of its
remaining errors still fall back to `resource_promotion`. My guess is that it
leans on surface cues like a `lnkd.in` URL or a title-shaped first line, not on
whether the post teaches anything without the click. That is exactly the
distinction my decision rule depends on.


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
