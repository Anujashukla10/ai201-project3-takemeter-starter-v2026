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
I labelled 20 posts cold first, with no AI and no help, using only my written definitions (`note = cold`). For the remaining 191, Claude extracted the usable comments from the pages I pasted and pre-labelled them using my definitions and decision rules. I then read every pre-labelled row and checked it, correcting the ones I disagreed with (`note = pre-labelled, checked` or `pre-labelled, corrected (was X)`). While collecting I excluded pure jokes, pure questions, deleted/bot comments, ads, and posts under five words.

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

**Anything I changed from the defaults, and why:** Nothing. I kept every default so that next unit's results reflect my data and labels, not tuning.

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
- *What I changed:* I labelled the 8 posts myself before looking at the answers and got 6/8. The mixed posts (a general claim plus a personal feeling, or a fact used as decoration) were the hardest, so I kept the "label by whichever part takes up more words" tie-break. While labelling I also added a third rule for fact-only posts, because many r/gaming comments just correct someone with a fact.

**Moment 2**

- *What I asked for:* Examples of acceptance criteria for a different, made-up community so I could see the format, and then a check of whether my own five criteria could be tested by someone else.
- *What came back:* Example criteria for a cooking forum, and feedback that three of mine needed wording fixes: a reason that contradicted my own decision rule, a timing problem in the consistency reason, and an unclear subtraction direction in the confidence criterion.
- *What I changed:* I fixed all three wordings. The numbers (0.65, 0.50, 45%/20%, 75%, 0.15) are mine and did not change.

**Pre-labelling disclosure:** I labelled 20 posts cold with no AI (rows marked `cold`). Claude extracted the remaining 191 comments from the thread pages I pasted and pre-labelled them using my definitions and rules. I read and checked every one and corrected 24 of them. Every pre-labelled row is marked in the `note` column.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

   **Stretch declared before starting unit 6:** a confidence check, comparing how accurate the model is on its most confident third of predictions vs its least confident third, across all three seeds.
   
<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.281 | 0.500 | +0.219 |
| Macro F1 | 0.146 | 0.396 | +0.249 |
| F1 — `analysis` | 0.439 | 0.000 | -0.439 |
| F1 — `hot_take` | 0.000 | 0.556 | +0.556 |
| F1 — `reaction` | 0.000 | 0.632 | +0.632 |

Both columns are scored on the same 32 held-out posts (`test_split.csv`, seed 42). Baseline: `facebook/bart-large-mnli`, zero-shot, using my definitions from `label_definitions.txt`. Device: `cpu` (torch 2.14.1+cpu).

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->
I expect that the trained model will beat the baseline on reaction and hot_take, but not on analysis.

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->
Fine-tuning helped overall (+0.22 accuracy, +0.25 macro F1), but the two models fail in opposite directions on the same label. The baseline labelled almost every post `analysis`: its 0.281 accuracy is 9/32, exactly the number of `analysis` posts in the test split, so it scored zero on `hot_take` and `reaction`. The trained model learned `hot_take` and `reaction` but never predicted `analysis` at all (F1 0.000). So training taught the model to recognise opinions and personal reactions, but not what makes a post `analysis`. That is the same `analysis` vs `hot_take` boundary I found hardest when labelling.


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