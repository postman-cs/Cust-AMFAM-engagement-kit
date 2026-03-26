# Build Log: Cust-AMFAM-engagement-kit

## Context
- AE / CSE: Daniel Shively (CSE)
- Customer technical lead: American Family Insurance (AMFAM) — API Platform team
- Sprint dates: TBD

## Hypothesis
- If we provide a pre-built discovery brief and Postman collection template covering AMFAM's multi-subsidiary stack, we will prove that Postman can address their API governance, auth standardization, and legacy integration modernization needs across AmFam core, Homesite, and Connect.

## Success Criteria
- Discovery brief accurately maps AMFAM's cloud topology (AWS, Azure/Sitecore, Vercel, on-prem) and auth (Okta)
- Service template collection imports cleanly into Postman with working Okta OAuth 2.0 client-credentials pre-request scripts
- Governance-style tests (correlation ID, API version, HSTS, response time) validate against AMFAM endpoints

## Environment Baseline
- SCM: GitHub (postman-cs/Cust-AMFAM-engagement-kit)
- CI/CD: N/A (engagement kit, not a deployed service)
- Gateway: Customer uses AWS API Gateway, Azure Front Door
- Cloud: AWS (core), Azure (Sitecore/Homesite), Vercel (marketing), on-prem (legacy)
- Dev Portal: None identified — recommended as engagement talking point
- Current Postman usage: Detected via DNS/header analysis; usage level TBD
- v11/v12: TBD

## What We Built
- Technical discovery brief (AMFAM_Discovery_Brief.md) covering cloud topology, auth, API maturity signals, friction points, and engagement approach
- Postman Collection v2.1 template (AMFAM_Service_Template.yaml) with three environment profiles (amfam_core, homesite, connect)
- Pre-request OAuth 2.0 client-credentials token flow for Okta
- Governance header test suite (collection-level)
- Example requests: platform health (K8s probes), Okta token/introspect, policy/quote, claims (FNOL, documents), agents, Sitecore Edge GraphQL, legacy batch/SFTP bridge, governance probes (401, rate limit, CORS)

## Value Unlocked
- Ready-to-import engagement kit reduces prep time for AMFAM workshops
- Multi-subsidiary environment profiles demonstrate Postman's workspace strategy for complex orgs
- Governance tests provide immediate value proof during live demos

## Reusable Pattern
- Customer engagement kit structure (discovery brief + service template collection) reusable for any enterprise with multi-subsidiary, multi-cloud architectures
- Okta OAuth 2.0 pre-request script pattern for client-credentials flow

## Product Gaps / Risks
- AMFAM has no public API — engagement depends on access to internal endpoints
- Okta scopes and client credentials must be provided by customer
- Sitecore Edge GraphQL queries may require customer-specific content type knowledge

## Next Step
- Schedule discovery call with AMFAM API platform team
- Validate collection against customer-provided sandbox endpoints
- Identify pilot API for governance automation
