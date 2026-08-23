# Gate 6 and Gate 7: the Meta token, and wiring the sync

Load this when the student holds their own copy of the template Base, the demo rows are cleared, and nothing is flowing into it yet. Two gates, in this order.

- **Gate 6 ends** when one live Insights call has succeeded with the student's token, the token has been shown to be the right *kind* of token, and the three runtime checks below have all been answered on this student's own account.
- **Gate 7 ends** when you have read real rows out of `Ad Daily` with your own eyes.

⛔ **Do not open gate 6 before the demo data is cleared.** Reconciliation at gate 8 sums spend out of `Ad Daily`, and it cannot tell demo spend from real spend. Clearing after real rows have landed means clearing both.

---

## Gate 6: the token

### Posture: accompany, do not operate

**The student logs in, passes 2FA, and clicks. You verify end states.** Three reasons, and each one bites in a different way:

1. **The credentials are theirs.** Login and 2FA are not yours to hold, and a token that passes through your context is a credential that now exists in one more place than the student authorised.
2. **The console gets redesigned.** Any click path written into this file is wrong within a couple of months, and a confidently wrong click path is worse than no path: the student trusts it, does not find the button, and concludes the material is broken.
3. **What actually blocks people is not the clicking.** It is one of four settings being absent, and none of the four is visible from the screen where the token is generated. That is a verification job, which is the half you are good at.

⛔ **Never write a click path into this skill, and never recite one from memory.** At runtime, search for Meta's current documentation for creating a System User token with Marketing API access, then guide from **what is actually on the student's screen**: ask them to read out the section headings and button labels they can see, and name the next control by the label they just read. If their screen does not match what you found, believe the screen.

**The four end states you are steering toward** (these are stable, the clicks are not):

| # | End state | Why it exists |
|---|---|---|
| 1 | An app exists, and **Marketing API is added to it as a product** | Without the product, ad endpoints are simply not served to that app |
| 2 | A **System User** exists in Business Manager | The only kind of identity whose token does not die when a person's session does |
| 3 | The System User is **assigned to this specific ad account** | Business-level admin is not account-level access; this is the one people skip |
| 4 | The generated token carries **`ads_read`** | Read is all this system ever needs; do not accept a broader scope "to be safe" |

### End-state verification: one Insights call

**Make one Insights call with the student's token. One success proves all four at once**, and it keeps proving them after Meta redesigns every screen in the flow.

**Use the URL the template already contains.** The copied Base brought the sync workflow with it, and that workflow's HTTP step holds the exact endpoint shape this system uses. Read that URL out of the workflow, substitute the student's ad account and token, and narrow the date range to something short and recent. ⭐ Composing a URL from memory is how you end up verifying a different endpoint from the one the sync will actually call.

**Token hygiene during the check:**

- The token exists in exactly two places when you are done: the student's clipboard, and the workflow placeholder.
- ⛔ It never enters the vault note, and it never enters a file.
- If you run the verification call from a shell, pass the token through an environment variable set for that single command and never echo it, so it does not land in shell history. A token in `~/.zsh_history` is a token you gave away.

### When the call succeeds, it still is not finished: provenance

⚠️ **A short-lived user token passes that call too.** This is the named typical failure of gate 6: the student ends up on a screen that hands out a personal access token, it works perfectly in front of you, and it dies days later.

**The cost of accepting it is not "the sync breaks loudly".** It breaks into silence: Meta stops returning rows, the workflow keeps running and writing nothing, and if the day gate is in place, each empty day is keyed and never retried. Weeks later the numbers look thin rather than absent, and thin numbers get read as "advertising got worse".

**So inspect the token before you accept it:**

- A **System User token** is issued long-lived or non-expiring.
- A token that reports an expiry hours or a couple of months out is **the wrong token**, no matter that the Insights call passed.
- On the wrong token: say so plainly and route back to end state 2. ⛔ Do not accept it "for now" and do not try to extend it. The student re-does one flow today, or re-does the whole diagnosis in two months without knowing that is what they are doing.

