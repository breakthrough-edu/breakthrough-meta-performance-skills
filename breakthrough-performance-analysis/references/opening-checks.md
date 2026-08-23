# The opening checks

Step zero plus five checks. **All of them run before any advice, every run, including the run where the student says "just tell me if I should scale ad 3".**

They exist because this Base can be stale, half-fed, duplicated, or renamed and still return numbers that sum, format, and read as facts. Nothing here raises an exception. The checks are the only thing standing between a broken Base and a fluent analysis of nothing.

---

## Step zero: the schema probe

**Before any check runs, resolve every table and every field this session will touch, by name.**

`table_id` regenerates when a Base is copied (`04_Resources/Tools/lark-lessons/lark-lessons.md:706`, verified: `tbl945l1vrceuSZQ` became `tbl8Fts8eOQFZLcV`). Names are the only stable handle. The table inventory is in the student's IT-Systems note, one line per table; read it there.

**Resolve, do not assume.** Two field names are declared in the student's own copy and are not knowable from this package: the Leads table's own date field, and the Targets table's volume-threshold field. Look them up. If either is absent, say so and refuse the checks that depend on it (check 3 needs the threshold, check 5 needs the Leads date).

**A miss is a refusal, in these words or their equivalent:**

> Your copy does not have `<name>` on `<table>`. Either this copy predates the current version of the skill, or that field was renamed by hand. Without it I cannot tell you `<the thing it answers>`. Everything else below still stands.

⛔ Never substitute a similarly named field. ⛔ Never treat an unresolved name as an empty column. **A blank is unmeasured; a missing field is broken; a zero is a number. Those are three different things and you never merge them.**

**Why this is a gate and not a formality.** Five measured failure modes, all of which turn a name miss into a plausible number rather than an error:

| Failure | What it returns instead of an error | Source |
|---|---|---|
| Cross-table rollup whose result column is itself a formula or lookup | silent 0 | `lark-lessons.md:213` |
| Rollup routed through a link field, over sync-written rows | silently omits exactly the new rows, because sync-written rows never auto-attach to links | `lark-lessons.md:680` |
| Link field sharing its target table's name | the whole table's values concatenated, for every row | `lark-lessons.md:95` |
| Two-stage `REGEXEXTRACT` that misses its target | blank, no error, no wrong number | `lark-lessons.md:673` |
| Two datetimes compared for same-day equality without `TEXT()` | silent 0 or blank | `lark-lessons.md:196` |

---

## Check 1: Sync age

**Computed from:** `last-synced` on the Ads table, which is one of the four columns the sync actually writes. **You compute the age yourself**, in this session, by comparing that timestamp to the current time. Cross-check it against the newest `date` in Ad Daily and say so if the two disagree.

⛔ **Do NOT read a field called `sync-age-hours`, and do not trust one if you find it.** Two independent reasons, and either alone is fatal:

1. **The sync no longer writes it.** It was one of seven sync-written columns until 2026-08-22, when all seven were converted to formulas; the surviving sync contract on the Ads table is four columns (`ad-id`, `ad-name`, `effective-status`, `last-synced`) and this is not among them (`Meta-Performance-Skill-Idea.md:63`, `:90`, `:98`).
2. **It cannot be a live formula.** An age in hours needs `NOW()`, and `NOW()` is computed once when the field is created and never again: a probe re-read 45 seconds later returned a value identical to nine decimal places (`lark-lessons.md:176`). `TODAY()` escapes this, but only at day granularity, so minute-level and hour-level "how long since" remains impossible (`lark-lessons.md:189`).

⇒ ⛔ **So a copy that still carries that field carries a fossil frozen on the day the field was made, and it will read as a small, healthy, entirely plausible number forever.** That is worse than the field being missing, because a missing field is visible. If you find it, name it to the student as a stale leftover and compute the real age from `last-synced` instead.

**Threshold.** These are this skill's declared defaults, not measured constants. Set them from the student's actual sync schedule, which the IT-Systems note records:

- Sync runs once a day: **stale above 36 hours** (one missed run plus slack), **hard refusal above 72**.
- Sync runs twice a day: halve both.

**Failure story.** The sync writes four columns on the Ads table (`ad-id`, `ad-name`, `effective-status`, `last-synced`) and the raw rows into Ad Daily; everything else in the Base is a formula over `TODAY()` windows. ⭐ **Graded, because this check is built entirely on that sentence.** **Measured 2026-08-24**, end to end on a copy: the sync creates the Ads rows, writes `ad-id` and `ad-name`, and stamps `last-synced` on the same run. ⚠️ **Unverified: `effective-status`.** The write path was proved with a stand-in value; that Meta's own status string arrives and lands in that column has never been observed, and per the select-option behaviour a value with no matching option fails **at the moment of writing**. ⛔ So do not read a blank or unchanged `effective-status` as an ad that did not change. **`last-synced` is the column this check depends on, and that one is measured.**

