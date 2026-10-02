# Web Application Security — domain reference

**Section label:** keep as `Vulnerable URL`.

**Value format:** the exact affected page/endpoint URL, including the vulnerable parameter where
relevant, e.g. `https://app.example.com/invoice?invoice_id=1042`. List every distinct affected URL
if more than one was demonstrated. Never use the bare homepage/base domain if a specific page or
endpoint was shown.

**Identifier style in Description:** name the exact parameter, cookie, header, form field, or
page/feature affected — e.g. `invoice_id` parameter, `session_token` cookie, the "Manage Banner"
page — not generic terms like "a parameter" or "a page".

**Typical evidence to look for:** browser request/response pairs, parameter tampering, cookie/
session values, DOM output, form submissions, error messages, redirect behavior, file upload
responses.

**Classification guidance:**
- Always give the most specific CWE supported (e.g. CWE-89 SQL Injection, CWE-79 XSS, CWE-639
  IDOR/Insecure Direct Object Reference, CWE-352 CSRF, CWE-287 Improper Authentication, CWE-611
  XXE, CWE-918 SSRF).
- Add the matching OWASP Top 10:2021 category when directly applicable (e.g. A01:2021 – Broken
  Access Control, A03:2021 – Injection, A07:2021 – Identification and Authentication Failures).
- Include a CVE only if the finding is a known, specifically identified CVE (e.g. in an outdated
  library) — not for custom application logic flaws.

**Recommendation patterns to draw from (tailor, don't copy verbatim):** parameterized queries/
prepared statements, output encoding/CSP, server-side authorization checks per object, anti-CSRF
tokens + SameSite cookies, strict input validation/allow-lists, secure session cookie flags
(HttpOnly, Secure, SameSite), rate limiting, disabling verbose error messages.
