# Git Hands-On Labs Solutions

This document contains the step-by-step commands and configurations for the 5 **Git Hands-On Labs (HOL)**.

---

## HOL 1: Git Configuration & Basic Operations

### Objectives
- Perform basic machine Git configuration.
- Integrate a custom editor (like Notepad++) as the default Git editor.
- Initialize repositories, create files, add to staging, and commit files.

### Steps and Commands

#### 1. Machine Level Git Configuration
Check if git is installed:
```bash
git --version
```

Configure global user name and email credentials:
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Verify current global configurations:
```bash
git config --global --list
```

#### 2. Default Editor Integration (Notepad++)
Create a command alias for Notepad++:
```bash
git config --global alias.npp '!f() { "C:/Program Files/Notepad++/notepad++.exe" "$@"; }; f'
```
*(Ensure the path points to where Notepad++ is installed on your machine)*

Set Notepad++ as the default global Git editor:
```bash
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession"
```

Verify Notepad++ config:
```bash
git config --global -e
```

#### 3. Source Control Operations
Create and enter the working directory `GitDemo`:
```bash
mkdir GitDemo
cd GitDemo
```

Initialize local repository:
```bash
git init
```

Verify hidden folders (including `.git` structure):
```bash
ls -la
```

Create a new file `welcome.txt` and verify creation:
```bash
echo "Welcome to Git session" > welcome.txt
cat welcome.txt
```

Check repository status:
```bash
git status
```

Add file to tracking area (staging):
```bash
git add welcome.txt
```

Commit changes using the default editor for comments:
```bash
git commit
```
*(This will launch Notepad++. Type your commit message, save, and exit the editor to complete the commit)*

Link local repository to remote GitLab repository and push:
```bash
git remote add origin https://gitlab.com/username/GitDemo.git
git pull origin master
git push -u origin master
```

---

## HOL 2: Ignoring Files with `.gitignore`

### Objectives
- Configure `.gitignore` to prevent tracking temporary log files and build folders.

### Steps and Commands

#### 1. Create temporary log files
Create a `.log` file and a logs folder:
```bash
echo "debug log content" > debug.log
mkdir log
echo "temp test logs" > log/temp.log
```

Check git status to see these tracked:
```bash
git status
```

#### 2. Configure `.gitignore`
Create a `.gitignore` file:
```bash
notepad++ .gitignore
```

Add the following patterns to the `.gitignore` file to ignore all `.log` files and any folder named `log`:
```text
*.log
log/
```

Verify status again. The `debug.log` and `log` folder will no longer appear as untracked files:
```bash
git status
```

Commit the `.gitignore` configuration:
```bash
git add .gitignore
git commit -m "Configure gitignore for log extensions and directories"
```

---

## HOL 3: Branching & Merging

### Objectives
- Create, list, switch, merge, and delete branches.

### Steps and Commands

#### 1. Branch Creation
Create a new branch:
```bash
git branch GitNewBranch
```

List all local and remote branches (the `*` indicates current active branch):
```bash
git branch -a
```

Switch to the newly created branch:
```bash
git checkout GitNewBranch
# Alternative command: git switch GitNewBranch
```

Add a file inside this branch and commit it:
```bash
echo "features implementation code" > feature.txt
git add feature.txt
git commit -m "Add feature file in GitNewBranch"
```

#### 2. Merging Branch to Master
Switch back to master:
```bash
git checkout master
```

View the text differences between the master and the branch:
```bash
git diff master..GitNewBranch
```

Merge the branch into master:
```bash
git merge GitNewBranch
```

Observe the commits logging in a graph view:
```bash
git log --oneline --graph --decorate
```

Delete the merged branch:
```bash
git branch -d GitNewBranch
```

---

## HOL 4: Conflict Resolution

### Objectives
- Handle merge conflicts caused by concurrent updates to the same file lines.

### Steps and Commands

#### 1. Simulate Conflict Scenario
Ensure master is clean, then create and switch to a branch named `GitWork`:
```bash
git status
git checkout -b GitWork
```

Create a file named `hello.xml` inside `GitWork` and commit:
```xml
<!-- hello.xml inside GitWork branch -->
<message>Hello from GitWork branch</message>
```
```bash
git add hello.xml
git commit -m "Add hello.xml on GitWork branch"
```

Switch back to master:
```bash
git checkout master
```

Create `hello.xml` on master with different content and commit:
```xml
<!-- hello.xml inside master branch -->
<message>Hello from Master branch</message>
```
```bash
git add hello.xml
git commit -m "Add hello.xml on master branch"
```

#### 2. Merge and Resolve Conflict
Merge branch `GitWork` into master:
```bash
git merge GitWork
```
*(This command will print a conflict error: "CONFLICT (content): Merge conflict in hello.xml. Automatic merge failed; fix conflicts and then commit the result.")*

Open the conflicted file to see the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`):
```bash
notepad++ hello.xml
```

Resolve the conflict by keeping the desired changes and removing conflict markers:
```xml
<message>Hello from Resolved Master and GitWork merges</message>
```

Stage the resolved file and commit to complete the merge:
```bash
git add hello.xml
git commit -m "Resolve merge conflict in hello.xml"
```

Clean up backup files created during the conflict resolution if any (and add them to `.gitignore`):
```bash
echo "*.orig" >> .gitignore
git add .gitignore
git commit -m "Ignore conflict backup files"
```

Delete the branch:
```bash
git branch -d GitWork
```

---

## HOL 5: Syncing with Remote Git

### Objectives
- Fetch, pull, and push updates between local and remote git servers.

### Steps and Commands
Verify local master is clean:
```bash
git status
```

Fetch and pull latest commits from the remote repository to ensure local master is up-to-date:
```bash
git pull origin master
```

Push all local commits to the remote master branch:
```bash
git push origin master
```
