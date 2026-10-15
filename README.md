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

**Community:** r/gaming (public subreddit, general gaming discussion — not tied to one game or release).

**What I read:** about 40 comments across four threads — a nostalgia post about Zuma, and the comment sections of three trailer posts (Hytale Early Access Chapter I, Where Dolls Hang, WARDOGS Season 02).

**Candidate distinctions I saw in the text:**

1. **Some comments cite a specific, checkable fact; most don't.** e.g. "For example Alpha Minecraft had a 50% discount and Beta Minecraft had a 25% discount", "Getting to level 20 took like 10-12 hours of playtime", "PopCap got bought by EA right at the peak of the Facebook gaming era." These use the fact to make a point.

2. **Many comments are confident general claims with no support at all.** e.g. "Early access is fake", "Catering to the whales is a death sentence", "Looks fairly mid. Nothing here makes me want to wishlist it." Stated as obvious, never argued.

3. **Many comments are personal feelings or memories, not claims about the world.** e.g. "I just had a small panic attack as soon as I recognized that ugly ass frog", "My grandpa loved this game when I was a kid", "lol I'm only level 20 wtf". In r/gaming these are often nostalgia, not just reactions to something that happened today.

4. **A big chunk of comments are jokes or simple questions with no claim at all.** Jokes: "i read that as 'where dongs hang'", "*ribbit*". Questions: "Linux support yet?", "Is there finally new content?" These may need their own label or may need to be excluded.

**Other things I noticed:** trailer threads are full of very short, low-effort comments and some deleted/empty or bot comments, which I'll skip when collecting. Ads appear between comments and are not posts.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

## Label Taxonomy

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
A post is `analysis` only if it contains at least one specific fact a stranger could look up (a number, price, date, patch note, or named event) **and** its conclusion depends on that fact. Reasoning alone, however long or sensible, is `hot_take`. A fact used only as decoration on a conclusion already reached is also `hot_take`. So the borderline post above is `hot_take`.

**Second rule (reaction vs hot_take):** Ask "is this about the writer, or about the world?" If it reports what the writer felt, did, or remembers, it's `reaction`, even if it includes a number about themselves ("After 18hrs I'm considering it a loss"). If it makes a general claim others could agree or disagree with ("This game is dead"), it's `hot_take`. If a post does both, label it by whichever part takes up more of the words.


---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**

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
