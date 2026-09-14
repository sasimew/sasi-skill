# Sasi Skill

Private internal skill library for the SASI.ASIA workflow.

## Purpose

This repository stores repeatable operating guides for:

- UTM campaign link creation
- GA4 attribution and reporting
- deployment decisions
- website QA and verification
- future reusable skills created for this project

This is a private repository. Do not store passwords, API keys, GitHub tokens,
private server credentials, customer data, or other secrets in Markdown or Git.

## Current Skills

- UTM_GA4_CREATION_GUIDE.md
  - Creates UTM links
  - Explains when deployment is required
  - Defines GA4 reporting and QA steps
  - Documents the Jenosize referral example

## Rule For Every New Skill

Every new skill must:

1. Be written as a Markdown file in this repository.
2. Include purpose, inputs, procedure, verification, and known risks.
3. Avoid secrets and personal data.
4. Be reviewed with a Markdown diff and secret scan.
5. Be committed with a clear message.
6. Be pushed to the main branch of this repository.

## Standard Push Flow

Run from the local clone:

~~~
cd /path/to/sasi-skill
gh auth status
git status -sb
git diff --check
rg -n "ghp_|github_pat_|BEGIN .*PRIVATE|password|api_key|token\\s*=" \
  --glob '!.git/**' .
git add README.md *.md
git diff --cached --name-status
git diff --cached --check
git commit -m "skill: add or update skill name"
git pull --rebase origin main
git push origin main
~~~

Use GitHub CLI authentication backed by the operating system keychain. Never
embed a token in the remote URL, command history, Markdown, or source code.

## Repository Metadata

- GitHub owner: sasimew
- Repository: sasi-skill
- Default branch: main
- Visibility: Private
- Website GA4 measurement ID referenced by the skills: G-S0N3M3191G

