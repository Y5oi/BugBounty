# Business Logic Flaws

No scanner finds these — which is exactly why they pay and why they're rarely
duplicates. They come from understanding what the app is *supposed* to do, then
breaking the assumption.

## Mindset
Map the intended flow (the "happy path"), list every assumption the developer
made, then violate each one:
- order of steps
- quantity / price / sign (negative numbers!)
- who is allowed to do what, when
- limits and quotas

## Checklist

### Money & quantity
- [ ] Negative quantities or amounts (refund yourself, reduce total below zero).
- [ ] Integer overflow / huge values.
- [ ] Currency or decimal manipulation (rounding, `1.005`).
- [ ] Apply a discount/coupon multiple times; stack coupons; reuse single-use.
- [ ] Change price in the request even though the UI fixes it.

### Workflow / state
- [ ] Skip a step (go straight to the confirmation/fulfillment endpoint).
- [ ] Replay a step (submit payment confirmation twice).
- [ ] Do steps out of order.
- [ ] Race conditions: fire the same request in parallel (e.g. redeem a
      gift card / withdraw balance twice before it decrements). Only if allowed
      and done minimally.

### Limits & entitlements
- [ ] Exceed a "one per customer" / trial / quota limit.
- [ ] Access a paid feature on a free plan.
- [ ] Downgrade/upgrade timing abuse (keep premium after refund).

### Identity & relationships
- [ ] Act on behalf of another user/org by changing a tenant/org ID
      (overlaps with access control).
- [ ] Invite/role flows that grant more than intended.

## Proving impact
Show the before/after state and quantify it ("I obtained X for free / gained
SAR Y of value / bypassed the N-item limit"). Use your own test accounts and
reverse any state change you can.

## Report hook
"The application assumes <X>; by <action> an attacker breaks that assumption and
<concrete gain>." Business-impact framing matters most for this class.
