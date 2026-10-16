# Synthetic interface examples

All people, company URLs, post/job identities, run IDs, measurements and budget amounts are invented. These are structural fixtures, not live results, price quotes, valid continuation receipts or guarantees that a budget covers current provider minimums.

- [budget-policy.json](budget-policy.json): illustrative owner configuration, omitted-cap request and 15 illustrative budget-policy cases. Values are not production defaults or observed enforcement.
- [tool-calls.json](tool-calls.json): one request for every tool.
- [sample.request.json](sample.request.json) and [sample.response.json](sample.response.json): illustrative people-search request using the owner default and success shape with a resolved $0.50 authorization. Assume the illustrative configuration in budget-policy.json; not an observed execution.
- [result-shapes.json](result-shapes.json): the five output families and a handled error. Some permissive effective-request fields illustrate family shape, not a complete semantic invocation for every tool.

Validate against the linked [tool contracts](../tools/). For real use, select your own target and criteria, configure owner policy or supply an allowed request cap, then obtain receipts/cursors from actual results. Do not send fixture run IDs or continuation state to a live reader.

Every new acquisition resolves owner-controlled cost authorisation, whether from the configured default or an allowed request cap. Reading an existing run should not start another scrape. Preserve native evidence, missing outcomes, coverage and cumulative-charge semantics; do not turn candidate data into verified employment, exhaustive lists or inferred intent.
