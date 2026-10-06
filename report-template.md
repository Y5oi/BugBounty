# Vulnerability Report

> Copy this file, fill it in, and verify every repro step from a clean session
> before submitting. Report quality often decides the payout as much as the bug.

**Title:** `<Concise, specific — e.g. "IDOR in /api/v1/invoices allows reading other users' invoices">`

**Program / Target:** `<program name>`
**Asset (in-scope URL/endpoint):** `<exact asset from scope>`
**Severity (your assessment):** `<Critical / High / Medium / Low>`
**CVSS 3.1 vector (optional):** `<e.g. AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N>`
**Date found:** `<YYYY-MM-DD>`

---

## Summary

One or two sentences: what the vulnerability is, where it is, and what an
attacker can do with it. A triager should understand the whole bug from this
paragraph alone.

## Impact

What's the real-world consequence? Be concrete and honest — don't inflate.
- Who is affected?
- What data or action is exposed?
- What does an attacker need (authenticated? specific role? user interaction?)
  to pull it off?

## Steps to Reproduce

Numbered, exact, copy-pasteable. Assume the triager starts from zero.

1. Log in as a standard user (test account A: `<id>`).
2. Navigate to `<url>`.
3. Intercept the request to `<endpoint>`.
4. Change parameter `<param>` from `<A's value>` to `<B's value>`.
5. Observe that the response returns user B's `<data>`.

**Request:**
```http
GET /api/v1/invoices/1002 HTTP/1.1
Host: target.example
Authorization: Bearer <user A token>
```

**Response (abridged — no exfiltrated PII beyond proof):**
```http
HTTP/1.1 200 OK
{ "invoice_id": 1002, "owner": "userB", ... }
```

## Proof of Concept

Screenshots, a short screen recording, or the minimal request/response pair.
Redact any real PII — show just enough to prove the bug, nothing more.

## Remediation

How to fix it. Showing you understand the fix builds credibility and helps the
team act fast.
- e.g. "Enforce an authorization check that the authenticated user owns the
  requested `invoice_id` before returning it (object-level access control)."

## Notes

- Affected only this endpoint, or a pattern across the app?
- Any preconditions, caveats, or related endpoints worth checking.
