# Customer Technical Profile: American Family Insurance (AMFAM)

**Prepared:** March 24, 2026
**Target:** American Family Insurance Group (amfam.com)
**Revenue:** >$10B | **Employees:** >10,000 | **Policyholders:** 13.1M across 14 operating companies

---

## Cloud Topology & Edge Strategy

| Layer | Technology | Evidence |
|-------|-----------|----------|
| **Primary Cloud** | **AWS** (preferred, multi-year deal) | BusinessWire announcement (Dec 2022); CloudFront headers on `myaccount.amfam.com`; Homesite subsidiary DNS on Route53 (`awsdns-*.com`); `www.amfam.com` proxied through AWS Ohio IPs (`3.142.155.74`, `3.13.204.8`) |
| **Secondary Cloud** | **Azure** (Sitecore CMS only) | `cm-prd.amfam.com` → `mc-*.azurewebsites.net` (Azure App Service, East US); IP-restricted (`403 Ip Forbidden` with `x-ms-forbidden-ip`) |
| **Edge / Web** | **Vercel** (public website) | `X-Vercel-Cache: HIT`, `X-Powered-By: Next.js`, `X-Vercel-Id` headers; Vercel team: `american-family-insurance`; exposed deploy URL: `amfam-prod-ku28cmh4z-american-family-insurance.vercel.app` |
| **Legacy On-Prem** | **AT&T Managed** (core services) | `api.amfam.com` → `165.200.238.15` (rDNS: `sftpmft1a.amfam.com`); `connectbyamfam.com` → `165.200.238.68/239.68`; firewalled from public internet |
| **CDN Mix** | CloudFront / Vercel Edge / Limelight | `myaccount`: CloudFront (`X-Cache: RefreshHit from cloudfront`); `www`: Vercel Edge; `connect.amfam.com` → `afi.lg4.net` (Limelight Networks) |

**DNS:** AT&T managed DNS (`els-gms.att.net`) + self-hosted (`ns1/ns2.amfam.com`) for `amfam.com`, `connectbyamfam.com`, `af1platform.com`. Homesite subsidiary uses AWS Route53 independently.

---

## Auth Pattern & Access Model

| Signal | Finding |
|--------|---------|
| **IAM Platform** | **Okta** (detected via Enlyft tech stack profiling) |
| **Consumer Auth** | Password + SMS/email 2FA on `myaccount.amfam.com` (Angular SPA, `ds-app-root`) |
| **Passkey Support** | Listed on passkeys directory—traditional password still primary |
| **OAuth/OIDC Public Endpoints** | **None detected.** No public `.well-known/openid-configuration` surface. |
| **API Keys** | No public API key issuance observed; no developer portal exists. |
| **CORS Posture** | Not observable; API surface is entirely firewalled. |

**Assessment:** Enterprise-internal auth only. No public API access model. Okta for workforce identity. Consumer portal is a siloed Angular app on CloudFront/S3 with session-based auth.

---

## API Maturity Level: 1 (Initial / Enterprise-Internal)

| Indicator | Detail |
|-----------|--------|
| **Public Developer Portal** | **None.** No `developer.amfam.com`, no public docs. |
| **Public API Surface** | **Firewalled.** `api.amfam.com` (165.200.238.15) times out from public internet. Reverse DNS reveals SFTP/MFT service sharing the same IP (`sftpmft1a.amfam.com`). |
| **Published OpenAPI / Swagger** | **None found** publicly. |
| **Public GitHub Org** | **None.** `amfamlabs.com` (Data Science lab) returns **502 Bad Gateway**. No official GitHub organization discovered. |
| **Public Postman Workspaces** | **None.** Despite having `postman-domain-verification` in DNS TXT records, no public workspaces or collections are visible on the Postman API Network. |
| **API Style** | Unknown publicly. Homesite subsidiary has migrated to **microservices on AWS** (515% reduction in quote-to-bind time). Core AMFAM likely mix of legacy + modernizing. |
| **Gateway** | **Not detected.** No Kong, Apigee, or AWS API Gateway headers visible. Rate limiting headers absent. |

---

## Developer Friction Signals

| # | Signal | Severity | Evidence |
|---|--------|----------|----------|
| 1 | **No public developer portal** | High | DNS probes for `developer.*`, `developers.*`, `portal.*` all return empty. No docs site found via web search. |
| 2 | **API domain completely firewalled** | High | `curl -sI https://api.amfam.com/` times out; 165.200.238.15 is on-prem AT&T-managed. |
| 3 | **SFTP/MFT co-located with API domain** | Medium | Reverse DNS: `sftpmft1a.amfam.com` on same IP as `api.amfam.com`—suggests file-based integration patterns still in use. |
| 4 | **Sitemap URL concatenation bug** | Medium | `sitemap.xml` contains malformed URLs: `https://www.amfam.comhttps://cm-prd.amfam.com/agents/...` (double-URL concatenation). |
| 5 | **Innovation lab site is down** | Medium | `amfamlabs.com` returns 502 Bad Gateway. |
| 6 | **No public Postman workspaces despite domain verification** | Medium | TXT record confirms Postman adoption internally, but zero public-facing collections. |
| 7 | **Homesite subsidiary still on legacy IIS/ASP.NET** | Low | `www.homesite.com`: `Server: Microsoft-IIS/10.0`, `X-Powered-By: ASP.NET`, redirects to `go.homesite.com`. |
| 8 | **CONNECT platform returning errors** | Medium | `policyservice.connectbyamfam.com` returns SSL errors and HTTP 500s. |
| 9 | **Three separate CDN strategies** | Low | CloudFront (myaccount), Vercel Edge (www), Limelight (connect)—fragmented delivery layer. |

