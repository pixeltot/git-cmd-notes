# git-cmd-notes
my understanding of git cmds

git config
---------------------------------------------
$ git init
$ git add .
$ git --all (stages all the changes,all the untracked files, ALSO THE DELETED FILES)
$ git commit -m "type your message in double quotes"
$ git push YourAppName master
or
$ git push -u YourAppName WhateverBranchYouWantToPushToRemote
(note: the -u is there so you dont have to specify the AppName and Branch next time you decide to push your code)

$ git pull (will pull from remote and merge to your local machine)
$ git fetch (will pull from remote, but will not merge with your local)

other useful git cmds
---------------------------------------------
$ git status 
(note: see what has changed since your last commit)

$ git diff
(note: check to see what has changed in your code, and if files been added, its more descriptive than git status)

$ git diff --staged
(note: check to see what has been staged and ready to be commited)

$ git commit -a -m 'This will stage and commit at the same time'
(not: im assuming the -a stands for add)

$ git checkout --FileName
(note: to undo changes done to last commit?)

$ git reset FileNameThatYouWantToUnstage.html


-------------------------------------------------
WARNING!, DONT DO THESE AFTER YOU PUSH AS YOU WILL REWRITE HISTORY

$ git commit --amend -m 'modify last commit' 
(incase you forgot to add a file or change something, this will override last commit/or add it to last commit)

$ git reset --soft HEAD^
(note: undo last commit, put changes into staging)

$ git reset --hard HEAD^
(note: undo last commit and all changes)

$ git reset --hard HEAD^^
(note: undo last 2 commites and all changes)

--------------------------------------------------

$ git remote add YourAppName TheAddressOfYourApp 

$ git push -u YourAppName Branch
(note: the -u is there so you dont have to specify the AppName and Branch next time you decide to push your code)

heroku
---------------------------------------------
$ heroku create (create a space on heroku to put your app)
$ heroku git:remote -a yourappname
$ git push heroku master (push your remote git repository to heroku)


Each time you wish to deploy to Heroku:
---------------------------------------------
$ git add -A
$ git commit -m "commit for deploy to heroku"
$ git push -f heroku
$ git push heroku master

--------------------------------------------
if you want to checkout someone elses code then you have to clone a copy from git
$ git clone URLofRepository
or if you want to call it something else you have to pass in a name after the URL
$ git clone URLofRepository NewLocalFolderName


Git Clone
---------
$ git clone URL
1. downloads the entire repository into a new git directory
2. adds the 'origin' remote, pointing it to the clone URL
3. you can check it by running... $ git remote -v
4. checks out the initial branch (likely master)
- - - - - - - - - - - - - - - - - - - - - - - - - - -
1. $ git branch cat (this will create a new branch called 'cat')
2. $ git branch
	cat
	*master
this will show you that you are still on the master branch even though you created a cat branch
to move to the cat branch you need to run this cmd

3. $ git checkout cat
	switch to branch 'cat' (we are switching timelines)
	so were not in the master timeline anymore

4. $ echo 'this is a cat file' > cat.txt
   $ git add cat.txt
   $ git commit -m 'create quantum cat'

