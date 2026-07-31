# Exercise 02 — Conflict #1: the same-line edit

> **The most common conflict.** Two teammates change **the same line** of the
> same file on different branches. Git cannot know which version you want, so it
> stops and asks you. This is normal and expected — you just resolve it.

## The setup — pick two teammates

Your repo has a shared file `team/config.js`:

```js
// team/config.js — shared app settings.
const config = {
  appName: "Team App",
  maxUsers: 10,
  theme: "light",
};

export default config;
```

**Two teammates create this conflict together.** Both change **the same line**
(`maxUsers`) to a different value, on their own branches:

- **Person A** raises the limit to `maxUsers: 25,`
- **Person B** raises the limit to `maxUsers: 50,`

The key move that guarantees the conflict: **both of you branch from the same,
up-to-date `main`.** Then Person A merges first (clean), and Person B merges
second (conflict).

## Step 1 — Both: branch from the same starting point

Each of you, on your own machine:

```bash
git checkout main
git pull origin main
```

- **Person A:** `git checkout -b raise-max-users-25`, edit `maxUsers: 25,`
- **Person B:** `git checkout -b raise-max-users-50`, edit `maxUsers: 50,`

Both run `node team/config.test.js` (it passes for any positive number), commit,
push, and open a PR.

## Step 2 — Person A merges first

Person A's PR gets one approval and merges into `main` normally — no conflict yet,
because A branched from clean `main`.

## Step 3 — Person B hits and resolves the conflict

Now Person B brings the updated `main` into their branch:

```bash
git checkout main
git pull origin main
git checkout raise-max-users-50
git merge main
```

1. Git stops with a **CONFLICT** message. Open `team/config.js`. You will see
   conflict markers around `maxUsers`:
   ```
   <<<<<<< HEAD
     maxUsers: 50,
   =======
     maxUsers: 25,
   >>>>>>> main
   ```
   - Everything between `<<<<<<< HEAD` and `=======` is **your** version.
   - Everything between `=======` and `>>>>>>> main` is the **incoming** version.

2. **Decide the real answer together** (Person A and Person B talk it through).
   Suppose the team agrees the cap should be **50**. Edit the file so it contains
   exactly one correct line and **delete all three marker lines**:
   ```js
     maxUsers: 50,
   ```

3. **Run the tests**, then finish the merge:
   ```bash
   node team/config.test.js
   git add team/config.js
   git commit          # completes the merge (keep or edit the default message)
   git push
   ```

## Check yourself

- [ ] No `<<<<<<<`, `=======`, or `>>>>>>>` markers remain anywhere in the file.
- [ ] The file has exactly one `maxUsers:` line with the agreed value.
- [ ] `node team/config.test.js` passes before Person B pushes.
- [ ] Person A and Person B agreed on the value instead of one silently overwriting the other.

> **Key idea:** resolving a conflict is a human decision about *intent*, not a
> mechanical one. Git just shows you the two options and gets out of the way.
