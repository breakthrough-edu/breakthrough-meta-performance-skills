# Gate 8 and Gate 10: reconciliation, and the handover

Load this once rows are landing in `Ad Daily` (gate 7 done, see [references/meta-token-and-sync.md](meta-token-and-sync.md)).

**Gate 8 comes before gate 9.** The vault note written at gate 9 records that reconciliation passed and what the three numbers were. ⛔ Writing the note first produces a permanent record saying a system is installed, sitting on top of totals nobody ever checked.

---

## Gate 8: reconciliation is a gate, not a request

**Why this is the gate the whole install turns on:** every mistake this system can make **looks like success**. A join key with a stray space, a rollup bound to the wrong object, a link-based aggregation quietly omitting every synced row: none of them error. Each one returns a plausible number. The student cannot see the difference, and then decides how much money to spend from a table that is wrong.

⛔ **Self-report is not a pass.** Asking "does it match?" and hearing "yes, looks right" is exactly the shape of trusting an exit code: you have observed that the student said something, not that the numbers agree. **You do the comparing, always.**

### The three legs

Evidence flows **from the student to you**. You state the range; they read one number off their screen and hand it over; you compare it against two numbers you obtained yourself.

| Leg | Where it comes from | What it can go wrong about |
|---|---|---|
| **A. Ads Manager** | The student reads the spend total for the range you named | Wrong account, wrong dates, a filter left on |
| **B. Insights** | You pull it with the token from gate 6, same range | Nothing much; this is your control |
| **C. Raw rows** | You sum `Ad Daily` yourself, in your own arithmetic | Sync truncation, duplication, a column landing wrong |

⛔ **Leg C never comes from the Base's own rollups.** The rollups are among the things under test. Summing with a formula that might itself be the bug and then declaring the bug absent is not a test, it is a mirror.

### The procedure

⛔ **First install: the range is one day, and that is correct.** Gate 7 ran the workflow once, and the day gate keys per date, so `Ad Daily` holds exactly the one day it fetched. **A seven-day range at this moment returns an empty leg C, which the table below reads as truncation, which stops the install and means the vault note never gets written.** So on a first install, reconcile **the single day gate 7 just synced**, say out loud that it is one day and that one day is thin evidence, and pass or fail on it. ⛔ Do not backfill to manufacture a week, and ⛔ do not widen the range to something the table does not hold.

⭐ **Then hand the real version forward**: the first weekly analysis run, once a full week exists, re-runs this gate over seven days. Write that into the vault note as an open item, or it will not happen.

1. **Name the range yourself.** On a **later** run of this gate, use at least 7 full days, **ending at least 2 days before today**. Two reasons: Meta revises very recent numbers, and the most recent day can be partial if the two clocks disagree (see the timezone check in [references/meta-token-and-sync.md](meta-token-and-sync.md)).
2. **Tell the student exactly what to read**: which ad account (name it by its id, not "your account"), the exact start and end dates, and the spend total for all ads in that range with **no filters applied**. Ads Manager opens on a date preset and remembers filters, and both of those are the usual cause of leg A being wrong.
3. **Pull leg B yourself** with the token, over the identical range.
4. **Sum leg C yourself.** Read the raw `Ad Daily` rows whose key ends in a date inside the range (the date is the last 10 characters of `<ad-id>__<YYYY-MM-DD>`), and add the spend column up in your own arithmetic. Report the **row count** alongside the sum: the count is half the diagnosis when the sum is wrong.

