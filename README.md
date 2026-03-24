# AmFam — Customer Engagement Kit

Pre-built engagement artifacts for **American Family Insurance (AMFAM)**, designed to accelerate Postman Customer Success discovery calls and technical workshops.

## Contents

| File | Description |
|------|-------------|
| `AMFAM_Discovery_Brief.md` | Technical discovery profile covering AMFAM's cloud topology (AWS, Azure/Sitecore, Vercel, on-prem), authentication (Okta), API maturity signals, friction points, and recommended engagement approach for Postman governance and developer portal conversations. |
| `AMFAM_Service_Template.yaml` | Postman Collection v2.1 template for the AMFAM insurance platform. Includes pre-configured environment profiles (`amfam_core`, `homesite`, `connect`), Okta OAuth 2.0 client-credentials auth, governance-style header tests, and example requests across platform health, policy/quote, claims, agents, Sitecore Edge GraphQL, and legacy batch/SFTP bridge APIs. |

## Usage

1. **Import the collection** — Open Postman and import `AMFAM_Service_Template.yaml`. Select the appropriate environment profile for the subsidiary you're demoing against.
2. **Review the brief** — Read `AMFAM_Discovery_Brief.md` before the engagement to understand AMFAM's stack, pain points, and recommended talking points.
3. **Customize** — Update placeholder secrets (Okta `client_id` / `client_secret`, API keys) with values provided by the customer or use Postman mock servers for a self-contained demo.

## Key Topics Covered

- Multi-subsidiary environment management (AmFam core, Homesite, Connect)
- Okta-based OAuth 2.0 client credentials flow
- API governance validation (correlation IDs, versioning headers, HSTS, response time)
- Kubernetes health probes (liveness, readiness, deep health)
- Legacy integration patterns (batch/SFTP bridge)
- Sitecore Edge GraphQL for content APIs
