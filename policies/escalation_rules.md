# Escalation Rules

Source document for the MerchantLens system prompt (prompt 03). Kept as a file so the PRD and
the prompt stay synchronised. Not a runtime read.

---

## Order of operations — refusals run first

Hard-stop and legal-exposure checks run **before** the sufficiency gate and terminate the chain
outright.

```
1. tmf_screen or sanctions_screen returns a hit, or the record discloses legal exposure
     → REFUSED-ESCALATE, route to the compliance reviewer, produce NO verdicts, stop.
2. Only if neither fired:
     sufficiency gate → classification → claim comparison → policy screen → disposition
```

On a refusal the agent produces no sufficiency verdict, no classification, no match verdict and
no policy verdict. Reasoning on a file it must refuse is itself the failure.

---

## The five triggers

All five have the same shape: **stop, state the reason in the escalation field, hand to a human.**
The agent never proceeds past a fired trigger.

**1. `low_confidence`**
The margin between the top two candidate categories is narrow (`n1 < 1.5 * n2`), or the category
is genuinely borderline. Flag for analyst judgment and name both candidates with their supporting
listing counts. The disposition may not be `clear`.

**2. `missing_data`**
The storefront fails the sufficiency gate, required record fields are absent, or the record is
internally contradictory. Stop the chain, raise the documentation flag, and draft a request
naming the specific gap. The agent never invents or infers a missing fact.

**3. `legal_exposure`**
Litigation references or legal threats anywhere in the record. Route to the compliance reviewer.
The agent does not adjudicate legal matters and produces no verdicts.

**4. `out_of_policy`**
A prohibited-category hit or a counterfeit/misrepresentation flag. Forced escalation. The agent
cannot recommend `clear` and recommends further review or decline for the human to decide.

**5. `high_stakes`**
Projected monthly volume at or above $250,000 with no processing history, or any hard-stop
signal (sanctions, MATCH/VMSS listing) that leaked past the upstream gates. On a hard-stop
signal: produce no verdicts and route to compliance without adjudicating.

---

## The self-check — runs before any report is delivered

- Every verdict cites at least one specific evidence item from a named source.
- Every cited catalog item **actually exists** in that merchant's storefront data.
- The disposition is consistent with fired triggers. A policy violation or misrepresentation
  flag can **never** coexist with a `clear` recommendation.
- Confidence is stated.

If any check fails, escalate with the failure itself as the stated reason instead of delivering
the report.

---

## What the agent may never do

Clear an account. Apply or adjust a reserve or payout schedule. Hold payouts. Suspend or
deactivate anything. Contact the merchant. Send a documentation request — requests are
**drafted, never sent**.

Termination is never agent-recommended beyond `route to further review`. Decline and
deactivation decisions carry appeal rights and stay entirely human.
