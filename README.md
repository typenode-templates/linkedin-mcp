# LinkedIn Lead Discovery MCP

A **LinkedIn Lead Discovery Model Context Protocol (MCP) server built by Typenode**, using Apify APIs and selected third-party scrapers to let AI assistants such as Claude and ChatGPT search people and companies, retrieve profiles, and research jobs, posts and engagement. Its **24 tools** supply source evidence for your assistant to interpret. It does not use LinkedIn's official API.

**Built and smoke-tested in a maintainer installation.** This repository contains current contracts, setup requirements, prompts and synthetic examples. Implementation code, Workflow exports, provider wiring and private endpoints remain within Typenode. Independent installation of the latest published template snapshot remains a release gate; see [verification](docs/verification.md).

**[Explore Typenode](https://typenode.ai)** · **[Setup requirements](docs/setup.md)** · **[Task prompts](prompts/getting-started.md)**

## What can it help you investigate?

| Question | Evidence path |
|---|---|
| Which people match a role, geography and company filter? | People discovery, followed by explicitly selected profile retrieval. |
| Which candidate companies match my market criteria? | Company search and known-company details; AI-owned qualification. |
| Who should I research at each of these accounts? | Independent employee searches with per-company result and spending allowances. |
| Which companies advertise relevant jobs? | Job search, company job lists and full known-posting evidence. |
| Who has commented on or reacted to these posts? | Bounded comment/reaction datasets with native event provenance. |
| What has a candidate or employer posted publicly? | Known-profile/company posts and observed profile activity. |
| Can I collect account research within a stated budget? | Independent facts, employee, job and company-post datasets. |
| Can I read more of an existing collection without paying for another scrape? | Existing-run/result reader, rather than a new acquisition. |

These are research questions, not promises of exhaustive leads, verified employment, buying authority or intent. The consuming AI client evaluates the evidence.

## Tools

Names, descriptions and input/output schemas match the current reviewed template and saved installation interfaces. Inspect each linked JSON for the complete contract. Safety annotations express reference intent; their propagation by every host has not been certified.

### Company and people discovery

| Tool | Status | What it provides |
|---|---|---|
| [`find_linkedin_company_pages`](tools/find_linkedin_company_pages.json) | Implemented | Find candidate LinkedIn company pages for website domains. Ambiguous or missing matches remain unresolved; this paid lookup does not establish company identity. |
| [`search_linkedin_companies`](tools/search_linkedin_companies.json) | Implemented | Discover candidate company pages using explicit keywords, location, industry and About-page size bands. Paid discovery returns basic companies; use company details for enrichment. Results are bounded and do not enumerate the whole market. |
| [`get_linkedin_companies`](tools/get_linkedin_companies.json) | Implemented | Retrieve full details for known company pages, including supplied website, industry, locations and employee facts. Paid batch returns an explicit outcome per company; size bands and associated-profile counts remain distinct. |
| [`get_linkedin_company_employee_counts`](tools/get_linkedin_company_employee_counts.json) | Implemented | Get LinkedIn-associated employee counts for known company pages. These are not payroll headcounts; unavailable values stay unknown. Collection costs money; use the result reader for pending or remaining results. |
| [`search_linkedin_people`](tools/search_linkedin_people.json) | Implemented | Discover basic candidate profiles without a company seed, using supported title, location, employer and seniority filters. Source pages are billable even when sparse; full-profile and email enrichment are separate opt-in reads. Candidate recent-change/posting flags are native fixed-window filters. |
| [`search_linkedin_company_employees`](tools/search_linkedin_company_employees.json) | Implemented | Search candidate employees at one known company using explicit filters. Returns basic profiles, not a complete roster or verified decision-makers. Paid collection; empty searches can incur start fees. |
| [`search_linkedin_company_employees_shared`](tools/search_linkedin_company_employees_shared.json) | Implemented | Search candidate employees across several companies under one shared limit; larger employers can crowd out smaller accounts. Returns basic profiles, not a complete roster or verified decision-makers. Paid collection; empty searches can incur start fees. |
| [`search_linkedin_company_employees_batch`](tools/search_linkedin_company_employees_batch.json) | Implemented | Search each supplied company independently with its own profile and spending allowance. More starts than shared search, but separate results, coverage and recovery per account. Explicit per-company budgets must fit the total authorized maximum before any launch. |
| [`get_linkedin_profile`](tools/get_linkedin_profile.json) | Implemented | Retrieve one known profile’s identity and experience. Optional email discovery adds cost and may yield no address; profile retrieval does not independently verify employment. |
| [`get_linkedin_profiles`](tools/get_linkedin_profiles.json) | Implemented | Retrieve a bounded batch of known profiles, with a separate outcome for every input. Email discovery is off unless explicitly requested and adds cost; resume existing results instead of restarting the batch. |

### Hiring evidence

| Tool | Status | What it provides |
|---|---|---|
| [`search_linkedin_jobs`](tools/search_linkedin_jobs.json) | Implemented | Search job postings across companies using an explicit query and optional location/recency. Matching can relax Boolean operators; inspect the posting evidence. Paid collection returns excerpts; read known jobs for full descriptions. |
| [`list_linkedin_company_jobs`](tools/list_linkedin_company_jobs.json) | Implemented | List jobs for a known company ID, with explicit source scope and a bounded paid result allowance. Results are excerpts and can be capped; they do not prove the company’s complete hiring activity. |
| [`get_linkedin_job`](tools/get_linkedin_job.json) | Implemented | Retrieve one known job posting with full description and supplied company details. Removed or unavailable postings have an explicit missing outcome. This is a paid read. |

### Posts and public activity

| Tool | Status | What it provides |
|---|---|---|
| [`search_linkedin_posts`](tools/search_linkedin_posts.json) | Implemented | Search posts by an explicit query and supported author/date filters. Source matching is fuzzy; local term hits are evidence, not a relevance score. Paid collection; inspect the existing invocation after an ambiguous failure before retrying. |
| [`get_linkedin_posts`](tools/get_linkedin_posts.json) | Implemented | Retrieve known posts by URL with content and available engagement totals. Preserve a separate outcome for every URL. This paid read does not retrieve the full commenter or reactor audience. |
| [`list_linkedin_posts_by_profile`](tools/list_linkedin_posts_by_profile.json) | Implemented | List bounded original posts, quote posts and reposts from profiles. Activity dates can differ from original publication. Paid collection; the observed feed is not complete. |
| [`list_linkedin_posts_by_company`](tools/list_linkedin_posts_by_company.json) | Implemented | List bounded company posts, with quote posts and reposts explicitly controlled. Paid collection excludes nested comments/reactions. This actor has no source-page continuation; only collected-result paging is available. |
| [`list_linkedin_comments_by_profile`](tools/list_linkedin_comments_by_profile.json) | Implemented | List observed comments made by profiles, including available underlying post context. Paid collection returns a capped history, not proof of all activity. |
| [`list_linkedin_reactions_by_profile`](tools/list_linkedin_reactions_by_profile.json) | Implemented | List observed reactions made by profiles and available post context. Reaction-event timestamps are unavailable, so this paid read does not establish recent engagement. |
| [`list_linkedin_post_commenters`](tools/list_linkedin_post_commenters.json) | Implemented | List bounded comments and their authors for known posts. Retain comment text and thread identity; capped comments do not equal all commenters or all nested replies. Paid collection. |
| [`list_linkedin_post_reactors`](tools/list_linkedin_post_reactors.json) | Implemented | List bounded reacting profiles and reaction types for known posts. A reaction is not buying intent; source audience limits apply. Paid collection; profile URL resolution never silently starts extra enrichment. |

### Retrieval presets and result recovery

| Tool | Status | What it provides |
|---|---|---|
| [`get_linkedin_account_datasets`](tools/get_linkedin_account_datasets.json) | Implemented | Collect company facts plus explicitly selected employee, job or company-post datasets for known accounts. Return independent evidence and continuation; no fit score, narrative or automatic full-profile/email enrichment. Paid acquisitions have separate budgets that must fit the total maximum. |
| [`get_linkedin_post_engagement_datasets`](tools/get_linkedin_post_engagement_datasets.json) | Implemented | Collect known-post details and bounded comment/reaction datasets under explicit budgets. Preserve independent post/event provenance; the AI client owns cross-source overlap and lead qualification. Paid collection never silently enriches profiles or emails. |
| [`read_linkedin_discovery_results`](tools/read_linkedin_discovery_results.json) | Implemented | Observe an existing allowlisted LinkedIn collection and read a bounded page of its normalized results. Never starts another scrape. Requires current configured Apify account access; polling reports cumulative run charges rather than new collection spend. |

## Setup

1. Obtain your copy through Typenode's supported template setup flow.
2. Configure your own Apify API token, default/hard budget caps and owner override permission securely in Typenode; check provider access and billing requirements.
3. Add your own Typenode MCP endpoint to a compatible client and complete Typenode authentication.
4. Supply concrete search criteria. Use the configured budget default or pass an allowed request cap. Make a first discovery call, then use its receipt to read existing results.

The public activation link and fresh installation proof are pending. Cloning this repository does not start a server. The JSON contracts cannot be imported as executable Workflows. See [setup](docs/setup.md) and [verification](docs/verification.md).

## Permissions and costs

You need a Typenode Workspace, an Apify account with usable allowance/access, an API token stored in secure Typenode configuration, and a compatible authenticated remote MCP client. No Google Cloud project is involved. The template uses an Apify token and does not require a LinkedIn browser session or LinkedIn OAuth grant.

**Reading LinkedIn data can still spend money.** New collections create provider runs and datasets. All 23 acquisition contracts accept optional `max_cost_usd`: omission uses the owner default, lower caps are allowed, and raising the default requires owner `allow_budget_override=true` within the hard ceiling. Independent batches and presets retain explicit child reservations. See [budget policy](docs/budgets.md). Preflight checks current qualified provider pricing and minimums before launching; allowed caps are sent to the provider. Prices and access can change, and a permitted budget may still be insufficient for the requested work.

Source limits, response page sizes and spending caps are distinct. A sparse or empty search can still incur provider charges. Reading an existing dataset does not launch another scraper, but platform resources may still be consumed. Keep cumulative run charges separate from new acquisition spend and from Typenode or AI-client costs.

## Example: candidate people discovery

An AI client can call `search_linkedin_people` using:

```json
{
  "job_titles": ["Head of Marketing"],
  "locations": ["Germany"],
  "max_cost_usd": 0.5
}
```

This is an illustrative authorisation amount, not a price quote or assurance that it covers current provider minimums. The wrapper must refuse insufficient or disallowed budgets rather than raise or clamp them silently. Omitting `max_cost_usd` would use the installation default. Source candidates are not verified decision-makers.

See [requests for all 24 tools](examples/tool-calls.json), a [complete request](examples/sample.request.json), a [synthetic response](examples/sample.response.json) and [example notes](examples/README.md). No real people, paid runs or observed live results are included.

## Collection, paging and recovery

The server separates three actions:

- **New collection:** launch a bounded paid acquisition for supplied criteria or targets.
- **Existing results:** use `read_linkedin_discovery_results` to inspect a known collection or read another stored-result page.
- **More source data:** collect another supported search page with new explicit spending authorisation.

A response cursor is not proof of an exhaustive search. If a submission is ambiguous, inspect the existing invocation/receipt before another paid launch. Account and post-engagement presets retain independent dataset outcomes; recovering one dataset should not repeat successful siblings.

## Limitations

- Scraper results can be fuzzy, capped, stale, missing or incomplete. Boolean matching is not guaranteed exact across providers.
- Basic search profiles are distinct from full profile retrieval; profile and optional email enrichment must be explicitly selected.
- Profile experience does not independently verify current employment. LinkedIn-associated employee counts are not payroll headcounts or company-size bands.
- Reactions do not prove purchase intent. Profile reaction-event timestamps are unavailable; a post date must not be presented as the reaction date.
- Comments can include multiple events from the same person and may omit nested replies. The AI client owns overlap and qualification.
- Native recent-change/activity filters use provider-defined windows. They are not ongoing monitoring or historical change detection.
- No tool sends messages, requests connections, publishes, changes LinkedIn data, writes to a CRM, schedules monitoring, scores leads or generates outreach.
- Email discovery can cost extra and return no address. Deliverability verification is a separate optional extension.

## Frequently asked questions

### Is this an official LinkedIn MCP server?

No. This is an independent Typenode server using third-party Apify scrapers, not LinkedIn's official API or an endorsed LinkedIn integration.

### Doesn't Apify already have an MCP server?

Yes. Apify's [official MCP server](https://docs.apify.com/mcp) can discover/run Actors and read results. This template adds a curated LinkedIn-specific interface, owner-controlled spending, saved-result recovery and named research presets.

### Can I use it in Claude or ChatGPT today?

The maintainer installation has been exercised through the intended MCP client. You need your own installation and a client supporting Typenode's authenticated remote MCP connection. This repository supplies no shared working endpoint. Independent customer installation and broader client compatibility remain separate checks.

### Why do acquisition annotations say read-only is false?

The tools do not change LinkedIn business data, but they create Apify runs/datasets and incur costs. Paid acquisition is therefore described as non-idempotent with `readOnlyHint: false`. The existing-run reader has `readOnlyHint: true` and never starts another collection. Verify actual annotation propagation during release testing.

### Are the source and schemas open?

The public contracts, documentation and examples are inspectable under the [MIT licence](LICENSE). Hosted implementation code and Workflow graphs are not included or licensed here. Editing a contract file does not change a hosted tool.

### Where will data and credentials go?

The Apify token is configured securely in Typenode. Collection results are stored by Apify and pass through Typenode execution to the connected AI client. Review each service's current data-handling terms for your account. No private captures or credentials are distributed in this repository.

## Repository contents

- [manifest.json](manifest.json): public reference metadata, access requirements and contract references.
- [tools/](tools/): 24 current public interfaces, without implementation or Actor mappings.
- [docs/setup.md](docs/setup.md): provider and client connection requirements.
- [docs/verification.md](docs/verification.md): live smoke evidence and remaining release gates.
- [prompts/](prompts/): copy-paste research recipes; not protocol-registered MCP prompts.
- [examples/](examples/): synthetic request/response examples.

For a contract question, identify the tool and include sanitised arguments or the ambiguous field. Never publish tokens, private profiles or provider access details in an issue.

Created by Typenode. Not affiliated with, endorsed by or sponsored by LinkedIn Corporation or Apify. Data is supplied through third-party Apify Actors; using an intermediary does not establish LinkedIn authorization or guarantee compliance. Use only where you have the necessary rights, subject to applicable terms, privacy requirements and law. MIT covers the distributed contracts and documentation, not the hosted implementation or runtime access. Third-party names and marks belong to their owners.
