# Changing a student's Base

This file ships identically inside `breakthrough-performance-db-setup` and `breakthrough-performance-analysis`, because each skill has to carry it into context on its own. The two copies are byte-identical on purpose. If they ever differ, one of them is wrong (see the repo README).

Read this before you touch a student's Base with anything other than a read.

---

## 0. Before ANY write: confirm whose Base this is

⛔ **This runs first, every session, in both skills, before the first write of any kind.** Not once at install. Every session, because a later session inherits nothing but the note and the Base in front of it.

**The template is the owner's live production Base, not a pre-made copy.** Its token is the master:

> **Master base token: `Yt2WbfTeQa0tjQsjMUwlfaEvgXb`**

**Read the base token of whatever you are about to write to, and compare.**

- **Different: correct.** This is the student's own copy. **Carry that token forward to gate 9**, which is where the vault note is created; if the note already exists, confirm it holds this token. ⛔ Do not write a note here to hold it: the note has one author and one contract, and it is gate 9's.
- ⛔ **The same: STOP.** You are pointed at the owner's live business data. Say so plainly, write nothing, and do not continue any gate. ⛔ Not "just clear the demo rows", not "just fill Targets", not "just check one thing". Every one of those writes.
- ⛔ **Cannot obtain a token to compare: also STOP.** An unknown identity is not a pass. Say you could not establish whose Base this is.

⚠️ **How to get the token, because guessing at this is how the check quietly inverts.** Take it from what you actually connect with, not from a URL the student pasted. **A `/wiki/` URL carries a node token, which is NOT a base token, and many do not begin with `wik`** (`lark-lessons.md:50`): resolve a wiki URL to its base token first, or you will compare two different kinds of string, find them different, and report "correct" from inside the master.

**The resolution, verbatim from `lark-lessons.md:50`:** `lark-cli wiki +node-get`, then read `data.obj_token`, valid when `data.obj_type` is `bitable`. ⚠️ Because many node tokens do not begin with `wik`, the CLI's auto-detect may treat a bare token as a raw obj_token, so pass the **full URL** rather than the bare token.

⚠️ **Why a human cannot be trusted to catch this instead.** Lark opens a Base link in place. Nothing on screen says whose Base it is, the table names are identical, and the demo rows the student expects **are genuinely there**, because they are the master's. A student who never duplicated sees exactly what a student who did would see, and so does the session. The token is the only thing that differs, which is why the token is the check.

---

## 1. The rule

**The only legal change to a student's Base is one that is skill-rule-driven and reproducible.** Four conditions, and it needs all four:

1. **Declared in the skill.** The change is written down in a skill file *before* any session runs it. Not decided in the session.
2. **Versioned with the skill.** It ships and changes with the skill's own text, so "which migrations exist" has exactly one answer at any moment.
3. **Recorded in the student's vault note.** The IT-Systems note says this migration was applied, and when.
4. **Detectable as already-applied by inspecting the Base at run time.** You open the Base and look for the field, the formula, the workflow step. If it is there, the migration ran.

⛔ **Shape is decided by inspection, never by a label.** A label is a *claim* about the Base, and a claim about shape goes stale the moment anyone edits by hand in the UI, which students do. A runtime inspection cannot go stale, because it is looking at the thing itself.

**One stamp exists, `Account.template-version`, and it answers a different question: which day this copy was taken from the template.** That is the copy's origin, not its shape, and hand-editing the Base cannot make it false, because an edit does not give the copy a different source. It exists because that question is the one thing inspection is structurally unable to answer: **inspection sees that a field is absent; it never sees why.** The same observation has two causes that want opposite responses. **This copy is older than the field** wants a migration. **Someone deleted or renamed it** wants the exact reverse, per §1a's `Never migrate a variant`, which that section calls the worst outcome available here. Without the stamp, a session facing an absent field has two moves: guess, or stop.

**Three clauses, and the stamp is legal only under all three:**

