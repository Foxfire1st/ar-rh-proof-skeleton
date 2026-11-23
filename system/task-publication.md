# Task Publication — GitHub Issues & Paired Pull Requests

Agents Remember stays the authority for task identity, requirements, execution, and acceptance. This repository publishes a public view of that work on GitHub (`Foxfire1st/rh-proof-skeleton` and `Foxfire1st/ar-rh-proof-skeleton`); nothing on GitHub writes back into AR state.

## What a Public Reader Sees

Someone opening `Foxfire1st/rh-proof-skeleton`, `Foxfire1st/ar-rh-proof-skeleton`, and the GitHub issues has no access to local coordination files. Write every issue and PR body in plain prose and leave out:

- local filesystem paths, notes/scratch/report names;
- agent IDs, internal decision numbers, review rounds, verdicts, and rulings;
- raw checksums or commit hashes as prose text (link to commits/PRs instead).

Requirement IDs (e.g. `RH-R01`), dates, commands, theorem names, and product names stay. Check every issue body for local leaks before posting:

```text
grep -nE '/home/|/mnt/|\bD[0-9]{3,}\b|\b[0-9a-f]{7,}\b' <body file>
```

## Hidden Marker (Top of Issue Body)

```text
<!-- ar-task: <task-id> -->
<!-- ar-kind: leaf|master|sprint -->
<!-- ar-parent: <parent-id> -->
<!-- ar-requirement: <req-id>@v<n> source-sha256:<64 hex> -->
```

## Labels & Public Project Board

Create once on `Foxfire1st/rh-proof-skeleton`:

```text
gh label create "kind:sprint" --color 6F42C1 --description "AR sprint" --force --repo Foxfire1st/rh-proof-skeleton
gh label create "kind:master" --color 0E8A16 --description "AR master" --force --repo Foxfire1st/rh-proof-skeleton
gh label create "kind:leaf"   --color 1D76DB --description "AR leaf"   --force --repo Foxfire1st/rh-proof-skeleton
```

Maintain one public GitHub Project board (**"RH Proof Explainer tasks"**) with a `Status` field (`Todo`, `In progress`, `Done`) grouped by `Status`.

## Lifecycle Mirroring

1. **Registration (Architect):** Search for existing `<!-- ar-task: ... -->` marker first; create issue with `kind:<kind>` label if absent, read back to verify, link sub-issue to parent, add to board as `Todo`, record issue URL in task document `references`.
2. **Start (Manager):** Assign `Foxfire1st` and set board `Status` to `In progress`.
3. **Paired PR Landing (Manager — Code AND Memory):**
   `system/settings.json` sets `landing.workflow = "pull-request"`. Let `B` be the side's target branch tip before integration, `H` the accepted integrated landing commit, and `M` the publication merge commit (parents `B`, `H`; tree `H`).
   For each changed side — **code (`Foxfire1st/rh-proof-skeleton`) then memory (`Foxfire1st/ar-rh-proof-skeleton`)** — where `H != B`:
   1. **Pin:** Verify `gh api repos/Foxfire1st/<repo>/git/ref/heads/<target>` equals `B` and `H` descends from `B`.
   2. **Push once:** `git push origin H:refs/heads/land/<slug>`.
   3. **Open PR:** `gh pr create --repo Foxfire1st/<repo> --base <target> --head land/<slug> --title "<plain title>" --body-file <body file>` (linking the task issue).
   4. **Merge (preserve both parents):** Verify `gh pr view <n> --json headRefOid,baseRefOid` matches `H` and `B`, then `gh pr merge <n> --merge --admin`. Never squash or rebase.
   5. **Read back & pull:** Verify `mergeCommit` `M` has parents `B`, `H` and tree `H`; run `git pull --ff-only` on the target branch.
   6. **Delete** `land/<slug>`.
   Then post a landing comment on the issue, update its status line to `landed on <date>` with links to both merged PRs (or naming an unchanged side), close `completed`, and set board `Status` to `Done`.
4. **Abandonment (Manager):** Update status line to `abandoned on <date>` with one sentence, close `not planned`, set board `Status` to `Done`.

## Branch Policy

Once the initial `main` bootstrap/baseline commits exist on GitHub, `main` is pull-request gated on **both** `Foxfire1st/rh-proof-skeleton` and `Foxfire1st/ar-rh-proof-skeleton`: never push directly to `main`.