⚠️ **Unverified, and it must be measured on the student's own first run:** Meta exposes a token-debug endpoint that reports a token's app, type, and expiry, but **the exact endpoint and field names in that response were never measured during this build**, and no live Meta call was made while writing this file. Look up the current shape at runtime, read what actually comes back, and treat the field you find rather than a field you expected. What would settle it once: one debug call against a known System User token and one against a known short-lived user token, compared side by side, and the two responses recorded.

### When the call fails: name which of the four is missing

⛔ **Never report a generic failure.** "Could not connect to Meta" sends the student back into a console with four candidate causes and no way to choose.

**Read the response body, not the HTTP status.** Meta's own message, its `code`, and its subcode carry the cause; the status alone flattens four different problems into one number.

Then walk the four in this order, cheapest first:

1. **Does the token work for anything at all?** If no call succeeds, the problem is the token or the app, not the ad account.
2. **Is this ad account reachable by that token?** If other calls succeed but this account is not visible, the System User is not assigned to this account (end state 3, the usual answer).
3. **Is the permission there?** A token that sees the account but is refused ad data points at `ads_read` (end state 4).
4. **Is Marketing API on the app?** If the endpoint itself is not served, the product was never added (end state 1).

⚠️ **The mapping from Meta's specific error codes to those four causes was not measured during this build.** Use the response body and the order above; do not hard-code a code table you cannot vouch for. If a student's failure produces a clean signature, record it in their vault note so the next session starts one step ahead.

---

## The three runtime checks

**These replace measurements that were never going to be done centrally.** No amount of testing on one account tells you anything about another: a five-ad account never paginates and a five-hundred-ad account always does. **The only account whose answer matters is the one in front of you**, so all three checks read the student's own account, off the same verification call you just made.

⛔ **Run all three before gate 7 writes anything.** Two of the three become invisible once rows are in the table.

### Check 1: pagination

Three steps, at the first live Insights call:

1. **Count the rows the response returned.**
2. **Compare that count to the number of ads that actually delivered in the range you asked for.** The student can read that off Ads Manager for the same dates. Rows should be fewer than or equal to that number (ads with no delivery return nothing); rows *cut short* of it is the signal.
3. **Look for a `paging` key in the response body**, and read what is inside it rather than only whether it exists.

   ⚠️ **Graded: unverified.** No live Meta call was made when this package was written. What is measured is Lark's side, not Meta's. Some Graph endpoints return a `paging` object carrying only cursors on a complete single-page response, in which case bare presence would fire on every account and the post-remedy test below could never pass. ⛔ So treat **a next-page link** as the evidence, treat bare presence as a prompt to look closer, and let step 2's row count decide when the two disagree. What would settle it: one call on an account small enough to fit in a page, and read the response.

⚠️ **Why this check has to pass before the first day's rows are allowed to stand:**

- **Missing rows look exactly like normal data.** There is no gap, no error, no zero. There is a table of plausible numbers with some ads absent from it.
- **The duplicate-row check cannot catch it.** That check counts rows per `<ad-id>__<YYYY-MM-DD>` key and flags anything greater than one, so it finds rows that should not be there and is structurally blind to rows that are not there.
- **The day gate never retries.** Once a date's key exists, that date is treated as done. A day that arrived truncated stays truncated forever.

⇒ **Therefore: pagination must pass before you accept the first day.** After that day is written, nothing downstream will ever tell you it was short.