1. ⛔ **Shape is always judged by inspection. The stamp takes no part in that judgement, not once.**
2. ⭐ **The stamp is read only after an inspection has already found a difference**, and only to choose between "an older copy" and "somebody changed this".
3. ⛔ **When the two disagree, inspection wins, and the disagreement is itself the finding.** Something the stamped version should carry is absent ⇒ **this copy has been altered.** Report that plainly; ⛔ do not migrate it.

⚠️ **A blank stamp is a real reading, not a fault.** It means the origin is unrecorded, which is exactly where every copy taken before the stamp shipped already stands, and a session treats it as a pre-stamp copy. A stamp holding a value the Base contradicts is caught by clause 3.

⛔ **The banned thing is improvisation**: a change you thought of in the moment, applied because it seemed better, and wrote down nowhere.

**The test, and apply it honestly:**

> Can the next session, using only the skill and the vault note, state what shape this Base is in?

If it cannot, what you did was improvisation, whatever your intentions were. Good intentions are not one of the four conditions.

**Why this is strict.** A student's Base is a copy of a template snapshot taken on the day they installed it, and copies never update themselves. So the population of student Bases fans out over time, and the only thing keeping any of them legible is that every difference is one the skill can *name* and *detect*. One undocumented hand-edit and the next analysis session is reading a Base of unknown shape while behaving as though it knows the shape. That session will not report uncertainty; it will report numbers.

⭐ **Most of the time the right answer is no change at all.** If the student's funnel does not fit the template, say so plainly: that is a real answer and it is honest. Bending the Base to make a bad fit look like a good one is how you get a system that reports confidently about a business it no longer describes.

## 1a. Three kinds of change, and only one of them needs the four conditions

The four conditions above are written for **migrations**. Applied to everything, they forbid two things the product actually does, so name which kind you are in before you reach for them.

**Install-time configuration.** Gate 7 fills the workflow's placeholders: the token, the request URL and its filters, the timer's time of day. These are per-student values the template ships as blanks, and filling a blank is what installing means. ⛔ The four conditions do not apply, because there is nothing to declare in advance and nothing for a later session to detect as "applied": the value is simply what this student's install holds. ⚠️ **The read-back duty applies in full** (§2), and every value you settle on goes in the vault note, because the next session cannot re-derive them.

**Migration.** A change to a Base that is already installed and working, made because the skill now expects a different shape. **All four conditions, no exceptions.** This is the case the rule was written for.

**Student-owned variant.** The student's funnel does not fit and, per the setup skill, the session may help them build their own version rather than pretend the template describes their business. ⛔ **This can never satisfy conditions 1 and 2** (nobody declared it in advance, and it does not ship with the skill), and that does not make it illegal. It makes it a different thing. What it owes instead:

1. **The vault note records what was built**, concretely enough that a session which has never seen this Base can tell what is there. This is condition 3 doing the whole job alone.
2. ⛔ **From that moment the skills treat this Base as unknown shape**, and `breakthrough-performance-analysis` resolves every table and every field by name before computing anything, refusing in plain language when something it needs is absent. It never assumes the template's shape again for this student.
3. ⛔ **Never migrate a variant.** A migration written against the template's shape, applied to a Base that no longer has that shape, is the worst outcome available here: it will mostly succeed and silently corrupt the parts that differ.

**The test at §1 still governs all three.** A variant that nobody wrote down fails it exactly as improvisation does, and for the same reason.

---

## 2. Read-back reconciliation, after every change

**Byte by byte, against what you intended, every time.**

- ⛔ "No error" is **not** a pass.
- ⛔ `ok:true` is **not** a pass.
- ⛔ An exit code is **never** evidence.

Two named traps make this non-negotiable rather than merely careful.

### Trap 1: `+field-update` silently wipes `formatter`

`+field-update` goes through v3 and is a **full PUT**, while v3 neither reads nor returns `property.formatter`. So the command treats that attribute as non-existent and clears it along with everything else it cannot see (`formatter` becomes `''`, `type.ui_property` becomes `null`), and returns **`ok:true`**.

