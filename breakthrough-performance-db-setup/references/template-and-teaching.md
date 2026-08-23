# Gates 2 to 5: the template, the teaching, the economics, the clearing

> Load this when gates 0 and 1 have passed and you are about to copy the Base for the student. It covers the four gates that happen entirely inside Lark. The Meta side is [meta-token-and-sync.md](meta-token-and-sync.md); anything that modifies the student's Base is governed by [changing-the-base.md](changing-the-base.md), and gate 5 is one of those things.

**Order is load-bearing here.** Gate 3 teaches on the demo rows and gate 5 deletes them. ⛔ Never clear before teaching. That demo board is the only place the student will ever see all six verdicts side by side; their own board will show two or three of them, and the four they never saw are exactly the ones that will confuse them later.

**Field and table names: read them off the student's own copy.** This file names the ones it needs in order to explain a rule (`Ad Daily`, `Leads`, `Targets`, `Account`, `ad-id`, `date`, `spend`, `is-qualified`). ⛔ Do not treat that as the roster. Resolve everything against what their copy actually contains, at the moment you touch it, for the same reason ids are never hardcoded: this package is not the template's source of truth about itself.

---

## Gate 2: copy the template

### The share link

> **OWNER-SUPPLIED SLOT: the template Base.**
> `https://kalozproduction.sg.larksuite.com/base/Yt2WbfTeQa0tjQsjMUwlfaEvgXb`
> **Master base token: `Yt2WbfTeQa0tjQsjMUwlfaEvgXb`**

⛔ **Do not invent a URL, and do not reconstruct one from a base token you saw somewhere.** If this slot is empty in the copy of the skill you are running, stop and say so: the student cannot be sent to a link that does not exist, and a guessed link either 404s or, worse, lands them somewhere real that is not the template.

### ⛔ The master-Base guard

⛔ **The guard itself is [changing-the-base.md](changing-the-base.md) §0, and that is the only copy.** Run it here, at gate 2, the moment the student has something to point at, and again before every later write. ⛔ Do not restate its outcomes in this file: a second copy of a three-outcome rule is how one of them quietly becomes two.

✅ **Settled: this template is shareable with copy permission, and an outside tenant has taken a copy of it.** Its sharing record reads `link_share_entity: anyone_readable`, `security_entity: anyone_can_view` (that field is the copy, export and print control, and this is its widest setting) and `external_access: true`; the owner separately made a working copy from a real external-tenant account. ⛔ **Do not reopen this as an open question.** If one particular student still cannot copy, the cause is on their side, which is gate 0's business, and not the owner's sharing setting.

✅ **Settled: the Base in that slot is already the purpose-built template, and it is the one to ship.** It is not a working Base that happens to be shared, and there is no second copy to make before publication. ⛔ **Do not reopen this either**, and in particular ⛔ do not propose swapping the slot for a fresh copy, locking the original down, or holding publication until one is made. The open link and the public repo that carries it are the delivery mechanism working as intended: a student who cannot reach and copy this link has nothing to install into.

⚠️ **Both settled lines exist because this question has come back more than once.** Each time it returned, the trigger was a record that stated half of what had been measured, and the reader supplied the missing half as doubt. ⭐ **If a later measurement changes either answer, edit these lines rather than adding a caveat beside them**, because a caveat is what starts the next round.

### The copy itself: you run it

**You make the copy. You do not build the Base, and you do not send the student off to click.**

```
lark-cli base +base-copy \
  --base-token Yt2WbfTeQa0tjQsjMUwlfaEvgXb \
  --name "<something that names this student>" \
  --time-zone "<the student's IANA zone, e.g. Asia/Kuala_Lumpur>" \
  --as user
```

🩸 **`--time-zone` is not optional, and leaving it off fails silently.** Measured 2026-08-24: **the copy does not inherit a time zone**, and the timer inside the copied workflow shifts with it, so the 07:00 you can read on the template is not the 07:00 the student's copy will actually fire at. ⛔ Nothing reports this. The workflow runs, rows arrive, and the run simply happens at an hour nobody chose, which is the same hour Check 3's timezone reasoning at gate 6 assumes it did not. ✅ **Passing the flag holds the timer in place**, measured the same day. ⭐ **Ask the student for their zone before you make the copy**, not after: the copy is where this is decided.

⛔ **Do not read the old instruction "the student clicks copy" as a rule against this.** The rule it came from is about **building** the Base through the API: several hundred calls, every one a failure point, which is the whole reason this is a template and not a provisioner. **A copy is one call.** It sits on the safe side of that rule, not the forbidden side.

