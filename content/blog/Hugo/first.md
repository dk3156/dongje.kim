---
title: "Hugo & Github pages"
date: 2023-07-17
mainSectionTitle: "hugo"
---
These are some errors / solutions / conjecture that I encountered when making hugo website.

Error_1 Submodule url

I had to delete original repo for my portfolio website and create a whole new repo because of this error. (Original website at dk3156.github.io)

For build and deploy site, github action give out error for not able to find anatole theme repo.
In hugo, themes are not the local files created inside your local files. It is a reference to another github repository for the theme creators. So by using git submodule url, github tries to find the repo that you can use as template to your website. 
This git submodule url is important becasue it is the only way for github to know WHERE to find the themes to copy.
If you change the content of the themes, your submodule url (for some reason) is changed and does not direct to the right repo url... This is completely my assumption. After changing content in the themes, submodule url was modified and give 404 not found error on the url.
So the conclusion is, it is better not to modify themes directly. 

### Instead, copy files from themes repo and paste to your hugo directories to override them!

Error_2 Build and Deploy

Always always always create branch before pushing to github by 
git checkout -b <branch-name>

BEFORE you make changes!!!
ALL the changes you make from now on will be stored in the new branch (NOT the main!)
```
git add.
git commit -m “msg”
git push origin <branch-name>
```
To push to the new branch.
Go to github and make pull requests. Merge it.
After merging, go to bash commands 
```
git checkout main
git pull
```
to pull from main branch and match changes.
This is integral for build-and-deploy process on github action!
Github will automatically go over the process before merging, so don’t have to worry about anything that might cause errors in the main branch,
Once you merge to the main, it’s difficult to go back.
So always make changes AFTER creating a new branch.

Error_3 Workflow permission

When creating cd.yml file inside .github/workflow directory
You CANNOT commit because github won’t allow without authorization.
The following errors occur for bash commit:
```
 ! [remote rejected] add-cd -> add-cd (refusing to allow a Personal Access Token to create or update workflow `.github/workflows/cd.yml` without `workflow` scope)
error: failed to push some refs to 'https://github.com/dk3156/dongje.kim.git'
```

Another error msg when build and deploy

```
  remote: Permission to dk3156/dongje.kim.git denied to github-actions[bot].
  fatal: unable to access 'https://github.com/dk3156/dongje.kim.git/': The requested URL returned error: 403
  Error: Action failed with "The process '/usr/bin/git' failed with exit code 128"
```

Always remember that you need to change Workflow permission to “Read and write permissions”
(Probably related to the error above, too)

Go to
Settings ->
Actions ->
General ->
Go to the bottom 
Workflow Permissions ->
Check Read and Write permissions

When github successfully build and deploy your website,
If you go to Settings -> Pages -> Build and Deployment
Choose from branch → There is another branch called “gh-pages”.
This will appear only if it is deployed correctly.
Change branch to gh-pages and hit save.

Error_4 Date

My main page did not show up in the deployed server. It showed up in the local host.
The problem was I did not set any date in the front matter of _index.md.
_index.md is responsible for the main page. The following front matter
```
---
title: "Home"
mainSectionTitle: "About me"
—--
```

Had to be updated like so:
```
---
title: "Home"
date: 2023-06-17
mainSectionTitle: "About me"
—
```

If you don’t specify the date, hugo automatically sets it as the latest modified date.
Somehow index.html skipped the main section without a date. 