5. This will be added to our 'cat' branch, NOT THE MASTER BRANCH
6. $ git checkout master (this will put you back onto the master branch)
7. $ git merge cat (this cmd will merge the cat branch into the master branch, if you're on the master branch)
8. $ git branch -d cat (this will delete the cat branch, since we are done with it and have merged it already)
-----------------------------------------------
SCENERIo example

lets work on a new admin feature
1. $ git checkout -b admin (creates and checks out branch because the -b is there)
2. $ git add admin/dashboard.html
3. $ git commit -m 'add dashboard'
4. $ git add admin/users.html
5. $ git commit -m 'add user admin'

boss calls you up and tell you there are bugs on master, must switch back to master

6. $ git checkout master
7. $ git branch (to make sure we are actually on the master branch)
8. $ git pull (to make sure we have the latest version and/or changes)
9. $ git add store.rb
10 $ git commit -m 'fix store bug'
11.$ git add product.rb
12.$ git commit -m 'fix product'
13.$ git push (push those changes back up to the master remote)

NOW THAT THE EMERGENCY IS TAKEN CARE OF WE CAN MOVE BACK TO OUR ADMIN FEATURE BRANCH

14.$ git checkout admin (switched to branch)
15.$ git checkout master (when we are ready to bring in changes a.k.a merge)
16.$ git merge admin
---------------------------------------------

git uses Vi if no default editor is set to edit commit messages.
but DONT PANIC! here are a list of Vi cmds:
j down
k up
h left
l right
i insert mode
ESC leave mode
:q! cancel & quit
:wq save & quit

$ git merge origin/master
$ git branch -r (-r stands for remote, to see remote branches)
$ git remote branch

------------------------------------------------
what if you wanted other people to work on your branch
then you have to create a remote branch, heres an example of a shopping cart remote branch

$ git checkout -b shopping_cart
(note: this will create a new branch 'shopping_cart')

$ git push origin shopping_cart
(note: this will link the local branch to the remote branch and start tracking it)

$ git add cart.rb (once you have gto some work done on the branch)

$ git commit -a -m 'add basic cart ability'

$ git push (push changes from local branch to remote branch)

Now that you've created and worked on a branch someone else can checkout that branch with...

$ git pull (here is what jenny from the block would type)

$ git branch (here is what jenny would type if she wanted to see which branch shes on)
	* master

$ git branch -r (this will list all the remote branches for jenny)
	origin/master
	origin/shopping_cart

$ git checkout shopping_cart (jenny has to run this cmd in order to start working on the branch, otherwise she will still be on the master branch, this will atuomatically be setup as a tracking remote branch, so from here she can make her changes and do a git push to the remote branch)

$ git remote show origin 
(note: useful cmd for showing all of your remote branches
and whether they are tracked or not, and any merges,
and any local branches conigured for when we do a git push,
it also checks the server to see if any of your branches are out of date)

remote branches, just like local branches, dont last forever, you have to delete them once you are finished with them and have merged into the master branch
- - - - - - - - - - - - - - - - - - - - - - - - - - - 

Deleting a Branch
-----------------

$ git push origin :shopping_cart 
('shopping_cart' is the branch name I would want to remove in this example)
this will only delete the remote branch

$ git branch -d shopping_cart
this will delete the local branch of shopping_cart

	error: The branch 'shopping_cart' is not fully merged.
	If you are sure you want to delete it, run 'git branch -D shopping cart'.

This message appears because git sees changes that have not yet been commited, its a safety feature of git 
If you actually want to delete the branch without commiting your changes use a capital '-D'
like this...

$ git branch -D shopping_cart

-----------------------------------------------------

now that jenny deleted the remote branch, when jon goes to git push his local branch there will be no remote branch to push to (its just a local branch now on jons computer)

so jon runs these cmds:

$ git commit -m -a 'add ability to pay'
$ git push
$ git remote show origin
	this will show that there is a stale branch (meaning its been deleted from the remote)

$ git remote prune origin
	this cmd will clean up deleted remote branches (stuff... remote branch jenny deleted)

$ git push heroku-staging staging:master 
	will push and deploy staging on heroku

------------------------------------------------------

TAGGING
-------
A tag is a reference to a commit (used mostly for release versioning)

$ git tag (this will list all tags)
	v0.0.1
	v0.0.2
	v0.0.3

$ git checkout v0.0.1 (check out code at commit)

$ git tag -a v0.0.4 -m "version 0.0.4" (To add a new tag)

$ git push --tags (to push our tags and version our application)

-------------------------------------------------------

REBASE
------
heres what the git rebase cmd does.

$ git rebase

1. Move all changes to master which are not in origin/master to a temporary area.
2. then its going to run all origin/master commits one at a time
3. then its going to run all commits in the temporary area on top of our master one at a time

If you notice there are no merge commits, its just one after the next, after the next, and so on...

LOCAL BRANCH REBASE
-------------------

$ git checkout WhateverYourBranchIsCalled
	switched to branch WhateverYourBranchIsCalled

$ git rebase master

If all goes well, merge master

$ git checkout master
$ git merge WhateverYourBranchIsCalled

all this is going to do is one of those fast-forwards 

WHAT ABOUT CONFLICTS?
---------------------
$ git status
$ git rebase add README.txt
$ git rebase --continue

other rebase cmds
- - - - - - - - - 
$ git rebase --skip (if you would prefer to skip this patch)
$ git rebase --abort (to check out the original branch and stop rebasing run)
------------------------------


Colorizing the log

$ git config --global color.ui true

$ git log --pretty=oneline

You can format the output of the log exactly how you want it like this:
can use placeholders like [%an] 

$ git log --pretty=format:"%h %ad- %s [%an]"
- - - - - - - - - - - - - - - - - - -

Patch
-----
will show you what lines were added and removed and modified for each commit

$ git log --oneline -p


Stats
-----
will show you how many insertions and deletions for each file for each commit

$ git --oneline --stat


Graph
-----
graph will give you a visual representation of the branch merging into master

$ git log --oneline --graph


Date Ranges
-----------
Its not always usefull to list the entire log, so you can specify a start date and end date of the log

$ git log --until=1.minute.ago

you cant also limit the log output based on time

$ git log --since=1.day.ago

$ git log --since=1.hour.ago

$ git log --since=1.month.ago --until=2.weeks.ago

$ git log --since=2000-01-01 --until=2012-12-21

to see logs plus commits run this:
$ git log -p

Diffs
-------
to see which lines have changed since the last commit

$ git diff HEAD

to see earlier commits

$ git diff HEAD^	parent of latest commit
$ git diff HEAD^^	grandparent of latest commit
$ git diff HEAD~5	five commits ago

you can also compare commits 

$ git diff HEAD^..HEAD

You cant also compare 2 commits by SHAs

$ git diff f5a539487598739..4s6788597245768983gfd7

you can use abbreviated SHAs too!

You can also compare two branches, diff between two branches

$ git diff master bird	this example compares the bird and master branches with eachother





TIME-BASED DIFFS
----------------

$ git diff --since=1.week.ago --until=1.minute.ago


BLAME
-----
$ git blame index.html --date short

this will give you the commit hash, author, date, line#, content


EXCLUDING FILES
---------------
.git/info/exclude

experiments/	will exclude this folder from git

the experiments directory is now invisible to git


EXCLUDE PATTERNS
----------------
exclude a specific file or files

*.mp4	this will exclude all files that have the mp4 exstension

logs/*.log	exclude .log files in logs directory


EXCLUDING FROM ALL COPIES
-------------------------
.gitignore

logs/server.log

DONT COMMIT LOG FILES, THEY CREATE CONFLICTS!


REMOVING FILES
--------------
$ git rm README.txtgit 

$ git status

$ git commit -m 'remove readme'

DELETED from the local filesystem & untracked


UNTRACKING FILES
------------------
what if you are already tracking files?  then you dont want to delete them

$ git rm --cached development.log

$ git status

you'll see its deleted from git , but NOT from your local file system

after you untracked that file, you can then do a gitignore on the file


CONFIG
------
$ git config --global user.name 'tony tiger'
$ git config --global user.email 'tony_tiger@gmail.com'
$ git config --global core.editor emacs (use emacs for interactive commands)
$ git config --global merge.tool opendiff (use opendiff for merging conflicts)

$ git config user.email 'tony_tiger@example.com' (sets email for current repo)
$ git config --list


ALIASES
-------
$ git config --global alias.st status		git st ---> git status
$ git config --global alias.co checkout		git co ---> git checkout
$ git config --global alias.br branch		git br ---> git branch
$ git config --global alias.ci commit		git ci ---> git commit

aliases are used for abreviations of longer cmds
i case you dont feel like having to type out the entire cmd
for lazy people









