# Mobile (Android) Application Security — domain reference

**Section label:** rename `Vulnerable URL` to `Vulnerable Component`.

**Value format:** the exact exported Activity/Service/Broadcast Receiver/Content Provider, Intent
action, API endpoint the app calls, or storage location involved, e.g.
`com.example.app/.InvoiceActivity` (exported, no permission check) or the backend endpoint the app
calls, `/api/v1/invoice/{id}`. Include the manifest component name whenever it is the actual
vulnerable surface, not just the backend URL.

**Identifier style in Description:** name the exact Activity/Intent/permission/storage mechanism —
e.g. "the exported `InvoiceActivity`", "the `ACTION_VIEW` intent filter", "data stored in
`SharedPreferences` in plaintext" — not generic terms like "a component" or "a file".

**Typical evidence to look for:** `adb` command output, AndroidManifest.xml excerpts, decompiled
code snippets, intent sniffing, logcat output, local storage/SharedPreferences/SQLite dumps,
Frida/dynamic instrumentation output, network traffic from the app.

**Classification guidance:**
- Give the most specific CWE supported (e.g. CWE-926 Improper Export of Android Application
  Components, CWE-312 Cleartext Storage of Sensitive Information, CWE-295 Improper Certificate
  Validation, CWE-284 Improper Access Control).
- Add the matching OWASP Mobile Top 10 (2024) category when applicable (e.g. M1 Improper Platform
  Usage, M2 Insecure Data Storage, M5 Insecure Communication, M8 Insufficient Binary Protections),
  and the relevant MASVS control ID (e.g. MSTG-STORAGE-1, MSTG-PLATFORM-11) if the evidence maps
  cleanly to one.
- CVE only for a known, specifically identified library/OS-level CVE.

**Recommendation patterns to draw from:** set `android:exported="false"` on internal components,
enforce signature-level permissions on exported components, use `EncryptedSharedPreferences`/
Android Keystore for sensitive data, enforce certificate pinning, validate all inter-app Intents,
strip debug/logging in release builds, apply root/jailbreak and tamper detection where relevant.
