# API Security — domain reference

**Section label:** rename `Vulnerable URL` to `Vulnerable Endpoint`.

**Value format:** the exact API route and method, e.g. `GET /api/v1/invoices/{id}` or
`POST /graphql` (operation name included if GraphQL, e.g. the `updateInvoice` mutation). List all
affected endpoints if more than one was demonstrated.

**Identifier style in Description:** name the exact path parameter, query parameter, JSON body
field, header, or GraphQL field/argument affected — e.g. `invoice_id` path parameter, the
`Authorization` bearer token, the `isAdmin` field in the request body — not generic terms like
"a parameter" or "a field".

**Typical evidence to look for:** raw HTTP requests/responses (e.g. via Postman/Burp), status
codes, JSON bodies, JWT/token contents, rate-limit headers, GraphQL introspection results, API
gateway/WAF responses.

**Classification guidance:**
- Give the most specific CWE supported (e.g. CWE-639 IDOR, CWE-287 Improper Authentication,
  CWE-798 Hard-coded Credentials, CWE-770 Missing Resource Consumption Limits for rate-limiting
  gaps, CWE-20 Improper Input Validation for mass-assignment/over-posting).
- Add the matching OWASP API Security Top 10 (2023) category when applicable (e.g. API1:2023
  Broken Object Level Authorization, API2:2023 Broken Authentication, API5:2023 Broken Function
  Level Authorization, API4:2023 Unrestricted Resource Consumption).
- CVE only for a known, specifically identified CVE in an API framework/library.

**Recommendation patterns to draw from:** enforce object-level authorization checks on every
request using the authenticated identity (not client-supplied IDs), validate JWT signature/
expiry/audience server-side, implement per-endpoint rate limiting and resource quotas, restrict
GraphQL introspection and query depth/complexity in production, apply strict schema/allow-list
validation to request bodies, return generic error messages instead of verbose stack traces.