⛔ **Page to the end before you add anything up.** `+record-list` caps every page at 200 rows and returns `has_more: true`, and raising `--limit` does not change that: a request for 1000 still returns 200 (`lark-lessons.md:89`, whose closing instruction is **never trust a single page's count**). Keep calling with `--offset` until `has_more` is false, then sum. Seven days of thirty delivering ads is 210 rows, so an ordinary account crosses the cap. ⚠️ **A single page looks exactly like a complete answer**: no error, a plausible total, simply low.

   ⛔ **If you skip this, leg C comes in low on a correctly installed system**, you read that as truncation upstream, and the remedy you are holding is to narrow what the sync collects. That permanently shrinks a working install to cure a defect that was never there.
5. **Compare all three, and say the three numbers out loud** in the session. A gate whose result is not stated is a gate nobody can check afterwards.

⚠️ **Two shape traps when you pull raw rows** (both measured on Lark's CLI, both cheap to avoid):

- ⛔ **Rows come back as positional arrays, and getting this wrong is SILENT.** Column names, field ids and types sit in **parallel arrays**, not inside each row (`lark-lessons.md:25`). Three measured failures, none of which raises anything:
  - Reading `data.records` instead of `data.data` returns `[]`, which presents as **a truthful-looking "0 records"** (`lark-lessons.md:83`).
  - Indexing a row by column name returns `None` for every row, which presents as **an empty table** (`lark-lessons.md:25`).
  - ⛔ **Zipping the names against the row and then indexing by full name is the trap, not the remedy.** Returned column names are **truncated at about 20 characters**, so every long field name resolves to `None`, which presents as *"those columns were never written"* (`lark-lessons.md:85`). This one bit during a verification pass, which is exactly where you are standing now.
  - ⇒ **The iron rule, verbatim from `lark-lessons.md:86`: on read-back verification always pass `--field-id` explicitly**, and unpack by the order you passed them. ⛔ Do not match on returned column names.
  - ⚠️ **Why this one governs the gate:** leg C is a hand sum. All three failures give you a small or empty number with no error, the diff table below reads C-low as truncation, and its remedy narrows what the sync collects. **You would permanently shrink a working install to cure a bug in your own read.** address fields explicitly.
- **Returned column names were once truncated at around 20 characters**, which made correctly-written values look like missing fields during a verification pass. It did not reproduce on a later CLI version, but ⭐ keep addressing fields explicitly anyway: it costs nothing, and "I did not see it this time" is not evidence that it is gone.

### Reading the diff

**Tolerance:** differences below one currency unit are rounding. **Anything larger is a finding.** ⛔ Do not wave off a two percent gap as "close enough": every failure mode in this system produces a plausible number, and a plausible number that is two percent off is exactly what a truncated day looks like.

| What disagrees | What it points at | The one action |
|---|---|---|
| **A differs, B = C** | The student read the wrong thing: another account, another range, or a live filter | Re-state the account id and the exact dates, have them read it again before you touch anything |
| **C differs, A = B** | The sync. **C low** means rows are missing (truncation); **C high** means rows are duplicated | Count rows per key for the range: more than one row per key is duplication; otherwise compare per-day row counts against ads that delivered, and re-run the pagination check |
| **All three differ** | The range or the account identity is not actually agreed yet | Redo the gate over a **single day** first; three unrelated numbers cannot be diagnosed |
| **A = B = C** | The rows are true | Pass. Go to gate 9 |

⭐ **One free fourth reading, and it tests something different.** Compare your own leg C sum against what the Base's **own rollup** shows for the same range. Legs A, B and C test whether the data is true; this comparison tests whether the arithmetic sitting on top of it is. A rollup that joins through a link field rather than the text key omits every synced row while still returning a number, so a rollup that disagrees with your hand sum is a real defect even though all three legs passed.

⛔ **A failed gate 8 does not proceed.** Do not write the vault note, do not run the handover, and do not soften it into a caveat. Say which leg disagreed, by how much, and what the next action is. An install that stops here with a named defect is worth more than one that completes with an unnamed one.

---

## Gate 10: the handover

The student now owns a system that runs without them. Keep this short. Four things, and every one of them converts to an action. ⛔ Do not quote a duration for it or for anything else in this install: this package promises no timings.

### 1. The order of reading, and why it is not negotiable

**Every week, three questions, in this order:**

1. **Is the sync fresh?** If the last sync is old, nothing below it is a fact yet. Every number on the board is describing a week that may already be over.
2. **Can the team absorb more?** If the capacity index is above 1, more leads are arriving than are being handled. **SCALE is not executable in that state**: the extra spend buys leads that nobody answers, and the results come back looking like an advertising failure.
3. **Only then, the verdict.**

⛔ **The failure this order prevents is going straight to SCALE.** It is the most attractive word on the board, it is the one that costs money the fastest, and it is the one that is wrong most often when read out of order.

**Action:** when a number looks wrong, the first stop is never the ads. It is the workflow's run history and the last sync timestamp.

### 2. The token is sitting in the workflow, in the open

**Anyone with edit rights on that Base can read it.** It is pasted into a workflow step, not stored in a vault, not in a password manager, and Lark does not mask it.

- **Action before inviting anybody into the Base:** decide who gets edit rights and who gets view rights. View rights cannot read the workflow's contents.
- **Action when somebody with edit rights leaves:** revoke the token in Meta and paste a new one. ⛔ Removing their Base access does not do it. A token they already read is a token they still have.

### 3. Timezone

**The Lark timer's "yesterday" and Meta's "yesterday" are two different clocks.** If they drift, a partial day gets written and the day gate never asks for that date again, silently and with no error. The check and the fix are in [references/meta-token-and-sync.md](meta-token-and-sync.md); both timezones are recorded in the vault note per [references/vault-note.md](vault-note.md).

### 4. Currency

**Meta reports spend in the ad account's currency; the margin in `Targets` is in whatever currency the student thinks in.** If those differ, the breakeven line is wrong by the exchange rate, permanently and in one direction, and reconciliation cannot see it because all three of its legs are Meta-denominated. The check is in [references/meta-token-and-sync.md](meta-token-and-sync.md).

### What happens next, and where the record lives

The weekly analysis reads this Base, and writes a **dated verdict snapshot** back into the same vault note it was pointed at, so that next week can see what last week concluded and whether it was acted on. The Base's own verdicts are live formulas: they rewrite their own history the moment a margin is edited, so the note is the only place where what was decided, on what date, survives.

⛔ **No "keep an eye on it".** If something is worth saying at handover, it converts to a question with an action attached, and everything above does. If it does not convert, it does not belong in the handover.
