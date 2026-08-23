# Gate 9: the vault note

> Load before anything touches the student's vault, and re-read it on any second setup run. This file is the entire contract for the one note this skill leaves behind: whether you are allowed to write at all, where it lands, what shape it takes, what it must never contain, and how you avoid destroying the previous run's copy of it.

This note is the only durable output of the whole setup. The Base lives in Lark, the token lives in a workflow, and both are invisible to a future session. This note is how `breakthrough-performance-analysis` finds the Base next week, and how a session six months from now can state what shape this Base is in. If setup finishes and this note does not exist, setup did not finish.

⛔ **The law is the student's own `99_Meta/structure-doctrine.md`, read live, at the moment you write.** This package carries no copy of it and neither do you. Section 8 declares the shape of the note, section 0 is the filing decision tree, section 5 is the law on names. Read them at the time. ⛔ Never a key list quoted here, and ⛔ never a remembered version of section 8: this student may have amended it.

---

## 1 · The probe, which is a check and not a question

**Look for `99_Meta/structure-doctrine.md` in the student's vault. Do not ask whether they have a vault.** People say yes to that question about a folder of markdown files. The file either exists or it does not, and the file is what the rest of this depends on.

**If it is absent: STOP.** Say plainly that this skill writes into a vault built by `my-second-brain`, that this student does not have one yet, and that installing it is the next step. Then stop. The Base itself is fine and the setup work is not wasted; it just has nowhere legal to be recorded yet.

⛔ **Do not degrade to writing a plain note somewhere sensible.** This is the tempting failure and it is worse than stopping. A note in a shape the vault does not recognise is invisible to every mechanism that would otherwise keep it alive: it is not in any family, so the weekly maintenance pass has no opinion about it, nothing checks it, and `breakthrough-performance-analysis` looking for an IT-Systems note will not find it. You would have produced a file that reads as done and behaves as nothing.

⛔ **Do not offer to amend the student's doctrine to make room.** Changing that vault's law is the owner's call, taken with `breakthrough-vault-guardian`, and it is not part of this job.

---

## 2 · Where the note lands

`0[4-9]_<Business>-Business-Wing/01_Assets/IT-Systems/`

**Why that room and not a marketing folder.** The room's own filing test names ad spend explicitly: the rows a system produces (invoices, POS lines, ad spend) stay in the system, and the note carries the pointer plus the monthly snapshot (`my-second-brain/references/rooms-assets.md:115`). This note is exactly that: a pointer note for one Lark Base system. And the Marketing-Assets room kicks the case back here in its own filing test, platform logins and account health live in IT-Systems (`rooms-assets.md:133`). ⛔ A marketing-assets folder is the wrong room, and an earlier draft of this design had it there.

**Resolving `<Business>`.** Scan for wing folders matching that pattern.

- **Exactly one business wing → resolve it silently.** Do not ask a question whose answer you can see.
- **Two or more → ask once**, in one sentence: which business owns this ad account. Then resolve and do not ask again for the rest of the session.

⚠️ **Do not assume `01_Assets/IT-Systems/` exists under that wing.** If the room folder is not there, that is a structural question about this student's vault, not something to create on a hunch. Check the doctrine's section 1 for what that wing is supposed to contain, and if the room genuinely does not exist, say so and stop rather than inventing a room.

---

## 3 · Frontmatter, read live

**The family you expect is `type: it-system` with `status: active`.** That expectation is what tells you which line of section 8 to go read; it is not the shape you write from.

**Read the required and optional keys off the student's own section 8 at write time, and write exactly what it declares.** `holder` is a known optional on that family and is a reasonable place to record who owns the ad account, if that section still declares it. ⛔ Do not hardcode a key list in this file, do not carry one in your head from a previous student, and do not reuse the list from the last time you ran this. Section 8 is amendable and amending it is the whole point of it being the single source.

⛔ **Never invent a `type:`.** An undeclared type is a hard BLOCK from the frontmatter guard, unlike an undeclared extra key, which is only flagged for the session to judge. If section 8 has no `it-system` family in it, this vault's constitution is older than the family this note needs. Say that plainly, say that `breakthrough-vault-guardian` is who opens that door, and stop. ⛔ Do not improvise a family. ⛔ Do not write the note anyway.

---

## 4 · What never enters this note

⛔ **The Meta token. Not the value, not a truncated prefix, not "starts with EAA...".** The vault's iron law is that passwords never enter it, only pointers to where they live (`rooms-assets.md:114`). This is not a style preference: a vault is synced, copied between machines, and read by every future session in full.

**What the note records instead, and it must record all three:**

1. **Where the token lives**: inside the sync workflow in this Base, in the HTTP step's request. That is the pointer.
2. **Who can see it**: anyone with edit rights on the Base can open that workflow and read the token in clear. Write that sentence down. It is the fact that decides who the student may add as a Base collaborator, and it is invisible from the outside.
3. **What to do when it needs replacing**: it is replaced in the workflow, not in this note, and this note is never the place a new one gets pasted "temporarily".

