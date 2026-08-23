# Naming the problem

Seven verdicts. The student gets **one**, with a number, a comparator, and one action.

**Read direction before level.** The Base holds a 28-day window, a 7-day bracket, and a prior-7-day bracket. Direction comes from the two short brackets; the verdict comes from the 28-day number. A verdict built on 7 days is a verdict one lead can flip.

**When several fit, name the one furthest upstream**, because each upstream problem corrupts the evidence for the ones below it:

**opening-check failures → technical handoff → execution → ad → content → marketing → sales → business model**

Technical handoff sits first among the seven because it is binary and it is the cheapest of the seven to test. Execution sits second because an overloaded team counterfeits both content and sales problems. Fixing a sales problem while the handoff is broken wastes the week.

---

## 1. Ad problem

**Fields:** `ctr` · `frequency` · `cpm`, per ad.

**Criteria.** All three move together: `ctr` falling across the 7-day bracket against the prior 7, `frequency` climbing, `cpm` rising. That combination is one thing, an audience that has been shown this ad until it stopped working.

**One of the three alone is not this verdict.** `cpm` rising with `frequency` flat is auction pressure, seasonal or competitive, and the ad is fine. `frequency` climbing with `ctr` flat means the audience is small but still responsive, which is a targeting width question, not a burn.

**Co-occurs with content problem** when `ctr` is falling and `qualified-rate` is low at the same time. Do not try to separate them from the data: one creative is doing both jobs badly and the numbers cannot tell you which half. Ship the refresh, then read them apart next week.

**Action:** replace the creative on that ad, new hook and new first frame, and hold targeting and budget unchanged so next week's read isolates what the creative did. Name the ad and quote the frequency figure.

---

## 2. Content problem

**Fields:** `ctr` · `qualified-rate`.

**Criteria.** `ctr` at or above the account's own level while `qualified-rate` sits below it. The ad is winning attention and losing fit: it is attracting people, and they are the wrong people.

⛔ **Never call this verdict while opening check 5 is failing.** A stale human side produces exactly this shape, and it is the most common wrong answer this skill can give. Check 5 is what separates "the wrong people are clicking" from "nobody has entered a lead since Tuesday". Confirm it passed, then call it.

**Co-occurs with execution problem** when `capacity-index` is above 1: leads arrive, nobody works them, nobody marks them qualified, `qualified-rate` falls. Same shape again. When both are present, **execution is upstream and execution is the verdict.**

**Action:** move the qualifier earlier. Put the price, the location, or the commitment into the ad copy or the first automated WhatsApp reply, so the wrong people rule themselves out before a human spends time on them. Say which of the three, and why that one.

---

## 3. Technical handoff

**Fields:** `click-to-conversation`.

**Criteria.** A step change, not a drift, and **concentrated across several ads at once**. That is the whole tell: creative decay hits one ad at a time and arrives gradually; broken wiring hits everything simultaneously and arrives on a specific day. If `click-to-conversation` fell on Tuesday across four ads, nothing about your creative changed on Tuesday.

**Co-occurs with nothing, and that is the point.** When this one is live, every downstream number is measuring a leak: fewer conversations means fewer leads means a worse cost-per-qualified on ads that are performing normally. Do not read the cost figures at all this week.

**Action:** click your own live ad, end to end, today, and report the step where it stops. One person, one sitting. ⛔ Do not recommend a creative change, a budget change, or a targeting change while this is open.

---

## 4. Marketing problem

**Fields:** `cost-per-qualified-28d` against `t-cost-per-qualified`.

**Criteria.** Cost per qualified lead above target on the 28-day window, while `ctr` and `click-to-conversation` are within their normal range. The front end works and the price is wrong: you are buying qualified leads, just too expensively.

**Not this verdict when the front end is broken.** If `ctr` is falling, that is verdict 1 and this cost figure is its symptom. Name the cause, not the symptom.

**Co-occurs with, and must be separated from, verdict 7.** Both show poor economics. The difference is one question: are the costs on target. Above target and the target is reachable, this is a marketing problem. On target and the business still loses money, that is verdict 7 and no amount of marketing fixes it.

⛔ **Not callable while opening check 5 is failing.** This verdict is built on a number check 5 voids, and the refusal you already gave the student says that number is not measuring their ads this week. Naming it anyway contradicts your own refusal one screen earlier, and the student cannot detect it: every figure is real, the comparator is their own target, and the recommendation looks exactly right.

**Action:** cut the ad with the worst cost per qualified lead and move its budget to the best one. Name both ads and both figures. One reallocation, not a portfolio rebalance.

---

## 5. Sales problem

**Fields:** `qualified-to-won-rate` · per-handler close rates · `lost-reason`.

**Criteria.** `cost-per-qualified-28d` at or under target while `qualified-to-won-rate` sits below the account's own prior level. The leads were bought cheaply and they are not being closed.

**The per-handler spread sharpens it.** One handler closing at half another's rate on comparable volume is a person or a process, not lead quality. Lead quality does not sort itself by handler.

