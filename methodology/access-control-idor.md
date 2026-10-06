# Access Control / IDOR / BOLA

**Why start here:** best value-to-difficulty ratio in bug bounty. Broken access
control is #1 on the OWASP Top 10, needs no exotic skills, and triagers rate it
high because impact is concrete (reading/modifying other users' data).

**Prereq:** two test accounts at the same privilege level (A and B), and ideally
one higher-privilege account. Set these up during recon.

## The core idea

The app checks *authentication* (are you logged in?) but fails *authorization*
(are you allowed to touch **this** object / action?).

## Checklist

### Object-level (IDOR / BOLA)
- [ ] Find every request containing an identifier: `id`, `user_id`, `uuid`,
      `invoice=`, `/orders/123`, filenames, etc.
- [ ] As user A, swap the identifier to one belonging to user B. Does A get B's
      data? → IDOR (read).
- [ ] Try the same on write/update/delete actions → IDOR (write), higher impact.
- [ ] Try non-numeric and guessable IDs; check if UUIDs leak elsewhere (so they
      aren't truly unguessable).
- [ ] Check APIs specifically — BOLA is the #1 API vuln. The web UI may hide an
      ID the API still trusts.

### Function-level (BFLA / privilege escalation)
- [ ] Find admin/privileged endpoints (from JS, Swagger, or guessing:
      `/admin`, `/api/admin/...`).
- [ ] Call them as a low-priv user. Does the server enforce the role, or only
      the UI hide the button?
- [ ] Try HTTP method swaps (GET→POST/PUT/DELETE) on protected resources.

### Common bypass tricks (only if in scope/allowed)
- [ ] Parameter pollution: `?id=A&id=B`.
- [ ] Wrap/encode the ID; send it in both body and query.
- [ ] Add/remove trailing path segments; path traversal in IDs.
- [ ] Mass assignment: add `"role":"admin"` or `"isAdmin":true` to a JSON body.

## Proving impact (minimally!)
Read exactly **one** record that isn't yours to prove it, then stop. Do not
enumerate or dump. Screenshot the one cross-account response, redact PII.

## Report hook
"Authenticated user A can read/modify resource X belonging to user B by changing
`<param>`, because the server does not verify object ownership." Include the two
requests side by side.
