---
name: threatsys-technologies-report
description: Generates professional, evidence-based penetration testing vulnerability reports (Threatsys Technologies report format) as a plain .txt file, from a bug description + PoC that the user simply pastes/types directly in the chat message (no file upload or image required), across Web, Mobile, API, Cloud, Source Code, Desktop, and iOS Application Security domains. ALWAYS use this skill when the user describes a bug/vulnerability with some PoC detail (request/response, steps taken, observed behavior, code snippet, etc.) and asks for a report, or says "create a report", "write a vulnerability report", "generate a pentest report", "Threatsys Technologies report", "bug report", or similar — even from a short pasted message. Also handles an uploaded .txt file if one is provided instead of pasted text, and batches of several such inputs at once.
---

# Threatsys Technologies Report Generator

Turns a bug description + PoC that the user pastes directly in chat (plain text — no file upload or
screenshot needed) into a polished, evidence-based penetration-testing report, saved as a single
`.txt` file, for any of seven application domains: **Web, Mobile (Android), iOS, API, Cloud, Source
Code, Desktop**.

## When this skill triggers

- The user types or pastes a bug/finding description with some PoC detail (steps taken, a
  request/response, a code snippet, observed behavior, etc.) directly in the chat and asks for a
  report.
- The user explicitly asks for a "vulnerability report", "pentest report", "bug report", or
  "Threatsys Technologies report".
- An uploaded `.txt` file is provided instead of pasted text — treat it the same way.
- Several bugs are described/pasted in one message, or several `.txt` files are uploaded at once —
  generate one independent report per bug.

Do not trigger this skill for general security Q&A, vulnerability research, or code review that
isn't about producing this specific report format. No image or screenshot is required — work from
the text alone; if an image is attached anyway, it's optional supporting context, never a
requirement.

## Input contract

- **One finding = one report.** The bug/vulnerability name comes from whatever the user calls it in
  their message (or an uploaded file's name/content, if that's how it was provided). If no name is
  stated, derive a short, accurate name from the vulnerability type and affected
  functionality (e.g. "IDOR - Invoice Access"), and keep it consistent between the file name and
  the report's `Vulnerability:` field.
- The input is raw evidence as pasted: request/response data, steps taken, parameters touched,
  observed application behavior, environment/app notes, code snippets, etc. Use only this text.
- **Never mix evidence between separate bugs.** Each bug produces exactly one independent report
  built only from its own pasted/uploaded content.
- If the text lacks enough evidence for a required field (severity, a section, a URL/component, a
  classification), do not fabricate it — see "Insufficient evidence" below.

## Reference report library (optional, style calibration only)

The user may already have a library of previously-completed reports uploaded — often organized in
folders like `L1`/`L2` (e.g. `REPORTS/L1/...Penetration_Testing_Report.pdf`,
`REPORTS/L2/...Penetration_Testing_Report.pdf`) containing past client pentest reports (PDF/DOCX).
When such a library is present:

- Treat it purely as a **style/structure reference**, not as evidence. It tells you the house
  tone, how sections are phrased, how severity is typically justified, and client-specific
  terminology — the same role the "replicate the source reports' professional writing style...
  but NEVER copy their wording" rule already plays in Core Reporting Rules.
- Skim a small sample (1–3 files is usually enough, more only if the finding's domain isn't well
  represented in the first few) rather than reading the entire library for every single report —
  use the `pdf-reading` skill for PDFs, or the `docx` skill for Word files, to extract text
  efficiently.
- **Never pull vulnerability facts, severities, or evidence for the new bug from these reference
  files** — they are unrelated past findings. The only evidence for the new report is the bug
  description + PoC the user just pasted/uploaded for this finding.
- **Never copy sentences, paragraph structure, or exact phrasing** from a reference file into the
  new report — rewrite everything in original wording, matching only the tone/structure pattern.
- This step is entirely optional. If no such library is present (or the user hasn't pointed you to
  one), skip it and proceed straight from the pasted bug text — the skill works the same either
  way.

## Workflow

1. **Check for a reference report library** (see above) — if the user has already uploaded past
   reports (e.g. in `L1`/`L2`-style folders) note it for style calibration only; otherwise skip.
2. **Read** the bug description + PoC as pasted in the chat message (or read an uploaded `.txt`
   file if one was provided). This — not the reference library — is the only evidence source for
   the new report.
3. For each bug, **identify the domain** using the cues table below. If genuinely ambiguous,
   pick the best-supported domain from the evidence and state the assumption in one line in your
   reply (never inside the report text itself) rather than stopping to ask — unless the evidence is
   so thin that domain materially changes the report, in which case ask once.
