---
tags:
  - type/vibe-specs
summary: Track changes in important branches
---

- This command which get you branches which are very active
```bash
git for-each-ref --format='%(refname:short)' refs/remotes/origin | grep -vE '^origin/HEAD$' | while read -r b; do   c=$(git rev-list --count --since='3 years ago' "$b" 2>/dev/null || echo 0);   [ "$c" -gt 0 ] && printf "%7d  %s\n" "$c" "$b"; done | sort -nr | head -n 50
```