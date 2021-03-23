# Git Learning Notes

## `git commit` 的信息格式

使用 `git commit -m` 命令时，可以在后方添加单行注释，若注释内容较多，可以使用 `git commit` 命令，此时会开启一个类似一 *Vim* 的编辑框，在其中输入相应的注释即可。

### 规范的提交说明

- 提供更多的历史信息，方便快速浏览
- 可以过滤某些commit，便于筛选代码review
- 可以追踪commit生成更新日志
- 可以关联issues

### Git提交说明结构

Git提交说明可分为三个部分：Header、Body和Footer。

``` shell
<Header> <Body> <Footer>
```

- Header：

    Header部分包括三个字段type（必需）、scope（可选）和subject（必需）

    - type：

        type用于说明 commit 的提交性质。

        | 值 | 描述 |
        | :----: | ---- |
        | feat | 新增一个功能 |
        | fix | 修复一个Bug |
        | docs | 文档变更 |
        | style | 代码格式（不影响功能，例如空格、分号等格式修正） |
        | refactor | 代码重构 |
        | perf | 改善性能 |
        | test | 测试 |
        | build | 变更项目构建或外部依赖（例如scopes: webpack、gulp、npm等） |
        | ci | 更改持续集成软件的配置文件和package中的scripts命令，例如scopes: Travis, Circle等 |
        | chore | 变更构建流程或辅助工具|
        | revert | 代码回退 |

    - scope：

        scope说明commit影响的范围。scope依据项目而定，例如在业务项目中可以依据菜单或者功能模块划分，如果是组件库开发，则可以依据组件划分。

    - subject：

        subject是commit的简短描述。

- Body：

    commit的详细描述，说明代码提交的详细说明。

- Footer：

    如果代码的提交是不兼容变更或关闭缺陷，则Footer必需，否则可以省略。
