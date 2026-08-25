# oats-tools

Reviewed small tools for the Oats coding worker (see
`docs/09-oats-capability-plan.md` in HomeInfra, Phase 7 step 5).

## How a tool gets here

1. Oats drafts the tool in an isolated coding job and opens a pull request
   to this repository through the GitHub publication broker. It cannot
   merge, cannot touch `.github/`, and cannot reach this repository any
   other way.
2. A human reviews the pull request. **The merge is the promotion step** —
   nothing Oats writes takes effect until a person merges it.
3. The weekly coding-image rebuild (or a manual
   `systemctl start rebuild-oats-coding-image.service`) bakes the current
   `main` into the worker image at `/opt/oats-tools`, with `bin/` on the
   worker's `PATH`. The baked commit is recorded at
   `/etc/oats-coding/tools-version` inside the image.

## Contract for tools

- Executables live in `bin/`, are POSIX sh or Python 3, and carry a usage
  comment header. No compiled binaries — everything reviewable as text.
- Tools run *inside* a coding job container: non-root, no LAN, egress only
  through the allowlisted proxy, destroyed with the job. A tool grants no
  new authority; it only packages a repeatable procedure.
- Tools must not embed credentials, tokens, or hostnames of LAN services.
- Anything unclear in review gets rejected — the reviewer owes no benefit
  of the doubt to generated code.
