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

- `git add <file>` stages one file
- `git add .` stages every changed and new file in the folder nnnn

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