⚠️ **Every per-handler close rate carries survivor bias.** The average excludes leads nobody ever replied to, so a handler who quietly skips the hard ones posts an excellent close rate. **Report every close rate with that handler's `open-leads` and `reply-miss-rate` beside it, or do not report it.** And when `replies` is blank across the Base, `reply-miss-rate` is unmeasured rather than zero; say unmeasured.

**Co-occurs with execution problem** when `open-leads` is large: a handler with 60 open leads is not underperforming, they are over capacity. Check `capacity-index` before you name a person.

⛔ **Not callable while opening check 5 is failing.** This verdict is built on a number check 5 voids, and the refusal you already gave the student says that number is not measuring their ads this week. Naming it anyway contradicts your own refusal one screen earlier, and the student cannot detect it: every figure is real, the comparator is their own target, and the recommendation looks exactly right.

**Action:** take the single most common `lost-reason` and change one thing about how it is answered. Quote the reason, its share of losses, and the one change.

---

## 6. Execution problem

**Fields:** `capacity-index` · `reply-miss-rate` · `open-leads`.

**Criteria.** `capacity-index` above 1. More leads arriving than the team works through. Nothing here is the ads' fault, and every ad-layer recommendation is premature until it is fixed.

⚠️ **`reply-miss-rate` may be unmeasured.** The `replies` column is not populated by the sync: the Meta reply action type was never confirmed, so it degrades to blank rather than to a wrong number. A blank column is not a perfect response record. Read `capacity-index` and `open-leads`, and say that reply-miss is unmeasured.

**Co-occurs with content problem, and this is the pairing that matters most.** Overloaded team → leads unworked → leads unmarked → `qualified-rate` falls → the numbers read as "wrong people clicking". ⚠️ **These two are only distinguishable while opening check 5 is PASSING.** When check 5 fails, the human side is stale and `capacity-index` is void along with every other qualified-side number (opening-checks.md, check 5 scope), so it cannot arbitrate anything: ⛔ **you may call neither execution nor content**, and saying execution here means capping the budget on a working ad because nobody did data entry. With check 5 passing, `capacity-index` over 1 does make execution the verdict, and the content read waits a week.

**Also co-occurs with sales problem**, same direction: capacity first, close rates after. A handler drowning in open leads has a close-rate number that measures their queue, not their selling.

**Action:** bring `capacity-index` to 1, one of two ways, and say which you picked. Either cap the daily budget on the highest-volume ad, quoting the ad and the new cap, or add the handler, quoting the leads-per-day the team currently absorbs.

---

## 7. Not a marketing problem

**Fields:** `roas-vs-breakeven` on the Account table.

**Criteria.** Persistently below 1 across the 28-day window **and** the prior one, while `cost-per-qualified-28d` is at or under target. That combination is the definition: the marketing hit its numbers and the business still lost money. This is the business model, and no creative, budget, or targeting change reaches it.

**Persistently means two windows, not one week.** One bad week below breakeven is variance and you do not say this sentence about it. Once said, it is heard as "your business does not work", so it earns its evidence.

⚠️ **Currency makes this verdict unavailable.** Breakeven ROAS is **average order value divided by gross margin per order**. An order worth RM 100 carrying RM 25 of margin breaks even at **4.0**, not at 0.25. ⛔ Invert this and every business on earth reads as profitable, which is the one error in this playbook that cannot be caught by looking at the output. The margin in Targets is in the student's own currency while Meta reports spend in the ad account currency. If those differ, `roas-vs-breakeven` is wrong by the exchange rate and wrong silently. The IT-Systems note records the account currency that setup read off the live response. If it does not match the Targets currency, say the verdict is unavailable and why, and stop. ⛔ Do not convert it yourself and present the result as the finding.

⛔ **Not callable while opening check 5 is failing.** This verdict is built on a number check 5 voids, and the refusal you already gave the student says that number is not measuring their ads this week. Naming it anyway contradicts your own refusal one screen earlier, and the student cannot detect it: every figure is real, the comparator is their own target, and the recommendation looks exactly right.

**Action:** state the price. Compute the average order value, or the gross margin, that would put breakeven ROAS below the ROAS the account is actually achieving, and give that number. "At your current return of 1.4, this only works if the margin per order is at least RM <X>, against RM <Y> today." One number, one decision for the owner.

---

## Writing the verdict

- **The named problem is the first sentence.** Then the numbers.
- **Every assertion carries a number and a comparator.** Against target, against the prior 7 days, against another ad, against breakeven. A number with nothing beside it is trivia.
- **One action.** Not a shortlist, not a sequence. If two things genuinely must happen, the second one is next week's, and say so.
- ⛔ **No "keep an eye on", "consider", "it may be worth".** If it does not convert to an action, it does not go in.
- **Where you overrode the Base's own `verdict` on an ad, say so and say which number moved you.** The Base recomputes its verdicts live and rewrites its own history whenever a margin is edited, so it has no memory to defend. You do.