⚠️ **The symptom is almost invisible.** The numbers are still there. The formula is still right. The column has just stopped displaying as a percentage or a currency. And **you can read back on v3 a hundred times and never see it**, because that key is simply absent from v3's output.

- **Verify:** only a **v1 GET** of the fields endpoint (`/open-apis/bitable/v1/apps/<app>/tables/<tbl>/fields`) shows `property.formatter`.
- **Repair:** only a **v1 PUT**, sending the whole `property` back (`formatter` plus `formula_expression` plus `type.{data_type,ui_property,ui_type}`). Measured: this preserves the field's description and its formula too, so there is no trip to the UI.
- **Order is fixed: v3 first, v1 last.** Do the expression and description on v3, then restore the formatter on v1. Reversed, the v3 step wipes the formatter you just repaired, and returns `ok:true` while doing it.
- **Discipline:** before *any* `+field-update`, take a v1 GET snapshot of `property`; afterwards, take another and compare. Especially for percentage, currency and decimal-place attributes, because display-only attributes are exactly the ones nobody thinks to read back.

Two details that will cost you a round otherwise: `lark-cli api --data @file` accepts **only a relative path** (an absolute one is rejected outright, so pipe the body in instead), and `formula_expression` in a v1 body must be the **internal form** `bitable::$table[tblXXX].$field[fldXXX]`, not `[Field Name]`. GET once and copy the existing string rather than composing one.

### Trap 2: the same command's rate limit leaves mixed state

`+field-update` is rate limited (`800004135 OpenAPIUpdateField limited`), and **it blocks only part of a batch**. Measured: eight fields changed in one go, five succeeded, three blocked, leaving a formula chain that was **half new and half old**, which is neither the version you left nor the version you meant to arrive at.

- **Reconcile field by field against your intent**, then re-run only the fields that were blocked.
- ⛔ **Never blind-retry the batch.** Re-sending an update to a field that already took is not a no-op: per Trap 1, every `+field-update` is another chance to wipe a formatter, including one you just repaired on v1.
- ⛔ Declaring the job done on the strength of the `ok:true` responses is precisely the failure mode. Those responses are true and the Base is still wrong.

---

## 3. Deletion

**Two containers cannot be deleted through the CLI: a Base, and a workflow.** (`+base-*` offers only create / copy / get / block-*; workflows offer disable / create / get / update / enable / list.) This is a fact about the tool, not about your permissions, so no amount of retrying changes it.

⚠️ **This does not generalise, and do not let it.** `+record-delete` exists and is measured working, batched at 200 ids per call (`lark-lessons.md:159-160`); fields and tables have their own verbs too. ⛔ Writing "the CLI cannot delete" as a blanket fact sends a session to hand the student manual work it could have done, and a stated capability limit that is false is the kind of thing the next reader inherits without rechecking.

**So when something must be deleted: hand the student the exact UI step, and stop.** ⛔ Never loop retrying the CLI. It will fail the same way each time, and the only thing it consumes is the session.

### Cleanup order: workflows first, tables last

⛔ **Delete a table first and you orphan every workflow pointing at it.** `+workflow-update` **revalidates the entire step list**, so a workflow with a single step referencing a deleted table is rejected outright (`table_name: Input invalid: 'tblXXX' not found`), even when all you wanted to change was its title. Combine that with "the CLI cannot delete a workflow" and that workflow is now **neither editable nor deletable**: it can only be disabled, or removed by hand in the UI.

Measured 2026-08-22, doing it backwards: 4 of 6 probe workflows jammed this way.

✅ **Correct order:** deal with the workflows first (rename, disable, or delete in the UI), and only then touch the tables.

---

## 4. Resolve by name, always

**`table_id` regenerates on every copy.** Measured on a copy of the template Base: `tbl945l1vrceuSZQ` became `tbl8Fts8eOQFZLcV`.

