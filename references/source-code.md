# Source Code Security Review — domain reference

**Section label:** rename `Vulnerable URL` to `Affected Code Location`.

**Value format:** the exact file path, line number(s), and function/class name, e.g.
`src/controllers/InvoiceController.php:142` in the `getInvoiceById()` function. List every
distinct affected location if more than one was reviewed/demonstrated.

**Identifier style in Description:** name the exact function, variable, or code construct
affected — e.g. "the `getInvoiceById()` function", "the unparameterized `$query` string", "the
`os.system()` call" — not generic terms like "a function" or "some code".

**Typical evidence to look for:** source code excerpts (with context), SAST/static-analysis
findings, git blame/commit references, data-flow traces from user input to a dangerous sink,
unit-test or manual trace output confirming exploitability.

**Classification guidance:**
- Give the most specific CWE supported — this domain is CWE-first (e.g. CWE-89 SQL Injection,
  CWE-78 OS Command Injection, CWE-502 Deserialization of Untrusted Data, CWE-327 Use of a
  Broken/Risky Cryptographic Algorithm, CWE-798 Hard-coded Credentials).
- Add OWASP Top 10:2021 only if the code backs a web application and the mapping is a clean fit;
  otherwise omit it rather than forcing an OWASP label onto non-web code.
- CVE only if the exact vulnerable third-party library/version is identified and has a known CVE.

**Recommendation patterns to draw from:** use parameterized queries/prepared statements instead of
string concatenation, replace shell `exec`/`system` calls with safe APIs or strict allow-lists,
avoid deserializing untrusted data (or use a safe/signed format), replace weak cryptographic
primitives with vetted modern algorithms, remove hard-coded secrets and load them from a secrets
manager, add input validation at the exact function identified.
