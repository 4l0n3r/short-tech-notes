git
git help command
git config --global user.name ""
git config --global user.email  ""
git init
git status
git add filename
git commit -a -m "message"
git diff filename
git log
git clone url.git
git remote -v
git push

git branch 					# See all the branches
git branch newBranch 		# creating new branch
git checkout newBranch 		# swift to new branch
git checkout -b newBranch 	# creating and swift to new branch
git merge otherbranch		# merging current branch with other branch
git branch -d branch 		# deleting branch



ssh-keygen -t rsa -b 4096 -C "<email>"    	# Generating key
ls | grep testkey  											# seeing key
adding this key to github
ssh-add -K ~/.ssh/testkey  									# adding ssh-key to ssh-agent










modified --> staging --> committed

git init

git status

git add

Removing :
git rm --cached filename
git restore --staged filename

History :
git log
git log --online

Undoing
1.checkout commit
2.revert commit
3.reset commit

Create Branch :
git branch branch_name
git branch -a

Switch to Branch :
git checkout branch_name
git checkout -b branch_name    creates and switch

Delete Branch :
git branch -D branchname

Push :
git push git_link branch_name ( to push )

	instead of using git_link we can use simple method to do that.
		git remote add origin git_link
					OR
		git clone git_link
		
	git push origin master 
	git remote -v

Merge :
git merge other_branch_name

	compare and pull
	create pull request
	merge pull request
	confirm merge
	delete temp branch


.gitignore
*.txt
!a.txt


git stash save












					Levels

1. git init

2. username , email

3. git add Readme

4. git commit -m "added"

5. git clone

6. git clone https://github.com/Gazler/cloneme my_cloned_repo

7. ignore files    *.swp

8. !lib.a   *.a

9. git status

10. git status

11. git rm --cached deleteme.rb

12 . git rm --cached deleteme.rb

13 . git stash save

14. git mv oldfile.txt newfile.txt

15. mkdir src
    git mv *.html src

16 . git log

17. git tag "new_tag" HEAD

18. git push --tags

19. git add forgotten_file.rb
    git commit --amend -m "commit_amend"

20. normal time change

21. git restore --staged to_commit_second.rb
    git commit -m "second"

22. git reset --soft HEAD@{1}

23. git checkout config.rb

23. git remote show

24. git remote show

25. git remote show remote_location
    git remote -v

26. 