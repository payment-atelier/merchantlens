# MerchantLens

A merchant risk verification agent for a self-serve commerce platform's payments risk team.

**Live: https://voxvulgaris.github.io/merchantlens/**

A merchant applies to accept payments. Automated screening clears the clean majority, and hard
stops — sanctions, terminated-merchant-file listings, prohibited categories — are declined or
routed to compliance upstream. What lands in the review queue is the ambiguous middle, where the
slowest and highest-signal step is cross-checking what the merchant *says* they sell against what
their storefront actually sells. Under queue pressure, that step gets skimmed.

MerchantLens pre-compiles each routed file, reasons on it, and hands a decision report to a
merchant risk analyst who approves, edits, or escalates.

## What it does

One reasoning chain, four steps, each gating the next:

1. **Storefront sufficiency** — deterministic heuristics decide the clear cases; the agent judges
   only the borderline band.
2. **Category classification** — what the store predominantly sells, assigned from a fixed
   15-code reference. Confidence is *derived* from the margin between the top two candidates, not
   self-reported.
3. **Claim comparison** — inferred category against the stated business, plus misrepresentation
   signals.
4. **Policy screen** — the catalog as sold, against the prohibited-and-restricted list.

It returns a ten-field report: disposition, escalation trigger, the four verdicts, evidence
citations naming specific listings, a drafted documentation request, and a plain-language reason.

## The human boundary

The agent has **no send, write, or account-action tool, and no live web access** — bypassing the
human is structurally unavailable, not merely forbidden.

- Documentation requests are **drafted, never sent**. The analyst sends them.
- Hard-stop determinations route to a **separate compliance reviewer**, and the agent produces no
  verdicts at all on those files.
- The agent's ceiling is `route to further review`. Declining a merchant carries appeal rights and
  stays with a human.
- Nothing completes without a click on **Approve**, **Edit**, or **Escalate**.

## Running it

Open the live link, or download `index.html` and double-click it — one file, no build step, no
runtime dependencies.

The agent runs on **your own Anthropic API key**, entered in the app's settings panel and stored
only in your browser's `localStorage`. No key is contained in this repository, and none is sent
anywhere except to the Anthropic API. Without a key the site loads and explains itself; it simply
will not run the agent. That is the intended design, not a limitation of the demo.

## Status

A **capstone prototype, piloted under human review** — not a production system, not autonomous.

Everything in `data/` is synthetic: 20 invented merchant applications, 227 catalog listings across
20 storefronts, and 3 policy files. No real merchant, person, company, or financial record is
represented anywhere in this repository.

## Repository contents

| Path | What it is |
|---|---|
| `index.html` | The whole product — console, agent, eval runner. The only file the site needs. |
| `data/` | Synthetic merchant records, storefronts, risk scores, and the category reference. |
| `data/eval_labels.csv` | The eval answer key. Deliberately never embedded in `index.html` and never placed in the agent's context — it is loaded at scoring time through a file picker and lives in memory only, so the agent cannot read its own answers. |
| `policies/` | The prohibited-and-restricted list, review criteria, and escalation rules. |
| `design/` | Design tokens the console renders with. |
| `examples/` | Two sample decision reports, showing the output format. |

## Evaluation

Twenty labeled records, six scored fields each, scored by exact match against the answer key —
plus an analyst verdict on the free text, which is where the interesting failures live. Two records
score a perfect 6/6 on exact match and are still marked "needs work", because reading the drafted
letter caught what the score could not.
