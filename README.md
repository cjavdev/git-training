# Git

## Why CLI first?
New features are added to cli first
All online help is provided as cli instructions
Tons of different CLI’s
Provides all power of git
Cross platform commands (windows, mac, linux)

## What is git?
Decentralized / Distributed source control
Super scalable (created for linux kernel project)
Most commands are local, makes operations fast
Its free and open source
Easy to find developers with git experience
Most popular version control system (defacto standard)
http://stackoverflow.com/research/developer-survey-2015

## Repository
Collection of files managed by git
History (all of it)
Working direcoty / workspace
.git folder keeps the repo

## Commits and Files
Versions files not folders
Can create empty dummy files to preserve folder
Commits are stored on a timeline called a branch
At least one branch and by default the branch is called `master`

## Install
https://git-scm.com/
Checkout as-is commit unix style line endings (more cross platform compatible)

## Configure
Git bash
`git config --global user.name “First Last”`
`git config --global user.email “first.last@email.com”`
`git config --global core.editor “nodepad++ -multiInst -nosession”`
`git config --global -e`

## Initialization for new repo
Create new repo locally
`git init foldername` will create folder name and initialize it as a git
repository

## Local Git States
Working Directory
All files and folders, may or may not be managed by git
Staging Area
Used for preparing for next commit
Repository
All committed and saved changes to the repo

Remote State (has it’s own three states)
Could be considered another state because it lives somewhere else

If the .git folder is missing git will respond saying there is no git repository

Initialization for an existing repo
`git init`

Sha1 id

Git show is last commit and all changes

Show how to setup alias and make pretty git logs
How to exclude files and folders (.gitignore)
How to resolve a conflict



## Resources



https://github.com/appacademy/ruby-curriculum/blob/master/w1d2/git-summary.md
https://github.com/appacademy/ruby-curriculum/blob/master/w1d5/git-workflow.md
https://github.com/appacademy/ruby-curriculum/blob/master/w1d5/alias.md
https://github.com/appacademy/ruby-curriculum/blob/master/w1d5/git-fix-authorship.md
http://www.ndpsoftware.com/git-cheatsheet.html#loc=stash;
https://github.com/appacademy/ruby-curriculum/blob/master/w1d5/git-workflow.md
http://pcottle.github.io/learnGitBranching/
http://staxmanade.com/2013/02/how-do-i-undo-bad-rebase-in-git/