---

## Verified Internal Toolchain (via DNS TXT Records & Headers)

| Tool | Category | Evidence |
|------|----------|----------|
| **Postman** | API Platform | `postman-domain-verification=0daa2ec5...` TXT record |
| **Atlassian** | Dev Collaboration | `atlassian-domain-verification=f+IZHzY...` TXT record |
| **Docker** | Containers | `docker-verification=ad8e2c32...` TXT record |
| **HashiCorp Cloud Platform** | Infrastructure-as-Code | `hcp-domain-verification=35c8a8c8...` TXT record |
| **Dynatrace** | Observability | `Dynatrace-site-verification=be571207...` TXT record |
| **1Password** | Secrets Mgmt | `1password-site-verification=IBJLGVHR...` TXT record |
| **Miro** | Collaboration | `miro-verification=1750055a...` TXT record |
| **OneTrust** | Privacy/Consent | `onetrust-domain-verification` TXT (x2) + `cdn.cookielaw.org` in CSP |
| **Sitecore XM Cloud** | Headless CMS | `edge.sitecorecloud.io` in CSP; Azure-hosted CMS (`cm-prd.amfam.com`) |
| **ContentSquare** | Digital Analytics | `t.contentsquare.net` in CSP |
| **Verint** | Customer Engagement | `ucm-us.verint-cdn.com` in CSP |
| **Adobe Audience Manager** | DMP | `americanfamilymutualinsurancecompany.demdex` in CSP |
| **Salesforce Marketing Cloud** | Email Marketing | `SFMC-*` TXT records + `cust-spf.exacttarget.com` in SPF |
| **Proofpoint** | Email Security | MX: `pphosted.com`; DMARC `rua`/`ruf` → `emaildefense.proofpoint.com` |
| **Praetorian Chariot** | Attack Surface Mgmt | `chariot=...@praetorian.com` TXT record |
| **Kubernetes** | Container Orchestration | Enlyft tech stack profile |
| **Okta** | Identity & Access | Enlyft tech stack profile |
| **CouchDB** | Database | Enlyft tech stack profile |
| **Tableau Online** | BI & Analytics | Enlyft tech stack profile |

---

## Domain Map

```
amfam.com (AT&T DNS + self-hosted NS)
├── www.amfam.com ──────── Vercel (Next.js) → AWS Ohio IPs
├── myaccount.amfam.com ── AWS CloudFront + S3 (Angular SPA)
├── api.amfam.com ──────── On-prem 165.200.238.15 (firewalled)
├── cm-prd.amfam.com ───── Azure App Service (Sitecore CMS, IP-restricted)
├── connect.amfam.com ──── Limelight Networks CDN (conn refused)
└── af1platform.com ────── AT&T DNS (subsidiary platform)

connectbyamfam.com (AT&T DNS + self-hosted NS)
├── root ───────────────── On-prem 165.200.238.68 → redirects to Vercel
├── www ────────────────── Vercel → redirects to amfam.com/welcome
└── policyservice ──────── AWS (same as www.amfam.com), returning errors

homesite.com (AWS Route53)
└── www.homesite.com ───── IIS/10.0 + ASP.NET → redirects to go.homesite.com

americanfamily.com (GCD DNS)
└── (separate domain, minimal infrastructure)
```

---

## Recommended Engagement Approach

### Entry Point: Internal API Standardization & Governance

AMFAM is a **confirmed Postman customer** (domain verification present) with **zero public API footprint**. This creates a high-value engagement opportunity:

1. **Start with what they already have.** Postman domain verification proves internal adoption. The conversation opener is: *"We see your teams are using Postman internally—let's talk about how to get more value from that investment across your 14 operating companies."*

2. **The multi-cloud, multi-subsidiary challenge.** AMFAM operates across AWS (primary), Azure (CMS), Vercel (web), and legacy on-prem—with subsidiaries like Homesite on separate stacks (IIS/ASP.NET) and CONNECT on yet another. API governance across this landscape is the natural pain point.

3. **Modernization is already underway.** The Sitecore monolith-to-composable migration, the AWS multi-year deal, and the Homesite microservices transformation all signal an organization in active modernization. APIs are the connective tissue they'll need.

### Opportunity Areas

| Opportunity | Signal | Potential Impact |
|------------|--------|-----------------|
| **Standardize internal API practices** across 14 operating companies | Postman adoption exists but no public collections; fragmented subsidiary stacks | Platform consolidation, API catalog |
| **Build a developer portal** | No portal exists; no public docs; API is firewalled | Partner/agent ecosystem enablement |
| **Governance across multi-cloud** | AWS + Azure + Vercel + on-prem with no visible API gateway | API gateway strategy, policy enforcement |
| **Accelerate Homesite modernization** | Homesite moving from monolith → microservices on AWS | API-first design, testing in CI/CD |
| **Sunset legacy integration patterns** | SFTP/MFT on same IP as API domain; file-based integration still active | API-based integration replacing file transfers |

### Risk Factors

| Risk | Detail |
|------|--------|
| **Enterprise-internal only posture** | Entire API surface is firewalled. Engagement will require internal champions; no public surface to demo against. |
| **AT&T managed infrastructure** | Core DNS and on-prem networking is AT&T managed—infrastructure decisions may involve telecom vendor relationships. |
| **Subsidiary fragmentation** | 14 operating companies likely have independent tech decisions. Platform standardization conversations need executive sponsorship. |
| **Active vendor ecosystem** | Dynatrace, Okta, Sitecore, HashiCorp, Atlassian, ContentSquare, Verint—many vendors at the table. Postman positioning must complement, not compete. |