⭐ **Nor does running it ask anything extra of the student.** Gate 4 writes Targets, gate 5 clears six tables, gate 7 edits the workflow and gate 8 re-sums the rows, all through the same CLI. The toolchain is required either way, which is why it is gate 0 and why using it here costs nothing new.

**Five conditions, and they hold together:**

1. ⛔ **The student's identity, never the owner's.** The API creates the new Base under whoever's token called it, and there is no parameter that redirects it. The verified direction is the student's own CLI taking a copy of the owner's template; the reverse, pushing a copy into someone else's Drive, cannot be expressed at all.
2. ⛔ **Gate 0 first.** If the CLI channel is not proven, do not attempt the copy and do not retry around the failure. Gate 0 is the fix, or the stop.
3. ⛔ **Read the returned token back with `+base-get` and run changing-the-base.md §0 against it.** `ok: true` is not evidence that a Base exists at the other end of it.
4. **Give `--name` something that names this student.** ⛔ Never accept the default. This is the whole defence against the failure in the next paragraph.
5. **Keep the manual path.** If the call fails after gate 0 passed, hand them the link, let them click, and rejoin at the pass test below. ⛔ Do not loop the CLI.

🩸 **The one new failure this introduces, and it is worth knowing before it happens: two copies.** A student who already clicked copy on their own, and then has the session make one too, ends up with two Bases that are identical in every way the pass test can see. Every later gate then writes into one of them while the student is looking at the other, and **nothing errors at any point**, because both copies are healthy. ⛔ The token check does not catch it: both tokens differ from the master. Condition 4 and the vault note at gate 9 are what catch it. ⭐ **So ask, before you copy, whether they have already made one**, and if they have, use theirs.

⛔ **If you do fall back to the manual path, do not hardcode Lark's UI wording for the duplicate control.** It gets renamed like every other product's menu. Find the current flow at runtime and guide them, the same discipline as the Meta gate.

### After the copy: resolve everything by name

🩸 **`table_id` regenerates on copy** (`lark-lessons.md:706`: `tbl945l1vrceuSZQ` became `tbl8Fts8eOQFZLcV` in the measured copy). ⛔ **So every table this skill ever touches is resolved by name at runtime, in every session, forever.** An id written into this package, into a note, or into a variable that survives a session is a bug that will surface on the second student and look like a permissions problem.

### Pass test

**A same-tenant copy carries everything** (`lark-lessons.md:694-702`, measured 2026-08-22 on an 8 table / ~140 field / 20 block / 1 workflow Base): tables and records, all 43 field descriptions, formulas and lookups and links computing correct values, the dashboard and its blocks, and **the workflow, which comes across and stays disabled** (`:702`).

⭐ **The copy's formulas rebind to the copy** (`lark-lessons.md:704`): pushing a spend row of 999 into the copy moved the copy's ad layer to 7699 while the original stayed at 6700. That was worth measuring for a reason worth remembering (`:705`): **when the two numbers agree, "computed correctly" and "still pointing at the original" look exactly the same.** You do not need to re-run that test per student; it is a property of Lark's copy operation, not of their account.

✅ **A cross-tenant copy has been made from a real external-tenant account and it works** (owner-measured, 2026-08-23, recorded at `lark-lessons.md:709`). ⛔ **The earlier split between "can be taken" and "carries everything" is retired, and reopening it is a known waste**: the pit-book entry originally said only "an outside tenant can take one", and three separate documents each read that silence as an open question and re-raised it. ⭐ **Run the pass test below because it is how you check this student's copy, not because cross-tenant completeness is in doubt.**

**So make gate 2's pass test the measurement**, and it costs one pass over their copy:

1. ⛔ **The base token is NOT the master token** (changing-the-base.md §0). This is item 1 because items 2 to 6 all pass on the master: it genuinely has the tables, the descriptions, the working formulas, the dashboard and the disabled workflow. ⛔ **Every other item on this list is evidence of a healthy Base and none of them is evidence of the student's Base.** ⭐ When you ran the copy yourself, this token is the value `+base-copy` returned, which is a better source than a pasted link, and ⛔ it is still checked. §0 governs every write in every session, not this gate alone, and a later session holds nothing but the vault note and whatever Base is in front of it.
2. All eight tables resolve **by name**, and each has the records you expect (demo rows still present at this point).
3. Field descriptions are present on the fields that carry them. ⛔ Do not eyeball one field; a copy that dropped descriptions drops them everywhere, so check a handful across two tables.
4. At least one cross-table formula returns a nonzero value. A formula that returns zero on a board full of demo data is the silent-rollup family, not a healthy copy.
5. The dashboard exists and its blocks are there.
6. **The sync workflow exists and is disabled.** ⭐ If it is absent, say so plainly rather than working around it: an absent workflow is the first cross-tenant completeness failure anyone has seen, it is worth telling the owner about, and gates 6 and 7 have nothing to wire without it.

