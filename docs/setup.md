# LinkedIn MCP setup requirements

The server is built and smoke-tested in a maintainer installation. This guide describes its current prerequisites. An independent customer installation of the latest published snapshot remains unverified; these contracts are not an executable import.

## Accounts and access

Use your own Typenode Workspace and access to the hosted template when available. Normal billing/trial rules apply. The final activation URL is pending; these public contracts contain no Workflow implementation to assemble.

Use an Apify account with access to the selected providers and usable allowance/payment configuration. Configure its API token securely in Typenode using the supported credential/secret setup. Keep the token out of tool arguments, public files, prompts, receipts and error reports. Consult [Apify API integration guidance](https://docs.apify.com/integrations/api) for current authentication support.

The runtime needs access to start qualified collections, read run metadata/original input, read dataset items and inspect pricing evidence for budget preflight. Use Apify's supported permissions; no Google-style OAuth scope list is invented here. An empty `required_scopes` array in the reference manifest does not mean no provider permissions are required. Validate access and token expiration/revocation behavior on the actual account before release.

No Google Cloud project, Google OAuth app, LinkedIn OAuth grant or LinkedIn browser session is required by this template. Its qualified providers use the Apify token.

## Hosted activation

Obtain the template through Typenode and configure its chosen Environment. Set `APIFY_TOKEN` through a secure Secret handoff. Save the owner-selected policy as JSON in `LINKEDIN_BUDGET_POLICY`; set `LINKEDIN_APIFY_CONTEXT` to a fresh stable installation UUID and `LINKEDIN_APIFY_ACCOUNT_ID` to the account ID verified through authenticated Apify identity access. These last three are non-secret configuration, not tool arguments. Change the context when switching accounts. Create the synchronous authenticated webhook triggers and MCP bindings, validate, deploy and observe every operation until terminal success. Keep provider INPUT, saved recipe metadata and datasets while results must remain readable: the reader needs access to run metadata, original inputs, recipe storage and dataset items. The manifest is a reference inventory, not a deployment command or Actor registry. Do not try to import `tools/*.json` as Workflows.

Get your own Environment-specific MCP endpoint from Typenode. Add it to a client supporting the authenticated remote connection, then complete client-to-Typenode OAuth consent. This is separate from Typenode's use of the Apify token. Verify appropriate client refresh access and renewal after known expiry; Apify token configuration cannot repair expired client authentication.

Fresh client discovery should expose all 24 implemented tools. A connected badge is not execution proof. All 24 tools were discovered in the intended client. Independent activation and other clients remain separate release checks. After a catalog change, use the client's refresh/reconnect path; a full restart may also be needed to clear cached schemas.

## Configure collection budgets

The owner sets `default_max_cost_usd` and `hard_max_cost_usd` in the Environment; `allow_budget_override` defaults false. Numeric values must be deliberately chosen—there are no universal numeric defaults. Configuration is separate from tool arguments. See [budget policy](budgets.md).

Pick concrete criteria or known target URLs. Omit `max_cost_usd` to use the configured default, or provide an allowed lower/equal cap. A higher request cap requires the owner override permission and must stay within the hard ceiling. For independent batches and dataset presets, supply explicit child reservations; their sum must fit the resolved total before any start.

Use current provider price/minimum information. The server must refuse insufficient budgets rather than silently raise them. The maintainer installation has representative preflight and provider-cap evidence. Verify the configured copy before authorizing substantial work; this is not a conversation-wide or account-wide spending ceiling.

Collection limits specify work to buy; `page_size` bounds one returned view. Supported source-page continuation buys new data and requires new authorisation. Stored-result continuation reads collected data; do not relaunch a collection just to see another page.

## First interaction when ready

Ask the first recipe in [Getting started](../prompts/getting-started.md) with your own filters and approved budget. Retain its actual receipt. If pending, observe that run through `read_linkedin_discovery_results`; if complete, use the same reader for remaining result pages. Do not launch again after an ambiguous submission until the original outcome is understood.

Success is a typed source result or legitimate empty result, clear budget/run/coverage evidence and safe continuation without a duplicated paid start. Then perform the complete per-tool and interaction acceptance plan. These patterns have maintainer-installation smoke evidence; the independent customer setup remains unverified.

## Troubleshooting and cost boundaries

| Symptom | Check |
|---|---|
| UNAUTHORIZED before a workflow starts | Client-to-Typenode grant, expiry and supported reconnect path. |
| Provider access failure | Apify token, account/Actor/run/dataset access and expiration or revocation. |
| Budget rejected | Owner default/ceiling/override policy, current provider minimums and child reservations. Clients cannot change owner policy; rejected caps are never clamped. |
| Empty search | Effective criteria, fuzzy matching, source caps and cost evidence. Empty can still be billable. |
| Pending or ambiguous collection | Preserve the run/invocation identity; read existing evidence before repeating acquisition. |
| More results needed | Decide whether a stored page remains or a new source acquisition is required. |

Apify collection costs, possible storage/platform usage, Typenode execution and any AI-client subscription are separate. Reported cumulative charges are not new spend on every poll. The MIT licence for these files does not include hosting or scraping credits.
