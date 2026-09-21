# Finding Broken Links

This section shows the shell workflow suggested by the project for spotting broken external links.

## Example command sequence

```bash
git clone https://github.com/trimstray/the-book-of-secret-knowledge && cd the-book-of-secret-knowledge

for i in $(sed -n 's/.*href="\([^"]*\).*/\1/p' README.md | grep -v "^#") ; do

  _rcode=$(curl -s -o /dev/null -w "%{http_code}" "$i")

  if [[ "$_rcode" != "2"* ]] ; then echo " -> $i - $_rcode" ; fi

done
```

## What the script does

1. Clones the repository locally.
2. Extracts `href` values from `README.md`.
3. Skips internal anchor links that begin with `#`.
4. Uses `curl` to request each URL and capture the HTTP status code.
5. Prints any link whose response is not in the `2xx` success range.

## Example output

```bash
 -> https://ghostproject.fr/ - 503
 -> http://www.mmnt.net/ - 302
 -> https://search.weleakinfo.com/ - 503
 [...]
```

## Interpreting the results

- `2xx` responses are treated as healthy links.
- Any non-`2xx` response is reported by this workflow.
- `3xx` responses are still flagged, which makes them a useful follow-up category for redirect cleanup.
- `4xx` and `5xx` responses are strong candidates for repair or replacement.

## When to use it

Run this workflow when checking documentation quality or before reporting broken links through the normal contribution channels.

---

_Source adapted from [`CONTRIBUTING.md`](https://github.com/trimstray/the-book-of-secret-knowledge/blob/master/.github/CONTRIBUTING.md)._
