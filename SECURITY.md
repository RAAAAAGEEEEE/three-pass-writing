# Security policy

## Scope

This repository contains markdown instructions for a Claude Code skill. There is no executable code, no dependency, and no service. The realistic risk surface is what the instructions cause an agent to read, write, or expose.

Relevant issues include:

- an instruction that could lead the agent to read a credential file or copy a secret into `.claude/writing-context.md`;
- an instruction that could cause data to leave the user's machine to any destination beyond their own Claude Code session;
- prompt-injection paths — repository content that could hijack the audit and redirect the agent's behaviour;
- guidance that would produce messages designed to disguise a sender or evade filtering.

## Reporting

Open a [security advisory](https://github.com/RAAAAAGEEEEE/three-pass-writing/security/advisories/new) on this repository. Please do not open a public issue for a security report.

Include: the file and line, what an agent following it would do, and the conditions required.

Expect an acknowledgement within a week. This is a solo-maintained project; there is no SLA beyond best effort.

## Never include in a report

A real `.claude/writing-context.md`, a prospect list, a customer email, an API key, or any personal data. Redact before sending.

## Supported versions

The latest release only.

## Enforcement boundary

The safety rules in this skill are instructions an agent follows, not a sandbox. The enforceable boundary is Claude Code's own permission system. If you handle regulated data, deny access to sensitive paths there — see [docs/PRIVACY_AND_SECURITY.md](docs/PRIVACY_AND_SECURITY.md).
