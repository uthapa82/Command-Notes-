* git rename current branch 

	`$ git branch -m <newname>

* git rename a branch while pointed to any branch 

	`$ git branch -m <oldname> <newname> `

* Pull specific commit 

	`$ git log `

	`$ git fetch origin <copy paste the sha from git log>`

	`$ git checkout FETCH_HEAD `
	
=====================Android Studio ====================================
cd <Android Studio project folder> 
git init 
git remote add origin <link to repo>
git fetch origin 
git checkout master (branch)

/// from android studio create new branch and push it to that branch 

-----------------------------
now add commit and push
git add .
git commit -m "message"
git push

========================================================================

cd to folder
git init
git add . 
git commit -m " Comment"
git branch -m master main
git remote add origin "URL"
git push -u origin master  // if showing error git push -f origin main

@create branch 
git branch main   //create a new branch main
git branch //should display all branch 
git checkout main 	//switching to main branch 
git branch -d master  //delete master branch

/****************************************************************************************************************
For the ones who have problem to merge into main branch (Which is the new default one in Github) you can use the following:
*************************************************************************************************************
git checkout master  
git branch main master -f    
git checkout main  
git push origin main -f


Student 2
git config --get remote.origin.url
git remote add origin https://github.com/TCNJSwEngg/CCYVC.git
git remote -v
git remote set-url origin https://github.com/TCNJSwEngg/CCYVC.git
git stash

//**********************************************************************
undo last commit if pushed
git log --oneline
git reset --hard <number > or
git reset --hard HEAD~1
undo last commit if not  pushed 

git reset 'Head~1'

Linkedin Learning git/github
* git init 
* go inside the project folder and type : 
	* git log 
	* git  log  -n 5 // most recent 5 logs 
	* git log --since=2021-01-08 // after the date specified 
	* git log --author="thapa" 
	* git log --grep="Initi" // globally searching for regular expression (grep) 

* Git Architecture 
	*git uses three -tire architecuture ( repository-> staging index -> working) 
	* from working (git add file.txt) to staging index and from staging index (git 
	commit file.txt to repository) 
	* working(file.txt) -> git add file.txt -> staging index -> git commit -> repo
	* working(file.txt(v2) ->git add file.txt(v2) ->staging index -> git commit >repo
	#Hash value (SHA -1) = git generated a checksum (is a number generated by taking a number and feeding in to a mathematical algorithms) for 		each chan
	   -ge set. 
	* Checksum algorithm convert data into a simple number 
	* Same data always equals same checksum
	* Used to guarentee data integrity 
	* changing data would change the checksum
	* Sha-1 => 40-character hexadecimal string (0-9 ,a-f)
	* example : 5c15e8bd540c113cd2d9eac6f64cacbc5ff6fe9c
	#Head 
	* Pointer to tip of current branch in repo
	
* Make changes to Files 
	@Add files 
	* git status // check the status 
	* git add . // to add all the files from the working directory 
	* git commit -m "Adding all files"
	* git add filename //add specific files from the working directory
	* git commit -m "Adding this specific file from working directory"
	* git status // status nothing to commit, working directory clean
	@Edit the files 
	* git diff // helps in differentiating what changes made to a file 
	* git diff --color-words
	@move and rename
	* git mv source file space destination file (new name)
	* git rm file name // to remove the file 
	* git rm -f filename // force delete
*Using git with a real project 
	@intialize git 
	* git commit -am "Comment"  /all at once 
	@view previous commits 
	* git show sha number just first 5-6 will work
	* git show sha number  --color-words
	@compare commits
	* git diff (name of the oldest commit)..(sha of new commit)
	* git diff(name of the oldest commit)..(sha of new commit) --color-words
	* fit dif(name of the oldest commit)..Head --color-words
                     @multiline commit message 
                     * 
	
