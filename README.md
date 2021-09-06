# How fix Git mistakes (EN)

### Tips

#### How to fix mistakes:

Some mistakes often made and how to correct them. It happens a lot that you forget to create a new branch before starting to work on a new feature. For our workflow you must always work in feature branches. So here is how to fix that.

##### Started working in master branch but no commits yet
If you accidentally started working in the master branch but you haven't yet committed anything, you can easily move your work to a new branch by just creating a new branch, like this:

```
git checkout -b <new-branch>
```
This will leave your current branch as is, create and checkout a new branch and keep all your changes. You can then make a commit as usual with:

```
git add <files>
or
git add --all
```
and commit to your new branch with:

```
git commit -m "commit message"
```
The changes in the working directory and changes staged in index do not belong to any branch yet. This changes where those changes would end in.
You don't reset your original branch, it stays as it is. The last commit on <old-branch> will still be the same. Therefore you checkout -b and then commit.

== Started working in master branch, and also already did a commit (or more than one).
If you accidentally started working in the master branch (forgot to create a feature branch) you can easily move your changes to a feature branch like this:

```
git branch newbranchname
git reset --hard HEAD~1 # Go back 1 commit. You *will* lose uncommitted work! So, first commit everything - you can go back more than one commit if you need to.
git checkout newbranchname
```

The above creates a new branch that is equivalent to all the committed work you have in the current branch. Then you "roll back" the current branch by 1 commit (you can roll back more than one commit, you must just make sure you roll back to the correct commit.
Then you checkout the new branch you created to continue working there.

If you did some work, committed it and then did some more work before realizing you were on the wrong branch you have 2 options.
1. Commit again to store the new un-committed work and follow the above process but only doing a `git reset --hard HEAD~2` or however far you need to go back.
2. Do a `git add --all` and then `git stash` to store your uncommitted work in the stash. Then do the same branch, reset and checkout as explained above, followed by a git stash pop to re-apply the stashed work to your working branch.

=Gitignore

The files we save to the repository should be only the required files to be able to build the project and use it. No build artifacts or test results etc. should be stored in the repository. Input files that are used for testing etc. can be saved, no problem. Only files that are generated during compilation etc. should not be a part of the repository.

Git supports a mechanism to automatically specify which files should be excluded from your repo. This is a very flexible system where you can specify the files using regular expressions in each folder if you like. For details you can read the Git manual [[ http://git-scm.com/docs/gitignore | here ]]. 

Since this is something that everybody needs to configure regularly and the type of files for similar projects are normally the same, there already exists a large repository of .gitignore files on the internet. Specifically I recommend the following project: [[ https://github.com/github/gitignore | https://github.com/github/gitignore ]].

I have downloaded the recommend .gitignore file for Visual Studio development from this repository and it can be found here: {F148} When creating a new repository it should normally be sufficient to just add this file to the root of the repo. In specific cases we might have to update the file or add additional .gitignore files to specific folders, but in general this file should be enough.

When you already have a repository (already added) all the files of a project without using a .gitignore there is a way to still fix it.
You must do the following:

copy the .gitignore file to the project root
git add .
git commit -m "commit all changes before fixing the .gitignore problem"
git rm -r --cached .
git add .
git commit -m "Fixed .gitignore problem"

==Git diffall

if you're having problems to execute "Arc diff" you can try to use "Git diffall". The command git diffall is not a default git command and for use it you need make some steps.

1. Firstly, you need install an diff tool. For this example, we use the Visual Studio code because it's a free software and meets the needs, but you can use a software of your choice.
 
 - Visual Studio Code Download: https://code.visualstudio.com/download 

2. Next you must open the .gitconfig file (locate on C:\Users\YourUser\.gitconfig) and add two variables (written below. You need only copy and paste the variables)

```[diff]
    tool = vscode
[difftool "vscode"]
    cmd = code --wait --diff $LOCAL $REMOTE

ps: if you use another software, change  the variables "tool" and "difftool" to your software name, and in the variable "cmd" change the name "code" to your software cmd code 
```
3. Now you need to add the file below to the following path "C:\Program Files\Git\cmd" (or "C:\Program Files (x86)\Git\cmd", if the previous path doesn't exist)
{F64407}

When you add this file the diffall command will be created. This command compares two branchs in you repository and generate the diff files on the VS Code, yours syntax is: "git diffall branch01..branch02". 
For example: if you want to compare the branch on where you are editing the code, and the master branch, you need use "git diffall master..branchName".

#### Common Question
-Why I need to add the diffall command, if the Git have the command "Git diff" as default?
Answer: The Git Diff command open the files one by one, so if you edit fifteen files, it opens fifteens Visual Studio Code instances, and the "git diffall" command opens all the files in only one instance.
