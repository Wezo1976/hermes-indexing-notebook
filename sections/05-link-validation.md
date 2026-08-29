# Finding Broken Links

## Overview

The README.md contains many external links. This guide shows how to find and report broken links.

## Validation Script

```bash
git clone https://github.com/trimstray/the-book-of-secret-knowledge && cd the-book-of-secret-knowledge

for i in $(sed -n 's/.*href="\([^"]*\).*/\1/p' README.md | grep -v "^#") ; do

  _rcode=$(curl -s -o /dev/null -w "%{http_code}" "$i")

  if [[ "$_rcode" != "2"* ]] ; then echo " -> $i - $_rcode" ; fi

done
```

## How It Works

1. **Clone** the repository
2. **Extract** all href links from README.md
3. **Filter** out internal anchors (starting with #)
4. **Check** HTTP status code for each link
5. **Report** any non-2xx responses

## Example Output

```bash
 -> https://ghostproject.fr/ - 503
 -> http://www.mmnt.net/ - 302
 -> https://search.weleakinfo.com/ - 503
 [...]  
```

## HTTP Status Codes

| Code | Meaning |
|------|----------|
| 2xx  | Success |
| 3xx  | Redirect |
| 4xx  | Client Error (not found) |
| 5xx  | Server Error |

## What to Do with Results

- **2xx codes** → Valid, no action needed
- **3xx codes** → Redirect, may need updating to final URL
- **4xx/5xx codes** → Broken, should be reported or fixed

---

**Tags:** #link-validation #bash #curl #testing #automation #maintenance

**Related:** [Issue Tracker](02-issue-tracker.md) | [Pull Requests](04-pull-requests.md)