## Undoing Commits
## One-word map
- Fix → `--amend`
- Backtrack → `revert`
- Rewrite → `reset`
- Surgical → `rebase -i`
- Rescue → `redflog`<br>

# 1. Basics
- Repo history is a **chain of snapshots(commits)**. `HEAD` points to the tip(most recent commit) of the current branch
<br>
**Three key places:**
  - **Working directory** - files you edit
  - **Index/Stagin area** - what will go into the next commit.
  - **Repository(commits)** - the chain of snapshots

**Undoing commits means one of two high-level operations:**
- Create new commits that negate earlier commits(safe, history preserving). → `git revert`

- Rewrite history(change/erase commits themselves). → `git reset`, `git commit --amend`, `git rebase -i`(dangerous if pushed)

Always ask: Has this been pushed/shared?
- If yes, prefer non-destructive methods(`revert`)
- If no (local only), rewriting history is fine. 

## 2) Fix the most recent commit
### a) You only need to change message or add a forgot file
**Commands**
  ```bash
  # add forgotten files(s)
  git add file.txt

  # replace last commit with a new one that includes stages changes
  git commit --amend

  # or amend message only
  git commit --amend -m "Better message"
  ```
  **Effect:** replaces the latest commit with a new one. If pushed already, this rewrites history - avoid unless you force-push and coordinate

  # 3) Remove commits from the branch tip - rewrite history (local use)
  `git reset` (three flavours)

  `reset` moves the branch pointer (and can change index and working tree).

```bash
# 1) soft: move HEAD, keep staged & working changes
git reset --soft <commit>

# 2) mixed (default): move HEAD, keep working changes but unstage them
git reset <commit> # same as --mixed

# 3) hard: move HEAD, discard index and working tree changes to match <commit>
git reset --hard <commit>
```

# `git reset --soft <commit>`
It only moves HEAD. 
Nothing else.
- Working directory? Untouched
- Staging area(index)? untouched
- Files? untouched
- Changes? untouched
# Scenario setup
- Let say we following git commit history and some uncommited changes
 1. commit D ← HEAD
 2. commit C
 3. commit B
 4. commit A

 now, `git reset soft HEAD~1` will move HEAD to 'commit C'.
 commit D will be removed from commit history but its content is still staged(index). meaning 'commit D' will be in staging area and Git shows them as "changes staged for commit"

 # Another scenario
  1. commit 3 ← HEAD
  2. commit 2 
  3. commit 1

  `git reset --soft HEAD~1`<br>
  - moves HEAD to 'commit 2'
  - brings 'commit 3' in staging area

  so `git log` gives
  ```bash
  commit a22467d44c3ff9baafe4d7ced9ed2048ecc7e723
Author: Vishal <vishalkiranpatel@gmail.com>
Date:   Sun Dec 7 19:38:00 2025 +0530

    commit 2

commit ac061f4bfc4a94e096ae461e9fc6f0fcf3121932
Author: Vishal <vishalkiranpatel@gmail.com>
Date:   Sun Dec 7 19:37:37 2025 +0530

  commit 1
  ```

# Why it's useful?
- Fix wrong commit messages
- Split a big commit into smaller ones
- Combine multiple commits
- Rewrite history safely
- Clean up before pushing


# Ways to perform soft reset
  ## 1. The most common soft reset
    `git reset --soft HEAD~1`

**Use when:** Latest commit ko undo karke again edit+commit karna ho.
  ## 2. Soft reset to a specific commit
    `git reset --soft <commit_hash>`
    eg: `git reset --soft a22467d`
  
  **Use when:** Commit chain me peeche kahin jump marna ho(like going back 3-4 commits).

  ## 3. Soft reset via HEAD movement
  ```bash
  git reset --soft HEAD~2
  git reset --soft HEAD~3
  git reset --soft HEAD~5
  ```
  **Use When:** Multiple commits ko squash/update karna ho.


# Summary Chart 
| Command                            | What it does                                       |
| ---------------------------------- | -------------------------------------------------- |
| `git reset --soft HEAD~1`          | Undo latest commit, keep changes staged            |
| `git reset --soft HEAD~N`          | Undo last N commits                                |
| `git reset --soft <hash>`          | Jump directly to a commit, keep all content staged |
| `git reset --soft branch`          | Make branch HEAD match another branch              |
| `git reset --soft HEAD^`           | Undo merge commit                                  |
| `git reset --soft $(first commit)` | Reset history to start without losing files        |



<br>

# git reset --mixed <commit>
  - This is default reset mode.
  - moves the HEAD to the target commit
  - It resets the index(staging area) to match that commit
  - But it leaves the working directory untouched

**This is default reset mode.**<br>
`git reset HEAD~1`<br> is same <br>`git reset --mixed HEAD~1`

## What does `reset --mixed` do?
- Let's say we have 4 commits:
  - commit 4 ← HEAD<br>
  - commit 3<br>
  - commit 2<br>
  - commit 1<br>

We run `git reset --mixed HEAD~2`
### 1. moves HEAD from `commit 4` to `commit 2`
- Our current commit is now commit 2
### 2. removes sanpshots of `commit 4` and `commit 3` from staging area
### 3. brings commit information of `commit 2`

### 4. Working directory keeps all your latest changes

→Noting is deleted from the disk(source file)
→Our code looks exactly like C4 on disk


## Unstages the files meaning:
once we run  `git reset --mixed HEAD~1`
latest commit is removed from the staging area. Git unstages the latest commit meaning if we have to commit it again we need to `git add <file>`. 
😵‍💫😵‍💫😵‍💫😵‍💫

**Staging area cleaned = all staged changes are removed from staging, but your actual files remain untouched.**<Br>
→Target commit is staged

```ini
--soft  = Undo commits, keep changes staged.
--mixed = Undo commits, unstage changes.
```

# `git reset --hard <target commit>`
- 1.Moves the HEAD to target 
- 2.Deletes the commit history ahead of that point<br>Commits after that point are basically destroyed<br>
- 3.Staging area is replaced with target commit's snapshot
- 4.WORKING DIRECTORY is replaced with target commit's snapshot


# `git revert`