Every student's Base *is* a copy. So **any procedure keyed to a table id is broken on every student's Base by construction**, not occasionally, not on unlucky ones. On all of them.

- **Resolve tables by name.** Every time, at run time.
- **Field ids are per-Base too.** Resolve fields by name as well, and fall back to looking up the id *in that Base* only where the name cannot address it: a field name containing `%` (or `/`) 404s when addressed by name.

**What the copy does carry**, measured on an 8-table, roughly 140-field, 20-block, 1-workflow Base: all tables and their records; all 43 field descriptions; all formulas, lookups and links, computing correct values; the dashboard and its blocks; and the workflow, arriving **disabled**.

⭐ And the copy's formulas bind to **the copy's own tables**, not back to the original. That was proved by manufacturing a disagreement rather than by observing agreement: one row of 999 spend into the copy's Ad Daily moved the copy's ad layer to 7699 while the original stayed at 6700. (When two numbers match, "computed correctly" and "still pointing at the original" look identical. Only a state where they *should* differ can tell them apart. Use that method whenever you need to prove isolation.)

**Cross-tenant copy is verified** (2026-08-23): a person in a different tenant can take the template. That question is closed; do not reopen it.

---

## 5. The sync-shape facts any migration must respect

The template's sync was built inside these constraints. Change the Base without respecting them and you can break the sync **without any error appearing anywhere**. All measured 2026-08-22.

**No conditional branch inside a Loop.** An `IfElseBranch` placed under `loop_start` is rejected: `[code=800004006] cannot nest if-else nodes under switch or loop`.
⛔ **Consequence: the per-record branch is impossible.** "For each row, find it, then update if present or add if not", the first thing almost everyone reaches for, cannot be written as a branch inside the loop. **That half is permanent.**

✅ **Upsert itself is buildable, by a route that does not need a branch.** Measured 2026-08-24: `FindRecordAction` and `SetRecordAction` **can** sit inside a Loop, and their filter conditions can reference the loop item. Only the branch cannot. So the decision stops being something the workflow computes and becomes **a state the workflow writes down**:

- **Each row, at the moment it is written, does a `FindRecord` for its parent and puts the result in a link field.** Found ⇒ the link is filled. Not found ⇒ the link is left empty. Nothing branches; the link simply records what was true.
- **A `FindRecord` outside the loop then collects every row whose link is empty.** That set is exactly "the keys with no parent row", no more and no less.
- **A second loop creates each missing parent and immediately sets the link on every row carrying that key.** Once filled, those rows can never appear in the empty-link find again.

⭐ **What makes this an upsert is that the decision is a written state rather than a recomputation.** It does not depend on a formula, on a time window, or on a count, so nothing about it drifts. ⛔ **Do not read the branch limitation as "upsert cannot be done here".** It cost a full build to discover otherwise.

**No delete-record action.** The whole action list is `AddRecordAction` / `SetRecordAction` / `FindRecordAction` / `HTTPClientAction` / `Delay` / `LarkMessageAction` / `GenerateAiTextAction`.
⛔ **Consequence: clear-and-reload is impossible too.** Both of the obvious escape routes are closed.

**A bare `AddRecordAction` honestly duplicates on every trigger.** Measured: the same workflow fired 3 times, returning 3 rows each time, giving 9 rows in the table, three copies of every key. **Sums silently multiply. Nothing errors.**

**The legal shape is the day-gate:** `FindRecord` plus `IfElse` **outside** the loop, gating on a predictable key. Measured in both directions: with the key present, the whole batch is skipped (table stayed at 9 rows); with the table cleared, 3 rows were written. A branch *outside* a Loop is legal, and a Loop *under* a branch is legal. Only "branch inside Loop" is banned.

**What the template ships is that gate followed by the link-state upsert above, in two stages.** ⛔ Read the workflow before you reason about any of it; this is the shape as of 2026-08-24 and a student's copy is whatever day it was taken.