⚠️ **A copy taken before 2026-08-24 does not write any of them**, and the tell is on the Ads table itself: a `last-synced` identical across every row and never moving. ⛔ **Treat that as a Base whose sync age cannot be read here**, not as a healthy old sync. Those formulas keep computing perfectly against data that stopped arriving. A Base that last synced two weeks ago will produce a complete, internally consistent, confidently worded read of a fortnight-old month. Nothing about the output looks stale. This is the single cheapest way for this skill to lie.

**Scope of the failure: everything.** Do not proceed to the ad layer. There is no partial version of this one.

**Refusal:**

> Your data last came in <N> hours ago, and the sync is meant to run <schedule>. Every number below that is computed from a window that has stopped moving, so nothing I could tell you about your ads this week would be about this week. **One thing to do today: open the sync workflow in the Base and run it manually once, then tell me whether new rows landed in Ad Daily.** Next run I check the sync age first and, if it is current, I read the full 28-day window from scratch.

---

## Check 2: Capacity

**Computed from:** `capacity-index`, on whichever table the schema probe resolved it to (the IT-Systems note's table inventory names it; ⛔ do not assume). Above 1 means more leads are arriving than the team is working through.

**Threshold:** `> 1`. This one is not a judgment call; it is the field's own definition.

⚠️ **Check 5 can void this check's input.** `capacity-index` is computed off human-entered Leads rows, so when check 5 fails, this number is stale in the same way and by the same amount. ⛔ Run check 5 before you act on check 2: if check 5 fails, say capacity is unmeasured this week rather than reporting a figure, and do not use it to choose between execution and content.

**Failure story.** SCALE advice on a team already over capacity buys leads that nobody answers. The money converts into `open-leads` and nothing else, and next week the numbers get worse in a way that looks like an ad problem, because cost-per-qualified rises while the ads themselves are unchanged. You will have caused the thing you then misdiagnose.

**Scope: the scaling half only.** The read continues. KILL verdicts still stand, cost diagnoses still stand, and a sales or content finding is still worth saying. What is suspended is every recommendation that adds volume.

**Refusal, spoken before the ad findings and not instead of them:**

> Your capacity index is <N>, which means <N times> more leads are arriving than your team is closing out. I am suspending every scale-up recommendation this week: adding volume on top of that converts budget into unanswered leads. **One thing to do today: <either cap the daily budget on the highest-volume ad to bring the index to 1, or add the handler>, and say which you chose.** The rest of the read follows, and next run I check capacity again before I let a SCALE through.

---

## Check 3: Volume, per ad

**Computed from:** the `verdict` field on each ad. INSUFFICIENT DATA means the engine declined to conclude because spend has not passed the threshold in Targets.

**Threshold:** whatever the Targets table declares. Resolve that field by name in step zero. ⛔ Do not invent one, and ⛔ do not lower the student's own bar because the ad looks promising.

**Failure story.** Underneath the threshold, a single lead moves cost-per-qualified by tens of percent. An ad that looks like the best performer in the account on four conversations is noise wearing a costume. This is the same rule as "28 days not 7", applied to spend instead of time.

**Scope: that ad only.** Every other ad still gets a verdict.

**The asymmetry, and it matters:** a volume threshold blocks optimistic verdicts and never pessimistic ones. Below threshold you may not say SCALE. You may still say KILL, because an ad that has spent real money and produced nothing has already told you what you needed to know.

**Refusal, per ad:**

> <Ad name> does not have enough behind it yet to call. It has spent RM <X> against a threshold of RM <Y>, so it needs roughly RM <Y minus X> more at the current daily rate, about <N> more days. I am not giving it a verdict either way. <If applicable: its cost per qualified lead is currently RM <Z>, which looks good, and that is exactly the number a threshold exists to distrust.>

---

## Check 4: Duplicate rows

**Computed from:** the Ad Daily row key, which is `<ad-id>__<YYYY-MM-DD>` (`lark-lessons.md:660`). Count rows per key across the last 14 days. **Any key with a count above 1 is a failure.**

⛔ **Page to the end before you add anything up.** `+record-list` caps every page at 200 rows and returns `has_more: true`, and raising `--limit` does not change that: a request for 1000 still returns 200 (`lark-lessons.md:89`, whose closing instruction is **never trust a single page's count**). Keep calling with `--offset` until `has_more` is false, then sum. Seven days of thirty delivering ads is 210 rows, so an ordinary account crosses the cap. ⚠️ **A single page looks exactly like a complete answer**: no error, a plausible total, simply low.

⛔ **Counting one page here does the opposite of what this check is for**: it under-counts rows, so duplicates on later pages read as clean.

**Threshold:** exactly 1 per key. There is no tolerance band.

**Failure story, measured.** A workflow whose loop only adds records writes the same batch again on every trigger: three triggers, three rows per key, nine rows where there should be three, and **the sums silently triple with no error** (`lark-lessons.md:651`). Lark's automation cannot branch per row, because conditional branches cannot nest inside a loop, and there is no delete-record action at all, so the daily rows are defended by a gate outside the loop that checks whether the account was already synced today. ⚠️ **The ad layer is defended by something else entirely**: a row that already exists has already had the daily rows linked to it, and a filled link never comes back out of the find that creates rows. ⛔ **Two mechanisms, and a Base can have one working and the other not.**

Whether the copy in front of you has either of them wired is not something the schema probe can tell you. **Run the count regardless**, and run it on both layers.

**The direction of the error is worth stating to the student:** duplicates inflate spend, so every cost-per metric inflates and ROAS deflates. A duplicated Base tilts the entire read pessimistic. You will recommend killing ads that are fine.

**What this check cannot do.** It finds extra rows and never absent ones. A day truncated on the way in produces fewer rows than it should, the key count stays at 1, and the check passes clean. Worse, **if this copy's workflow carries the day gate**, it skips any date whose key already exists, so a day that landed half-empty is never retried: it stays wrong forever and looks normal. ⚠️ **Whether it does is recorded in the IT-Systems note's Open items, from what gate 7 actually saw in the workflow.** ⛔ Read it there rather than assuming; without the gate the failure is the opposite one (duplicates), and the two need opposite responses. **Say this out loud when check 4 passes.** "No duplicates" is not "complete".

**Scope: everything.**

**Refusal:**

> <N> days in your Ad Daily table have more than one row for the same ad. That triples the spend those days contribute, which pushes every cost figure up and every return figure down, so the whole read below would tilt toward killing ads that are actually fine. **One thing to do: open Ad Daily, sort by the row key, and remove the duplicate rows for <list the dates>.** Tell me when it is done and I will re-count before reading anything else.

⛔ **Do not delete them yourself.** Not because you cannot: `+record-delete` exists and works, batched at 200 ids per call (`lark-lessons.md:159-160`). Because an analysis run is a read, and a read that quietly destroys rows in the student's Base is the last thing they would expect from asking what their ads did this week. Hand them the step, and stop. If deletion ever becomes this package's job it goes through [changing-the-base.md](changing-the-base.md) as a declared migration, never as a session's own idea.

---

## Check 5: Human-side liveness

**Computed from:** the newest row in the Leads table, by that table's own date field (resolve the name in step zero). Compare it against the newest `date` in Ad Daily.

**Threshold, this skill's declared default:** the human side is stale when the newest Leads row is more than **3 days** older than the newest Ad Daily row. The gap is the signal, not the absolute age: a business with no ads running this week has no leads to enter and is not stale.

**Also check `replies`.** That column is not populated by the sync: the Meta reply action type was never confirmed, so it degrades to blank rather than to a wrong number. **A blank `replies` column means `reply-miss-rate` is unmeasured, not excellent.** ⛔ Never read a blank as perfect responsiveness.

**Failure story, and this is the one that produces the most confident wrong answer in the whole skill.** `qualified`, `is-qualified`, and the Leads rows are typed in by a human. The machine half keeps syncing perfectly. So spend keeps climbing, lead rows stop arriving, and `qualified-rate` sags smoothly. The shape that makes is **identical to a content problem**: clicks fine, qualification low, "you are attracting the wrong people". The truth is that nobody has done data entry since Tuesday. The student then rewrites their ad copy to fix an admin gap, and next week it looks worse.

**Scope: every qualified-side number.** `qualified-rate`, `cost-per-qualified-28d`, `qualified-to-won-rate`, and `capacity-index` are all void. The pure ad-layer numbers survive: `ctr`, `cpm`, `frequency`, and `spend` come from Meta and do not depend on anyone typing. Say which half you are still standing on.

⛔ **While check 5 is failing, you may not call ANY verdict whose criteria include a voided number.** ⛔ Do not memorise a list of two: apply the rule. Every verdict above reads its fields off the criteria line in the classification playbook; if any of `qualified-rate`, `cost-per-qualified-28d`, `qualified-to-won-rate` or `capacity-index` appears there, that verdict is unavailable this week. Today that rules out **content, execution, marketing, sales, and the business-model verdict**, leaving the ad layer and the technical handoff, both of which read Meta-sourced numbers no human types.

**Refusal:**

> Your ad data is current to <date>, but the last lead anyone entered was <N> days ago, on <date>. Everything that depends on someone marking a lead qualified is therefore not measuring your ads, it is measuring your data entry: qualified rate, cost per qualified lead, close rate, and capacity index are all off the table this week. **One thing to do today: <name the person> enters the leads from <date> to now, and tells you when it is caught up.** I can still tell you what is happening at the ad level, because click-through, CPM, frequency and spend come from Meta and do not need anyone to type them. Next run I compare the two dates again first.

---

## Speaking order and passing checks

**Failures first, in check order, before any ad finding.** Two failures means two refusals, both spoken, before the read. ⛔ Do not bury a failed check under a heading at the bottom.

**When all five pass, say so in one line, not five.** "Data is current to yesterday, no duplicate rows, leads entered through Thursday, capacity at 0.7." The student needs to know the checks happened, not to read them. And attach the honest caveat: this confirms no duplicate rows, not that no day is missing.