⛔ **Also out: rows.** No ad-level spend tables, no lead lists, no daily numbers. Those stay in the Base. This note is the pointer half; the recurring numbers arrive as the dated verdict snapshots that `breakthrough-performance-analysis` appends (see section 9 below).

---

## 5 · Write with the Write tool, never through the shell

⛔ **No heredoc, no `>`, no `>>`, no `cp`, no `tee`.**

**The reason is mechanical, and it is stated in the guard's own refusal:** the guard reads a Write's content and checks the frontmatter against section 8 before it lands, and it cannot read a heredoc's, so a note written through the shell skips the only gate that would have caught a missing required key or an illegal value (`my-second-brain/scripts/fm-guard-hook.sh:391-401`).

⚠️ **And an append through the shell is blocked too, so do not reach for one as a workaround.** The Bash surface matches `cat`/`tee`/`printf`/`echo` followed by a redirect (`fm-guard-hook.sh:391`), and `>>` contains the `>` that pattern looks for. You will get a block, not a silent success. The correct way to add to this note is section 6.

---

## 6 · Read before write. This is the one that has teeth.

⛔ **NEVER blind-Write this path.**

**The mechanism, so you do not talk yourself out of it.** The frontmatter guard's Write surface returns early on a path that already exists: `if os.path.exists(path): allow()  # an edit, not a birth; this guard watches births` (`fm-guard-hook.sh:345-346`). And Edit is not a matcher at all, the hook registers for Write and Bash only (`fm-guard-hook.sh:3`). So on a second setup run, a Write to this path is not checked, not warned about, and not refused. It replaces the entire file, silently, and the previous run's record plus every verdict snapshot `breakthrough-performance-analysis` has appended since goes with it.

**The procedure, every time, including runs you are certain are the first:**

1. **Read the path first.**
2. **If it does not exist**, write it. That is a birth and the guard will check it and inject the filing protocol.
3. **If it exists**, merge. Keep everything already recorded, update what actually changed, and preserve every verdict snapshot section verbatim. Then write back the full updated content.
4. ⭐ **If it exists and describes a different Base than the one you just set up**, stop and ask. Two Bases in one business wing is a real situation (two ad accounts, an old Base and a new one) and the answer is two notes with distinct names, not one note overwritten. That is the student's call, not yours.

---

## 7 · The filing-log line

**After the note lands, append one line to `99_Meta/filing-log.md`:** date, what, where, which rule decided it.

Two reasons it is not optional. The doctrine requires it of every filing (`my-second-brain/templates/structure-doctrine.template.md:29`), and the weekly maintenance pass reads that log for patterns, so a filing that never appears in it is invisible to the vault's own upkeep. The guard also injects the same demand as the third line of its filing protocol on every note this session creates (`fm-guard-hook.sh:155`).

⭐ **Obey the guard's live demand over this file's paraphrase.** It is quoting the student's doctrine as it stands today; this paragraph is a description of it written earlier.

⚠️ Only a birth gets a log line. A merge under section 6 step 3 is an edit to a note already logged, and does not get a second one.

---

## 8 · What the note records

Prose under plain headings. Written so a stranger opening it cold can operate the system.

**The Base itself**
- Base name, Base URL, `base_token`. `breakthrough-performance-analysis` connects with the token; the URL is for the human.
- **One line per table**, saying what that table holds. The template ships eight tables (`lark-lessons.md:694`, the copy test measured "8 表 / 约 140 栏 / 20 组件 / 1 条 workflow"). This is what saves the analysis session from re-deriving the schema every week.
- ⛔ **The resolve-by-name warning, written into the note in as many words: `table_id` regenerates on every copy** (`lark-lessons.md:706`, measured: `tbl945l1vrceuSZQ` became `tbl8Fts8eOQFZLcV`). Anything operating this Base resolves tables by name. Hardcoding an id is the single most common way a later session breaks this system, and the failure looks like "table not found" against a table that is plainly sitting there.

**The sync**
- The workflow's name, its schedule, and which columns it writes. ⚠️ **It writes two tables, not one, and both belong here**: the raw daily rows plus the link back to the ad, and on the `Ads` table the ad's id, its name, its status and its synced-at time. ⭐ Record them separately, because "nothing is updating" is a different diagnosis depending on which one stopped. This is the first stop when the student says that, and without it the first ten minutes of the conversation are spent finding the workflow.
- The token pointer and visibility facts from section 4.

**Open items** (⛔ this group exists so that a deferred check has somewhere to live; a hand-forward with no receiver never happens)
- ⭐ **The seven-day reconciliation, if gate 8 ran on a single day.** A first install reconciles the one day gate 7 synced, which is thin evidence: small numbers can agree by luck while a join-key or aggregation defect sits underneath. Write it down here as owed, with the install date, and `breakthrough-performance-analysis` clears it on the first run where a full week exists.
- **Whether this copy's workflow carries the day gate, and whether it carries the `Ads` sync.** Gate 7 finds both out by reading the workflow, and ⛔ writes down what was actually seen rather than what this package assumes. The duplicate-row check and the never-retried-truncated-day behaviour hang off the first; the whole ad layer hangs off the second. **Two shape tells, and they are what you record:** a `FindRecord` on `Account` with an `IfElse` outside the loop is the gate, and a second HTTP step together with a find for daily rows whose link is empty is the `Ads` sync. ⚠️ Step count is a fast first read and never the test: measured 2026-08-24, the template carrying both runs 17 steps where the gate-only one before it ran 7, ⛔ but the next template revision moves that number and the shape tells do not.
  ⭐ **This is the case the stamp in Provenance was built for.** The `Ads` sync absent, with a stamp dated before it shipped ⇒ an older copy. Absent, with a stamp dated on or after ⇒ **this copy was altered**, which per `references/changing-the-base.md` §1 clause 3 is reported plainly and ⛔ never migrated.
