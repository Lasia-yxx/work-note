# Markdown 学习作业

## Git 入门教程

### 初始化

- 安装 Git 工具：

    在[官网](https://git-scm.com/download/win)下载对应的 Git 安装包，下载完成后进行默认安装即可。

- 设置用户标识：

    在完成安装后，打开 Git Bush 输入如下命令用以配置用户名和邮箱：

    ```shell
    git config --global user.name `${userName}`
    git config --global user.email `${email}`
    ```

- 创建仓库：

    在一个文件夹下右键打开 Git Bash 输入命令 `git init` 此时这个文件夹下会多出一个 `.git` 的目录，此目录是 Git 用来跟踪管理版本的。此时一个仓库就创建完毕了。

- 配置 .gitingore：

    在项目开发中会出现一些我们不需要上传到仓库的文件，比如临时文件、Npm 包、调试文件等。而 Git 在保存文件时会根据 `.gitingore` 中的一些配置自动过滤这些文件。

    其中 `.gitingore` 遵守的一些规则如下：

    ```shell
    /mtk/ 过滤整个文件夹
    *.zip 过滤所有 .zip 文件
    /mtk/do.c 过滤某个具体文件

    !src/   不过滤该文件夹
    !*.zip   不过滤所有 .zip 文件
    !/mtk/do.c 不过滤该文件
    ```

### 开发流程

- 添加文件：

    在 Git Bash 上输入命令 `git add ${fileName}` 来添加文件。如果没有提示，则说明文件添加成功。

    如果要提交全部已修改文件，可以使用 `git add .` 命令，在原有的命令基础上加上了英文句号，用以选择全部已修改文件。

- 提交到仓库：

    命令 `git commit -m ${describ}` 用于将存在暂存区的文件提交到仓库中。其中 describ 指提交文件时的备注。

- 查看 Git 状态：

    使用 `git status` 命令可以查看当前工作区的状态，使用此命令可以查看当前工作区有哪些文件还没有提交。

- 查看 Git 提交记录：

    使用 `git log` 命令可以在 Git Bash 中打印出 commit 的提交记录。其格式如下：

    ```shell
    git log
    # commit f012b9546d90c5c0c7534cd01c70b6a5109b9fce (HEAD -> master)
    # Author: Lasia Yan <lasia.yan@maiscrm.com>
    # Date:   Wed Mar 10 15:34:02 2021 +0800

    # 第二次提交

    # commit 3f03475ffc90c43ac18c2c8410a9dd432aa8952a
    # Author: Lasia Yan <lasia.yan@maiscrm.com>
    # Date:   Wed Mar 10 15:32:44 2021 +0800

    # 第一次提交
    ```

- 查看文件改动：

    如果想要查看一个文件的具体改动，可以使用 `git diff ${fileName}` 命令，此时 Git 会在 Git Bash 中打印出文件的具体改动情况。

- 版本回退：

    `git reset ${router}` 命令用于版本的回退，其中 router 是指提交版本的哈希值。假设我们需要返回上一个 commit 的版本，我们可以使用如下命令 `git reset --hard HEAD^` 此命令，其中 `HEAD^` 是指以当前 HEAD 向上的一个版本。

    当我们执行完回归上一个版本的操作后，使用 `git log` 命令可以看到之前的提交记录，而看不见回退之前的版本提交记录。

    如果我们需要重新回到新版本时，我们需要执行 `git reset ${hash}`，此时我们需要新版的哈希值，但 `git log` 命令已经不打印新版本的提交记录了，所以此时我们需要使用 `git reflog` 命令，此条命令可以在 Git Bash 中打印所有分支的所有操作记录包括已经被删除的 commit 记录和 reset 的操作。

### 远程仓库

- 初始化：

    由于代码仓库与 Git 之间是通过 SSH 加密进行传输，所以使用 `ssh-keygen` 命令生成公钥，将生成的公钥复制到代码仓库的 key 中。

- 克隆项目：

    使用 `git clone ${projectUrl}` 命令将代码仓库的代码 clone 到本地。其中 projectUrl 指代码仓库的链接。

- 查看分支：

    项目克隆完毕后，在项目目录中打开 Git Bash，使用 `git branch` 命令查看分支。或者使用 `git branch -a` 来查看本地和远程分支，如下所示：

    ```shell
    git branch -a
    # develop
    # master
    # remotes/origin/HEAD -> origin/develop
    # remotes/origin/develop
    # remotes/origin/master
    ```

    其中 `remotes/origin/` 指的时远程分支。

- 创建分支：

    使用 `git checkout -b ${branchName}` 命令可以创建一个本地分支，并跳转到该分支。

- 上传文件：

    当在本地使用 commit 提交文件之后，我们可以使用 `git push` 来提交代码。

- 合并修改：

    代码上传后，我们需要提交 Merge Request 用于进行代码审查。在提交 MR 时，需要选取源分支和目标分支，填写 MR 的标题和描述，选取审查人，当审核通过后，该分支会自动与目标分支合并。

### 常见问题

- 删除远程分支后，在本地查看分支，发现远程分支仍然存在：

    此时我们执行 `git remote show origin`，这条命令会在 Git Bash 中打印远程分支的状态，如下所示：

    ```shell
    git remote show origin
    # remote origin
    # Fetch URL: git@gitlab.maiscrm.com:lasia.yan/training.git
    # Push  URL: git@gitlab.maiscrm.com:lasia.yan/training.git
    # HEAD branch: develop
    # Remote branches:
        # develop                  tracked
        # master                   tracked
        # refs/remotes/origin/base stale (use 'git remote prune' to remove)
    # Local branches configured for 'git pull':
        # develop merges with remote develop
        # master  merges with remote master
    # Local refs configured for 'git push':
        # develop pushes to develop (up to date)
        # master  pushes to master  (up to date)
    ```

    我们可以看到输出的信息中有这样一条 `resf/remotes/origin/base stale`，这个信息说明了 base 这个分支在远程已经被删除了。此时我们执行 `git remote prune` 就可以同步这些信息。

- 远程有一个新分支，本地新建同名分支并进行关联：

    对于这种远程有一个新的分支而本地没有同步时，我们可以使用 `git checkout -b ${branchName} origin/${remoteBranchName}`。这条命令中的 `origin/${remoteBranchName}` 就是将本地新建的分支和远程的分支合并并且相关联。

## TodoMVC 需求

### 页面布局

- 总体布局：

    整个页面应该由标题，输入框，以及展示列表组成。

    标题用来表明这个应用的大致功能及品牌信息。

    输入框用来处理用户输入，将用户的输入转化为数据存储起来。

    展示列表用来展示用户的数据，并处理用户一些操作，如删除、新增、设置完成状态、取消完成状态等。

- 详细布局：

    输入框在没有展示列表没有数据可以展示的状况下只显示输入框的 placeholder 字段。当展示列表有数据时，在输入框左边显示一个功能按钮。

    展示列表在没有数据可以展示的时候是隐藏的，而且在每一个任务框左边有一个 Radio 组件，右边有一个删除功能按钮。

    展示组件的底部有一个分页栏，分别为 All、Active、Completed。左边有一个未完成统计任务个数功能，当存在状态未已完成的任务时，右边会出现一个清除所有已完成任务的功能按钮。

### 功能

- 新增任务：

    用户在输入框输入待做事项后，系统监听键盘回车事件，当用户敲击回车时，此项待做被存储。

- 删除任务：

    用户点击任务展示框右边的关闭按钮时，就将该条任务从任务列表中清楚，无论任务是否完成。

- 改变任务状态：

    用户点击待做事项展示框左边的 Radio 组件，此项待做任务就变为已完成任务。当用户再次单击此组件时，该条任务的状态就由已完成变为未完成。

- 一键全选：

    当用户点击输入框左边的功能按钮时，触发一键全选功能，如所有任务的状态都是已完成的话就将所有任务的状态设置为未完成，否则就都设为已完成。

- 任务统计：

    该功能实时统计当前未完成任务的数量。

- 一键清除已完成任务：

    用户点击此功能后，会自动清除任务列表中已完成的任务。

- 分页：

    分页分为 All、Active、Completed，分别对应着三种不同的展示模式。

    - All 模式：

        此模式将所有任务按照创建先后的顺序展示出来，无论任务的状态是已完成还是未完成。

    - Active 模式：

        此模式将所有未完成的项目项目展示出来，不展示已完成的项目。

    - Completed 模式：

        此模式只展示已完成的项目，不展示未完成的项目。
