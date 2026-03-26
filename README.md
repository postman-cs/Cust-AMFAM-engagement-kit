# Cust-AMFAM-engagement-kit

Pre-built engagement artifacts for **American Family Insurance (AMFAM)**, designed to accelerate Postman Customer Success discovery calls and technical workshops.

## How It Works

```mermaid
flowchart TD
    subgraph prep [Pre-Engagement Prep]
        A1["Read AMFAM_Discovery_Brief.md"]
        A2[Understand cloud topology]
        A3[Review talking points]
    end

    subgraph import [Collection Setup]
        B1["Import AMFAM_Service_Template.yaml into Postman"]
        B2[Select environment profile]
        B3[Configure Okta credentials]
    end

    subgraph demo [Live Demo / Workshop]
        C1[Platform Health Probes]
        C2[Okta OAuth Token Flow]
        C3[Policy & Claims APIs]
        C4[Governance Header Tests]
        C5[Sitecore Edge GraphQL]
        C6[Legacy SFTP Bridge]
    end

    prep --> import --> demo
    A1 --> A2 --> A3
    B1 --> B2 --> B3
```

## AMFAM Technology Landscape

```mermaid
flowchart LR
    subgraph subsidiaries [Subsidiaries]
        S1[AmFam Core - AWS]
        S2[Homesite - Azure/Sitecore]
        S3[Connect - Vercel]
    end

    subgraph auth [Authentication]
        O[Okta IdP]
    end

    subgraph apis [API Surface]
        A1[Policy / Quote APIs]
        A2[Claims - FNOL / Documents]
        A3[Agent APIs]
        A4[Sitecore Edge GraphQL]
        A5[Legacy Batch / SFTP Bridge]
    end

    subgraph postman [Postman Governance]
        P1[Correlation ID Check]
        P2[API Version Header]
        P3[HSTS Validation]
        P4[Response Time Gate]
    end

    S1 & S2 & S3 --> O --> apis --> postman
```

## Contents

| File | Description |
|------|-------------|
| `AMFAM_Discovery_Brief.md` | Technical discovery profile covering AMFAM's cloud topology (AWS, Azure/Sitecore, Vercel, on-prem), authentication (Okta), API maturity signals, friction points, and recommended engagement approach. |
| `AMFAM_Service_Template.yaml` | Postman Collection v2.1 template with pre-configured environment profiles (`amfam_core`, `homesite`, `connect`), Okta OAuth 2.0 client-credentials auth, governance-style header tests, and example requests. |

## Setup

### 1. Import the Collection

1. Open Postman
2. Click **Import** and select `AMFAM_Service_Template.yaml`
3. Select the appropriate environment profile for the subsidiary you're demoing against:
   - `amfam_core` — AWS-hosted core platform
   - `homesite` — Azure/Sitecore subsidiary
   - `connect` — Vercel-hosted services

### 2. Configure Authentication

1. Set the `client_id` and `client_secret` variables with values provided by the customer
2. The pre-request script automatically handles Okta OAuth 2.0 client-credentials token refresh
3. For self-contained demos, use Postman mock servers instead of live endpoints

### 3. Review the Brief

Read `AMFAM_Discovery_Brief.md` before the engagement to understand:
- AMFAM's multi-cloud topology and DNS structure
- Okta-based auth patterns across subsidiaries
- API maturity signals and friction points
- Recommended talking points and engagement approach

## Key Topics Covered

- Multi-subsidiary environment management (AmFam core, Homesite, Connect)
- Okta-based OAuth 2.0 client credentials flow
- API governance validation (correlation IDs, versioning headers, HSTS, response time)
- Kubernetes health probes (liveness, readiness, deep health)
- Legacy integration patterns (batch/SFTP bridge)
- Sitecore Edge GraphQL for content APIs

## Example Requests Included

| Category | Endpoints |
|----------|-----------|
| Platform Health | K8s liveness, readiness, deep health + Dynatrace probes |
| Auth | Okta token, introspect |
| Insurance | Policy/quote REST, claims FNOL, document upload |
| Agents | Agent lookup and management |
| Content | Sitecore Edge GraphQL queries |
| Legacy | Batch/SFTP bridge APIs |
| Governance | 401 probe, rate limit test, CORS preflight |
