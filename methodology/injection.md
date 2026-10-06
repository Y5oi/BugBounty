# Injection (SQLi, Command, NoSQL, Template, etc.)

Input crosses into a query, command, or interpreter. Classic, high-impact, but
increasingly WAF-protected — worth it when you find a gap.

> Be careful and minimal. Injection PoCs can accidentally modify or destroy
> data. Prove read-only, prove small, never run destructive payloads.

## SQL injection
- [ ] Probe inputs with `'`, `"`, and observe errors/behavior changes.
- [ ] Boolean-based: `' AND 1=1-- ` vs `' AND 1=2-- ` → different responses.
- [ ] Time-based (blind): `' AND SLEEP(5)-- ` style, carefully (don't hammer).
- [ ] Confirm with a harmless read (e.g. retrieve DB version) — **do not** read
      user tables, drop, or write.
- [ ] Check all inputs: query params, body, headers (`User-Agent`, `Referer`),
      cookies, JSON fields, order/filter params.

## Command injection
- [ ] Inputs that feed OS commands (ping tools, file converters, exports).
- [ ] Separators: `;`, `|`, `&&`, `` `cmd` ``, `$(cmd)`.
- [ ] Prove with a benign command (`id`, OOB DNS callback), never a shell.

## NoSQL injection
- [ ] JSON operators: `{"$ne": null}`, `{"$gt": ""}` on login/filter endpoints
      → authentication bypass or data disclosure.

## Server-Side Template Injection (SSTI)
- [ ] Inputs reflected into server-rendered templates: test `{{7*7}}`,
      `${7*7}`, `#{7*7}` → if it renders `49`, investigate the engine.
- [ ] Escalate cautiously; SSTI often leads to RCE — prove the minimum.

## XXE
- [ ] XML inputs: external entity to read a benign file or trigger OOB request
      (overlaps with SSRF).

## Proving impact minimally
Retrieve the smallest possible proof (a version string, a single boolean
difference, a DNS callback). Never exfiltrate real data or run destructive
payloads — doing so can break the program's rules and cause real damage.

## Report hook
"Input `<param>` on `<endpoint>` is injected into a `<SQL/command/template>`
context, confirmed by <minimal proof>." State demonstrated impact only.
