Here's a focused breakdown of what you need to know about Azure API Management (APIM) for the AZ-204 exam:

---

## Azure API Management – AZ-204 Exam Guide

### 1. Core Concepts

- **What APIM is**: A gateway that sits in front of your backend APIs, providing a unified entry point, security, throttling, transformation, and documentation.
- **Components**: API Gateway, Developer Portal, Management Plane.
- **Tiers**: Consumption, Developer, Basic, Standard, Premium — know that Consumption is serverless/pay-per-call and has no SLA, Developer is not for production.

---

### 2. Creating and Configuring APIs

- Import APIs from: OpenAPI (Swagger), WSDL, WADL, Azure Functions, Logic Apps, App Service.
- Define **operations**, **products** (groups of APIs with subscription requirements), and **subscriptions**.
- APIs can be versioned and have **revisions**.

---

### 3. Versioning vs. Revisions — Know the Difference

- **Versions**: Breaking changes. Uses URL path (`/v1/`, `/v2/`), query string, or HTTP header to differentiate. Each version is a distinct API.
- **Revisions**: Non-breaking changes within the same version. Changes are isolated until you make a revision "current". Supports rollback. Appears in the changelog.
- Exam frequently tests: use revisions for non-breaking changes, versions for breaking changes.

---

### 4. Policies — This Is the Core Exam Topic

Policies are XML-based transformations applied to requests/responses. Know the structure:

```xml
<policies>
  <inbound>   <!-- applied to the incoming request -->
  <backend>   <!-- applied before forwarding to backend -->
  <outbound>  <!-- applied to the response -->
  <on-error>  <!-- applied if an error occurs -->
</policies>
```

**Key policies to know:**

|Policy|Purpose|
|---|---|
|`rate-limit`|Limit calls per subscription per time window|
|`rate-limit-by-key`|Limit by custom key (e.g., IP address)|
|`quota`|Long-period call/bandwidth caps per subscription|
|`ip-filter`|Allow/deny by IP|
|`set-header`|Add/remove/modify headers|
|`rewrite-uri`|Transform the URL before hitting backend|
|`cache-lookup` / `cache-store`|Response caching|
|`validate-jwt`|Validate JWT tokens (OAuth/Entra ID)|
|`check-header`|Verify header presence/value|
|`set-backend-service`|Dynamically change backend URL|
|`mock-response`|Return a mocked response|
|`forward-request`|Send request to backend (used in `<backend>`)|
|`cors`|Handle CORS|
|`choose` / `when`|Conditional policy logic|

**Policy scopes** (inheritance order): Global → Product → API → Operation. More specific scopes override or extend less specific ones. Use `<base />` to include parent scope policies.

---

### 5. Security

- **Subscriptions**: APIs can require a subscription key sent via header (`Ocp-Apim-Subscription-Key`) or query string.
- **OAuth 2.0 / Entra ID**: Use `validate-jwt` policy to validate bearer tokens. You configure authorization servers in APIM.
- **Client certificates**: Mutual TLS authentication.
- **Managed Identity**: APIM can use a managed identity to authenticate to backends (e.g., Key Vault, other Azure services).

---

### 6. Products and Subscriptions

- **Products** bundle one or more APIs. They can be Open (no subscription needed) or Protected (requires subscription).
- **Subscriptions** are scoped to: All APIs, a Product, or a single API.
- Subscription keys are passed as `Ocp-Apim-Subscription-Key`.

---

### 7. Developer Portal

- Auto-generated, customizable portal for API consumers.
- Allows developers to discover, test, and subscribe to APIs.
- Not heavily tested, but know it exists and what it does.

---

### 8. Backends and Named Values

- **Named Values**: Key-value pairs (like variables/secrets) referenced in policies using `{{my-value}}`. Can be plain text, secrets, or Key Vault references.
- **Backends**: Define reusable backend services. Useful for dynamic routing.

---

### 9. Monitoring and Diagnostics

- **Application Insights integration**: APIM can send telemetry (requests, responses, latency) directly to App Insights.
- **Azure Monitor / Diagnostic Logs**: Log API calls, errors.
- **Built-in analytics**: Request counts, latency, top APIs in the portal.

---

### 10. Things the Exam Specifically Tests

- Choosing **revisions vs. versions** for a given scenario.
- Writing or identifying the correct **policy** for a requirement (rate limiting, JWT validation, caching, header manipulation).
- Knowing that `validate-jwt` goes in the `<inbound>` section.
- Understanding **subscription key** mechanics.
- When to use `rate-limit` (per subscription) vs. `rate-limit-by-key` (per arbitrary key like IP).
- The **Consumption tier** has no VNet integration and no developer portal.
- Policy scope inheritance and the role of `<base />`.

---

Focus your time on **policies** — that's where the exam questions concentrate. If you can read a policy XML snippet and explain what it does, or identify the right policy for a scenario, you'll handle most APIM questions fine.


## Revisions vs Versions in Azure API Management

**Revisions** and **versions** solve different problems and are often confused.

---

### Revisions — "Safe changes to the same API"

Use a revision when you want to make a **non-breaking change** to an existing API without disrupting current consumers.

- Each API has one _current_ revision; others are accessible via a special query parameter (`?api-version=...` is NOT used — instead it's `api-revision=2` or a revision-specific URL)
- Only one revision is "current" at a time
- Old revisions remain accessible but aren't the default
- Designed for **internal iteration and testing** before promoting to current
- Consumers don't need to change anything when you promote a revision

**Use when:** fixing a policy bug, tweaking a backend URL, adjusting rate limits, adding an optional parameter, refactoring without breaking contracts.

---

### Versions — "Breaking changes, multiple audiences"

Use a version when you're introducing a **breaking change** and need **multiple variants to coexist long-term** for different consumers.

- Versions are surfaced via URL path (`/v1/`, `/v2/`), query string (`?api-version=2`), or header
- Consumers explicitly opt into a version
- Multiple versions run simultaneously and are all "live"
- Each version can itself have multiple revisions

**Use when:** removing/renaming fields, changing authentication schemes, restructuring endpoints, overhauling the API contract.

---

### The Decision Rule

|Question|Answer → Use|
|---|---|
|Will existing consumers break?|Yes → **Version**|
|Is this a draft/test before going live?|Yes → **Revision**|
|Do multiple consumers need different contracts long-term?|Yes → **Version**|
|Is this a safe tweak to the current live API?|Yes → **Revision**|

---
