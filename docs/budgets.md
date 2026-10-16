# Collection budget policy

All 23 acquisition tools use the implemented owner-controlled policy; the existing-run reader starts no collection. The maintainer installation has live smoke and local guard evidence. Independent installation remains a separate check.

## Owner configuration

| Setting | Meaning |
|---|---|
| `default_max_cost_usd` | Required owner-selected cap used when the request omits `max_cost_usd`. |
| `hard_max_cost_usd` | Required owner-selected ceiling for the whole invocation, including child acquisitions; at most $10 in this catalog. |
| `allow_budget_override` | Owner-only permission for above-default request caps; defaults false. |

Configure these centrally in the installation's Environment, separately from the Apify token. They are not client-set tool arguments. Amounts must be finite and positive, with default ≤ hard ceiling. Missing/invalid policy rejects before paid work; the server does not silently choose numeric defaults. The [manifest](../manifest.json) includes the configuration schema and requirements.

## Request resolution

- Omit `max_cost_usd` to use the configured default.
- Request a lower or equal cap without override permission.
- Request a higher cap only when the owner enabled `allow_budget_override`, up to the hard ceiling.
- Reject excess, null, nonpositive/nonfinite or disallowed caps; never silently clamp or raise them.
- Check the complete plan and current provider minimums before paid work; an allowed cap is not a guarantee that the requested work fits.

Illustration only: default $0.50, hard ceiling $2 and override false permits omission ($0.50) or $0.25, but refuses $1. Enabling override permits $1 and exactly $2; $2.01 still rejects. These are invented configuration examples, not prices or shipped defaults. See [fixtures](../examples/budget-policy.json).

## Batches, presets and enrichment

Keep explicit child caps: each independent company, company facts/post details, and each selected dataset. Their reservations must sum to no more than the resolved aggregate authorization before any paid start. Omission of the top-level cap does not remove child requirements. Do not allocate unused estimated balance again within a bundle. Unselected section budgets reject; work-selection/enrichment flags cannot bypass caps.

Provider launches must receive the appropriate total/child cap. The tested installation sends provider caps after local preflight; this is not an atomic account-wide spending ledger. Budgets apply per invocation; they do not enforce conversation/day/month caps or aggregate simultaneous invocations.

## Authorization versus cost

Acquisition results show the resolved cap in `effective_request.max_cost_usd` and `cost.max_authorized_usd`. Actual `cost.charges[]` separately records estimated, provider-reported, settled or unknown amounts. Unknown is null, never invented zero; a spending cap is not spend. Apify charges exclude separately billed Typenode and AI-client costs.

Existing-run reads preserve the original evidenced acquisition authorization, even after owner configuration changes; if unavailable, authorization stays null. They never substitute today's default or authorize new scraping. Repeated observations update cumulative charge evidence for the same run; do not add the same cumulative amount again on every poll. Current reader access checks still apply.

## Release evidence

Exercise omitted/default, lower/equal, both override settings, exact/excess ceilings, invalid configuration, insufficient provider minimums, excessive/missing child caps and historical reads after policy changes. The [verification notes](verification.md) distinguish local checks, live smoke evidence and remaining installation/client gates. The fixtures contain design expectations, not an executed budget resolver.
