# Desktop Application Security — domain reference

**Section label:** rename `Vulnerable URL` to `Vulnerable Component`.

**Value format:** the exact binary, DLL/shared library, installed service, or registry key
involved, e.g. `C:\Program Files\App\updater.exe` or registry key
`HKLM\SYSTEM\CurrentControlSet\Services\AppService` (writable by non-admin users).

**Identifier style in Description:** name the exact executable/service/registry key/IPC channel
affected — e.g. "the `updater.exe` process", "the `AppService` Windows service binary path", "the
named pipe `\\.\pipe\AppIPC`" — not generic terms like "a file" or "a process".

**Typical evidence to look for:** process monitor (Procmon)/Process Explorer output, file/registry
ACL dumps (`icacls`, `Get-Acl`), service binary path permissions, local privilege-escalation PoC
steps, IPC/named-pipe traffic, installer behavior, DLL search-order evidence.

**Classification guidance:**
- Give the most specific CWE supported (e.g. CWE-250 Execution with Unnecessary Privileges,
  CWE-269 Improper Privilege Management, CWE-427 Uncontrolled Search Path Element (DLL hijacking),
  CWE-732 Incorrect Permission Assignment for Critical Resource).
- OWASP Top 10 generally does not apply to native desktop findings — omit it unless the desktop
  app embeds a web view with a genuinely web-style flaw.
- CVE only for a known, specifically identified CVE in the application or a bundled component.

**Recommendation patterns to draw from:** restrict file/registry/service ACLs to administrators
only, run the service/process with the minimum privilege required (avoid SYSTEM/root where
unnecessary), validate and fix the binary's DLL search order or use fully-qualified load paths,
sign and verify the integrity of auto-update packages before execution, restrict IPC endpoints to
authenticated local callers only.
