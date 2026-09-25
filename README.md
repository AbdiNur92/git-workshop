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
- `git log --oneline --graph` draws the history as a tree: `*` is a commit, and
  the lines show where branches split off and where they were merged back.
  `--all` also shows branches I'm not on

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

### git remote set-url origin <new-url>
Changes which repository an existing remote points at. The name (`origin`)
stays the same, only the address behind it changes, so commands like
`git push` keep working and now go to the new repository.

- I used it after cloning Lexicon's Hello-World, to point `origin` at my own
  GitHub repository instead of Lexicon's
- Check the result with `git remote -v`

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

### ls
Not a Git command: a terminal command that lists the files and folders in the
current folder.

- Hidden files and folders (names starting with `.`, like `.git`, `.env` and
  `.gitignore`) are NOT shown
- `ls -a` lists everything, hidden ones included

### git clone <url>
Downloads a full copy of a repository from GitHub to my computer, in a new
folder named after the repository. The original on GitHub stays as it is.

- It copies the whole history (every commit), not just the current files,
  including the hidden `.git` folder
- It sets up `origin` automatically, pointing at the URL I cloned from
- Files that were ignored by .gitignore never get committed, so they are never
  on GitHub and don't come with the clone

### git switch -c <branch>
Creates a new branch and moves me onto it. A branch is a movable label on a
commit: it copies no files, and each new commit I make moves the label forward.
Working on a branch keeps `main` unchanged until I merge the branch back in.

- `git switch <branch>` moves between existing branches; Git swaps the files in
  my folder to match that branch
- Older notes write `git checkout -b <branch>`, which does the same thing
- A branch is not a clone: all branches live in the same repository and folder,
  while a clone is a complete separate copy of the repository

### git branch
Lists the branches in my repository. The one marked with `*` is the branch
I'm on right now.

- Example: `* add-about` and `main` means both branches exist and I'm on
  add-about
- It only lists my local branches. `git branch -a` also shows the ones on
  GitHub (they start with `remotes/origin/`)

### git merge <branch>
Brings the commits from another branch into the branch I'm on right now.
To merge a feature into main, I first `git switch main`, then
`git merge <feature-branch>`.

- Fast-forward: if main hasn't moved since the branch was made, Git just moves
  the main label forward to the branch's latest commit
- Merge commit: if both branches have new commits, Git makes a new commit that
  joins them (it has two parents)
- If both branches changed the same lines, Git stops and I choose what to keep

### git branch -d <branch>
Deletes a branch once it's merged. Only the label is removed: the commits are
safe, because they're already part of the branch I merged them into.

- If the branch is NOT merged, `-d` refuses with "not fully merged", which
  protects me from losing work
- `git branch -D <branch>` (capital D) forces the delete anyway. Its unmerged
  commits are then very hard to get back
- I can't delete the branch I'm on; I switch to another branch first
- `git push origin --delete <branch>` deletes the branch on GitHub

### cat <file>
Not a Git command: a terminal command that prints a file's contents in the
terminal, so I can see what's inside without opening an editor.

- Example: `cat title.txt` prints `Title: Git Practice`
- Good for short files; for long ones an editor is easier

### Merge conflicts
A merge conflict happens when the two branches I'm merging both changed the
same lines of the same file. Git can't know which version is right, so it
stops the merge and asks me to choose. Changes to different files, or to
different parts of one file, are combined automatically.

Git marks the clash inside the file:

    <<<<<<< HEAD
    Title: Git Practice          (my branch's version)
    =======
    Title: Learning Git          (the other branch's version)
    >>>>>>> new-title

To solve it:
1. Edit the file so it says what I want, and delete all the `<<<<<<<`,
   `=======` and `>>>>>>>` lines (VS Code has "Accept Current / Incoming" buttons)
2. `git add <file>` to mark it as solved
3. `git commit` to finish the merge
- `git merge --abort` cancels the merge and puts everything back as it was
