# Exercise 03 — Conflict #2: add / add (both add at the same spot)

> Two teammates each **add new content in the same place** — for example both
> append a new function to the end of the same file. Neither deleted anything,
> but Git still can't tell how to stack the two additions, so it conflicts.
> Unlike the same-line edit, the resolution here is usually **keep BOTH**.

## The setup — pick two teammates

Your repo has a shared helpers file `team/utils.js`:

```js
// team/utils.js — small shared helper functions. Add new helpers below.
function capitalize(word) {
  return word[0].toUpperCase() + word.slice(1);
}

export { capitalize };
```

**Two teammates create this conflict together.** Each adds a helper, both
inserting **just before the `export` line**, and both branching from the same
up-to-date `main`:

- **Person A** adds `shout(word)` → returns the word in upper case.
- **Person B** adds `whisper(word)` → returns the word in lower case.

Each must also add their function to the `export { ... }` line. Person A merges
first; Person B merges second and resolves.

## Step 1 — Both: branch from the same starting point

```bash
git checkout main
git pull origin main
```

- **Person A:** `git checkout -b add-shout-helper`, add `shout` + export it.
- **Person B:** `git checkout -b add-whisper-helper`, add `whisper` + export it.

Both run `node team/utils.test.js`, commit, push, and open a PR.

## Step 2 — Person A merges first

Person A's PR is approved and merged into `main` cleanly.

## Step 3 — Person B hits and resolves the conflict

You are Person B, on branch `add-whisper-helper`. Bring in `main` and resolve so
that **both helpers survive**.

1. Bring the latest `main` into your branch:
   ```bash
   git checkout main
   git pull origin main
   git checkout add-whisper-helper
   git merge main
   ```

2. Open `team/utils.js`. You'll see two conflicting additions, e.g.:
   ```
   <<<<<<< HEAD
   function whisper(word) {
     return word.toLowerCase();
   }
   =======
   function shout(word) {
     return word.toUpperCase();
   }
   >>>>>>> main
   ```
   and a second conflict on the `export { ... }` line.

3. **Keep both additions.** Remove the marker lines and arrange the file so it
   contains `capitalize`, `shout`, AND `whisper`, and exports all three. Put the
   functions in a sensible order (alphabetical is a fine convention).

4. Run the tests, then finish the merge:
   ```bash
   node team/utils.test.js
   git add team/utils.js
   git commit
   git push
   ```

## Check yourself

- [ ] All three functions exist: `capitalize`, `shout`, `whisper`.
- [ ] The `export { ... }` line lists all three.
- [ ] No conflict markers remain.
- [ ] `node team/utils.test.js` passes before you push.

> **Key idea:** "add/add" conflicts are usually *not* a disagreement — both
> pieces of work are wanted. Your job is to merge them cleanly, not to pick a
> winner.
