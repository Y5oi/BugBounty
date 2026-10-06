# Test Plan — nour.net.sa

Tailored strategy for this specific program. Read `scope.md` first.

## Strategic read of this program

Two facts shape everything:

1. **The out-of-scope list kills all the "easy/informational" findings** —
   missing headers, cookie flags, SSL/TLS, SPF/DKIM/DMARC, no-captcha, rate
   limiting, enumeration, self-XSS, CSP, clickjacking on non-sensitive pages,
   404 text injection. Do NOT spend a minute on these; they're auto-rejected.
2. **Low severity pays nothing.** You need a Medium+ with real security impact
   and working exploit code. So this is a "quality over quantity" target.

Net: you win here by finding a **real application bug in Nournet's own custom
code/config** — not a scanner nit, not a known CVE in an off-the-shelf
component (third-party / open-source libs are explicitly out of scope).

## What's actually IN scope and worth hunting (ranked)

### Tier 1 — highest value, go here first
- **Injection** (SQLi, command, SSTI, XXE) in any custom endpoint, search, or
  form handler → High/Critical. See `methodology/injection.md`.
- **SSRF** via any feature that fetches a URL server-side (contact forms that
  pull a URL, link previews, PDF/export generators, integrations) → High.
  See `methodology/ssrf.md`.
- **File upload abuse** — if there's a careers/CV upload, contact attachment, or
  any upload: test for web shell / RCE, path traversal, stored XSS via
  filename or content, content-type bypass → High/Critical.
- **Stored / Blind XSS** in form submissions that staff later view in an admin
  panel (contact, careers, support). This is a classic win on marketing sites:
  your payload fires in the *admin's* session, not yours → not self-XSS, real
  impact, Medium/High. See `methodology/xss.md`.

### Tier 2 — strong if any authenticated area exists on nour.net.sa
- **Access control / IDOR / BOLA** — only if the main site has a logged-in area
  (customer/account portal hosted *on* nour.net.sa, not a subdomain).
  See `methodology/access-control-idor.md`.
- **Authentication logic flaws → account takeover** — password-reset token
  weaknesses, host-header injection in reset links, OAuth `redirect_uri`
  abuse, JWT flaws, auth bypass. NOT rate-limiting/enumeration (those are out).
  See `methodology/authentication-session.md`.
- **Business logic** — any multi-step flow, quote request, order, or
  entitlement on the site. See `methodology/business-logic.md`.

### Tier 3 — allowed but lower ceiling; only report with real impact
- **Reflected XSS** on a page/parameter with a realistic delivery vector → Medium.
- **Open redirect** — not explicitly excluded; low on its own, but valuable as
  a chain (e.g. to boost an OAuth or SSRF finding).
- **CSRF** — ONLY on *authenticated* forms performing *sensitive* actions
  (unauth / non-sensitive CSRF is explicitly out of scope).
- **Clickjacking** — ONLY on a page with a genuine sensitive action.

## Recon focus (do this in your browser + Burp/Caido)
Since the whole scope is one site, recon is about mapping *its* functionality,
not finding subdomains:
- [ ] Click through every page; proxy all traffic. Catalog every form, every
      parameter, every background API/XHR call.
- [ ] Find: search box, contact form, careers/jobs + any CV upload, newsletter,
      any "request a quote / demo" flow, any login or account area ON nour.net.sa.
- [ ] Identify the stack from responses (headers, cookies, JS bundles, URL
      patterns). This tells you which injection/SSTI payloads to try.
- [ ] Note any parameter that looks like an ID, a URL, a filename, a redirect
      target, or an email/recipient field.
- [ ] Pull `robots.txt`, `sitemap.xml`, and JS files for hidden endpoints.

## Hard constraints from policy (keep you qualified)
- Manual testing only — **no automated scanners** (reports from scans rejected).
- No destructive/high-intensity scanning; don't spam forms; don't modify/delete data.
- Every report needs **working exploit code / PoC** and clear repro steps.
- One vuln per report.
- Prove impact minimally; securely delete any retrieved data afterward.
- Don't touch anything that isn't `nour.net.sa`.

## Suggested first session (2–3 hrs)
1. Full click-through + proxy capture; fill in `recon.md`.
2. Enumerate every form and upload on the site.
3. On the contact/careers forms: plant a **blind-XSS canary** (so it fires later
   if an admin views it) and test the upload thoroughly.
4. On search + any reflected parameter: test reflected XSS and SQLi manually.
5. Log every lead in `findings.md` with a confidence rating.
