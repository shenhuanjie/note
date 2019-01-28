# Git写出好的 commit message

## 为什幺要关注提交信息

- 加快 Reviewing Code 的过程
- 帮助我们写好 release note
- 5年后帮你快速想起来某个分支，tag 或者 commit 增加了什么功能，改变了哪些代码
- 让其他的开发者在运行 `git blame` 的时候想跪谢
- 总之一个好的提交信息，会帮助你提高项目的整体质量

## 基本要求

- 第一行应该少于50个字。 **随后是一个空行** 第一行题目也可以写成：`Fix issue #8976`
- 喜欢用 vim 的哥们把下面这行代码加入 *.vimrc* 文件中，来检查拼写和自动折行

```ruby
autocmd Filetype gitcommit setlocal spell textwidth=72
```

- 永远不在 `git commit` 上增加 `-m <msg>` 或 `--message=<msg>` 参数，而单独写提交信息

一个不好的例子 `git commit -m "Fix login bug"`

一个推荐的 commit message 应该是这样：

```
Redirect user to the requested page after login

https://trello.com/path/to/relevant/card

Users were being redirected to the home page after login, which is less
useful than redirecting to the page they had originally requested before
being redirected to the login form.

* Store requested path in a session variable
* Redirect to the stored location after successfully logging in the user
```

- 注释最好包含一个连接指向你们项目的 issue/story/card。一个完整的连接比一个 issue numbers 更好

- 提交信息中包含一个简短的故事，能让别人更容易理解你的项目

## 注释要回答如下信息

**为什么这次修改是必要的？**

要告诉 Reviewers，你的提交包含什么改变。让他们更容易审核代码和忽略无关的改变。

**如何解决的问题？**

这可不是说技术细节。看下面的两个例子：

```
Introduce a red/black tree to increase search speed
Remove <troublesome gem X>, which was causing <specific description of issue introduced by gem>
```

如果你的修改特别明显，就可以忽略这个。

**这些变化可能影响哪些地方？**

这是你最需要回答的问题。因为它会帮你发现在某个 branch 或 commit 中的做了过多的改动。一个提交尽量只做1，2个变化。

你的团队应该有一个自己的行为规则，规定每个 commit 和 branch 最多能含有多少个功能修改。

## 小提示

- 使用 *fix*, *add*, *change* 而不是 *fixed*, *added*, *changed*
- 永远别忘了第2行是空行
- 用 *Line break* 来分割提交信息，让它在某些软件里面更容易读
- 请将每次提交限定于完成一次逻辑功能。并且可能的话，适当地分解为多次小更新，以便每次小型提交都更易于理解。

## 例子

```
Fix bug where user can't signup.

[Bug #2873942]

Users were unable to register if they hadn't visited the plans
and pricing page because we expected that tracking
information to exist for the logs we create after a user
signs up.  I fixed this by adding a check to the logger
to ensure that if that information was not available
we weren't trying to write it.
Redirect user to the requested page after login

https://trello.com/path/to/relevant/card

Users were being redirected to the home page after login, which is less
useful than redirecting to the page they had originally requested before
being redirected to the login form.

* Store requested path in a session variable
* Redirect to the stored location after successfully logging in the user
```

## 依赖 Github Issue 的 Commit message 格式

这种工作方式期望团队使用 Github 的 Project 和 Issue 来管理开发任务。这时 Commit message 的 Header 部分应该包含 Issue Number。

```
[#123] Diverting power from warp drive to torpedoes
[Closes #123] Torpedoes now sufficiently powered
[Closes #123][#124][#125] Torpedoes now sufficiently powered
```

### Message Template

```
[<optional state> #issueid] (50 chars or less) summary of changes

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so.

Further paragraphs come after blank lines.

- Bullet points are okay, too

- Typically a hyphen or asterisk is used for the bullet, preceded by a
single space, with blank lines in between, but conventions vary here
```

## Angular 规范的 Commit message 格式

每次提交，Commit message 都包括三个部分：Header，Body 和 Footer。

```
<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>
```

其中，Header 是必需的，Body 和 Footer 可以省略。

### Header

Header 部分只有一行，包括三个字段：**type**（必需）、**scope**（可选）和 **subject**（必需）。

**type** 用于说明 commit 的类别，只允许使用下面7个标识。

- **feat** 新功能（feature）
- **fix** 修补bug
- **docs** 文档（documentation）
- **style** 格式（不影响代码运行的变动）
- **refactor** 重构（即不是新增功能，也不是修改bug的代码变动）
- **test** 增加测试
- **chore** 构建过程、辅助工具的变动
- **perf** 提高性能

如果 **type** 为 **feat** 和 **fix**，则该 commit 将肯定出现在 Change log 之中。

**scope** 用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

**subject** 是 commit 目的的简短描述，不超过50个字符。

- 以动词开头，使用第一人称现在时，比如 change，而不是 changed 或 changes
- 第一个字母大写
- 结尾不加句号

### Body

Body 部分是对本次 commit 的详细描述，可以分成多行。下面是一个范例。

```
More detailed explanatory text, if necessary.  Wrap it to
about 72 characters or so.

Further paragraphs come after blank lines.

- Bullet points are okay, too
- Use a hanging indent
```

应该说明代码变动的动机，以及与以前行为的对比，参考前文提到的 **注释要回答如下信息**。

### Footer

Footer 部分只用于不兼容变动和关闭 Issue。

```
BREAKING CHANGE: isolate scope bindings definition has changed.

    To migrate the code follow the example below:

    Before:

    scope: {
      myAttr: 'attribute',
    }

    After:

    scope: {
      myAttr: '@',
    }

    The removed `inject` wasn't generaly useful for directives so there should be no code using it.
Closes #123, #245, #992
```

## 相关工具

- [Commitizen](https://github.com/commitizen/cz-cli) 是一个撰写合格 Commit message 的工具
- [validate-commit-msg](https://github.com/kentcdodds/validate-commit-msg) 用于检查 Node 项目的 Commit message 是否符合格式
- [conventional-changelog](https://github.com/ajoslin/conventional-changelog) 是生成 Change log 的工具

## 延伸阅读

- [A Note About Git Commit Messages](http://web-design-weekly.com/blog/2013/09/01/a-better-git-commit/)
- [Proper Git Commit Messages and an Elegant Git History](https://github.com/erlang/otp/wiki/Writing-good-commit-messages)
- [A Better Git Commit](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)
- [5 Useful Tips For A Better Commit Message](http://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message)
- [好玩的提交信息](http://whatthecommit.com/)
- [Commit message 和 Change log 编写指南](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)
- [git_commit_message](https://github.com/joelparkerhenderson/git_commit_message)
- [Write good git commit message](https://juffalow.com/other/write-good-git-commit-message)