# SSRF (Server-Side Request Forgery)

The server fetches a URL you control. High impact when it can reach internal
services or cloud metadata. Rated high-to-critical by most programs.

## Where to look
- [ ] Any feature that takes a URL: webhooks, "import from URL", avatar/image
      fetch by URL, PDF/screenshot generators, link preview, document converters.
- [ ] URLs hidden in parameters: `url=`, `next=`, `dest=`, `feed=`, `callback=`,
      `image=`, `proxy=`, XML/SVG/`<img>` that the server renders.
- [ ] Parsers that fetch remote resources (XXE → SSRF via external entities).

## Checklist
- [ ] Point the parameter at a server you control (a collaborator/OOB endpoint).
      Confirm the server makes the request (DNS or HTTP hit).
- [ ] If confirmed, test reach to internal ranges **cautiously and minimally**:
      `127.0.0.1`, `localhost`, `169.254.169.254` (cloud metadata), internal
      hostnames. Stop as soon as you have proof — do not harvest credentials or
      pivot.
- [ ] Try bypasses if filtered (and allowed): alternate IP encodings, DNS
      rebinding, redirects (your server 302s to the internal target), `[::]`,
      `0.0.0.0`, adding ports.
- [ ] Note blind vs. full SSRF — blind (OOB only) is still reportable; full
      (response reflected back) is higher impact.

## Safety / scope
- Do **not** actually read cloud metadata credentials and use them. Proving the
  request reaches `169.254.169.254` (e.g. a 200 or a metadata path responding)
  is enough. Dumping and using IAM creds is pivoting — out of bounds.
- Keep OOB interaction to your own infrastructure.

## Report hook
"The `<param>` on `<endpoint>` causes the server to fetch an attacker-supplied
URL, and that request reaches `<internal target>`, as shown by <OOB proof>."
State the maximum demonstrated reach, not speculation.
