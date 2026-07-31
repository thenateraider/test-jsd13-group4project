# Exercise 01 — The GitHub Flow loop (team contribution)

> **Goal:** make one small change to your team's shared repository the "right"
> way — a short-lived branch, passing tests, a pull request, a review, and a
> merge to `main` **within the same day**. This is the whole loop the rest of
> the week builds on.

```
main
└── short-lived branch
    ├── small change
    ├── automated tests
    ├── pull request
    └── merge within one day
```

## Before you start

Your team has created its shared repo and everyone has cloned it (exercise
`00-setup-team-repo.md`). It contains a file `team/roster.js` that looks like this:

```js
// team/roster.js — one entry per teammate. Keep this list alphabetical by name.
const roster = [
  { name: "Ada", role: "author" },
];

export default roster;
```

There is also a test file `team/roster.test.js` you can run with **plain Node**:

```bash
node team/roster.test.js
```

## Your task

Add **yourself** to the roster, following the flow below. Do not commit directly
to `main` — the whole point is the branch → PR → merge loop.

1. **Sync `main`.** Get the latest starting point:
   ```bash
   git checkout main
   git pull origin main
   ```

2. **Create a short-lived branch.** Name it for the small change you're making:
   ```bash
   git checkout -b add-<yourname>-to-roster
   ```

3. **Make ONE small change.** Add a single object for yourself to the `roster`
   array in `team/roster.js`, keeping the list alphabetical by name.

4. **Run the automated tests** before you push. Green tests are your ticket to a PR:
   ```bash
   node team/roster.test.js
   ```
   You should see `All tests passed ✅`. If not, fix your change first.

5. **Commit** with a clear, present-tense message:
   ```bash
   git add team/roster.js
   git commit -m "Add <yourname> to team roster"
   ```

6. **Push** your branch and **open a pull request** on GitHub targeting `main`.
   Write one sentence describing the change.

7. **Get one review.** A teammate reviews and approves. If they request changes,
   push more commits to the same branch — the PR updates automatically.

8. **Merge within the day**, then delete the branch:
   ```bash
   # after the PR is merged on GitHub:
   git checkout main
   git pull origin main
   git branch -d add-<yourname>-to-roster
   ```

## Why this discipline (trunk-based mindset)

- **Short-lived branch:** the longer a branch lives, the further it drifts from
  `main` and the worse the eventual conflicts. Merge within a day.
- **Small change:** small pull requests are reviewed quickly and revert cleanly.
- **Automated tests before the PR:** never ask a teammate to review red code.
- **`main` is always releasable:** everyone branches from `main` and returns to
  it fast, so the trunk stays healthy and everyone stays close together.

## Check yourself

- [ ] You never committed directly to `main`.
- [ ] Your branch name described the change.
- [ ] `node team/roster.test.js` printed `All tests passed ✅` before you opened the PR.
- [ ] Exactly one teammate approved your PR.
- [ ] The branch was merged and deleted the same day.

➡️ Next: the three conflict exercises (`02`, `03`, `04`). Conflicts are normal —
learning to read and resolve them calmly is the skill.
