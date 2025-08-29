## Merge conflict example
- Branches: feature-a, feature-b
- File: app.txt
- Conflict markers seen: 
  <<<<<<< HEAD
  Line 1: from feature-a
  =======
  Line 1: from feature-b
  >>>>>>> feature-b
- Final resolution: kept 'combined' content
- Commands used:
  git add app.txt
  git commit -m "fix: resolve merge conflict between feature-a and feature-b"
