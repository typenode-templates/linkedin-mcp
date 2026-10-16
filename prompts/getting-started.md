# Getting started: candidate discovery

Usage recipes, not protocol-registered MCP prompts. To use the owner default, replace an aggregate [USD amount] instruction with “use the configured default budget”; the AI client cannot set owner policy. Explicit child reservations in batches/presets still apply. Use them after your hosted server is configured and connected. Replace bracketed criteria and amounts with your own choices; do not infer spending authorisation from an example.

## Find candidate people

> Use search_linkedin_people for [role/title] in [location], with [supported employer/industry filters]. I authorise at most [USD amount] for this collection. Retrieve basic candidates only: no full-profile or email enrichment. Show effective filters, cost evidence, coverage and the receipt. Don't describe candidates as verified decision-makers.

## Read existing results

> Read this existing receipt with read_linkedin_discovery_results. If pending, observe the same run; if complete, retrieve one bounded stored-result page. Do not start a new collection. Distinguish cumulative run charges from any newly acquired data.

## Enrich selected candidates

> Retrieve profiles only for these selected LinkedIn URLs, using get_linkedin_profiles, within [USD amount]. Leave email discovery off. Return an independent outcome for every URL and preserve unknown employment evidence.

## Need more source coverage?

> Explain whether more stored rows remain or whether another source page must be acquired. Do not start a new collection unless requested; each requested acquisition resolves a new cap under the owner policy. Explain any proposed paid source continuation before launching it. If the previous submission is ambiguous, inspect its receipt/invocation first.

An empty search is not proof that no matching people exist and may still incur cost. Exclusion-only or unconstrained searches must fail before a paid start where the public contract requires a positive discovery scope.
