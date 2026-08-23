# The verdict snapshot: what every analysis run writes back

> Load at the end of every analysis run, before anything is written. This file covers finding the student's note, what one snapshot contains, why the write-back exists at all, and the mechanics that stop a second run from destroying the first one.

**Every analysis run appends exactly one dated snapshot to the student's IT-Systems note.** Not a summary of the Base. A record of what this session concluded and recommended, on this date, in the session's own words.

⛔ **The law is the student's own `99_Meta/structure-doctrine.md`, read live.** This package carries no copy of it. You are editing a note that already exists and already carries its family's frontmatter; ⛔ do not touch that frontmatter, and ⛔ do not re-derive it from anything written here.

---

## 1 · Finding the note

**In this order, and stop at the first hit:**

1. **The IT-Systems note.** `0[4-9]_<Business>-Business-Wing/01_Assets/IT-Systems/`, the note `breakthrough-performance-db-setup` wrote at its gate 9. This is the designed path and it is where the Base name, the URL, the `base_token`, the table list, and the known limitations already are.
2. **Grep the vault for `base_token`.** Catches a note that was filed under a different name, or moved.
3. **Ask the student.**

⚠️ Keep this order identical to the one in the analysis `SKILL.md`. Two find orders that disagree is the same failure as no find order: the session takes whichever it read most recently.

**If step 1 and step 2 both miss and step 3 gets you a Base:** you can run the analysis, and you should. But you have found a Base whose declared shape lives nowhere. ⛔ Do not invent a note to hold the snapshot. Say plainly at the close that there is no setup note for this Base, that the snapshot therefore has nowhere legal to land, and that `breakthrough-performance-db-setup`'s gate 9 is what creates it. The contract for that note is that skill's `references/vault-note.md`, and creating it is that skill's job, not yours.

**If the vault itself is absent** (no `99_Meta/structure-doctrine.md`): deliver the analysis in the session and write nothing. ⛔ Do not fall back to a plain note somewhere sensible. A note in a shape the vault does not recognise is invisible to every mechanism that would keep it alive, which makes it worse than none: it reads as done and behaves as nothing.

---

## 2 · What one snapshot contains

One section, headed with the date of the run. Written for a person, not for a parser.

- **Per-ad verdicts as delivered.** The verdict this session gave for each ad it spoke about, with the number that drove it. ⛔ Not a dump of the Base's verdict column; the ads the session actually formed a view on.
- **Every disagreement with the Base's frozen verdict, and why.** This is the highest-value line in the whole snapshot. When you refused a SCALE the Base was showing, or read a KILL as an execution problem the ad is not responsible for, write the Base's verdict, your verdict, and the reason in one sentence. ⛔ **Note which direction is not available to you: below the volume threshold you may not upgrade the Base to SCALE**, because thresholds block optimistic verdicts and never pessimistic ones. Disagreement in this skill runs toward caution, and the snapshot is permanent, so it must not record a call the skill is forbidden to make. ⛔ Recording only your conclusion turns a disagreement into an unexplained contradiction the next session has to relitigate from nothing.
- **The classification conclusions.** Which of the seven problem classes this week's picture falls into, per the classification in the analysis `SKILL.md`, with the evidence that put it there.
- **The one-action recommendations, as given.** Each one converted to a single action, the way they were delivered. ⛔ No "keep an eye on X" here either; if it was not worth an action it is not worth a line in the record.
- **Any opening check that refused.** If the run stopped on sync age, capacity index, an INSUFFICIENT verdict, duplicate rows on `<ad-id>__<date>`, or human-side liveness, that refusal IS the snapshot for the week. Write what refused and what has to be true for next week to proceed. ⭐ A refusal recorded three weeks running is a finding in itself, and it is only visible because each one was written down.

**Keep each snapshot short enough that ten of them still read as a note.** The rows stay in the Base; this is the judgment, not the data.

---

## 3 · Why this exists

**Two reasons, and each is sufficient on its own.**

