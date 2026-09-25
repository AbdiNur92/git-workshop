# Git Workshop

## Commands I used

### git init
Creates a new empty Git repository in the current folder
The git folder is hidden and stores the history

### git status
It shows the state of the repo, which branch I'm on, and which files are changed, staged or untracked.

### git add
Adds changes from my working folder to the staging area, where I choose
what goes into the next commit. It takes a snapshot of the file as it is
right now, so if I edit the file again, I need to `git add` it again.

- `git add <file>` stages only that file, e.g. `git add index.html`;
  other changed files are left unstaged
- `git add .` stages every changed and new file in the folder

After `git add`, `git status` shows the staged files in green under
"Changes to be committed".

### git commit -m "message"
Saves everything in the staging area as a new snapshot (a commit) in my local history. Each commit gets a unique ID (hash), my name, the date, and the message I write after `-m` explaining what I changed. It's important to write good messages for reference then you know exactly what every snapshot changed and you or someone you are working with can understand it better.

- `git commit -am "message"` stages and commits in one go: `-a` adds the changes
  to every file Git already tracks, and `-m` adds the message. Brand-new
  (untracked) files are NOT included. They still need `git add` first.

### git log --oneline
Short form of the commit history one line per commit
first 7 characters of the hash/commitID and commit message, `HEAD -> main` marks the commit I'm on and the branch I'm on origin/main shows where GitHub copy is
Regular git log shows the full version with the long hash, author, date, message

- `git log --stat` shows each commit with the files it changed, and how many
  lines were added (`+`) and removed (`-`) in each file. It combines with
  other options, e.g. `git log --oneline --stat`

### git push -u origin main
Uploads my commits on the `main` branch to GitHub (`origin`).

- `origin` = where to send them (the GitHub repository)
- `main` = which branch to send
- `-u` = remember this pairing, so my `main` is linked to GitHub's `main`
  (called setting the "upstream")

I only need `-u` the first time. After that, a plain `git push` or
`git pull` knows where to go, and `git status` can say if I'm ahead of
or behind GitHub.

Only commits are pushed. Changes I haven't committed stay on my computer.

### git remote add origin <url>
Connects my local repository to a repository on GitHub.
`origin` is a nickname for the GitHub URL, so later commands can say
`origin` instead of typing the whole address.

It only saves the connection. Nothing is uploaded until I `git push`.
I only need to do it once per repository. (`git clone` sets up origin
automatically.)

- `git remote -v` shows which URL origin points to
- `git remote set-url origin <new-url>` changes it

### echo "text" > file
Not a Git command, but a terminal (Git Bash) command I use to create files.
`echo` prints the text, and `>` sends it into a file instead of the screen.

- `echo "<h1>My Git Workshop</h1>" > index.html` creates index.html with that line
- `>` creates the file, or **replaces** everything in it if it already exists
- `>>` **adds** the text as a new line at the end of the file instead

Keep the quotes around the text, or characters like `<`, `>` and `;` are read
as terminal commands instead of text.

### git diff
Shows the difference between my working folder (the files as they are now)
and the staging area (what I last `git add`-ed). If I haven't staged anything
since the last commit, that means everything I've changed since that commit.

- Lines starting with `+` (green) were added, lines with `-` (red) were removed
- Only unstaged changes show up. Once I `git add` a file, its changes disappear
  from `git diff`
- `git diff --staged` shows what IS staged instead: the difference between the
  staging area and the last commit (what the next commit will contain)
- `git diff HEAD~2` compares my files now with the version from 2 commits ago.
  `HEAD` is the commit I'm on, `HEAD~1` the one before it, `HEAD~2` the one
  before that. It shows the combined result of all the commits in between as
  one diff, not each commit separately (for that, use `git show` on each commit)

### git show
Shows the details of one commit: its hash, author, date and message, followed
by the changes it made (the same `+`/`-` view as `git diff`).

- `git show` or `git show HEAD` shows the newest commit
- `git show <hash>` shows any commit (copy the hash from `git log --oneline`)
- Press `q` to exit if the output fills the screen

### git check-ignore -v <file>
Checks whether a file or folder is ignored by Git, and if it is, shows exactly
which rule is blocking it: the ignore file, the line number and the pattern.

- `git check-ignore -v debug.log` prints `.gitignore:5:*.log  debug.log`,
  meaning line 5 of .gitignore (`*.log`) is what ignores debug.log
- `-v` (verbose) is what shows the rule. Without it, Git only prints the file
  name if it's ignored
- If the file is NOT ignored, it prints nothing

### .gitignore
A file in the project folder that lists files and folders Git should ignore.
Ignored files don't show up in `git status` and can't be added by accident
with `git add .`, which keeps secrets, logs and downloaded dependencies out
of the repository. The .gitignore file itself is committed like any other file.

- `.env` ignores one exact file
- `*.log` ignores every file ending in .log, in any folder (`*` = any name)
- `node_modules/` ignores a whole folder (the trailing `/` means folder)
- Lines starting with `#` are comments

### git rm --cached <file>
Stops Git from tracking a file, but keeps the file in my folder. "Cached" is
another name for the staging area (index): the file is removed from there, so
the next commit records it as deleted from the repository. It stays on my disk
and becomes untracked (so a .gitignore rule for it now works).

- `git rm <file>` (without `--cached`) removes the file from Git AND deletes
  it from my folder
- Older commits still contain the file. `--cached` only stops tracking it from
  now on, so it doesn't undo a secret that was already committed and pushed
