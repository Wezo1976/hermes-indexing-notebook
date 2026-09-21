# Commit Signatures

This section captures the repository rule that every commit must include a sign-off line.

## Requirement

The source guide says every commit must include a `signed-off-by` line with the contributor's name and email address. This acts as a contributor sign-off attached to each change.

## Git hook setup

To add the sign-off automatically, place the following snippet in `.git/hooks/prepare-commit-msg` and make the hook executable with `chmod +x .git/hooks/prepare-commit-msg`:

```bash
SOB=$(git var GIT_AUTHOR_IDENT | sed -n 's/^\(.*>\).*$/- signed-off-by: \1/p')
grep -qs "^$SOB" "$1" || echo "$SOB" >> "$1"
```

## How the hook works

The script:

1. reads the current Git author identity,
2. formats it as a `signed-off-by` line, and
3. appends it only if that line is not already present in the commit message.

## Result

With the hook enabled, contributors can keep commit messages compliant without adding the sign-off manually every time.

---

_Source adapted from [`CONTRIBUTING.md`](https://github.com/trimstray/the-book-of-secret-knowledge/blob/master/.github/CONTRIBUTING.md)._
