# Git merge Conflict Resolution

## Scenario
- Branches involved: `feature-a`, `feature-b`
- File with conflict: `app.txt`

## Conflict Example
During merging, the following conflict appeared in `app.txt`:

### Step 1. Create example repo and initial commit
**Commands:**
```bash
mkdir git-conflict-demo
cd git-conflict-demo
git init
git config user.name "Your Name"
git config user.email "you@example.com"
echo "This repo demonstrates merge conflicts" > README.md
git add README.md
git commit -m "chore: initial commit - add README"
```

Expected output: Initialized empty Git repository ... and a commit summary.

One-line test: git log --oneline should show the initial commit

---

### Step 2. Create a file and commit on main
```bash
echo "Line 1: original" > app.txt
git add app.txt
git commit -m "feat: add app.txt with original line"
```

Test: cat app.txt → shows Line 1: original.

---

### Step 3. Make two branches that change the same line (so we can create a conflict)

Create and commit on feature-a:
```bash
git checkout -b feature-a
sed -i 's/original/from feature-a/' app.txt    # Linux/macOS; Windows: edit manually
git add app.txt
git commit -m "feat(feature-a): change line to feature-a"
```
Create and commit on feature-b:
```bash
git checkout main
git checkout -b feature-b
sed -i 's/original/from feature-b/' app.txt
git add app.txt
git commit -m "feat(feature-b): change line to feature-b"
```
Test: git log --oneline --graph --all shows three branches/commits.

---

### Step 4. Merge feature-a into main, then merge feature-b into main — you will get conflict
```bash
git checkout main
git merge feature-a      # should merge cleanly
git merge feature-b      # this should produce a conflict
```
If conflict happens you’ll see something like:
```bash
Auto-merging app.txt
CONFLICT (content): Merge conflict in app.txt
Automatic merge failed; fix conflicts and then commit the result.
```
One-line test: git status will show both modified: app.txt.

---

### Step 5. Resolve conflict locally
Open app.txt — you will see markers:
```pqsql
<<<<<<< HEAD
Line 1: from feature-a
=======
Line 1: from feature-b
>>>>>>> feature-b
```
Choose which content you want (or combine), then:
```bash
# after editing to final content, for example:
# Line 1: combined from feature-a and feature-b

git add app.txt
git commit -m "fix: resolve merge conflict between feature-a and feature-b"
```
Test: git show --name-only shows the merge commit; git log --graph --oneline --all shows merged history.

---

### Step 6. Push to GitHub and create a PR to demonstrate web conflict resolution
1. Create a repo on GitHub (name it git-conflict-demo).
2. Add remote and push branches:
```bash
git remote add origin git@github.com:YOURUSERNAME/git-conflict-demo.git
git push -u origin main
git push -u origin feature-a
git push -u origin feature-b
```
3. Open a Pull Request for one branch, merge it on web, then open PR for the other — GitHub will show conflict and web editor can also resolve it (or you resolve locally and push).

Test: git remote -v and GitHub Actions/PR shows commit history.
