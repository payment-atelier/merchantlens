# MerchantLens

A merchant risk verification agent for a self-serve commerce platform's payments risk team.

**Live demo: https://payment-atelier.github.io/merchantlens/** (no API key needed)

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

## Two ways to use it

Open the live link, or download `index.html` and double-click it — one file, no build step, no
runtime dependencies. The app runs in one of two modes, and the badge in the top bar always says
which.

### Recorded demo — the default, no key needed

Anyone without an API key lands here. Every one of the 20 merchants has a recording: pick one
and press **Play recorded run**, or press **Play all 20 recorded** to watch the whole queue fill
the run log. The app replays a real run of this exact build: the agent's response, saved word
for word from the Anthropic API, goes through the same parser, report and Approve / Edit /
Escalate step a live run uses. Nothing in a recording is written by hand.

The recordings come from one live sweep of all 20 merchants on 3 November 2026. It reached the
right disposition on 19 of 20 and took 14.9 seconds per report on average. The one miss is kept
in the demo: on M-009 the agent's own listing table shows 8 equipment items against 7 apparel,
but its arithmetic says 9 against 6, so it clears a merchant it should have sent for further
review. One record was re-run: on M-011 the sweep's drafted request named 2 of the merchant's 3
gaps, and the recording used is a re-run on the same build that names all 3.

The tabs pick out six worth starting with:

| Tab | Merchant | What happens |
|---|---|---|
| Clean match | M-001 Cedar & Loom | Catalog matches the stated business: `clear` |
| Flagged: prohibited goods | M-002 Willow & Wax | "Home goods" store selling vapes: `route to further review` |
| Unjudgeable storefront | M-006 Nine Pines Trading | Too little to judge: drafts a documentation request |
| Subtle drift | M-004 Thistle & Vine | Catalog has drifted category: `apply risk terms` |
| Sparse-but-judgeable | M-007 Rowan & Bracket | Thin but genuine store: `clear`, not over-flagged |
| Boundary: terminated-merchant file | M-010 Ashgrove Mercantile | Refuses, produces no verdicts, routes to compliance |

Each replay plays in about four seconds and then shows how long the live run actually took. The
Evals page shows the analyst's graded scoreboard from the earlier August sweep, read-only. The
demo never writes to your browser's storage.

### Live mode — with your own API key

Paste your own Anthropic API key in **Settings** and the app switches to live mode at once: every
merchant runs against the real API, and the Evals page can run and score the full set. The key is
stored only in your browser's `localStorage`. No key is contained in this repository, and none is
sent anywhere except to the Anthropic API. Clear the key and the app goes back to the recorded
demo.

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
| `data/demo/` | The offline demo: 20 recorded live runs (`recordings.json`, responses only, no request and no key) and the graded August scoreboard (`board.json`). |
| `policies/` | The prohibited-and-restricted list, review criteria, and escalation rules. |
| `design/` | Design tokens the console renders with. |
| `examples/` | Two sample decision reports, showing the output format. |

## Evaluation

Twenty labeled records, six scored fields each, scored by exact match against the answer key —
plus an analyst verdict on the free text, which is where the interesting failures live. On the
graded August sweep the agent reached the right disposition on 20 of 20 merchants, 19 of 20
matched on every scored field, and a report took 17.3 seconds on average; the analyst graded 17
Pass and 3 Needs work. The November sweep on the current build reached 19 of 20, at 14.9 seconds. Two records score a perfect 6/6 on exact match and are still marked "needs work",
because reading the drafted letter caught what the score could not.
