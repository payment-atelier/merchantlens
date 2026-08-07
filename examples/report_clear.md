# Example Decision Report — Cleared

Illustrative only. **M-EX1 is not a record in the dataset** — it exists solely to show field
format, tone and citation style. Do not treat any value here as an expected answer.

---

## VERDICT

**Field 1 — Recommended disposition:** clear

**Field 2 — Escalation reason:** none

**Field 3 — Sufficiency verdict:** pass — 12 listings, all with substantive descriptions;
about page, refund policy and shipping policy are all merchant-written and specific.

**Field 4 — Inferred category:** 5942 Book Stores.
Top candidate 5942 with 10 supporting listings; runner-up 5947 Gift, Card, Novelty and Souvenir
Shops with 2 supporting listings. Margin 10 vs 2 → **high confidence**.

**Field 5 — Claim comparison:** match. Stated "independent bookshop" against inferred 5942.
No misrepresentation signals: no brand-reference language, no authorisation claims, no pricing
inconsistent with the category.

**Field 6 — Policy screen:** clear. No listing matches any category in
`prohibited_list.md` Section 1. No Section 2 elevated-risk category present.

---

## SUPPORTING

**Field 7 — Terms sizing** `[v2 sample]`
Standard terms. No reserve. Standard payout schedule. No exposure factors identified in the
catalog or fulfilment terms.

**Field 8 — Reputation** `[v2 sample]`
No retained flags. Two screening hits discarded: one same-name entity in a different country,
one historical matter closed in 2023 with corrective action accepted.

**Field 9 — Evidence citations**
- Sufficiency → `storefronts/M-EX1.json` products[0..11]; pages.about; pages.refund_policy
- Classification → `storefronts/M-EX1.json` products: "Hardback Fiction Title", "Poetry
  Collection", "History Paperback" (+7 further book listings); `category_reference.csv` row 5942
- Claim comparison → `merchants.csv` M-EX1 stated_business_type
- Policy screen → `policies/prohibited_list.md` Sections 1.1–1.9, no match
- Review criteria → `policies/review_criteria.md` 3.1, 3.3

**Field 10 — Drafted documentation request:** not applicable — no documentation flag raised.

---

## WHY

Ten of twelve listings are books sold individually by title and format, against a stated
independent bookshop; the two accessory listings do not shift the predominant category. Nothing
in the catalog touches the prohibited list, and the fulfilment terms are proportionate to the
goods. Recommending `clear` for analyst sign-off.
