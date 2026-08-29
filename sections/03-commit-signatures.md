# Commit Signatures

## Requirement

Moving forward, all commits to this project must include a **"signed-off-by"** line indicating:
- The name of the contributor
- The email address of the contributor

## Setup Instructions

### Enable Git Commit Signatures

Add the following lines to `.git/hooks/prepare-commit-msg`:

```bash
SOB=$(git var GIT_AUTHOR_IDENT | sed -n 's/^\(.*>\).*$/- signed-off-by: \1/p')
grep -qs "^$SOB" "$1" || echo "$SOB" >> "$1"
```

### What This Does

- Automatically extracts your Git author identity
- Appends a "signed-off-by" line to every commit message
- Prevents duplicate signatures

## Example Commit Message

```
Fix broken link in README

- signed-off-by: John Doe <john@example.com>
```

---

**Tags:** #git #commits #signing #security #contributor-agreement

**Related:** [Pull Requests](04-pull-requests.md)