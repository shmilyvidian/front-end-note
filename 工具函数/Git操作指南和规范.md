## Git操作指南和规范

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

#### .git
- hooks 客户端和服务端的钩子脚本
- info 全局性排除文件
- logs 日志信息
- objects 存储所有的数据内容
- refs 存储指向数据（分支）的提交对象的指针
- config 文件包含项目特有的配置选项
- description 显示对仓库的描述信息
- HEAD 目录输出的分支
- index 文件保存暂存区的信息
#### 三大区域
- 工作区
- 暂存区
- 版本库
三大对象
#### Git对象
核心部分简单键值对数据库，插入任意数据会返回一个键值，通过该键值可以检索该内容，全量压缩存储
- 内容存储操作
`echo 'content' | git bash-object -w --stdin`
`git cat-file -p hash_key`
- 查看数据内容
`find .git/objects -type f`
`git cat-file -p hash_key`
`git cat-file -t hash_key` blob对象
- 问题（树对象解决）
    - 记住每一次对应的hash_key繁琐复杂
    - 文件名并没有被保存，文件内容被保存
    - 任意一个Git对象只能存储对应自己的内容，对应一个文件，无法反应整个项目的快照
#### 树对象
既能解决文件名保存的问题，也允许将多个文件组织到一起
构建树对象(并塞入暂存区) update-index write-tree read-tree
- git update-index -add --cacheinfo 10064 hash_key filename (暂存区放置数据)
- git write-tree(树对象，暂存区放置快照，放入本地仓库)
查看暂存区
- git ls-files -s
- read-tree 将一个树对象加入第二个对象，使其成为新的树对象
- 问题
 - 重用快照必须记住对应的hash_key，未维护生成快照的操作和修改记录
#### 提交对象
调用commit-tree命令创建一个提交对象，为此需要指定一个树对象的hash_key，以及该提交的父提交对象（如果有的话 第一次将暂存区做快照就没父对象）
包裹树对象，提供记录和其他相关信息


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
### GIT准则
- master分支永远是稳定线上的版本
- feature功能分支，release预发布分支
- 提交到master、release分支只能通过pull request方式，不可以直接提交
- dependencies依赖不可以提交 package.json和相关配置文件找对应的业务组项目负责人更改
- 每次开发新功能，都应该新建一个单独的分支
- 本地配置文件涉及自身个人喜好不可以提交
- 每天下班前必须提交feature分支，每次提交前都执行下git status，并push到远程，第二天开始工作前必须拉取代码
- 合并到release前必须组织过一次CR

### 工具
- 统一的编辑器和setting.json配置文件

## 代码风格
- Eslint 统一Eslint配置文件
- 提交前Eslint 校验和 commit 信息的规范校验


### 提交规范
- 提交message清晰明了，要用精简的语言说明本次提交的目的，其主要作用是为
了后续的搜索、版本的回滚、合并冲突的追溯等操作
- 格式要求： <type>(<scope>): <subject>
- type:

    | 类型 | 描述 |
    | :-----| ----:|
    |feat | 新增新功能 |
    |fix | bug修复 |
    |docs | 仅修改文档，例如README |
    |style | 修改了代码换行、缩进，不改变代码逻辑 |
    |refactor | 代码重构 |
    |perf | 优化相关 |
    |chore | 改变构建流程、增加依赖库等 |
    |test | 测试案例 |
- scope
    - 范围可以是指定提交更改位置的任何内容
        - 对 package.json 文件新增依赖库，chore(package.json): 新增依赖库
        - 或对代码进行重构，refactor(CardItem.vue): 重构卡片进件
- subject
    - 没有更合适的范围，可以直接写提交内容
- 工具
    - 依赖
        - @commitlint/cli
        - @commitlint/config-angular
        - @commitlint/config-conventional
        - husky
        - lint-staged
    - 配置
        ```
        "husky": {
            "hooks": {
                "pre-commit": "lint-staged",
                "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
            }
        },
        "lint-staged": {
            "*.js": [
                "eslint --fix",
                "git add"
            ]
        },
        ```
        ```
        commitlint.config.js
        ```