**The room's own filing test asks for both halves.** IT-Systems: the rows a system produces (invoices, POS lines, ad spend) stay in the system, and this note carries the **pointer plus the monthly snapshot** (`my-second-brain/references/rooms-assets.md:115`). Setup writes the pointer half. Without this, the note is half a note, and "the measurement void, money moving through a system whose numbers nobody ever reads" is the exact failure that room's insight angles name.

**And the Base cannot serve as its own history.** The verdicts are derived from the Targets the student fills in: the pass mark for ROAS is the breakeven point, and breakeven is average order value divided by gross margin. Edit a margin in month three and every verdict the Base displays for month one changes with it, without a trace. So the Base can tell you what it thinks today and can never tell you what it told you last week. Week 2 cannot see week 1's advice unless week 1 wrote it somewhere the Base does not control.

⚠️ **Unverified by this package: whether the shipped template computes that verdict column as a live formula field or stores it statically.** The copy test confirms formulas travel with a copied Base and compute correctly on the copy (`04_Resources/Tools/lark-lessons/lark-lessons.md:694` onward), but it did not measure this particular field. **What would settle it: read that field's definition off a real Base once (a v1 field GET returns the formula) and record the answer in the setup note.** If it turns out to be static, this second reason weakens and the first one stands unchanged, so the write-back is required either way.

---

## 4 · Mechanics

**Every run, in this order:**

1. **Read the whole note first.** Not a grep of it, the whole file. You are about to write it back in full and you cannot preserve what you did not read.
2. **Compose the new section** and place it in the note's snapshot area, after the most recent existing snapshot.
3. **Write back the full updated content with the Write tool.** The path exists, so this is an edit, and the frontmatter guard allows it: `if os.path.exists(path): allow()  # an edit, not a birth; this guard watches births` (`my-second-brain/scripts/fm-guard-hook.sh:345-346`).

⛔ **Never blind-Write this path.** That same `allow()` is exactly why the discipline matters: the guard is not checking you here, and Edit is not even one of its matchers, the hook registers for Write and Bash only (`fm-guard-hook.sh:3`). A Write composed from memory of what the note "probably says" replaces the entire file, silently, taking the setup pointer, the known limitations, and every previous snapshot with it. There is no error to notice.

⛔ **Never append through the shell.** No `>>`, no `cat`, no `tee`, no `printf`. The guard's Bash surface matches those commands followed by a redirect (`fm-guard-hook.sh:391`) and `>>` contains the `>` it looks for, so you will be blocked; and where it does not block, it cannot read what you wrote, which is the reason the block exists in the first place.

⛔ **Snapshots are append-only. Never rewrite a previous one**, not to correct it, not to tidy it, not because this week proved last week wrong. Last week's advice being wrong is information, and the way you record it is a line in this week's snapshot saying so. A record that gets quietly corrected is worth nothing, because nobody can tell which parts were corrected.

⚠️ **When the note gets long, it gets long.** ⛔ Do not silently prune. If the student wants old snapshots archived, that is their call and their maintenance ritual's job; say it out loud and let them decide.

**No filing-log line.** The doctrine's log line belongs to a filing (`my-second-brain/templates/structure-doctrine.template.md:29`); this is an edit to a note that was already filed and already logged when setup created it. ⭐ If the guard injects the demand anyway on some run, obey the guard: it is quoting this student's doctrine as it stands today, and this paragraph is not.

---

## 5 · What never enters a snapshot

⛔ **The Meta token, in any form.** The vault's iron law is that passwords never enter it, only pointers to where they live (`rooms-assets.md:114`). If a run of this skill saw the token at all, the snapshot records nothing about it beyond what setup already wrote: where it lives, and that anyone with Base edit rights can read it.

⛔ **Rows.** No ad-level tables, no lead lists, no daily spend lines. They stay in the Base. A snapshot that starts reproducing the Base's contents has stopped being a snapshot.

⭐ **Names of people, only when the finding is about a person's workload and the student already knows it.** A per-handler sales finding is a legitimate conclusion; it is also a durable written statement about a named employee sitting in the business owner's vault. Write it as the number and the pattern, and say it to the student before it lands.
