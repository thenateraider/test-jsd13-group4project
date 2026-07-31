# Exercise 04 — Conflict #3: delete / modify

> One teammate **deletes** a file while another **modifies** the same file on
> their branch. Git can't reconcile "it should be gone" with "here are my edits
> to it," so it stops and makes you decide: **keep the file (with edits) or
> delete it?** This one looks scarier than it is.

## The setup

Your repo has an old helper `team/legacy.js` that the team wants to retire:

```js
// team/legacy.js — old formatting helper, scheduled for removal.
function formatOldDate(str) {
  return str.replace(/-/g, "/");
}

export { formatOldDate };
```

**Two teammates create this conflict together.** Both branch from the same
up-to-date `main`, then work in parallel without realizing they're touching the
same file:

- **Person A** decides it's dead code and **deletes the file** (`git rm
  team/legacy.js`).
- **Person B** didn't know that, and **modifies** `team/legacy.js` to fix a small
  bug (any edit — e.g. add a guard for empty input).

Person A merges first; Person B merges second and hits the conflict.

## Step 1 — Both: branch from the same starting point

```bash
git checkout main
git pull origin main
```

- **Person A:** `git checkout -b remove-legacy`, then `git rm team/legacy.js`, commit.
- **Person B:** `git checkout -b fix-legacy-date`, edit `team/legacy.js`, commit.

Both push and open a PR.

## Step 2 — Person A merges first

Person A's "remove legacy helper" PR is approved and merged into `main`. The file
is now gone on `main`.

## Step 3 — Person B hits and resolves the conflict

You are Person B, on branch `fix-legacy-date`. Bring in `main` (where the file is
deleted) and resolve the delete/modify conflict.

1. Bring the latest `main` into your branch:
   ```bash
   git checkout main
   git pull origin main
   git checkout fix-legacy-date
   git merge main
   ```

2. Git reports a **delete/modify** conflict, e.g.:
   ```
   CONFLICT (modify/delete): team/legacy.js deleted in main and modified in HEAD.
   ```
   Unlike the other conflicts, there are **no `<<<<<<<` markers inside the file**
   — the conflict is about the file's *existence*. Git leaves your modified copy
   in the working tree and waits for your decision.

3. **Decide with your team.** There are two valid resolutions:

   **Option A — agree the file should go (recommended here).** The team confirms
   the helper is truly dead. Accept the deletion:
   ```bash
   git rm team/legacy.js
   git commit
   git push
   ```

   **Option B — the file is still needed.** Your bug fix matters and the delete
   was premature. Keep your modified version:
   ```bash
   git add team/legacy.js
   git commit
   git push
   ```

   For this exercise, the team confirms the helper is unused → use **Option A**.

## Check yourself

- [ ] You made a deliberate keep-or-delete decision *with the other author*, not
      a guess.
- [ ] After Option A, `team/legacy.js` no longer exists and nothing imports it.
- [ ] `git status` is clean and the merge is committed.
- [ ] The rest of the test suite still passes (`node team/roster.test.js` etc.).

> **Key idea:** a delete/modify conflict is a conversation, not a text edit. The
> two branches disagree about whether the file should exist — a human resolves
> that, then tells Git the answer with either `git rm` or `git add`.
