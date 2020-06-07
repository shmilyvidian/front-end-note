## GIT准则
- master分支永远是稳定线上的版本
- feature功能分支，release预发布分支
- 提交到master release分支 只能通过pull request方式不可以直接提交
- dependencies依赖不可以提交 package.json和相关配置文件找对应的业务组项目负责人更改
- 每次开发新功能，都应该新建一个单独的分支
- 本地配置文件涉及自身个人喜好不可以提交
- 每天下班前必须提交feature分支，每次提交前都执行下git status，并push到远程，第二天开始工作前必须拉取代码

## 工具
- 统一的编辑器和setting.json配置文件

## 代码风格
- Eslint 统一Eslint配置文件
- 提交前eslint 校验和 commit 信息的规范校验


## 提交规范
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