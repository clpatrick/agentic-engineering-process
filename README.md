# Agentic Engineering Process

**The written operating model I use to ship production software with a team made of people and AI agents, and to keep both honest.**

Status: in daily use on a production platform since mid-2026. This repository documents the process; the skill files that implement it live in the private repositories they govern.

## The principle

People matter, so the process exists to make a technician's judgment the deciding input, not to remove it. Agents do the grinding: audits, drafts, tests, adversarial review. A person refines the issue, approves the plan, and merges. "Merging is the owner's decision" is written into the tooling.

## The pipeline

Every change moves through three named steps, each implemented as a reusable skill an agent can be handed.

1. **Refine the issue.** Audit first: read the code and the git history before proposing anything. Reconcile the request against what actually exists. Ask the owner the questions that matter, and treat a question from the owner as a question, not an instruction. Output is an agent-ready work packet: goal, context pointers, constraints, acceptance criteria, verification steps, suggested agent, risk and rollback.
2. **Work the issue.** In an isolated git worktree, never on the serving tree. A short brief file travels with the worktree so any agent, or a fresh session after context compaction, can resume. Tests are written alongside the change. Commits reference the issue.
3. **Ship.** Unit tests, build, and end-to-end tests on a spare port with isolated data, then a pull request. No CI runner in this environment, so the local gate is the only gate and it runs on every pull request.

## The roles

- **Worker** implements the packet.
- **Verifier** adversarially checks the worker's result with fresh context and no inherited motivated reasoning; findings go back to the worker.
- **Adversarial reviewer** is a forked agent that challenges the approach at two checkpoints: before the approach is chosen, and before the work is called done.
- **Parallel auditors** read the codebase from different angles before a design is written, so the design starts from what is true rather than what is remembered.
- **Owner** refines, approves, merges, and records rulings.

Stacked worktrees let several issues advance at once with an explicit contract that no issue regresses the stack below it. The record includes a five-issue train audited, designed, implemented, verified, and opened as pull requests overnight.

## The standing rules

- **Durable by default.** Assume the conversation will be compacted at any turn. State lives in files, not in chat. Sub-agents write reports to disk and return summaries.
- **The README is the source of truth** for people and agents alike, with an editing policy: bias toward deletion; if a paragraph would not still be correct in six months, it does not belong.
- **Drift tests.** When a security mechanism is documented, a test fails the build if the mechanism changes without the documentation.
- **Verify convenient claims.** A measurement that kills an option or confirms a prior gets a fresh-context audit before it is believed. Corrections are new entries, never edits.
- **Quiet reporting.** Work, then report at the end. Plain language; if a junior technician cannot follow it, it is not finished.
- **Shared resources are governed.** Concurrent sessions reserve GPU and RAM through a ledger before launching anything heavy (see the gpu-queue-ledger repository).

## What it produced

On one production platform between July and September 2026: roughly 120 issues shipped, the unit-test count raised from 2,473 to 3,277, and reliability defects root-caused and recorded next to their fixes. On research projects: numbered journals, pre-registered decisions, and headline results overturned by the process's own audits, which is the process working.
