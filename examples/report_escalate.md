# Example Decision Report — Escalated on Insufficiency

Illustrative only. **M-EX2 is not a record in the dataset** — it exists solely to show field
format, tone and citation style when the chain stops early. Do not treat any value here as an
expected answer.

Note the shape: when the sufficiency gate fails, the chain stops. Fields 4, 5 and 6 are `n/a`
because no classification was attempted. Filling them in anyway would be inventing evidence.

---

## VERDICT

**Field 1 — Recommended disposition:** request documentation

**Field 2 — Escalation reason:** `missing_data` — storefront is insufficient to judge.

**Field 3 — Sufficiency verdict:** insufficient. Four listings are present but every
`description` field is empty and every price is 0. The about page and refund policy are unedited
platform template text. There is no merchant-authored content from which the business can be
determined.

**Field 4 — Inferred category:** n/a — no classification attempted. The chain stopped at the
sufficiency gate.

**Field 5 — Claim comparison:** n/a — no inferred category to compare against the stated
business.

**Field 6 — Policy screen:** n/a — no catalog content to screen.

---

## SUPPORTING

**Field 7 — Terms sizing** `[v2 sample]`
Not assessed. Terms cannot be sized before the business is established.

**Field 8 — Reputation** `[v2 sample]`
No retained flags. One screening hit discarded: same-name entity, different jurisdiction.

**Field 9 — Evidence citations**
- Sufficiency → `storefronts/M-EX2.json` products[0..3] (all `description` = ""),
  pages.about ("Welcome to our store. Add your about page content here."),
  pages.refund_policy (contains unreplaced `[X]` placeholder)
- Review criteria → `policies/review_criteria.md` 2.1 (accuracy of product descriptions),
  2.4 (unedited template counts as absent), 3.1 (storefront substance)
- Escalation → `policies/escalation_rules.md` trigger 2, `missing_data`

**Field 10 — Drafted documentation request** — DRAFTED, NOT SENT. For analyst review and send.

> Subject: Additional information needed for your payment application
>
> Thank you for your application. Before we can complete our review we need a little more detail
> about your storefront. At present we are unable to determine what your business sells.
>
> Please could you:
> 1. Add a description to each of your four product listings explaining what the item is.
> 2. Set a price on each listing — all four currently show as 0.
> 3. Replace the default text on your About page with a short description of your business.
> 4. Complete your refund policy — it currently contains unfilled placeholder fields.
>
> Once these are updated, reply to this message and we will resume the review. We expect to
> reach a decision within two business days of your response.

---

## WHY

Four listings exist, so the storefront is not empty, but none of them carry a description, a
price or a tag, and both policy pages are unedited templates. Listing count alone does not
establish substance (`review_criteria.md` 3.1). There is no basis on which to classify the
business, so no classification was attempted and no verdicts beyond sufficiency are stated.
Asking is the correct action; inferring would be inventing.