- Anything else deferred at install, in one line each, with what would settle it.

**Provenance**
- Setup date, and who ran it.
- ⭐ **What `Account.template-version` held when you read it at install, copied across exactly as it stands.** Blank is a real reading and is written down as blank. ⛔ **Do not interpret it here, and do not fill it in from what you believe the template to be**: this line records what the Base said on the day, and `references/changing-the-base.md` §1 governs the only thing it may ever be used for.

**Known limitations**, written as facts about the system rather than as bugs, so nobody spends a day debugging a design decision:
- ⛔ **Retroactive Meta spend corrections are never absorbed.** The sync is idempotent at the day level: once a day's key exists, that day is skipped. That is the accepted cost of the only structure Lark automation allows here, recorded in the pit book verbatim: only day-level idempotency, it cannot pick up numbers the source corrects afterwards; fine for spend and impressions which converge fast, attribution-style numbers that revise backwards over 28 days need another approach (`lark-lessons.md:659`). Consequence for the student: small refunds and invalid-click adjustments Meta posts after the fact will not appear.
- **The `replies` column's status.** As of the source plan it is not filled: the Meta reply `action_type` is unconfirmed, so the column degrades to blank rather than to a wrong number (`Performance-Skills-Planning.md:141`). ⚠️ Write down whichever state is true at setup time. Blank is a limitation the student needs to know about, because a blank column and a genuine zero look identical in a rollup.
- **The currency finding from setup's runtime check.** What `account_currency` actually came back as, and whether it matches the currency the margin figures are typed in. If it does not match, that fact belongs here in bold, because the breakeven ROAS is wrong by the exchange rate and no reconciliation in this system can see it.
- **The timezone finding.** The ad account's timezone and the Lark tenant's timezone, both written down, and whether they agree. A mismatch loses a day at the boundary while everything continues to look correct.
- **What gate 7 actually settled**, not merely what it found: the **timer's time of day** you moved it to, and the **row limit or narrowed filter** on the request if the pagination check made you change either. changing-the-base.md §1a calls these install-time configuration, which is exactly why they have to live here: they are per-student values nobody can re-derive, and a later session that cannot see them will read a deliberately narrowed sync as a broken one.

---

## 9 · What this note becomes after setup

**Setup writes the pointer half. `breakthrough-performance-analysis` writes the other half into this same note, as a dated verdict snapshot appended on every run.** That is the room's filing test satisfied in full: the pointer plus the recurring snapshot (`rooms-assets.md:115`). The mechanics of the append are that skill's `references/verdict-snapshot.md`, not this file's business.

⭐ **Two consequences for you.** Leave the note in a shape that can be appended to: end it cleanly, do not close with a footer that a new section would have to be inserted above. And when you merge under section 6, the snapshot sections are the part that must survive untouched.

---

## 10 · The variant clause

**When the student's funnel did not fit the template and this session helped them build a variant, this note MUST record what was built.**

Say it plainly at the top of its own section: which tables and fields differ from the template, and why they differ. Not "customised for their funnel". The actual differences.

**The reason is that this note plus the skill is the only way a later session can state what shape this Base is in.** The skill declares the template. The note declares the delta. Anything not in either one is a change nobody can reconstruct, and the first session that tries to run an analysis against it will read a missing column as missing data.

**Applied migrations are recorded here too**, in the same section: which migration, on what date. ⛔ **That record is a convenience, never the detection mechanism.** Detection is done by inspecting the Base itself (`references/changing-the-base.md` §1, condition 4). A note trusted as the source of truth about the Base's shape is a label under another name, and it goes stale the first time anyone edits in the UI.

⚠️ **One line is carved out of that, and exactly one: the `Account.template-version` value section 8 records under Provenance.** ⛔ **It is not a claim about shape and must never be read as one.** It is a transcription of what the Base held at install, kept for the single job `references/changing-the-base.md` §1 gives it: telling an older copy apart from an altered one, **after an inspection has already found a difference**. Shape is still inspected, every time, and the paragraph above governs everything else in this note.

⛔ **This file does not carry the migration law.** What counts as a legal change to a student's Base, and what counts as improvisation, is in `references/changing-the-base.md` in this same package. Read it before you change anything in the Base; this section only says where the result gets written down.

⚠️ **When the funnel does not fit and no variant was built, say that here too.** A note that records "walk-in leads cannot carry an ad-id, so the qualified-rate half of this system is not wired for this business" is worth more than silence, because silence reads as "it works" to everyone who opens it later.
