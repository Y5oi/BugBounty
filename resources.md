# Resources & Learning Path

Curated, not exhaustive. Depth beats breadth. If you do one thing, do
PortSwigger's Web Security Academy end to end.

## Core training (free)

- **[PortSwigger Web Security Academy](https://portswigger.net/web-security)** —
  the single best free resource. Interactive labs for every major web vuln
  class, written by the Burp Suite team. Work it in this order for bounty ROI:
  1. Access control / IDOR
  2. Authentication
  3. SSRF
  4. Business logic
  5. SQL injection
  6. XSS
  7. Everything else as you grow
- **[OWASP Top 10](https://owasp.org/www-project-top-ten/)** — the vocabulary
  and categories you'll use in every report.
- **[OWASP Web Security Testing Guide (WSTG)](https://owasp.org/www-project-web-security-testing-guide/)**
  — the systematic "what to test" reference.
- **[OWASP API Security Top 10](https://owasp.org/www-project-api-security/)** —
  most modern bounties are API bugs; BOLA/BFLA (access control) dominate.

## Tools

- **Burp Suite Community** or **[Caido](https://caido.io/)** — the capturing
  proxy is your primary tool. Learn one deeply.
- **ffuf / dirsearch** — content discovery (respect rate limits and scope).
- **subfinder / amass** — subdomain enumeration for wildcard scopes.
- **Browser dev tools** — underrated; the Network tab alone finds a lot.

## Report-writing

- Read other researchers' **disclosed reports** (e.g. HackerOne Hacktivity) to
  see what a payable writeup looks like. Clarity and reproducibility win.

## Staying sharp / community

- **Black Hat MEA Bug Bounty Cup** — the bugbounty.sa leaderboard feeds the
  annual competition (top 100). A concrete goal to aim the grind at.
- Follow a few working hunters for methodology, not for "one weird trick."

## Study rhythm suggestion

- Pick one vuln class. Finish all its PortSwigger labs. Then find that class on
  a real in-scope target. Repeat. You'll have a payable skill in weeks, not
  months, by going one class deep at a time.