📌 **The precondition this gate used to state as unverified is now gate 0**, in `SKILL.md`, and it is a gate with a pass test rather than a note. It was promoted because programmatic access is not a gate 2 nicety: gates 4, 5, 7 and 8 all need it too, so discovering it missing here, or worse at gate 4, means the install was never going to finish. ⛔ If it is absent, run gate 0 or stop there. ⛔ Never improvise a manual walkthrough of eight tables around it.

---

## Gate 3: teach the demo board

⛔ **This is teaching, not an exam.** Never ask "do you understand what this means" and then decide whether to continue based on the answer. A student who says yes to be polite has told you nothing, and a student who says no has just been told they failed something.

✅ **Walk them across the demo rows and let them say what they see.** The demo data was built for this: six verdicts, each with a live example, and two of them deliberately arranged as mirrors.

### The mirror pair, which is the whole lesson

| | A3 | A5 |
|---|---|---|
| Cost per conversation | **RM 11.7**, the cheapest on the entire board (target: 25) | **RM 44**, the most expensive on the entire board |
| Cost per qualified lead | **RM 583** | **RM 200** |
| Verdict | **KILL** | **SCALE** |
| What it is | Cheap garbage traffic | Expensive and precise |

⭐ **Put those two side by side and stop talking for a second.** A3 wins every front-end metric a normal advertiser looks at and is the worst ad on the board. A5 loses every front-end metric and is the best one. Once a student has seen that, they understand why front-end cost alone cannot be trusted, which is the reason this entire system exists. ⛔ You do not need to build up to it, and you do not need to soften it.

### Stop there

⛔ **Do not walk the other four verdicts.** They are on the board and a student who asks gets a one-line answer, but ⛔ do not tour them: the mirror pair is the entire lesson, and everything after it dilutes what they just saw. The other verdicts arrive on their own every week, attached to their own ads, which is a far better teacher than a demo row at install time.

### No gate here, on purpose

⛔ **Do not add a comprehension check.** They re-hear all of this every week when they run `breakthrough-performance-analysis`, and by then it is their own money and their own ads on the board. That is when it lands. A quiz at install time only produces a student who is embarrassed and an installer who feels thorough.

---

## Gate 4: fill in Targets

⭐ **The student says their numbers out loud. You write them into `Targets`.** ⛔ Do not send them into the table to type it themselves.

**The reason is not convenience.** This gate exists to stop one specific error: the average order value being entered into the gross margin field. If you are holding the keyboard, **that error cannot occur at all**, rather than occurring and having to be caught. You put each number in the field you meant it for, because you know which field is which and they do not.

⭐ **And it lets you show the consequence before you commit it**: take their numbers, compute the breakeven ROAS, say it back, and write only after they have agreed with it. A student who types first sees the breakeven afterwards, as a result to be explained rather than a number to be checked.

⚠️ **You are writing to their Base, so this runs under [changing-the-base.md](changing-the-base.md)**: §0's token check first, and read the row back afterwards. ⛔ `ok: true` is not evidence that the numbers landed in the fields you meant.

This is the gate where one wrong number silently poisons every verdict the system will ever produce, all in the same direction, and nothing ever errors.

**Why it propagates:** the pass line for ROAS is not 1.0, it is their **breakeven** point, and breakeven is derived from gross margin. Get margin wrong and every ad on the board is judged against the wrong line for the life of the system.

**Ask for each number with its unit spoken out loud**, and ask for gross margin as **money kept per order after cost of goods**, not as a percentage and not as revenue. ⭐ **Repeat each answer back with its field name before you write it** ("so, RM 40 kept per order after cost of goods, going into gross margin"): that is the moment a misunderstanding is still free to fix. The single most common error is typing the average order value into the margin field, and it is not carelessness: to a business owner "what I make per sale" honestly sounds like the order value.

### Two hardening checks you run on their answers, before writing