**If the response carries a next-page link** (not merely a `paging` object, per the grading on step 3 above: bare presence may appear on complete responses too, and step 2's row count is the tiebreak), say it plainly: *this account is bigger than the single-call shape this sync was built for, and the sync cannot be trusted until that is fixed.* Then choose with the student:

⛔ **You are at gate 7, still configuring the workflow, so a change to the request is install-time configuration and not a migration** (changing-the-base.md §1a). It still carries the read-back duty: after any change, re-run this check and confirm **the next-page link is gone and step 2's row count now matches the delivering-ad count**. ⛔ Do not use the bare `paging` key as the exit test. Per the grading at step 3 it may never disappear, and an exit test that cannot pass silently drives every account to the third option.

- ✅ **Raise the request's row limit**, if the endpoint accepts one large enough to cover the whole account in a single call. Re-run the check.
- ✅ **Narrow what one request asks for** by restricting the filter on the existing workflow's single call. Re-run the check.
  ⛔ **Do NOT solve this by creating additional workflows** (one per campaign or otherwise). `lark-cli` has no verb that deletes a workflow, so every one you add is permanent for that student and can only be disabled, or removed by hand in the Lark UI. A shape that needs N workflows to cover one account is a shape that gets worse every time the account grows.
- ✅ **Accept that this account does not fit**, and say so. This template makes one call per run and does not follow a cursor. That is a real answer, and on a big account it is usually the honest one.

⛔ **Whichever you pick, do not invent a fourth option in the session.** If none of these three works, the answer is the third one.

⚠️ **The same ceiling sits on the workflow's other request, and this check does not look at it.** The sync also calls the ads endpoint for each ad's status, and that call takes a row limit the same way and follows no cursor either. ⇒ **An account big enough to paginate Insights is big enough to truncate that one too**, and the symptom is different enough to misread: the daily rows arrive complete, while some ads keep a stale status and a stale synced-at time. ⭐ **Apply the same three options to it**, and note that when the third option is the honest answer for Insights it is the honest answer for both.

⚠️ **Unverified: whether Lark's HTTP step can chase a `paging` cursor at all.** What is measured is that Lark's variable references are a fixed path tree derived from the sample response, with no concept of "the next one", which is why arrays cannot be indexed. Whether an HTTP step's URL can be built from a previous step's output was **never tested**. ⛔ Do not promise the student a cursor-following sync. What would settle it: one probe workflow whose HTTP step references a prior step's output in its URL, and whether it validates and runs. ⛔ **Never build that probe in a student's Base.** A workflow cannot be deleted through the CLI, so a probe left behind is permanent for them; this belongs in a throwaway Base owned by whoever maintains this package.

### Check 2: currency

**Read `account_currency` off the same response.** If it is not in the response, add it to the requested fields and call again.

**The default assumption of this whole system is MYR**, and ⛔ **a default is not a measurement.** The margin and average order value that went into `Targets` at gate 4 are in whatever currency the student thinks in, and **gate 4 is where that currency gets asked and written down** (the session does the writing there, so it is holding the answer already). ⇒ **Compare `account_currency` against what gate 4 recorded**, not against a hardcoded MYR. ⛔ If gate 4 somehow did not record it, ask now and record it now; do not assume. Comparing against the default makes the check pass for a student who typed SGD while their account runs SGD, and fail for one who typed MYR against an MYR account only if you forgot to ask.

⚠️ **If the account currency differs from the currency the Targets figures were typed in, say it LOUDLY, at gate 6, before anything is wired.** ⛔ Not "is not MYR": a student who typed SGD against an MYR account is the mismatch and would sail past an MYR test, while a student who typed MYR against an MYR account is fine and would not. Meta reports spend in the ad account's currency. The margin is in the student's own currency. Breakeven ROAS is average order value divided by gross margin per order, and those two arrive in different currencies here, so a mismatch does not produce an error, it produces a **breakeven line that is wrong by the exchange rate**, permanently, in one direction. Every verdict in the Base then leans the same way, and it looks like a business result rather than a units bug.

⛔ **Gate 8 cannot catch this.** All three of its legs (Ads Manager, the Insights pull, the re-summed rows) are denominated in the ad account's currency. They will agree with each other perfectly while the margin sits in a different currency entirely.

**One action on mismatch:** the student either re-enters `Targets` in the ad account's currency, or changes nothing and accepts that every verdict is wrong. There is no third option, and there is no exchange-rate field in this template.

### Check 3: timezone

**Two clocks, and nobody set them together:**

- **The Lark timer's "yesterday" is the tenant's timezone.**
- **Meta Insights' dates are the ad account's timezone.**

**Compare the two.** Read the ad account's timezone off Meta and the tenant's timezone off Lark, and if they differ, say which day boundary drifts and in which direction.

**What a mismatch does:** the timer fires and asks for a date that, in the ad account's clock, has not finished yet. Meta answers with a partial day. That partial day is written, its key now exists, and the day gate will never ask for it again. Nothing errors. Spend for that date is simply low, forever, for every ad.

**One action:** move the timer to a time of day that is safely past the ad account's midnight in both clocks (later is cheaper than earlier; the data is a day old either way).

⚠️ **This is a workflow edit, so it carries the same two duties as every other one:** read the workflow back after the change and confirm the timer actually holds the new time (a returned `ok` is not evidence), and record **both timezones and the timer time you settled on** in the vault note. Recording only the timezones leaves the next session able to see the offset but not what was done about it.

---

## Gate 7: wire the sync

### What the sync actually does, so you can tell wired-wrong from working

**It does no arithmetic.** Every rolling window in this Base is a live formula over `TODAY()`, so the sync's entire job is: pull Meta's raw rows, write them into `Ad Daily` and the dimension tables, and stamp a sync time. Nothing is computed on the way in. If a window number looks wrong, the sync is rarely the culprit; the rows underneath it are.

**Four shape facts you need in your hands, because the student cannot look them up:**

1. **Write types are decided by the sample response, not by the live response.** Lark derives each column's type from the sample JSON pasted into the workflow's `response_value`. Meta returns `spend` and `impressions` as **strings** (`"200.00"`), so if the sample shows them as strings, the workflow will not even validate against numeric columns. Declaring them as numbers in the sample is not a trick to pass validation: it was measured that execution really does convert the incoming string `"200.00"` into the number `200`.
2. **Date columns cannot be converted that way.** No sample declaration makes a date string land in a date column. That is why **the date rides inside the row key**, `<ad-id>__<YYYY-MM-DD>`, written once as text, and the date column is a formula that reads it back out with `TODATE(RIGHT([row-key],10))`. ⛔ Do not "fix" that by trying to write the date directly. It was tried, and it does not convert.
3. **Every rollup in this Base joins on the text key, not on link fields.** Rows written by a sync do not attach themselves to link fields, so a rollup written as `[reverse-link].[column].SUM()` **silently omits every synced row** while still returning a plausible number. The whole system runs on the text key because a text key is the only thing a sync can reliably write.
4. **Values buried in Meta's nested arrays are extracted after the fact, not during the write.** Lark's variable references cannot index or filter an array, so the array (for example Insights' `actions`) is dumped whole into a text column and mined by a **two-stage** `REGEXEXTRACT`: first isolate the one object containing the target name, then pull the number out of that object. Two stages because **key order inside those objects is not guaranteed**, measured within a single response: a one-shot regex that assumes the value sits next to the name is right some rows and wrong others. And a row that does not contain the target returns **blank, with no error and no wrong number**, so the formula needs an `IF(...,0)` wrapper to be complete.

### The day gate, and the one thing it cannot do

**The shape is drawn in full in [changing-the-base.md](changing-the-base.md) §5, and it is drawn there only.** ⛔ Do not restate it here. What you need at this gate is why it makes a same-day re-run safe, and what it costs.

**The branch sits OUTSIDE the loop, and it has to.** Lark refuses to nest an if-else inside a loop, so the per-row branch that everyone reaches for first cannot be written. ⚠️ **That is a limit on the branch, not on upsert**: §5 has the route that does work, and it does not use a branch. There is also no delete-record action, so "clear it and reload" is not available either. The gate outside the loop was measured working in both directions: with the key present the whole batch is skipped, and with the table emptied the same trigger writes its rows again.

**Without that gate, a plain add-record workflow repeats itself honestly.** Measured: the same workflow triggered three times, three rows each, left nine rows in the table, three copies of every key, **and the totals silently tripled with no error**.

⚠️ **The cost of the gate, and it is permanent:** the system is idempotent **per day only**, so **numbers the source revises after the fact are never absorbed**. Meta's retroactive corrections (invalid-click refunds are the common case) land on a date whose key already exists, and that date is never fetched again. Spend and impressions converge fast enough that this is acceptable; attribution-style numbers that Meta rewrites weeks backwards are not covered by this design at all.

⇒ **That limitation belongs in the student's known-limitations list**, so that a future session does not spend an afternoon debugging a difference that is working as designed. File it per [references/vault-note.md](vault-note.md).

⚠️ **A student's copy is whatever day it was taken, so read this copy's workflow before you run anything** rather than assuming what the current template ships. One question: **is there a `FindRecord` on `Account` with an `IfElse` branch outside the Loop?** ⛔ It reads `sync-last-run`, not the daily row key; a session hunting for the row key will conclude the gate is absent when it is present.

- **Gate present:** a manual re-run on the same day is safe, and the check above is the reason you know that.
- **Gate absent:** run the workflow **once**, and treat every extra manual run as a table you will have to clean by hand. Tell the student that out loud before you run it.

⛔ **Adding the gate to a student's Base is a change to their Base.** It runs under [references/changing-the-base.md](changing-the-base.md), or it does not happen. Do not improvise it in the session.

### The wiring, in order

1. **Find every placeholder by reading the workflow, and let the workflow tell you how many there are.** ⛔ **Do not carry a count into this step**, and do not assume their names either. **Read how many HTTP steps this workflow has**, then read each step for its own: each one carries **a request URL** (it holds the ad account) and **a token**. Replace all of them with the student's values.

   ⛔ **Missing one is silent, which is why the count has to come from the workflow.** A wrong or absent token comes back as Meta error **190**; a wrong URL comes back as an **empty array**. ⚠️ **Neither interrupts the rest of the chain.** A half-wired workflow therefore runs to completion, writes the rows fed by the step you did fix, and leaves the table fed by the step you missed simply unchanged. It is not distinguishable from a table that had nothing to update.

   ⇒ ⭐ **So the check is per step, not per workflow:** after wiring, run once and confirm that **every table this workflow writes has moved**, not only the first one.
2. **Any edit to that workflow is a Base change.** Follow [references/changing-the-base.md](changing-the-base.md) for how it is made and how it is read back. Do not restate its rules here and do not work around them.
3. **Enable it.** A copied Base brings its workflow across **still disabled**, by design, so that nothing starts moving in someone's account without a person saying so. It will not run until you enable it.
4. **Run it once, manually.**
5. **Read the rows back.** See below.

⚠️ **Two edit-time traps worth knowing before you touch it:**

- **A workflow update re-validates every step**, not the one you changed. If any step points at something that no longer exists, the whole update is rejected, including a rename.
- **There is no delete-workflow action in the CLI.** A workflow can be disabled, or deleted by hand in the Lark interface, and that is all. ⛔ So do not create a second "test" workflow to try something out: you cannot take it back from here.

### Verification: read the rows, not the exit status

⛔ **"No error" is not a pass, and neither is a green run in the automation panel.** Every failure this system can produce returns a plausible number and no error. That is the entire character of the thing.

**Read back three things:**

1. **Row count** in `Ad Daily` for the date you just synced. Compare it to the number of ads that delivered on that date. This is the same arithmetic as the pagination check, now on the writing side.
2. **Spot values.** Pick two ads, and compare their `spend` and `impressions` in the table against the same two ads in Ads Manager for the same date. Two rows is enough to catch a units error, a truncation, or a column landing in the wrong place.
3. **The key.** Confirm the row keys read `<ad-id>__<YYYY-MM-DD>` and that the date column resolved to a real date rather than sitting blank. A blank date column means the `TODATE(RIGHT(...))` formula did not get the shape it expected, and every window in the Base is about to read zero.

**Two failure signatures recorded during the build** (recorded from the wiring, not re-measured on every account): a bad token comes back as Meta error **190**, and a wrong URL comes back as an **empty array**, which writes nothing and reports success. ⭐ The empty array is the dangerous one: a workflow that runs perfectly and writes nothing looks identical to an account that spent nothing.

**Nothing is finished here.** Rows landing correctly for one day proves the wiring. It does not prove the totals. That is gate 8, in [references/reconciliation-and-handover.md](reconciliation-and-handover.md), and it is a gate, not a formality.