```
STAGE 1, the gated daily load
  Trigger       timer
  FindRecord    Account, sync-last-run is today            <- the gate reads this
  IfElse        nothing found ? then carry on              <- outside the loop, and it has to be
  HTTPClient    Meta Insights
  Loop  over the returned rows
    FindRecord  the ad's row on the dimension table
    AddRecord   the daily row, link set to what was found (empty when nothing was found)

STAGE 2, the missing parents, then the refresh
  FindRecord    every daily row whose link is empty        <- the ads with no row of their own
  Loop  over those rows
    AddRecord   the ad's row
    SetRecord   fill the link on every daily row carrying that ad's key
  Loop  over the insights rows again
    SetRecord   the ad's name
  HTTPClient    the ads endpoint, for status
  Loop  over that response
    SetRecord   the ad's status, and its synced-at time
  SetRecord     Account, sync-last-run = now               <- the gate's own stamp, and it is last
```

⛔ **The gate governs stage 1 only, and the two stages are protected by two different mechanisms.** Stage 2 does not repeat itself because of the gate. It does not repeat itself because **an ad whose row already exists has already had its link filled**, so it never comes back out of the empty-link find. ⚠️ **Conflating the two is how a migration breaks one while testing the other**, and the break is silent in both directions.

⛔ **`Account.sync-last-run` is what the gate asks about**, not the daily row key. `<ad-id>__<YYYY-MM-DD>` is still the key every rollup joins on; it is no longer the key the gate reads.

⚠️ **The gate stamps last, so a stamping failure is invisible on the run that causes it.** The rows land, the run looks healthy, and the duplicate does not appear until the next trigger. ⇒ **Verifying the gate means reading `Account.sync-last-run` back**, never counting rows.

⚠️ **One boundary that is real and has not been closed.** If many days of rows for the same ad are pasted in by hand *before* the first sync, all of them carry an empty link, the empty-link find returns all of them at once, and duplicate parent rows are created. Following the install order, where the demo data is cleared first, this cannot arise. It is still there.

⚠️ **Its cost, and this is the half that gets dropped when people summarise it: daily idempotency only.** You never absorb the source's after-the-fact corrections. That is fine for spend and impressions, which converge fast. It is **not** fine for attribution-type numbers that Meta revises backwards over 28 days, and **Meta's invalid-click refunds are never picked up by this shape at all**. Re-running does not fix that, because the gate sees the key already exists and skips the day. If a migration needs retroactive correction, say plainly that this shape is not the tool. ⛔ Do not quietly assume a re-run will absorb it.

**Run count is per trigger, not per loop iteration.** Checked in the automation panel: 5 triggers with 3 loop turns each showed **5 runs**. So the number of ads does not move the meter; they all run inside one trigger's loop. Twice-daily syncing is about 60 runs a month against Starter's 1,000. ⛔ Do not scare a student out of a design with "50 ads times 12 syncs equals over quota"; that arithmetic is wrong.

**Write types come from the `response_value` sample JSON, not from the actual response, and that is what saves you.** Symptom: Meta Insights returns `spend` and `impressions` as **strings** (`"200.00"`) while the Base column is a number, and workflow creation is rejected outright (`reference ... resolves to pathType "string". This referenced variable path can only use [...] pathType(s)`). Fix: write those values as numbers **in the sample** (`"spend":200.00`, not `"spend":"200.00"`). And execution really converts. Measured against httpbin's echo endpoint, which always returns strings: with the sample declaring a number, an actual returned string `"200.00"` was written as the number `200`. This is not fooling the validator; the conversion is real.

⚠️ **But date columns do not convert.** Meta's `date_start` is the string `"2026-08-21"` and it will not go into a date column. The template's workaround: **the date rides inside the row key** (`<ad-id>__<YYYY-MM-DD>`) and is read back with `TODATE(RIGHT([row-key],10))`. The value is written once and the date column is a formula. ⛔ A migration that "helpfully" adds a real date column fed directly by the sync will fail validation.

