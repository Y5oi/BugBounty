# Recon Checklist

A repeatable flow for mapping a target **before** testing. Good recon is where
most findings actually come from. Keep notes per target in
`targets/<program>/recon.md`.

> Only run recon against in-scope assets, and only with techniques the program
> allows. Passive recon is almost always fine; active scanning may not be —
> check the rules.

## 0. Scope lock-in

- [ ] Copy the exact in-scope assets into `targets/<program>/scope.md`.
- [ ] Note wildcard scopes (`*.example.com`) vs. single hosts.
- [ ] Note what's explicitly out of scope.
- [ ] Note allowed techniques (scanners? brute force? rate limits?).

## 1. Passive recon (no traffic to target)

- [ ] Understand the business: what does the app *do*? Who are the users?
      Roles? Money flows? (Business logic bugs live here.)
- [ ] Public footprint: certificate transparency logs (crt.sh) for subdomains,
      search engines, public code/docs.
- [ ] Technology fingerprint: frameworks, languages, CDNs, WAF presence.

## 2. Attack surface mapping (in-scope only)

- [ ] Enumerate subdomains (within allowed scope).
- [ ] Crawl the app as an authenticated user; catalog every page and feature.
- [ ] Capture all endpoints via a proxy (Burp/Caido) while clicking through.
- [ ] Find API docs / Swagger / GraphQL introspection if exposed.
- [ ] Note every parameter, especially IDs, file paths, URLs, and redirects.
- [ ] Identify authentication and session mechanisms.
- [ ] Map user roles — create two test accounts (A and B) at the same privilege
      level, and if possible one higher-privilege account. Essential for
      access-control testing.

## 3. Prioritize

- [ ] Which features touch other users' data? (access control)
- [ ] Which take a URL or server-side fetch? (SSRF)
- [ ] Which handle money, quantity, or state transitions? (business logic)
- [ ] Which reflect input or render user content? (XSS)
- [ ] Which build queries, commands, or file paths from input? (injection)

Rank by **likely impact × likely presence**, then work the top class to depth
using the matching file in `methodology/`.

## Tooling notes

- A capturing proxy (Burp Suite Community or Caido) is the core tool.
- Keep automated scanning minimal and only if the program allows it — noisy
  scanners get accounts banned and rarely find the good bugs anyway.
