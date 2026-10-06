# Authentication & Session Management

High-impact and common. Account takeover (ATO) is one of the best-paying
outcomes in bug bounty.

## Checklist

### Login & credentials
- [ ] Username enumeration via different error messages / timing / response
      codes on login, signup, and password reset.
- [ ] Rate limiting on login — is credential stuffing / brute force possible?
      (Only probe lightly; never run a real brute-force unless explicitly
      allowed.)
- [ ] Default/weak credentials on any admin or test interface.

### Password reset (a classic ATO source)
- [ ] Is the reset token predictable, short, or non-expiring?
- [ ] Host header poisoning: does the reset link use a `Host`/`X-Forwarded-Host`
      you control, leaking the token to your domain?
- [ ] Can you request a reset for victim and receive the token via response,
      referrer leak, or IDOR on the reset endpoint?
- [ ] Does the token get invalidated after use / after a new one is requested?

### Session management
- [ ] Does the session token change after login (session fixation)?
- [ ] Is the token invalidated on logout and on password change?
- [ ] Cookie flags: `HttpOnly`, `Secure`, `SameSite`.
- [ ] JWT issues: `alg:none`, weak secret, unverified signature, sensitive data
      in the payload, no expiry.

### MFA
- [ ] Can MFA be skipped by going straight to the post-login endpoint?
- [ ] Is the MFA code rate-limited / brute-forceable?
- [ ] Is MFA actually enforced on all sensitive actions, or just at login?

### OAuth / SSO
- [ ] `redirect_uri` validation — can you redirect the code/token to your host?
- [ ] `state` parameter present and validated (CSRF on the OAuth flow)?
- [ ] Account linking abuse — pre-account takeover via unverified email.

## Proving impact
Demonstrate takeover on **your own** second test account (B), not a real user.
Never access a real person's account.

## Report hook
"An attacker can take over account B without B's credentials by <mechanism>,"
with the exact request chain and the pre/post state.
