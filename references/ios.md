# iOS Application Security — domain reference

**Section label:** rename `Vulnerable URL` to `Vulnerable Component`.

**Value format:** the exact ViewController, API endpoint the app calls, URL scheme/Universal Link,
or storage mechanism involved, e.g. `LoginViewController` (biometric bypass), a custom URL scheme
`myapp://reset-password?token=...`, or Keychain item `com.example.app.authToken`.

**Identifier style in Description:** name the exact ViewController/delegate method, Keychain
entry, URL scheme handler, or API call affected — e.g. "the `authToken` Keychain entry", "the
`application(_:open:options:)` URL scheme handler", "the `/api/v1/invoice` endpoint called by
`InvoiceViewController`" — not generic terms like "a screen" or "a value".

**Typical evidence to look for:** Xcode/LLDB or Frida output, IPA decompilation/strings dumps,
Keychain dump contents, URL scheme/Universal Link handling, TestFlight/App Store build notes,
network traffic captured from the app (e.g. via a proxy), NSUserDefaults/plist contents.

**Classification guidance:**
- Give the most specific CWE supported (e.g. CWE-312 Cleartext Storage of Sensitive Information,
  CWE-295 Improper Certificate Validation, CWE-926 improper component export equivalent for
  URL-scheme handling, CWE-284 Improper Access Control, CWE-798 Hard-coded Credentials).
- Add the matching OWASP Mobile Top 10 (2024) category (e.g. M2 Insecure Data Storage, M5
  Insecure Communication) and the relevant MASVS control ID (e.g. MSTG-STORAGE-1,
  MSTG-NETWORK-3) when the evidence maps cleanly to one.
- CVE only for a known, specifically identified library/OS-level CVE.

**Recommendation patterns to draw from:** store secrets exclusively in Keychain with appropriate
accessibility/`kSecAttrAccessible` flags, validate and whitelist all incoming URL scheme/Universal
Link parameters, enforce certificate/public-key pinning, disable insecure `NSAllowsArbitraryLoads`
exceptions in ATS config, require biometric/passcode re-authentication for sensitive actions,
remove hard-coded secrets/API keys from the binary, enable jailbreak/tamper detection where
relevant.