**Values buried in nested object arrays cannot be reached by variable reference.** The path tree is derived from the `response_value` sample and has no concept of "the Nth one" or "the one that matches": `$.loop_1.item.reviews[0].rating` is rejected with `path "item.reviews[0].rating" does not exist in the output path tree of step loop_1`. Meta Insights' `actions` is exactly this shape, `[{action_type, value}, ...]`.

✅ **The way through: dump the whole array into a text column, then extract with a TWO-stage `REGEXEXTRACT`.** A nested array writes into a text column with its JSON structure intact (`{"comment":"...","rating":3,"reviewerName":"..."},{...}`); a plain string array collapses to a comma-joined string instead.

```
stage 1   REGEXEXTRACT([raw], "\{[^{}]*TARGET_NAME[^{}]*\}")   ← isolate the one object
stage 2   REGEXEXTRACT([stage 1], "VALUE_KEY.:([0-9]+)")       ← pull the number out of it
```

⭐ **Two stages, not one, because key order inside the objects is not guaranteed.** Measured: within a single response, some objects carried `rating` before the name and some after. A single regex that assumes adjacency is right sometimes and wrong sometimes, which is worse than being wrong always.

⚠️ **Wrap the whole thing in `IF(...,0)`.** Measured: rows containing the target return the correct number; rows **not** containing it return **blank in both columns, with no error and no wrong number**. Blank and zero are not the same thing downstream, and an absent action reading as blank will quietly poison an average.

**Rollups walk text keys, never link fields.** ⚠️ **Two facts that sound like one, and they have come apart.**

**Fact one, changed:** the link *does* get written now. Since 2026-08-24 the sync fills it, using exactly the `FindRecord`-inside-the-Loop step this rule once cited as the reason it was not done, plus a `SetRecord` in stage 2 for the ones that found nothing.

**Fact two, unchanged:** ⛔ **a rollup still may not walk that link.** A link filled by a later pass is not filled when the row lands, and a link that found nothing stays empty until stage 2 reaches it, so `[reverse-link].[column].SUM()` reads a value that is still moving and **silently omits rows**: the number is wrong and nothing anywhere says so. ⛔ Do not read "the sync links now" as permission to write a rollup across the link. Write it as:

```
[Table].FILTER(CurrentValue.[key]=[key]).[column].LISTCOMBINE().SUM()
```

so the entire system runs on text keys, which are the one thing any sync can reliably write. Measured: 10 rollups converted this way, and not a single number changed.

**Workflows created via `+workflow-create` arrive disabled** and need `+workflow-enable`. That is a good property, not an annoyance: a skill can build the thing without it starting to run on its own.

---

## 6. Provenance, and what wins

Every fact in this file was **measured on the author's own Lark tenant on 2026-08-22, 2026-08-23 and 2026-08-24**, on the Base this template was built from. They are measurements, not readings of documentation. That distinction matters here: Lark's documented behaviour and its actual behaviour diverge often enough that a documented claim and a measured one are not the same grade of fact, and this file carries only the measured grade.

⭐ **A contradicting observation on the student's own account wins.** If you run one of these procedures and the Base does something else, the Base is right and this file is stale.

- Say so out loud in the session, in plain words.
- Write down what you actually saw: the command, the response, the state before and after.
- Report it back so the skill can be corrected.

**The student's account is the only account whose answer matters.** ⛔ Do not talk yourself into "it must have been something else on my side". That is exactly how a stale rule outlives the behaviour it was describing, and once it has been inherited once, nothing will ever discover it was wrong.

⚠️ **If you add a fact to this file, mark its grade.** Either **measured** (with the date and what was run) or **unverified** (with what would have to be measured to settle it). An unmarked claim gets inherited as fact by the next session, and an inference written as a declarative sentence is indistinguishable from a measurement six months later. That has already cost two days once.
