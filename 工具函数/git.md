## Git操作指南

### GIT vs SVN
- SVN是集中式版本控制系统，版本库是集中放在中央服务器的，而干活的时候，用的都是自己的电脑，所以首先要从中央服务器哪里得到最新的版本，然后干活，干完后，需要把自己做完的活推送到中央服务器。

- Git是分布式版本控制系统，那么它就没有中央服务器的，每个人的电脑就是一个完整的版本库，这样，工作的时候就不需要联网了。

### GIT基本概念
GIT为了管理文件、维护操作历史记录、方便追溯文件和回溯历史。
![git](../images/git.jpeg)
* Workspace：工作区
* Index / Stage：暂存区
* Repository：仓库区（或本地仓库）
* Remote：远程仓库

### 基本操作
- 安装和初始化
    -
    -
    - 设置基本信息

        设置用户名:
        ```
        git  config --global  user.name  '你再github上注册的用户名'
        ```
        设置用户邮箱:
        ```
        git  config --global  user.email  '注册时候的邮箱'
        ```

    -   直接本地初始化一个全新仓库
        ```
        mkdir gitTest && cd gitTest && git init
        ```
        git init会生成一个.git文件，就代表当前这个文件夹被Git管理
    - 远程仓库克隆
        ```
        git clone remote_url
        ```
- 日常工作流程
    - git clone 克隆远程资源到本地目录，作为工作目录；
    - 然后在本地的克隆目录上添加或修改文件；

    - 如果远程修改了，需要同步远程的内容，直接git pull就可以更新本地的文件；

    - 本地在修改之后，可以通过git status 查看修改的文件。然后使用git add 添加修改的文件暂到缓冲区；

    - 在添加之后，可以使用git commit添加到当前的工作区；

    - 在修改完成后，如果发现错误，可以撤回提交并再次修改并提交；

    - git push将本地的修改推送到远程的git服务器。

- 分支操作
    - git branch #查看本地当前分支名
    - git branch <branch_name> #创建指定名称的分支
    - git branch -v #查看分支详情，包括分支指向的commitId及提交信息
    - git branch <newBranch_name> [commitId] #根据commitId创建新分支
    - git checkout <branch_name> #切换到指定分支
    - git checkout -b <branch_name> #创建并切换到指定分支
    - git checkout -B <branch_name> #创建并切换到指定分支，重名会被覆盖
    - git checkout -b localbranch origin/remotebranch
    - git branch -d branch_name
    - git branch -D branch_name
    - git push origin --delete branch_name
    - get merge localbranch
    - get merge origin remotebranch
    - git push origin newBranch:newBranch # git push <远程主机名> <本地分支名>:<远程分支名>
    - git branch --set-upstream-to=origin/newBranchName 设置当前关联远程分支
- 回退
    - git commit之前
    ```
        git checkout -- filename
        git checkout -- .

    ```

    - 暂存区撤销
    ```
        git reset HEAD filename
        git reset HEAD
    ```
    - git commit之后
    ```
        git revert [commitId] # 操作之前和之后的提交记录都会保留
        git reset --hard [commitId]
        git reset –hard HEAD ^ #表示回到上一个版本
    ```
    - git push之后
    ```
        git log
        git reset --soft [commitId] #参数soft指的是：保留当前工作区，以便重新提交。
        git push origin master --force #不加--force会报错，因为版本低于远端，无法直接提交
    ```

- 冲突处理
    当我们多个分支代码合并到一个分支时；或者多个分支向同一个远端分支推送代码时，当目标分支也有相同修改是就很会出现冲突；处理冲突最好找到提交人过来一起看一起解决冲突。
    Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容

    `<<<<<<< `当前修改

    `>>>>>>> `传入修改

    - 第一种方式
        暂存更改然后拉取代码
    ```
        git stash
        git pull
    ```
        处理冲突在暂存后提交
    - 第二种方式
    ```
        git pull --rebase
    ```
    解决当前冲突暂存后执行还处于rebase状态，则继续解决冲突，没有则直接push
    ```
        git add .
        git rebase --continue
    ```
    - 第三种方式（放弃当前修改）
    ```
        git reset --hard
        git pull
    ```
