# Rules of Engagement — read before every session

Bug bounty hunting is **authorized** security testing. The authorization comes
from the program's scope and rules page, and it is narrow. Staying inside it is
what separates a researcher from an attacker.

## Hard rules

- [ ] **Scope is law.** Test only assets explicitly listed as in-scope. If an
      asset isn't listed, treat it as out-of-scope until the program says
      otherwise.
- [ ] **Respect the allowed techniques.** Many programs forbid automated
      scanners, brute force, DoS/stress testing, social engineering, or physical
      attacks. Read the "out of scope" and "prohibited" sections, not just the
      asset list.
- [ ] **No data exfiltration.** Prove impact with the *minimum* necessary. For
      an IDOR, read one record that isn't yours and stop — don't dump a table.
      For RCE, run `id`/`whoami`, not a reverse shell into their infra.
- [ ] **No pivoting.** Don't use one finding to reach systems or data you
      weren't authorized to touch.
- [ ] **Don't degrade service.** Rate-limit yourself. If a test could knock a
      service over, don't run it unless the program explicitly allows it.
- [ ] **Stop and report if you hit real user data (PII), payment data, or
      credentials.** Don't keep poking. Note it, report it, and disclose
      responsibly.
- [ ] **Honor the disclosure policy.** Don't publish or share a finding until
      the program allows it. Most require coordinated disclosure.

## Before you start on a target

- [ ] Read the full program brief, scope, and reward table.
- [ ] Copy the scope verbatim into `targets/<program>/scope.md`.
- [ ] Note the testing window, allowed hours, and any required test-account
      setup or header/identifier the program asks you to include.
- [ ] Note what's explicitly **out** of scope.

## If you're unsure whether something is allowed

Ask the program (most platforms have a contact/question channel), or don't do
it. "I thought it was probably fine" is not a defense.

## Handling sensitive data in this repo

- Never commit real credentials, tokens, session cookies, or exfiltrated data.
- Target folders under `targets/` are git-ignored by default (see
  `.gitignore`) so working notes with sensitive detail don't get pushed by
  accident. Keep it that way unless you're certain a file is safe to publish.