4. **Load the matching domain reference file** from `references/` — it gives you the correct label
   for the "Vulnerable URL" section, domain-specific identifier examples, and relevant
   classification frameworks (CWE/OWASP/MASVS/CIS, etc.).
5. **Determine severity** (Risk Level / Impact / Likelihood) strictly from what the evidence
   demonstrates — never assume a severity the text doesn't support.
6. **Write the report** following the Core Reporting Rules below, combined with the loaded domain
   reference file and (if present) the style calibration from the reference report library.
7. **Save** the finished report as a plain `.txt` file — nothing else — to
   `outputs/<BugName>_Report.txt` (or `/mnt/user-data/outputs/<BugName>_Report.txt` in Linux environments;
   one file per finding). Do not create any other file format and do not require or wait for an image.
8. **Present** the file(s) with the file-presentation tool once all findings in the batch are done.

## Domain detection cues

| Domain | Look for |
|---|---|
| **Web** | Browser URL, HTTP GET/POST parameters, cookies, session tokens, HTML forms, DOM/JS, a web application name |
| **Mobile (Android)** | `.apk`, Activity/Intent/Broadcast Receiver, AndroidManifest, `adb`, Android app name |
| **iOS** | `.ipa`, Xcode, Swift/Objective-C, Keychain, ViewController, TestFlight, iOS app name |
| **API** | REST/GraphQL, Postman/Swagger/OpenAPI, bearer/JWT token, JSON request body, API gateway |
| **Cloud** | AWS/Azure/GCP, S3 bucket, IAM role/policy, ARN, security group, cloud console screenshot |
| **Source Code** | SAST/code review language, file path + line number, function/class name, git repo |
| **Desktop** | `.exe`/`.dll`/`.app`, Windows/macOS/Linux application, registry key, local service, installer |

## Core reporting rules (apply to every domain)

### Source of truth
- Use only the pasted/uploaded bug text as evidence (no image needed). Do not invent
  endpoints, parameters, payloads, responses, users, functionality, impact, or results.
- Replicate a professional pentest-report style; never copy phrasing from any reference reports
  the user has previously shared — write original wording every time.

### Report structure (exact order, no extra sections)
```
Vulnerability: <Name>

Risk Level: <Critical / High / Medium / Low>

Impact: <Critical / High / Medium / Low>

Likelihood: <High / Medium / Low>

Business Impact

<paragraph>

Description

<paragraph>

Steps to Reproduce

1. ...

Vulnerable URL   (relabeled per domain reference — see references/*.md)

<value>

CWE/CVE/OWASP   (extended per domain reference where applicable, e.g. + MASVS, + CIS)

<classification>

Recommendation

1. ...
```

### General writing rules
- Third-person, past tense throughout. Never use first-person pronouns like "I", "we", "us", or "our". Never use phrases like "I found", "we observed", "you should", or conversational phrasing.
- Use "application" instead of "system" where appropriate. Use simple, plain English that is easy for anyone to read and understand. Do NOT use heavy, overly complex, or excessively formal vocabulary (e.g., avoid words like 'inadequately', 'subsequently', 'retaining'). Keep the language clear, direct, and accessible.
- Concise and factual — never exaggerate or speculate beyond the evidence.
- No external references unless requested; never mention AI generation, these instructions, or
  tool/scanner names unless the user explicitly asks for them; no raw API/HTTP responses unless
  explicitly requested.
