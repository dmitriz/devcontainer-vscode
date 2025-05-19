# Container Sandbox

This document defines the execution boundaries and file system constraints for AI agents running inside containerized environments. The goal is to prevent leakage, unauthorized access, or unintentional mutation of user data.

---

## Approved Scope

- All AI execution must occur inside a container
- Agent write access is limited to scoped subdirectories
- Secrets must be mounted from inside the container only (not host-bound)
- Host mounts are read-only unless explicitly declared

---

## Required Constraints

- Read-only mounts for project source
- Write access limited to `/workspace/` or `/output/`
- No host secrets via environment variables
- Internal container secret folder `/run/secrets/` (or similar)
- Optional user-level enforcement via post-create hooks

---

## Approved Features

- Dev Container config includes:
  - `features`: mount policies, security profiles
  - `postCreateCommand`: enforce policy setup
- All agent tools must respect the container I/O boundaries

---

## Proposed Ideas

- Use AppArmor or seccomp profiles to limit execution (optional)
- Auto-generate mount layout from `sandbox.config.json`
- Provide log of all agent-written files for review
- Snapshot/rollback state after each agent execution

---

## Open Questions

- Should sandbox policies vary by agent role?
- How do we prevent misuse of terminal commands inside containers?
- Do agents run as system users or with a sandboxed UID?

---

## Folder Policy Example
