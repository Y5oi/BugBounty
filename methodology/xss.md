# Cross-Site Scripting (XSS)

Common and good for building momentum, though often lower-paying than access
control. Impact depends on context — demonstrate a real consequence, not just
an `alert()`.

## Types
- **Reflected** — payload in the request reflects into the response.
- **Stored** — payload saved and served to other users (higher impact).
- **DOM-based** — client-side JS writes attacker input into a sink.

## Checklist
- [ ] Map every input that ends up rendered: search, profile fields, comments,
      filenames, error messages, URL params, headers.
- [ ] Inject a unique canary string first; find where it reflects and in what
      context (HTML body, attribute, JS, URL, etc.).
- [ ] Build the payload for that context:
      - HTML body: `<img src=x onerror=...>`
      - Attribute: break out with `">`
      - JS string: break out with `';`
      - URL/`href`: `javascript:` (if not sanitized)
- [ ] For stored XSS, confirm it executes in a **different** session/account
      (your account B), proving cross-user impact.
- [ ] DOM XSS: trace `location`/`document` sources to `innerHTML`/`eval`/
      `document.write` sinks in the JS.
- [ ] Check for filter/WAF bypasses only if in scope.

## Proving impact (do this — it raises severity)
Don't stop at `alert(1)`. Demonstrate something real but harmless:
- read a CSRF token or non-sensitive DOM value,
- show you could make a request as the victim,
- for stored XSS, show it fires for another user.
Never actually steal a real user's session or data. Use your own test accounts.

## Severity notes
- Stored XSS hitting other users / admins → high.
- Reflected needing user interaction → medium, lower if heavily mitigated.
- Self-XSS with no path to another user → usually **not** rewarded; don't report
  it alone.

## Report hook
"Input `<param>` renders unsanitized in a `<context>`, executing attacker JS in
the victim's session, demonstrated by <real but harmless action>."
