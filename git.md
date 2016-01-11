# Git

config username & email & push in repository

	git config user.name "Jiang Hongfei"
	git config user.email "jiang.hongfei@gmail.com"
	git config push.default simple

this wil save useranme & email in .git/config

config username & email & push globally

	git config --global user.name "Jiang Hongfei"
	git config --global user.email "jiang.hongfei@gmail.com"
	git config push.default simple

these setting will be saved into ~/.gitconfig

## Create a new branch
    git checkout -b branch_name

## Gi merge conflict
    git checkout --ours/--theirs file
    git reset file

## Go back to an old commit
    git reset --hard commit-id

## Merge from another commit without commit
    git merge hash_code --no-commit --no-ff
    
## Git show detail of a commit
    git show commit-id

## Git show files of a commit
	git show --pretty="format:" --name-only hash

## Git show log of a file
    git log --follow file
    or
    gitk file

## Git show log of amended commits
    git reflog

## editor
	git config --global core.editor vim

## Diff tools
	git config --global merge.tool vimdiff

## list all configurations
	git config --list

## show single config item
	git config user.name

## basic
	git init
	
### tracking new files
	git add *.c
	git add README
	
### commit your changes
	git commit -m 'initial project version'

### checking the status of file
	git status
	
### remove files
	git rm grit.gemspec
	
### keep the file in your working tree but remove it from your staging area
	# to tracted files
	git reset [HEAD] <file>
	or
	# to new files
	git rm --cached <file>

	# reset the whole staging area, and keep modifications
	git reset
	
### discard changes in workding directory
	git checkout -- <file>
	or
	git checkout HEAD <file>

### revert changes made to working copy, but leave untracked files
	git checkout .

### to remove untracked files
	git clean -fd

so rollback to last commit:

	git checkout .
	or
	git reset --hard [origin/master]
	then
	git clean -fd

### showing your remotes
	git remote -v

### mv files (kind of rename)
	git mv file_from file_to
	
### viewing the commit history
	git log
	git log master
	git reflog show HEAD
	
### changing your last commit
	git commit --amend

## Fixing whitespace when applying patches with git
git config --global core.whitespace trailing-space,space-before-tab
git config --global apply.whitespace fix
