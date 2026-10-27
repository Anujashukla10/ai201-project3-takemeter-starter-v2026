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

TakeMeter sorts comments from **r/gaming** (a public, general gaming subreddit — not tied to one game or release) into three labels: `analysis` (a point backed by a specific, checkable fact), `hot_take` (a confident claim with no support), and `reaction` (the writer's own feelings, experience, or memories). The goal is to separate the comments that actually argue from evidence from the much more common confident opinions and personal reactions. Pure jokes, pure questions, deleted/bot comments, and posts under five words are excluded rather than labelled.

**What I read first (Milestone 1):** about 40 comments across four threads — a nostalgia post about Zuma, and the comment sections of three trailer posts (Hytale Early Access Chapter I, Where Dolls Hang, WARDOGS Season 02).

**Candidate distinctions I saw in the text:**

1. **Some comments cite a specific, checkable fact; most don't.** e.g. "For example Alpha Minecraft had a 50% discount and Beta Minecraft had a 25% discount", "Getting to level 20 took like 10-12 hours of playtime", "PopCap got bought by EA right at the peak of the Facebook gaming era." These use the fact to make a point.

2. **Many comments are confident general claims with no support at all.** e.g. "Early access is fake", "Catering to the whales is a death sentence", "Looks fairly mid. Nothing here makes me want to wishlist it." Stated as obvious, never argued.

3. **Many comments are personal feelings or memories, not claims about the world.** e.g. "I just had a small panic attack as soon as I recognized that ugly ass frog", "My grandpa loved this game when I was a kid", "lol I'm only level 20 wtf". In r/gaming these are often nostalgia, not just reactions to something that happened today.

4. **A big chunk of comments are jokes or simple questions with no claim at all.** Jokes: "i read that as 'where dongs hang'", "*ribbit*". Questions: "Linux support yet?", "Is there finally new content?" I decided to exclude these rather than add a label for them.

**Other things I noticed:** trailer threads are full of very short, low-effort comments and some deleted/empty or bot comments, which I skipped when collecting. Ads appear between comments and are not posts.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

Three labels. Posts that are **only** a joke, **only** a question, a bot/deleted comment, or under five words are **excluded** when collecting, not labelled. They are common in r/gaming, but none of the three labels fits them, and a fourth "other" label would be a junk drawer.

### `analysis`

**Definition:** Makes a point about a game, studio, or the industry that is backed by at least one specific, checkable fact (a price, date, number, patch note, or named event), and the fact does real work in the argument.

**Example 1:**
> Sorry but the idea behind the concept of EA when it began was to let players purchase the game at a significant discount, proportional to the game's stage of development, as a way to compensate them. It was a win win deal.
>
> For exemple Alpha Minecraft had a 50% discount and Beta Minecraft had a 25% discount.
>
> The devs are now used to make the player pay the full price for a half baked game, where is the win win deal ? There is no incentives for them to finish the game anymore. This is borderline anti-consumer practice.
>
> I'm sorry "it's an EA" is not a shield against any criticisms, even more when players paid the full price. They are entitled to put pressure on the devs to release the full game as a reasonable pace.

**Example 2:**
> PopCap got bought by EA right at the peak of the Facebook gaming era. They then converted every single game in progress into the most greedy money begging game possible, turned a family friendly franchise into an FPS, and then bailed on all of the PopCap IP right when mobile gaming was getting big. All of those less-popular games like Alchemy or Mummy Maze or Popcorn Dragon would have made a killing as tiny polished mobile games.
>
> I'm only a little salty.

### `hot_take`

**Definition:** A confident general claim about a game, studio, players, or the industry, with no specific checkable fact offered to support it — it might be true, but it isn't backed up.

**Example 1:**
> Early access is just a term that companies love to use to deflect any blame for how broken the game is

**Example 2:**
> Catering to the whales is a death sentence.

### `reaction`

**Definition:** The writer's own feeling, experience, or memory of playing or watching something — it is about the writer, not a claim about the world.

**Example 1:**
> I just had a small panic attack as soon as I recognized that ugly ass frog.

**Example 2:**
> After not beating it as a kid, I bought it on steam as an adult and beat it. I was so proud of myself.

### The hardest boundary

**Which two labels:** `analysis` vs `hot_take`.

**The borderline post:**
> Releasing games before they're finished is the entire point of EA. Its purpose is to get funding while receiving feedback from players.
>
> You pay to become a QA/beta tester. Idk why anyone expects an early access game to be anywhere near complete.

It gives a reason, so it reads like an argument. But nothing in it can be checked: no number, date, price, or named event.

**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->
A post is `analysis` only if it contains at least one specific fact a stranger could look up (a number, price, date, patch note, or named event) **and** its conclusion depends on that fact. Reasoning alone, however long or sensible, is `hot_take`. A fact used only as decoration on a conclusion already reached is also `hot_take`. So the borderline post above is `hot_take`.

**Second rule (reaction vs hot_take):** Ask "is this about the writer, or about the world?" If it reports what the writer felt, did, or remembers, it's `reaction`, even if it includes a number about themselves ("After 18hrs I'm considering it a loss"). If it makes a general claim others could agree or disagree with ("This game is dead"), it's `hot_take`. If a post does both, label it by whichever part takes up more of the words.

**Third rule (fact-only posts):** A post whose main point is a specific, checkable fact used to inform or correct someone (e.g. "Redfall was made by arkane Austin which already closed in 2024 this is arkane lyon") is `analysis`, even without a longer argument. A number that can't be checked ("For every 1 game it hurts, another 10-20 benefit from it") does not count as a fact.

---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** Public comments from six r/gaming threads: a Zuma nostalgia post, three trailer posts (Hytale Early Access Chapter I, Where Dolls Hang, WARDOGS Season 02), a news post about Microsoft possibly shutting down Arkane Lyon, and a news post about a former Xbox VP saying Game Pass cuts into retail sales. I copied the comment pages by hand. The first four threads were mostly reactions and hot takes, so I chose the last two on purpose to find more `analysis` posts.

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->
I labelled 20 posts cold first, with no AI and no help, using only my written definitions (`note = cold`). For the remaining 191, Claude extracted the usable comments from the pages I pasted and pre-labelled them using my definitions and decision rules. I then read every pre-labelled row and checked it, correcting the ones I disagreed with (all pre-labelled rows are marked `note = pre-labelled, checked`). While collecting I excluded pure jokes, pure questions, deleted/bot comments, ads, and posts under five words.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| hot_take | 82 | 38.9% |
| reaction | 73 | 34.6% |
| analysis | 56 | 26.5% |
| **Total** | 211 | 100% |

No label is above 45% or below 20%, so my balance criterion (criterion 3) is met.

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* "It's always a balance between sales potential and financial stability for the devs. … I know a studio owner that went through a gamepass deal. His take on it was that it didn't maximize potential revenue but it absolutely guaranteed that he had paid back his production costs day one. … Things like Hollowknight or Expedition 33 are the game dev equivalent of winning the powerball."
>
> *Could have been:* `analysis` or `hot_take`
>
> *I chose, because:* `hot_take`. It is carefully argued, but its main evidence is a second-hand story about an unnamed studio owner that nobody can check. Under my decision rule, reasoning without a checkable fact is `hot_take`, however good the reasoning is.

**2.**
> *The post:* "For every 1 game it hurts, another 10-20 benefit from it."
>
> *Could have been:* `analysis` or `hot_take`
>
> *I chose, because:* `hot_take`. It contains numbers, but they are made up — there is no source and no way to check them. My third rule says a number only counts as a fact if someone could look it up.

**3.**
> *The post:* "I'm pretty sure that Prey (2017) was made by them too. It's a really phenomenal game if you haven't tried it yet."
>
> *Could have been:* `analysis` or `hot_take`
>
> *I chose, because:* `analysis`. Its main point is a specific, checkable fact (which studio made Prey), so it falls under my fact-only rule. The fact is actually wrong — Prey was made by Arkane Austin, not Lyon — which made me realise my rule says *checkable*, not *correct*. I kept it as `analysis`, because labelling by correctness would mean fact-checking every post, which no labeller could do consistently.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** <!-- epochs, learning rate, batch size, seed -->
3 epochs, learning rate 2e-5, batch size 16, max length 128 tokens, seed 42.

**Device:** `cpu` — PyTorch 2.14.1+cpu, Python 3.13 (x64) running under emulation on a Windows on ARM (Snapdragon) laptop. I had to use the x64 build of Python because the pinned packages (pandas, torch) have no ready-made ARM Windows versions for Python 3.13.

**Anything I changed from the defaults, and why:** Nothing in unit 5. I kept every default so the results reflect my data and labels, not tuning. (In unit 6 my one improvement changed `EPOCHS` to 10 — see The Improvement. Because of that later run, the committed `results.json` now holds the 10-epoch seed-42 result, not the unit 5 3-epoch result of accuracy 0.500.)

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->
train 147 · val 32 · test 32 (70/15/15, stratified).

| Label | Train | Val | Test |
|---|---|---|---|
| analysis | 39 | 8 | 9 |
| hot_take | 57 | 13 | 12 |
| reaction | 51 | 11 | 11 |

Every label has at least 8 posts in the test split, but `analysis` (9) is close, so its F1 will be the jumpiest across seeds next unit.

---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* I pasted the r/gaming threads I had read and asked Claude to draft label definitions and a decision rule from them, and then to write 8 posts that sit on the boundaries between my labels.
- *What came back:* Draft definitions for `analysis`, `hot_take`, and `reaction` using real comments as examples, a decision rule for `analysis` vs `hot_take`, and 8 boundary posts.
- *What I changed:* I labelled the 8 posts myself before looking at the answers and got 6/8. Both misses were mixed posts (a general claim plus a personal feeling, or a fact used only as decoration), which showed me where my boundaries were weakest. So I kept the "label by whichever part takes up more words" tie-break, made my main rule say the fact must carry the conclusion, and later added a third rule for fact-only posts, because many r/gaming comments just correct someone with a fact.

**Moment 2**

- *What I asked for:* Examples of acceptance criteria for a different, made-up community so I could see the format, and then a check of whether my own five criteria could be tested by someone else.
- *What came back:* Example criteria for a cooking forum, and feedback that three of mine needed wording fixes: a reason that contradicted my own decision rule, a timing problem in the consistency reason, and an unclear subtraction direction in the confidence criterion.
- *What I changed:* I fixed all three wordings. The numbers (0.65, 0.50, 45%/20%, 75%, 0.15) are mine and did not change.

**Moment 3 (unit 6)**

- *What I asked for:* Help reading my baseline, three-seed and confusion-matrix outputs, and a structure for my verdicts and diagnoses.
- *What came back:* It pointed out that the baseline's 0.281 accuracy was exactly 9/32 (it labelled everything `analysis`), and that my 3-epoch training loss of 0.979 was close to the 1.099 of a model guessing evenly across 3 labels, which suggested undertraining.
- *What I changed:* I checked those numbers against my own outputs and chose the epochs change as my one improvement because of that diagnosis. I labelled the 30 staff posts myself before seeing the staff labels, and kept my labels unchanged after seeing them. Claude helped me compare the two files and draft the reasoning for each disagreement from the staff rule text; I checked each call against `data/staff_taxonomy.md`.

**Pre-labelling disclosure:** I labelled 20 posts cold with no AI (rows marked `cold`). Claude extracted the remaining 191 comments from the thread pages I pasted and pre-labelled them using my definitions and rules. I read and checked every one. I did not record which rows I changed, so every pre-labelled row is marked `pre-labelled, checked` in the `note` column.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

**Stretch declared before starting unit 6:** a confidence check, comparing how accurate the model is on its most confident third of predictions vs its least confident third, across all three seeds. (Reported under The Improvement.)

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.281 | 0.500 | +0.219 |
| Macro F1 | 0.146 | 0.396 | +0.249 |
| F1 — `analysis` | 0.439 | 0.000 | -0.439 |
| F1 — `hot_take` | 0.000 | 0.556 | +0.556 |
| F1 — `reaction` | 0.000 | 0.632 | +0.632 |

Both columns are scored on the same 32 held-out posts (`test_split.csv`, seed 42). Baseline: `facebook/bart-large-mnli`, zero-shot, using my definitions from `label_definitions.txt`. Trained: the unit 5 model (3 epochs, seed 42). Device: `cpu` (torch 2.14.1+cpu).

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->
I expect that the trained model will beat the baseline on reaction and hot_take, but not on analysis.

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->
My prediction was right. Fine-tuning helped overall (+0.22 accuracy, +0.25 macro F1), but the two models fail in opposite directions on the same label. The baseline labelled almost every post `analysis`: its 0.281 accuracy is 9/32, exactly the number of `analysis` posts in the test split, so it scored zero on `hot_take` and `reaction`. The trained model learned `hot_take` and `reaction` but never predicted `analysis` at all (F1 0.000). So training taught the model to recognise opinions and personal reactions, but not what makes a post `analysis` — the same `analysis` vs `hot_take` boundary I found hardest when labelling.

---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

Device: `cpu` (torch 2.14.1+cpu). 3 epochs. Accuracy moved 0.094 across seeds, under the 10-point instability line.

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.65 | 0.500 | 0.531 | 0.594 | MISSED |
| 2. Every label F1 (lowest shown) | ≥ 0.50 | 0.000 (`analysis`) | 0.000 (`analysis`) | 0.000 (`analysis`) | MISSED |
| 3. Balance in labels.csv | each label 20–45% | 26.5% – 38.9% | 26.5% – 38.9% | 26.5% – 38.9% | MET |
| 4. Agreement with staff | ≥ 75% | 60% (18/30) | 60% (18/30) | 60% (18/30) | MISSED |
| 5. Confidence gap (most − least confident third) | ≥ 0.15 | +0.100 | −0.300 | +0.800 | MISSED |

Criteria 3 and 4 don't depend on the seed, so the same value appears in all three columns.

Full before-run spread table:

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.500 | 0.531 | 0.594 | 0.094 |
| Macro F1 | 0.396 | 0.413 | 0.465 | 0.069 |
| F1 — `analysis` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `hot_take` | 0.556 | 0.615 | 0.645 | 0.090 |
| F1 — `reaction` | 0.632 | 0.625 | 0.750 | 0.125 |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

Seed 42, before the improvement (3 epochs).

| true \ predicted | analysis | hot_take | reaction |
|---|---|---|---|
| **analysis** | 0 | 9 | 0 |
| **hot_take** | 0 | 10 | 2 |
| **reaction** | 0 | 5 | 6 |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->
All 9 real `analysis` posts were called `hot_take`, and 0 went the other way, so the model never learned the `analysis` vs `hot_take` boundary at all. It treats every argued post as a hot take, whether or not it contains a checkable fact. The same happened on the other seeds (seed 7: 9 → `hot_take`; seed 2024: 7 → `hot_take`, 2 → `reaction`). The second-largest error is also toward `hot_take` (5 real reactions called `hot_take`), so `hot_take` is the model's default guess whenever it's unsure.

---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

Verdicts below are for the **before** run (my criteria as filed in unit 5, no revisions). The after-run verdicts are in Run Log — After.

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | ≥ 0.65 | MISSED | Best seed was 0.594; none of the three reached 0.65 |
| 2 | Every label F1 | ≥ 0.50 | MISSED | `analysis` F1 was 0.000 on all three seeds |
| 3 | Balance | each label 20–45% | MET | Counts are 82/73/56 of 211 (38.9%, 34.6%, 26.5%), fixed for every seed |
| 4 | Agreement with staff | ≥ 75% | MISSED | `agreement.py` gave 18/30 = 60%, below 75% |
| 5 | Confidence gap | ≥ 0.15 | MISSED | Only seed 2024 cleared it (+0.80); seed 42 was +0.10 and seed 7 was −0.30 |

I revised no criteria.

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->

**Criterion 2 (`analysis` F1 0.000) — the root miss. Cause: a genuinely hard label pair, plus an undertrained model.** The model never predicts `analysis` on any seed; the matrices show those posts going to `hot_take` (9/9, 9/9 and 7/9). Too few examples is not the main cause: `analysis` had 39 training posts against 51 for `reaction`, which the model did learn (F1 0.63–0.75). The evidence points elsewhere:
- **Hard pair.** Both labels are long, argued, opinionated posts. The only difference is whether a checkable fact carries the conclusion — a small, subtle feature to learn from 147 training posts. Even I missed 2 of 8 on the boundary stress test in unit 5 (6/8), and both misses were mixed posts on this kind of boundary.
- **Undertrained.** Training loss at epoch 3 was 0.979, barely below 1.099 (the loss of a model guessing evenly across 3 labels), and validation accuracy was still rising every epoch (0.50 → 0.56 → 0.59). The model stopped before it learned more than "short and emotional = `reaction`, everything else = `hot_take`".
- **Labelling or model? (agreement evidence):** I agreed with staff on only 18/30 posts, and 3 of my 12 disagreements are on exactly this pair: I called sp_003 and sp_006 `hot_take` where staff (correctly, by rule 1) said `analysis`, and I called sp_014 `analysis` where staff said `hot_take`, even though sp_014 is almost word-for-word rule 1's own example of a decorative statistic. So I apply the `analysis` vs `hot_take` boundary inconsistently myself, in both directions. Two of the three go the same way as the model's error (real `analysis` → `hot_take`). That means part of this miss is my labelling, not only the model: if my 211 training labels mix these two the way my 30 staff-set labels did, the model was given a blurry signal for exactly the boundary it failed to learn.

**Criterion 1 (accuracy 0.50–0.59). Cause: follows from criterion 2.** With `analysis` (9 of 32 test posts) always wrong, the best possible accuracy is about 0.72, and every `hot_take`/`reaction` mistake lowers it further. The seed-42 matrix also shows 5 real `reaction` posts called `hot_take`, so `hot_take` is the model's default guess.

**Criterion 5 (confidence gap +0.10 / −0.30 / +0.80). Cause: undertraining plus too little data per third.** In `results_three_seeds_before.json` the model's mean confidence was about 0.40–0.42 whether it was right or wrong (seed 42: 0.418 right vs 0.410 wrong; seed 7: 0.402 vs 0.409). That is barely above the 0.33 of a 3-way guess, so "most confident" and "least confident" barely differ and the gap is noise. Each third also holds only 10 posts, so one post moves a third's accuracy by 0.10 — the noise I warned about in my unit 5 reason.

**Criterion 4 (agreement 60%, target 75%). Cause: a boundary I applied inconsistently — my own labelling, not ambiguity.** All 12 disagreements are pile (a): the staff rule covered the post and I applied it loosely (see Agreement Report). The strongest evidence that this is carelessness rather than genuinely hard posts: staff marked 7 posts as borderline/hard, and I matched them on 6 of those 7. Eleven of my 12 misses were on posts staff considered *clear*. The biggest cluster is `reaction` → something else: 6 of the 10 staff `reaction` posts (sp_021–026) I called `hot_take` or `analysis`, mostly because a number appeared ("Four minutes off", "Got up at 5", "mile 22") and I treated its presence as a reason instead of asking rule 3's question, whether the fact is load-bearing.

---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** 18 / 30 = 60%

I labelled all 30 posts in `data/staff_posts.csv` under the staff taxonomy (`my_staff_labels.csv`) before receiving `data/staff_labels.csv`, and did not change my labels afterwards. `agreement.py` wrote `agreement_results.json`.

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

The 12 disagreements, each argued from the staff rule text:

| # | Post | I said | Staff said | My call, and why (from `staff_taxonomy.md`) | Pile |
|---|---|---|---|---|---|
| 1 | sp_002 "I tracked sleep against session quality for eleven weeks…" | `reaction` | `analysis` | Eleven weeks of tracked data carries the conclusion. Rule 3: recency/personal does not outrank evidence, and this isn't even recent | (a) |
| 2 | sp_003 "The club championship scoring changed in 2019 from top-6 to top-10…" | `hot_take` | `analysis` | Rule 1: the scoring change and the B-team result *are* the argument, not decoration | (a) |
| 3 | sp_006 "Checked the entry lists. 41% … against 26% five years ago…" | `hot_take` | `analysis` | Rule 1: the 41% vs 26% comparison is the reason for the conclusion | (a) |
| 4 | sp_007 "Ran the same loop in 4C and in 24C… 90 seconds slower…" | `reaction` | `analysis` | Rule 3: a personal test, but the measured 90 seconds carries the claim about heat | (a) |
| 5 | sp_013 "The club needs to stop pretending the Tuesday session is for everyone…" | `reaction` | `hot_take` | Rule 2: a general claim about the world, not something that happened to the writer | (a) |
| 6 | sp_014 "Times are down about 3% since the shoe rules changed, so obviously…" | `analysis` | `hot_take` | Rule 1: almost word-for-word the rule's own example. The 3% decorates a conclusion already reached | (a) |
| 7 | sp_021 "Four minutes off. FOUR… sat on the kerb outside the finish…" | `hot_take` | `reaction` | Rule 2: reports what just happened to the writer and how it felt; no general claim | (a) |
| 8 | sp_022 "Got up at 5, drove ninety minutes, and it's cancelled…" | `analysis` | `reaction` | Contains times, but they don't argue anything. It's an immediate personal event | (a) |
| 9 | sp_023 "First run back after eight weeks out…" | `hot_take` | `reaction` | Rule 2: about the writer's own run today, no claim about the world | (a) |
| 10 | sp_024 "Watching the last 400 of that and I had to stand up…" | `analysis` | `reaction` | No fact and no argument; an immediate feeling | (a) |
| 11 | sp_025 "Blew up at 20 with the pace band still in my hand…" | `hot_take` | `reaction` | Rule 2: what just happened to the writer | (a) |
| 12 | sp_026 "Someone handed my kid an orange slice at mile 22…" | `hot_take` | `reaction` | Rule 2: an immediate personal moment, no general claim | (a) |

In every row, staff is right, and each call is argued from a specific rule rather than "staff is right". I looked for a pile (c) case I could win and didn't find one: the closest is sp_013 ("The club needs to stop pretending the Tuesday session is for everyone"), which sounds like a local complaint the writer feels strongly about, but rule 2 says explicitly that a heated post is still `hot_take` if it makes a general claim, and "it isn't and it never has been" is a general claim. So all 12 are pile (a): the written rule covered the post and I applied it loosely. None are pile (b). Notably, the posts staff flagged as genuinely hard (sp_005, 010, 014, 019, 020, 028, 029) are where I did *best*: I matched 6 of those 7. I lost points on the easy ones.

**What the pattern in my disagreements tells me:**
My 12 disagreements are spread across every boundary, not just one: 3 are `analysis` vs `hot_take` (sp_003, 006, 014), 4 are `analysis` vs `reaction` (sp_002, 007, 022, 024), and 5 are `reaction` vs `hot_take` (sp_013, 021, 023, 025, 026). Two habits stand out. First, I called short, emotional, first-person race posts `hot_take` (5 times), which is the same `reaction` → `hot_take` direction my 3-epoch model showed (5 real reactions called `hot_take` in the seed-42 matrix). Second, I let the *presence* of numbers decide `analysis` (sp_014, 022) instead of asking whether the number carries the argument, and missed facts that clearly did (sp_002, 003, 006, 007). The second habit is exactly the `analysis` vs `hot_take` boundary the model never learned. So the agreement evidence points at my labelling as part of the cause: the model was trained on labels where I applied my hardest boundary inconsistently. And because I got the hard posts right but the easy ones wrong, the problem is consistency and care, not that the rules are unclear — which is fixable by relabelling with the rule questions written next to me.



---

## The Improvement

**What I changed:** One training setting: `EPOCHS` from 3 to 10. Same data, same labels, same seeds (42, 7, 2024), same learning rate (2e-5), batch size (16) and max length (128).

**Which diagnosis pointed at it:** The criterion 2 diagnosis. At epoch 3 the training loss was 0.979, barely below the 1.099 of a model guessing evenly, and validation accuracy was still rising every epoch (0.50 → 0.56 → 0.59). The model stopped before it had learned the `analysis` vs `hot_take` boundary, so more passes over the same data is the most direct test of that diagnosis.

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.65 | 0.656 | 0.656 | 0.844 | MET |
| 2. Every label F1 (lowest shown) | ≥ 0.50 | 0.364 (`analysis`) | 0.462 (`analysis`) | 0.667 (`analysis`) | MISSED |
| 3. Balance in labels.csv | each label 20–45% | 26.5% – 38.9% | 26.5% – 38.9% | 26.5% – 38.9% | MET |
| 4. Agreement with staff | ≥ 75% | 60% (18/30) | 60% (18/30) | 60% (18/30) | MISSED |
| 5. Confidence gap (most − least confident third) | ≥ 0.15 | +0.500 | +0.100 | +0.200 | MISSED |

Criterion 4 measures my labelling, not the model, so the epochs change could not affect it.

Full after-run spread table (device `cpu`, torch 2.14.1+cpu, EPOCHS = 10):

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.656 | 0.656 | 0.844 | 0.188 |
| Macro F1 | 0.606 | 0.630 | 0.822 | 0.216 |
| F1 — `analysis` | 0.364 | 0.462 | 0.667 | 0.303 |
| F1 — `hot_take` | 0.621 | 0.667 | 0.889 | 0.268 |
| F1 — `reaction` | 0.833 | 0.762 | 0.909 | 0.147 |

After-run confusion matrix, seed 42 (from `results_three_seeds_after.json`):

| true \ predicted | analysis | hot_take | reaction |
|---|---|---|---|
| **analysis** | 2 | 7 | 0 |
| **hot_take** | 0 | 9 | 3 |
| **reaction** | 0 | 1 | 10 |

**Did it help, and how do I know:**
<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->
Yes, on every measure and every seed. Comparing the two run logs row by row: accuracy rose from 0.500 / 0.531 / 0.594 to 0.656 / 0.656 / 0.844, so criterion 1 went from MISSED to MET (though only by 0.006 on two seeds). `analysis` F1 rose from 0.000 on all seeds to 0.364 / 0.462 / 0.667, so the model now predicts `analysis` and gets some right. That confirms the undertraining diagnosis: the boundary was learnable, the model just hadn't trained long enough. Criterion 2 is still MISSED because `analysis` is below 0.50 on two seeds. The seed-42 matrix shows why: `analysis` precision is now 1.00 (every post it calls `analysis` really is), but recall is only 0.22 — 7 of 9 real `analysis` posts still go to `hot_take`. The model is now cautious rather than blind on that boundary.

One honest caveat: the spread across seeds grew. Accuracy now moves 0.188 between seeds (0.094 before), and `analysis` F1 moves 0.303. With only 32 test posts, and 9 of them `analysis`, one or two posts landing in a different split changes the result a lot. So criterion 1's MET is real but fragile, and the dataset is too small for a stable measure of `analysis`.

### Stretch: confidence check

Are the model's confident predictions right more often than its unsure ones? Accuracy on the most-confident third vs the least-confident third of the 32 test posts (10 posts per third), from `results_three_seeds_before.json` and `results_three_seeds_after.json`:

| Run | Seed | Most-confident third | Least-confident third | Gap | Mean confidence when right / wrong |
|---|---|---|---|---|---|
| Before (3 ep) | 42 | 0.40 | 0.30 | +0.10 | 0.418 / 0.410 |
| Before (3 ep) | 7 | 0.40 | 0.70 | −0.30 | 0.402 / 0.409 |
| Before (3 ep) | 2024 | 0.90 | 0.10 | +0.80 | 0.386 / 0.364 |
| After (10 ep) | 42 | 0.80 | 0.30 | +0.50 | 0.747 / 0.633 |
| After (10 ep) | 7 | 0.80 | 0.70 | +0.10 | 0.656 / 0.625 |
| After (10 ep) | 2024 | 0.90 | 0.70 | +0.20 | 0.693 / 0.556 |

**Before:** confidence meant nothing. The model was about 0.40 sure of everything, right or wrong (barely above the 0.33 of a 3-way guess), and on seed 7 its least-confident third was actually *more* accurate than its most-confident third. **After:** confidence means something on every seed. The confident third is right more often on all three, and the model is more sure when right than when wrong on all three (e.g. seed 2024: 0.69 vs 0.56). But the gap is small on seed 7 (+0.10), so criterion 5 is still missed. With 10 posts per third, one post moves the accuracy by 0.10, so I would not trust a confidence threshold from this model yet.

---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->

**Still missed after the improvement:** criterion 2 (every label F1 ≥ 0.50), criterion 4 (agreement 60% vs 75%) and criterion 5 (confidence gap ≥ 0.15).

- **Criterion 4, agreement (60%):** the fix is in my labelling habits, not the model. Next time I would label with two questions written next to me — "does the fact carry the argument?" and "is this about the writer just now, or about the world?" — and re-check my own 211 rows the same way, especially short first-person posts that contain a number. I stopped because changing my labels counts as a second change, and the brief allows one improvement per unit.

- **Criterion 2, `analysis` F1 (0.364 / 0.462 / 0.667):** the model now has high precision but low recall on `analysis` (seed 42: 1.00 precision, 0.22 recall). Next I would add about 30 more `analysis` posts from fact-heavy r/gaming threads (sales numbers, prices, patch notes), and re-check the `analysis` vs `hot_take` rows in my labels using the agreement findings. I stopped because the brief allows one measured improvement per unit, and I used it on training length, which is what my diagnosis pointed at first.
- **Criterion 5, confidence gap (seed 7 only +0.10):** each third holds only 10 test posts, so one post changes a third's accuracy by 0.10. The real fix is more data and a bigger test split, not a model change.
- **Stability:** accuracy moved 0.188 across seeds after the change. My dataset (211 posts, 32 in test) is too small for a stable measure, which also makes my criterion 1 MET borderline (0.656 against a 0.65 target on two seeds).

**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->
I meant `analysis` to mean "an argument carried by a checkable fact", but the 3-epoch model learned only "short and emotional = `reaction`, anything argued = `hot_take`": all 9 real `analysis` posts in the seed-42 matrix were called `hot_take`, with 0 going the other way. Training longer let it start to pick up the fact-carrying feature, but it still sends 7 of 9 `analysis` posts to `hot_take` on seed 42, so the model has only partly learned the distinction I cared about most.

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