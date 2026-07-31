# Exercise 00 — Create your team's repository

> **Do this once, together, as a team, before anything else in Set 2.** You will
> create one shared GitHub repository that your whole team contributes to for the
> rest of the day. Everything in exercises 01–04 happens inside this repo.

## Who does what

Pick **one** person to be the **repo owner** for setup (you'll rotate other roles
later). The owner creates the repo; everyone else joins as a collaborator.

## Part A — Owner: create and seed the repo

1. **Create an empty repo on GitHub.** Name it something like
   `team-<yourteam>-practice`. Do **not** add a README/.gitignore/license — you'll
   push your own starter files. Copy the repo's SSH or HTTPS URL.

2. **Grab the starter files.** This kit ships a ready-made starting point in the
   `starter/` folder (see `starter/README.md`). You'll copy its `package.json`
   **and** its `team/` folder into your new repo. (The `package.json` sets
   `"type": "module"` so Node runs the `import`/`export` code.)

3. **Create the repo locally, add the starter files, and push:**
   ```bash
   mkdir team-<yourteam>-practice
   cd team-<yourteam>-practice
   git init
   git branch -M main

   # copy the starter package.json and team/ folder into this new repo
   cp /path/to/this-kit/starter/package.json ./package.json
   cp -r /path/to/this-kit/starter/team ./team

   git add .
   git commit -m "Seed team practice repo"
   git remote add origin <your-repo-url>
   git push -u origin main
   ```

4. **Confirm the tests run** on the fresh repo:
   ```bash
   node team/roster.test.js   # -> All tests passed ✅
   node team/config.test.js   # -> All tests passed ✅
   node team/utils.test.js    # -> All tests passed ✅
   ```

5. **Add your teammates as collaborators.** GitHub → your repo → **Settings →
   Collaborators** → add each teammate. They accept the invite by email.

6. **Protect `main`.** GitHub → **Settings → Branches → Add branch ruleset** (or
   "Add rule"): target `main`, and require a **pull request with at least 1
   approval** before merging. This makes the GitHub Flow discipline real: nobody
   can push straight to `main`.

## Part B — Everyone else: clone the repo

Once you've accepted the collaborator invite:

```bash
git clone <your-repo-url>
cd team-<yourteam>-practice
node team/roster.test.js   # -> All tests passed ✅
```

## Check yourself (as a team)

- [ ] The repo exists on GitHub and every teammate can see it.
- [ ] `package.json` and the `team/` folder from `starter/` are on `main`, and all three test files pass.
- [ ] `main` is protected: a direct push is rejected and a PR + 1 approval is required.
- [ ] Every teammate has cloned the repo locally.

> **No GitHub yet?** You can do all of Set 2 with local Git only — ask your
> instructor for the local-only setup (it's in the instructor guide).

➡️ Next: `01-github-flow.md`, where each of you makes your first real
contribution to this repo.
