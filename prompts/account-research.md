# Account discovery and research

Planned client-side recipes. The server retrieves independent evidence; the AI client owns qualification. Supply your own targets and supported filters. Top-level caps may be omitted to use the owner default; explicit child reservations remain required.

## Discover candidate companies

> Use search_linkedin_companies for [keywords, industry, location and size band], within [USD amount]. Retrieve basic companies only. Show source limits. Then ask me which candidates to enrich with get_linkedin_companies rather than enriching every result automatically.

## Resolve website domains

> Find candidate LinkedIn company pages for these website domains using find_linkedin_company_pages, within [USD amount]. Keep ambiguous and missing matches unresolved. For selected pages, retrieve associated employee counts under a separate budget; don't equate those with payroll headcount.

## Balance employee discovery across accounts

> Use search_linkedin_company_employees_batch for these selected companies and [role filters]. Allocate [per-company USD amounts and result allowances], with a total maximum of [USD amount]. Return an outcome for every account. If one fails or is pending, recover it without repeating successful siblings.

Use the shared-company tool only when you accept one shared allowance and uneven company coverage. Neither path establishes a complete employee roster or verified current employment.

## Collect account datasets

> Use get_linkedin_account_datasets for [company pages], selecting only [employees/jobs/posts]. Provide [company-facts budget], [required per-account section reservations] and [optional aggregate maximum; otherwise use owner default]. Return separate source datasets and errors, with no profile/email fan-out, fit score, generated narrative or CRM write.

## Inspect hiring evidence

> Search postings for [query/location/recency] within [budget]. For selected results, retrieve full known-job descriptions under another approved budget. Quote the relevant posting evidence; don't infer an employer's technology purchase or budget from a job alone.

Present size bands and associated-profile counts separately. Missing numeric company identity should leave dependent job retrieval not attempted, not trigger an alternative paid company search silently.
