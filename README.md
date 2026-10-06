# BugBounty Workspace

A personal workspace for hunting bugs on authorized bug bounty programs
(primarily [bugbounty.sa](https://bugbounty.catchify.sa/)). This repo holds
**methodology, checklists, notes, and report templates** — the things that
turn scattered testing into consistent, payable findings.

> This is a workflow repo, not an exploit dump. Nothing here is a substitute
> for reading each program's scope and rules before you touch a target.

## The one rule that matters most

**Only test what a program explicitly puts in scope, only with the techniques
it allows.** Testing out-of-scope assets — even on a bounty platform — is not a
bounty; it can be a crime. When in doubt, it's out. See `RULES.md`.

## How to use this repo

1. **New target?** Copy `targets/_TEMPLATE/` to `targets/<program-name>/` and
   fill in `scope.md` from the program's scope page first.
2. **Recon:** Work through `recon-checklist.md`, saving notes in the target folder.
3. **Testing:** Pick a vuln class from `methodology/` and work it systematically.
   Don't spray — go deep on one class at a time.
4. **Found something?** Copy `report-template.md`, write it up, verify the
   repro steps from a clean session, then submit on the platform.
5. **Learning:** `resources.md` is the curated study path. PortSwigger's Web
   Security Academy is the backbone.

## Layout

```
RULES.md               Scope discipline, legal/ethical guardrails
recon-checklist.md     Repeatable per-target recon flow
report-template.md     Report structure triagers like
scope-tracker.md       Quick index of every program you're engaged with
resources.md           Curated learning path and tools
methodology/           Per-vuln-class testing checklists
targets/               One folder per program (git-ignored contents by default)
```

## A note on expectations

The first valid bounty is the hard one; the tenth is much easier. Early on,
optimize for **learning a few vuln classes deeply** and **report quality**,
not for volume. Income compounds once your methodology is repeatable.