1. ⛔ **Gross margin greater than or equal to average order value: judge it dead on the spot.** Margin is a part of the order value by definition. Equality means zero cost of goods, which for a business buying traffic is essentially always a typo. Say what you think happened and ask for cost of goods per order explicitly.
2. ⚠️ **Breakeven ROAS landing at or near 1.0: hard-flag it.** Breakeven is order value divided by margin, so **typing the order value into the margin field produces exactly 1.0**. That is the signature of the error, not a coincidence, and 1.0 is also the number an untrained eye finds most reassuring. ⛔ Never let a breakeven of 1.0 pass without making the student restate their cost of goods in their own words.

### Pass test

⭐ **They have seen the breakeven ROAS their own numbers produce, and they agree with it, before it was written.** Not "the fields are populated". Say the computed number, say what it means in one sentence (below this, the ad loses money even though it looks like it is selling), and get an actual answer. ⛔ Silence is not agreement.

⭐ **Ask which currency these figures are in, and write that down too.** Gate 6 compares the ad account's currency against this answer, and it has no other source for it. A mismatch makes the breakeven line wrong by the exchange rate, silently and permanently, and no reconciliation in this system can see it.

⭐ **Then write, then read the row back and say what landed.** They spoke the numbers and somebody else typed them; the read-back is how they know their words became the right fields.

---

## Gate 5: clear the demo data

**Six tables go to zero. `Targets` and `Account` keep their one row each**, because those two hold the student's own numbers from gate 4 and the account-level settings, not demo content.

⛔ **Forgetting this gate is the quietest failure in the install.** Demo rows and real rows sit in the same tables, every rollup adds them together, and every figure on the board is wrong by an amount nobody can see.

**This is a change to their Base, so it runs under [changing-the-base.md](changing-the-base.md).** Read that file before you delete anything.

### Verify by counting, yourself

⛔ **`ok: true` is not a pass, and neither is an exit code.** Two measured reasons this matters specifically here:

- 🩸 **`+record-get` on a deleted record returns `ok: true`** (`lark-lessons.md:92`). The only tell is an extra `data.record_not_found` key. A script that checked existence per record once reported five records "still present" when the deletes had in fact all succeeded, which nearly sent someone debugging a delete that worked. ⭐ **So the correct verification is a re-list and a comparison of the id set, never a per-record get.**
- 🩸 **`+record-list` caps at 200 rows per page and has no `.data.total`** (`lark-lessons.md:89-90`). `--limit 1000` still returns 200 with `has_more: true`. ⛔ **Never trust a single page's count**, in either direction: page to the end before you say a table is empty, and page to the end before you say it is not.

**So the pass test is:** re-list each of the six tables to the end and confirm zero rows; re-list `Targets` and `Account` and confirm exactly one row each, holding the numbers from gate 4.

### The ad layer comes back on its own, and that is checkable

⭐ **You are emptying the ad layer here, and nobody refills it by hand.** The first sync builds those rows itself: it creates a row for every ad that has spend it cannot already match, and links the daily rows to it. ⛔ **So do not create ad rows at this gate, and do not warn the student that their ad layer is permanently empty.** It is empty until gate 7 runs, which is the design.

⇒ ⭐ **That gives gate 7 a pass test it did not have:** after the first manual run, **the number of rows in the ad layer equals the number of ads that had spend on the day you synced**. ⛔ Neither number is guessed: the first is a re-list, and the second is the same count the student can read off Ads Manager for those dates, which is already in your hands from the pagination check. **Fewer rows than ads means the refill did not complete**, and it will not report that itself.

### When the CLI cannot delete it

🩸 **`lark-cli` cannot delete a Base and cannot delete a workflow** (`lark-lessons.md:707`, `:685`). Records it can clear; those two objects it cannot.

⛔ **If you hit something the tooling cannot delete, hand the student the exact step to do it in the UI, and stop.** Do not loop retrying, do not try a different verb hoping one of them is the delete, and do not leave it half-done and move on. Say what needs deleting, say it has to happen in the interface, and wait.

🩸 **And at this gate you are clearing records, never tables.** ⛔ Do not delete a table to empty it. Deleting a table orphans the workflow that points at it, and `+workflow-update` revalidates the entire step list, so the orphaned workflow can no longer even be renamed (`lark-lessons.md:682-684`; four of six probes ended up stuck this way). Since the CLI also cannot delete a workflow, that state is unrecoverable from the command line, on the student's only copy.
