# Commit Signatures

This section captures the repository rule that every commit must include a sign-off line.

## Requirement

All commits must include a sign-off trailer with the contributor's name and email address. The upstream guide spells this as `signed-off-by`; this notebook intentionally normalizes the example below to the canonical `Signed-off-by: Name <email>` form so the generated trailer matches common Git/DCO tooling.

## Git hook setup

To add the sign-off automatically, place the following snippet in `.git/hooks/prepare-commit-msg` and make the hook executable with `chmod +x .git/hooks/prepare-commit-msg`. This is an intentional notebook adaptation of the source example so the hook emits a standard trailer format:

```bash
SOB=$(git var GIT_AUTHOR_IDENT | sed -n 's/^\(.*>\).*$/Signed-off-by: \1/p')
grep -qs "^$SOB" "$1" || echo "$SOB" >> "$1"
```

## How the hook works

The script:

1. reads the current Git author identity,
2. formats it as a `Signed-off-by:` trailer, and
3. appends it only if that line is not already present in the commit message.

## Result

With the hook enabled, contributors can keep commit messages compliant without adding the sign-off manually every time.

---

_Source adapted from [`CONTRIBUTING.md`](https://github.com/trimstray/the-book-of-secret-knowledge/blob/master/.github/CONTRIBUTING.md)._