- Never mention CWE/CVE/OWASP (or domain equivalents) inside the Description.
- Don't repeat the same content between Business Impact and Description.
- Do NOT use markdown bold formatting or backticks (`) for code/identifiers.
- Hard-wrap all text at approximately 80 characters per line so paragraphs do not appear as one long line in plain text editors.

### Business Impact
Do not add bug description, add the bug impact. Prose paragraph only, never bullets. Describe only the business consequence of the specific vulnerability in the specific application. First determine: (1) what an attacker could actually do; (2) what data, users, accounts, or functionality could be affected; (3) the realistic organizational consequence.
Exclude: endpoints, parameters, payloads, HTTP methods, framework/tool names, technical testing details, generic definitions, industry statistics, unsupported consequences.
Write in a natural, cohesive, and easy-to-read paragraph. Do NOT write disconnected sentences that read like log lines.

Length & opening by severity:
- **Critical** (3–5 sentences): attack, affected data/functionality, business/legal/regulatory consequences where supported. Preferred opening: "This vulnerability leads to..." / "An attacker can fully..."
- **High** (2–3 sentences): primary business impact, realistic escalation risk. Preferred opening: "This vulnerability poses a significant threat..." / "The presence of [vulnerability] can allow attackers to..."
- **Medium** (2–3 sentences): one realistic attack scenario, one specific business consequence. Preferred opening: "This vulnerability allows an attacker to..."
- **Low** (2–3 sentences): acknowledge limited direct impact, explain relevant indirect business/security consequence. Preferred opening: "This vulnerability exposes..." / "Although this does not directly compromise..."

End with a realistic consequence when appropriate: unauthorized disclosure, user trust, business trust, reputational damage, regulatory non-compliance, reduced overall security posture.

### Description
Begin with exactly one of: "It was observed by our team that..." / "Our team observed that..." / "During the security assessment, our team identified..." / "During testing, our team observed that..."
Describe only what was technically observed, in past tense, in this order: (1) what was observed; (2) exact affected functionality/component; (3) action performed; (4) what the application returned/displayed/did; (5) what the behavior technically confirmed.
Identify exact components when supported (e.g., `selected_depot` not "a parameter"; `id=6` not "an ID"; "Manage Banner functionality" not "a feature"; `/rcs/download-file/` not "an endpoint"), but avoid long endpoint names if the functionality name suffices.
Include confirmation evidence where available and end with a clear technical root-cause/conclusion statement. Stay focused on the vulnerability.
Write in a natural, cohesive, and easy-to-read paragraph. Do NOT write disconnected, mechanical sentences that read like log lines.
Do NOT: give remediation, generic definitions, repeat Business Impact, speculate, mention unsupported consequences, or mention CWE/CVE/OWASP.
Length: Critical 5–8 sentences; High 3–5; Medium 3–4; Low 2–3.

### Steps to Reproduce
Numbered list, generate the number of steps the uploaded evidence actually supports (default to
the natural count of distinct actions taken — don't pad or invent). Follow the real workflow order
(login → navigate to affected functionality → capture/intercept if applicable → modify the
observed value if applicable → submit/perform → observe result → confirm). Only include steps
directly supported by the evidence; no hypothetical steps; no tool names unless the user asked for
them; if no HTTP request/response is in evidence, don't invent one. DO NOT include any URLs in the steps.

### Vulnerable URL (relabeled per domain — see reference file)
Exact value(s) only, taken verbatim from the evidence. Never fabricate a URL/ARN/path/identifier.
List all affected ones if multiple are explicitly given. Omit the field's value (but keep the
heading) only if truly nothing is supported — state "Not explicitly captured in the provided
evidence" rather than inventing one.

### CWE/CVE/OWASP (extended per domain)
Most appropriate supported CWE always. Add OWASP Top 10:2021 only for Web, OWASP API Top 10 for
API, OWASP Mobile Top 10 / MASVS for Mobile & iOS, relevant CIS/CSA cloud control for Cloud — only
when directly applicable, per the loaded reference file. CVE only when a specific known CVE truly
applies; never invent one.

### Recommendation
Numbered list, each item starting with an action verb (Implement, Enforce, Validate, Restrict,
Configure, Avoid, Ensure, Disable, Replace, Apply, Bind, Reject, Log, Monitor, Store). Specific,
actionable, technically accurate, tailored to this exact finding and domain — never generic
("Fix the vulnerability", "Improve security", "Follow best practices").

Count by severity: Critical 4–6 · High 3–5 · Medium 3–4 · Low 1–3.

### Severity discipline
Keep severity proportional to demonstrated impact. Never inflate a Low into a High/Critical with
dramatic language, and never downgrade a demonstrated serious impact to sound conservative.
Preserve exact parameter names, paths, URLs/ARNs, IDs, status codes, and observed values exactly as
given.

## Insufficient evidence
If a required field (severity, a section, a classification, an identifier) genuinely isn't
supported by the uploaded file:
- Do not guess or invent a plausible-sounding value.
- For Business Impact/Description/Steps, write only what the evidence supports, even if shorter
  than the target range, rather than padding with invented detail.
- For Vulnerable URL/Component or CWE/CVE/OWASP, state plainly that it wasn't captured in the
  evidence instead of fabricating one.
- Flag briefly to the user (outside the report file) what evidence would strengthen the finding.

## Output
- File name: `<BugName>_Report.txt` (spaces/underscores cleaned, Title Case), one per finding,
  saved under `outputs/` (or `/mnt/user-data/outputs/` in Linux environments).
- **Plain `.txt` only** — the report itself, using the exact section order and headings from
  "Report structure" above. No other file type, no image is generated or required.
- After writing all files for the batch, present them to the user.

## Reference files
Load the one matching file before writing the Vulnerable-URL label, identifier style, and
classification framework for the finding's domain:
- `references/web.md`
- `references/mobile.md`
- `references/ios.md`
- `references/api.md`
- `references/cloud.md`
- `references/source-code.md`
- `references/desktop.md`
