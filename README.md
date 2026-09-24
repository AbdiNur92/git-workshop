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
- `git add .` stages every changed and new file in the folder nnnn

After `git add`, `git status` shows the staged files in green under
"Changes to be committed".

### git commit -m "message"
Commiting what's on the stagging area and leaves a message
Saves everything in the staging area as a new snapshot (a commit) in my local history. Each commit gets a unique ID (hash), my name, the date, and the message I write after `-m` explaining what I changed. It's important to write good messages for refrence then you know exactally what every snapshot changed and you or someone you are working with can understand it better.
### git log --oneline
Short form of the commit history one line per commit
first 7 characters of the hash/commitID and commit message, Head -> Main marks the commmit I'm on and the branch I'm on origin/main shows where GitHub copy is
Regular git log shows the full version with the long hash, author, date, message
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
