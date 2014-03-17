你不一定知道的几个很有用的 Git 命令
03/06. 2014
这里给大家分享一些很有用的 Git 命令，其中很多用法你可能都不知道，无论你是工作在团队环境中或在您的个人项目中，这些命令将对你帮助很大，让你可以更加高效的进行项目开发，更轻松愉快的工作和生活。

1. 导出最后一次提交修改过的文件

我一直在使用这个命令定期进行发送给其他人进行审查/整合。这条命令将把近期提交的修改过的文件导出到一个zip文件。


1
git archive -o ../updated.zip HEAD $(git diff --name-only HEAD^)
 2. 导出两次提交之间修改过的文件

同样，如果你需要导出两次提交之间修改过的文件，你可以用这一个。


1
2
git archive -o ../latest.zip NEW_COMMIT_ID_HERE $(git diff --name-only 
OLD_COMMIT_ID_HERE NEW_COMMIT_ID_HERE)
 3. 克隆一个特定的远程分支

如果你想从远程仓库克隆特定的一个分支，这条命令对你很有用：


1
2
3
4
5
git init 
 
git remote add -t BRANCH_NAME_HERE -f origin REMOTE_REPO_URL_PATH_HERE 
 
git checkout BRANCH_NAME_HERE
 

4. 从无关的本地仓库应用补丁

如果您需要申请从提交的一些其他不相关的创库到本地存储库，这里是一个快捷的方式：


1
2
3
git --git-dir=PATH_TO_OTHER_REPOSITORY_HERE/.git format-patch -k -1 --stdout
 
COMMIT_HASH_ID_HERE| git am -3 -k
 

5. 检查您的分支变化是是否其他分支的一部分

cherry 命令可以让你检查你的分支的变化是否存在于其他一些分支之中。它会显示在当前分支相对于给定的分支的修改，用+或-标志提示提交合并与否。+表示不存在，而-表示存在于给定的分支。


1
2
3
4
git cherry -v OTHER_BRANCH_NAME_HERE 
 
#For example: to check with master branch 
git cherry -v master
 

6. 启动一个无历史的新分支

有时候，你需要启动一个新的分支，同时想摒弃历史信息，例如，你想将代码放在公共领域（开源）又不想共享历史信息。


1
git checkout --orphan NEW_BRANCH_NAME_HERE
 

7. 在不切换分支的情况下从其它分支检出文件

下面的命令是从其他分支获取文件，而不用切换分支。


1
git checkout BRANCH_NAME_HERE -- PATH_TO_FILE_IN_BRANCH_HERE
 

8. 忽略跟踪文件的修改

如果你工作在一个团队，他们都是工作在同一个分支，你需要频繁的读取/合并文件。但是有时复位了你环境的特定配置，你必须在合并后每一次都再改一下。使用这个命令，你可以忽略更改特定的文件：


1
git update-index --assume-unchanged PATH_TO_FILE_HERE
 

9. 检查提交的修改是否发布版本的一部分

这个 name-rev 命令可以告诉你提交相对于最新发布版本的位置。利用这一点，你可以检查你的变化是否发布版本的一部分。


1
git name-rev --name-only COMMIT_HASH_HERE
 

10. 使用 pull rebase 操作替代 merge

如果你工作的团队正工作在同一个分支，那么你所要做的获取/合并或经常拉取。分支合并的 git 记录与合并提交时提示功能分支被并入主干。但在多个团队成员工作的同一分支的情况下，经常合并导致在日志中多个合并的消息引起混乱。所以你可以使用 pull rebase，以保持历史信息清除了无用合并的消息。


1
git config branch.BRANCH_NAME_HERE.rebase true
此外，您可以配置一个特定的分支总是衍合：


1
git pull --rebase
 
1. Export changes done in last commit
This command i have been using regularly for sending the changes done to another person for review/integration who is not on git. It will export the recent committed changed files to a zip file.
git archive -o ../updated.zip HEAD $(git diff --name-only HEAD^)  
2. Export changed files between two commits
Similarly if you need to export changed files between two commits, you can use this one.
git archive -o ../latest.zip NEW_COMMIT_ID_HERE $(git diff --name-only OLD_COMMIT_ID_HERE NEW_COMMIT_ID_HERE)   
3. Clone a specific remote branch
If you wish to clone only specific branch from a remote repository without having to clone whole of the repository branches, this will be useful to you.
git init  
git remote add -t BRANCH_NAME_HERE -f origin REMOTE_REPO_URL_PATH_HERE  
git checkout BRANCH_NAME_HERE  
4. Apply patch from Unrelated local repository
If you need to apply a patch from a commit on some other unrelated local repository to your current repository, here is a shortcut way to do that
view plaincopy to clipboardprint?
git --git-dir=PATH_TO_OTHER_REPOSITORY_HERE/.git format-patch -k -1 --stdout COMMIT_HASH_ID_HERE| git am -3 -k  
5. Check if your Branch changes are part of Other branch
cherry command lets you check whether your branch’s changes are present in some other branch or not. It will display the changes on current branch to given branch and indicate with a + or – sign to indicate if that commit is merged or not. + indicated not present while – indicates present in the given branch. Here is how to do that:
view plaincopy to clipboardprint?
git cherry -v OTHER_BRANCH_NAME_HERE  
#For example: to check with master branch  
git cherry -v master  
6. Start a new Branch with No History
Sometimes you need to start a new branch and do no want to carry the long history along, for example, you want to place the code in public domain(open source) but do no want to share the history.
git checkout --orphan NEW_BRANCH_NAME_HERE  
7. Checkout File from Other Branch without Switching Branches
Here is how to fetch just that file you need from other branch without even have to switch branches.
git checkout BRANCH_NAME_HERE -- PATH_TO_FILE_IN_BRANCH_HERE   
8. Ignore Changes in a Tracked File
If you are working in a team and all of them are working on same branch, chances are you are going to use fetch/merge quite often. but this sometimes resets your environment specific config files which you have to change every time after merge. Using this command, you can ask git to ignore the changes to specific file. So next time you do merge, this file won’t be changed on your system.
git update-index --assume-unchanged PATH_TO_FILE_HERE  
9. Check if committed changes are part of a release
The name-rev command can tell you the position of a committ with respect to a last release. Using this you can check if your changes were part of the release or not.
git name-rev --name-only COMMIT_HASH_HERE  
10. Pull with rebase instead of merge
If you are working in a team which is working on same branch, then you have to do fetch/merge or pull quite often. Branch merges in git are recorded with merge commit to indicate when a feature branch was merged with mainstream. But in the scenario of multiple team members working on same branch, the regular merge causes multiple merge messages in the log causing confusion. So you can use rebase with pull to keep the history clear of useless merge messages.
git pull --rebase  
Also, you can configure a particular branche to always rebasing:
git config branch.BRANCH_NAME_HERE.rebase true  
- See more at: http://webdeveloperplus.com/general/10-useful-advanced-git-commands/#sthash.aHPxubgA.dpuf