## ```git init```
**To initialize a git repository in a local folder**

## ```git status```
**To check if git is aware about the current folder**

## ```git add```
**To start tracking files**<br>
*Syntax:* ```git add fileName```<br>
or ```git add .``` to start tracking all files<br>
```git add``` brought files in staging area, where changes to commit are prepared


# Removing a directory
**Command:** ```rm -rf folderName/```<br>
eg: ```rm -rf .git/```

# Changing branch name
**Command:** ```git init --initial-branch=main```
<br>

```git status -s``` to check brief status
```git status -v``` for detailed status<Br>
```-v``` stands for ```verbose```

<br>
```git status -s``` give 'A' followed by file name if file is added or in staging area

# Unstaging a file
**Command:**   (use ```git rm --cached <file>...``` to unstage)


# Ammending commit message
**command:** ```git commit --amend```

# empty commit
**command:** ```git commit --allow-empty -m "message"```

# signing a commit
**command:** ```git commit -s -m "signing a commit```


# checking commit history
**command:** ```git log```

# bounding commit history
**command:** ```git log -n 3``` shows last three commits

# pretty log history
**command:** ```git log --pretty=short``` for shorter logs and ```git log --pretty=full``` for full history with commit message and ```git log --pretty=fuller``` for dates

## ```git log -p``` for detailed history
it shows the files commited too

## ```git log --pretty=oneline``` to see id and commit message

# formating log history
**command:** ```git log --pretty=format:"%h"``` for hashing info

# Beautiful graph view
**comman:** ```git log --oneline --graph --decorate --all```
| Placeholder | Meaning                    |
| ----------- | -------------------------- |
| `%h`        | Short commit hash          |
| `%H`        | Long commit hash           |
| `%an`       | Author name                |
| `%ae`       | Author email               |
| `%ar`       | Time ago (“3 days ago”)    |
| `%ad`       | Date                       |
| `%s`        | Commit message subject     |
| `%d`        | Decorations (branch, tags) |
| `%p`        | Parent commits             |


# git log of a week
**comman:** ```git log --since="1 week ago"

# log of a period
**command:** ```git log --since="2025-12-06" --until="2025-12-07```

# commits by an author
**command:** ```git log --author="Vishal"```

# get commits by keywords
**command:** ```git log --grep="Keyword"```

| Goal             | Command                                             |
| ---------------- | --------------------------------------------------- |
| Last 7 days      | `git log --since="1 week ago"`                      |
| Last 24 hours    | `git log --since="1 day ago"`                       |
| Before a date    | `git log --until="2024-12-01"`                      |
| Between dates    | `git log --since="2024-12-01" --until="2024-12-10"` |
| By range + graph | `git log --oneline --graph --since="2 weeks ago"`   |
| Log unitl a certain time | `git log --until="2025-12-06`

# Combine with pretty formats
**command:** `git log --since="1 week ago" --pretty=oneline`

### Git parses natural language 
"```yesterday"```

```"last Tuesday"```

```"2 months ago"```

```"2024-01-01"```

```"2024-01-01 14:30"```

```"midnight"```

```"noon"```