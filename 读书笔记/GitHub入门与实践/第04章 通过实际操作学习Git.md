# 第4章 通过实际操作学习Git

在本章中，我们将学习Git相关的基本知识与操作方法。已经将Git实际运用于开发的读者大可跳过本章。本章中将要解说，是理解本书内容所必不可少的一些Git操作。请随着我们的解说，一边实际操作，一边学习并掌握Git。

## 4.1 基本操作

### git init——初始化仓库

要使用Git进行版本管理，必须先初始化仓库。Git是使用git init命令进行初始化的。请实际建立一个目录并初始化仓库。

```bash
$ mkdir git-tutorial
$ cd git-tutorial
$ git init
Initialized empty Git repository in C:/Users/shenh/Documents/GitHub/git-tutorial/.git/
```

如果初始化成功，执行了git init命令的目录下就会生成.git目录。这个.git目录里存储着管理当前目录内容所需的仓库数据。

在Git中，我们将这个目录的内容称为“附属于该仓库的工作树”。文件的编辑等操作在工作树中进行，然后记录到仓库中，以此管理文件的历史快照。如果想将文件恢复到原先的状态，可以从仓库中调取之前的快照，在工作树中打开。开发者可以通过这种方式获取以往的文件。具体操作指令我们将在后面详细解说。

### git status——查看仓库的状态

git status命令用于显示Git仓库的状态。这是一个十分常用的命令，请务必牢记。

工作树和仓库在被操作的过程中，状态会不断发生变化。在Git操作过程中时常用git status命令查看当前状态，可谓基本中的基本。下面，就让我们来实际查看一下当前状态。

```bash
$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

结果显示了我们当前正处于master分支下。关于分支我们会在不久后讲到，现在不必深究。接着还显示了没有可提交的内容。所谓提交（Commit），是指“记录工作树中所有文件的当前状态”。

尚没有可提交的内容，就是说当前我们建立的这个仓库中还没有记录任何文件的任何状态。这里，我们建立README.md文件作为管理对象，为第一次提交做前期准备。

```bash
$ touch README.md
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        README.md

nothing added to commit but untracked files present (use "git add" to track)
```

可以看到在Untracked files中显示了README.md文件。类似地，只要对Git的工作树或仓库进行操作，git status命令的显示结果就会发生变化。

### git add——向暂存区中添加文件

如果只是用Git仓库的工作树创建了文件，那么该文件并不会被记入Git仓库的版本管理对象当中。因此我们用git status命令查看README.md文件时，它会显示在Untracked files里。

要想让文件成为Git仓库的管理对象，就需要用git add命令将其加入暂存区（Stage或则Index）中。暂存区是提交之前的一个临时区域。

```bash
$ git add README.md
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   README.md
        
```

将README.md文件加入暂存区后，git status命令的显示结果发生了变化。可以看到，README.md文件显示在Changes to be committed中了。

### git commit——保存仓库的历史记录

git commit命令可以将当前暂存区中的文件实际保存到仓库的历史记录中。通过这些记录，我们就可以在工作树中复原文件。

#### 记述一行提交信息

我们来实际运行一下git commit命令。

```bash
$ git commit -m "First commit"
[master (root-commit) 4a479ef] First commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md
```

-m 参数后的“First commit“称为提交信息，是对这个提交的概述。

#### 记述详细提交信息

刚才我们只简洁地记述了一行提交信息，如果想要记述更加详细，请不加-m，直接执行git commit命令。执行后编辑器就会启动，并显示如下结果。

```bash
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
# Changes to be committed:
#       modified:   README.md
#
```

在编辑器中记述提交信息的格式如下。

* 第一行：用一行文字简述提交的更改内容。
* 第二行：空行。
* 第三行以后：记述更改的原因和详细内容。

只要按照上面的格式输入，今后便可以通过确认日志的命令或工具看到这些记录。

在以#（井号）标为注释的Changes to be committed（要提交的更改）栏中，可以查看本次提交中包含的文件。将提交信息按格式记述完毕后，请保存并关闭编辑器，以#（井号）标为注释的行不必删除。随后，刚才记述的提交信息就会被提交。

#### 中止提交

如果在编辑器启动后想中止提交，请将提交信息留空并直接关闭编辑器，随后提交就会被中止。

#### 查看提交后的状态

执行完git commit命令后再来看看当前状态。

```bash
$ git status
On branch master
nothing to commit, working tree clean
```

当前工作树处于刚刚完成提交的最新状态，所以结果显示没有更改。

### git log——查看提交日志

git log命令可以查看以往仓库中提交的日志。包括可以查看什么人在上面时候进行提交或合并，以及操作前后有怎样的差别。关于合并我们会在后面解说。

我们先来看看刚刚的git commit命令是否被记录了。

```bash
$ git log
commit dc2e2510e3a50fcb451360046c99d9b576b1a945 (HEAD -> master)
Author: shenhuanjie <shenhuanjie@live.cn>
Date:   Tue Nov 13 14:41:49 2018 +0800

    commit

commit 4a479effb41d58702126dce6cb3eedbe3c4ce55a
Author: shenhuanjie <shenhuanjie@live.cn>
Date:   Tue Nov 13 12:02:06 2018 +0800

    First commit

```

如上图所示，屏幕显示了刚刚的提交操作。commit栏旁边显示的“dc2e2510e3a50fcb……”是指向这个提交的哈希值。Git的其他命令中，在指向提交时会用到这个哈希值。

Author栏中显示我们给Git设置的用户名和邮箱地址。Date栏中显示提交执行的日期和时间。再往下就是该提交的提交信息。

#### 只显示提交信息的第一行

如果只想让程序显示第一行简述信息，可以在git log命令后加上--pretty=short。这样一来开发人员就能够更轻松地把握多个提交。

```bash
$ git log --pretty=short
commit dc2e2510e3a50fcb451360046c99d9b576b1a945 (HEAD -> master)
Author: shenhuanjie <shenhuanjie@live.cn>

    commit

commit 4a479effb41d58702126dce6cb3eedbe3c4ce55a
Author: shenhuanjie <shenhuanjie@live.cn>

    First commit
    
```

#### 只显示指定目录、文件的日志

只要在git log命令后加上目录名，便会只显示该目录下的日志。如果加的是文件名，就会只显示与该文件相关的日志。

```bash
$ git log README.md
```

#### 显示文件的改动

如果想查看提交所带来的改动，可以加上-p参数，文件的前后差别就会显示在提交信息之后。

```bash
$ git log -p
```

比如，执行下面的命令，就可以只查看README.md文件的提交日志以及提交前后的差别。

```bash
$ git log -p README.md
```

如上所述，git log命令可以利用多种参数帮助开发者把握以往提交的内容。不必勉强自己一次记下全部参数，每当有想查看的日志就积极去查，慢慢就能得心应手了。

### git diff——查看更改前后的差别

git diff命令可以查看工作树、暂存区、最新提交之间的差别。单从字面上可能很难理解，各位不妨跟着笔者的解说亲手试一试。

我们在刚刚提交的README.md中写点东西。

```xml
# Git教程
```

这里用Markdown语法写下了一行题目。

#### 查看工作树和暂存区的差别

执行git diff命令，查看当前工作树和暂存区的差别。

```bash
$ git diff
diff --git a/README.md b/README.md
index e9168db..567b605 100644
--- a/README.md
+++ b/README.md
@@ -1 +1 @@
-# Hello World
\ No newline at end of file
+# Git教程
\ No newline at end of file
```

由于我们尚未用git add命令向暂存区添加任何东西，所以程序只会显示工作树与最新提交状态之间的差别。

这里解释一下显示的内容。“+”号标出的是新添加的行，被删除的行则用“-”号标出。我们可以看到，这次只添加了一行。

用git add命令将README.md文件加入暂存区。

```bash
$ git add README.md
```

#### 查看工作树和最新提交的差别

如果现在执行git diff命令，由于工作树和暂存区的状态并无差别，结果什么都不会显示。要查看与最新提交的差别，请执行以下命令。

```bash
$ git diff HEAD
diff --git a/README.md b/README.md
index e9168db..ae5cf4d 100644
--- a/README.md
+++ b/README.md
@@ -1 +1 @@
-# Hello World
\ No newline at end of file
+# Git教程
```

不妨养成这样一个好习惯：在执行git commit命令之前先执行git diff HEAD命令，查看本次提交与上次提交之间有什么差别，等确认完毕后再进行提交。这里的HEAD是指向当前分支中最新一次提交的指针。

由于我们刚刚确认过两个提交之间的差别，所以直接运行git commit命令。

```bash
$ git commit -m "Add index"
[master 4ccb5ed] Add index
 1 file changed, 3 insertions(+), 1 deletion(-)
```

保险起见，我们查看一下提交日志，确认提交是否成功。

```bash
$ git log
commit 4ccb5edf8e815e94217e77c23caca07514d890f0 (HEAD -> master)
Author: shenhuanjie <shenhuanjie@live.cn>
Date:   Tue Nov 13 15:13:23 2018 +0800

    Add index

commit dc2e2510e3a50fcb451360046c99d9b576b1a945
Author: shenhuanjie <shenhuanjie@live.cn>
Date:   Tue Nov 13 14:41:49 2018 +0800

    commit

commit 4a479effb41d58702126dce6cb3eedbe3c4ce55a
Author: shenhuanjie <shenhuanjie@live.cn>
Date:   Tue Nov 13 12:02:06 2018 +0800

    First commit
```

成功查到了第二个提交。

## 4.2 分支的操作

在进行多个并行作业时，我们会用到分支。在这类并行开发的过程中，往往同时存在多个最新代码状态。如图4.1所示，从master分支创建feature-A分支和fix-B分支后，每个分支中都拥有自己的最新代码。master分支是Git默认创建的分支，因此基本上所有开发都是以这个分支为中心进行的。

不同分支中，可以同时进行完全不同的作业。等该分支的作业完成之后再与master分支合并。比如feature-A分支的作业结束后与master合并，如图4.2所示。

通过灵活运用分支，可以让多人同时高效地进行并行开发。在这里，我们将带大家学习与分支相关的Git操作。

### git branch——显示分支一览表

git branch命令可以将分支名列表显示，同时可以确认当前所在分支。让我们来实际运行git branch命令。

```bash
$ git branch
* master
```

可以看到master分支左侧有“*”（星号），表示这是我们当时所在的分支。也就是说，我们正在master分支下进行开发。结果中没有显示其他分支名，表示本地仓库中只存在master一个分支。

### git checkout -b——创建、切换分支

如果想以当前的master分支为基础创建新的分支，我们需要用到git checkout -b命令。

#### 切换到feature-A分支并进行提交

执行下面的命令，创建名为feature-A的分支。

```bash
$ git checkout -b feature-A
Switched to a new branch 'feature-A'
```

实际上，连续执行下面的两条命令也能收到同样效果。

```bash
$ git branch feature-A
$ git checkout feature-A
```

创建feature-A分支，并将当前分支切换为feature-A分支。这时再来查看分支列表，会显示我们处于feature-A分支下。

```bash
$ git branch
* feature-A
  master
```

feature-A分支左侧标有“*”，表示当前分支为feature-A。在这个状态下像正常开发那样修改代码、执行git add命令并进行提交的话，代码就会提交至feature-A分支。像这样不断对一个分支（例如feature-A）进行提交的操作，我们称为“培育分支”。

下面来实际操作一下。再README.md文件中添加一行。

```
# Git教程

	- feature-A
```

这里我们添加了feature-A这样一行字幕，然后进行提交。

```bash
$ git add README.md
$ git commit -m "Add feature-A"
[feature-A cc48086] Add feature-A
 1 file changed, 1 insertion(+), 1 deletion(-)
```

于是，这一行就添加到feature-A分支中了。

#### 切换到master分支

现在我们再来看一看master分支有没有受到影响。首先切换到master分支。

```bash
$ git checkout master
Switched to branch 'master'
```

然后查看README.md文件，会发现README.md文件仍然保持原先的状态，并没有被添加文字。feature-A分支的更改不会影响到master分支，这正是在开发中创建分支的优点。只要创建多个分支，可以在不互相影响的情况下同时进行多个功能的开发。

#### 切换回上一个分支

现在，我们再切换回feature-A分支。

```bash
$ git checkout -
Switched to branch 'feature-A'
```

像上面这样用“-”（连字符）代替分支名，就可以切换至上一个分支。当然，将“-”替换成feature-A同样可以切换到feature-A分支。

### 特性分支

Git与Subversion（SVN）等集中型版本管理系统不同，创建分支时不需要连接中央仓库，所以能够相对轻松地创建分支。因此，当今大部分工作流程中都用到了特性（Topic）分支。

特性分支顾名思义，是集中实现单一特性（主题），除此之外不进行任何作业的分支。在日常开发中，往往会创建数个特性分支，同时在此之外再保留一个随时可以发布软件的稳定分支。稳定分支的角色通常由master分支担当（图4.3）。

之前我们创建了feature-A分支，这一分支主要实现feature-A，除feature-A的实现之外进行任何作业。即便在开发过程中发现了BUG，也需要再创建新的分支，在新分支中进行修正。

基于特定主题的作业在特性分支中进行，主题完成后再与master分支合并。只要保持这样一个开发流程，就能保证master分支可以随时供人查看。这样一来，其他开发者也可以放心大胆地从master分支创建新的特性分支。

### 主干分支

主干分支是刚才我们讲解的特性分支的原点，同时也是合并的终点。通常人们会用master分支作为主干分支。主干分支并没有开发到一半的代码，可以随时供他人查看。

有时我们需要让这个主干分支总是配置在正式环境中，有时又需要用标签Tag等创建版本信息，同时管理多个版本发布。拥有多个版本时，主干分支也有多个。

### git merge——合并分支

接下来，我们假设feature-A已经实现完毕，想要将它合并到主干分支master中。首先切换到master分支。

```bash
$ git checkout master
Switched to branch 'master'
```

然后合并feature-A分支。为了在历史记录中明确记录下本次分支合并，我们需要创建合并提交。因此，在合并时加上 --no-ff参数。

```bash
$ git merge --no-ff feature-A
```

随后编辑器会启动，用于录入合并提交的信息。

```bash
Merge branch 'feature-A'

# Please enter a commit message to explain why this merge is necessary,
# especially if it merges an updated upstream into a topic branch.
#
# Lines starting with '#' will be ignored, and an empty message aborts
# the commit.
```

默认信息中已经包含了是从feature-A分支合并过来的相关内容，所以可不比做任何更改。将编辑器中显示的内容保存，关闭编辑器，就会看到下面的结果。

```bash
Merge made by the 'recursive' strategy.
 README.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

这样一来，feature-A分支的内容就合并到master分支中了。

### git log --graph——以图表形式查看分支

用git log --graph命令进行查看的话，能很清楚地看到特性分支（feature-A）提交的内容已被合并。除此以外，特性分支的创建以及合并也都清楚明了。

```bash
$ git log --graph
*   commit c9d4a7e15c88f71829537fd34fd485c48ca37806 (HEAD -> master)
|\  Merge: 4ccb5ed cc48086
| | Author: shenhuanjie <shenhuanjie@live.cn>
| | Date:   Tue Nov 13 16:19:38 2018 +0800
| |
| |     Merge branch 'feature-A'
| |
| * commit cc4808697802c95cbec76dccefbcbd7acd3fe53a (feature-A)
|/  Author: shenhuanjie <shenhuanjie@live.cn>
|   Date:   Tue Nov 13 15:43:23 2018 +0800
|
|       Add feature-A
|
* commit 4ccb5edf8e815e94217e77c23caca07514d890f0
| Author: shenhuanjie <shenhuanjie@live.cn>
| Date:   Tue Nov 13 15:13:23 2018 +0800
|
|     Add index
|
* commit dc2e2510e3a50fcb451360046c99d9b576b1a945
| Author: shenhuanjie <shenhuanjie@live.cn>
| Date:   Tue Nov 13 14:41:49 2018 +0800
|
|     commit
|
* commit 4a479effb41d58702126dce6cb3eedbe3c4ce55a
  Author: shenhuanjie <shenhuanjie@live.cn>
  Date:   Tue Nov 13 12:02:06 2018 +0800

      First commit

```

git log –graph命令可以用图表形式输出提交日志，非常直观，请大家务必记住。

## 4.3 更改提交的操作

### git reset——回溯历史版本

通过前面学习的操作，我们已经学会如何在实现功能后进行提交，累积提交日志作为历史记录，借此不断培养一款软件。

Git的另一特征便是可以灵活操作历史版本。借助分散仓库的优势，可以在不影响其他仓库的前提下对历史版本进行操作。

在这里，为了让各位熟悉对历史版本的操作，我们先回溯历史版本，创建一个名为fix-B的特性分支（图4.4）。

#### 回溯到创建feature-A分支前

让我们先回溯到上一节feature-A分支创建之前，创建一个名为fix-B的特性分支。

要让仓库的HEAD、暂存区、当前工作树回溯到指定状态，需要用到git rest --hard命令。只要提供目标时间点的哈希值，就可以完全恢复到该时间点的状态。事不宜迟，让我们执行下面的命令。

```bash
$ git reset --hard 4ccb5edf8e815e94217e77c23caca07514d890f0
HEAD is now at 4ccb5ed Add index
```

我们已经成功回溯到特性分支（feature-A）创建之前的状态。由于所有文件都回溯到了指定哈希值对应的时间点上，README.md文件的内容也恢复到了当时的状态。

#### 创建fix-B分支

现在我们来创建特性分支（fix-B）。

```bash
$ git checkout -b fix-B
Switched to a new branch 'fix-B'
```

作为这个主题的作业内容，我们在README.md文件中添加一行文字。

```bash
# Git教程

	- fix-B

```

然后直接提交README.md文件。

```bash
$ git add README.md

$ git commit -m "Fix B"
[fix-B 2940e74] Fix B
 1 file changed, 1 insertion(+), 1 deletion(-)
```

现在的状态如图4.5所示。接下来我们的目标是图4.6中所示的状态，即主干分支合并feature-A分支的修改后，又合并了fix-B的修改。

#### 推进至feature-A分支合并后的状态

首先恢复到feature-A分支合并后的状态。不妨称这一操作为“推进历史“。

git log命令只能查看以当前状态为重点的历史日志。所以这里要使用git reflog命令，查看当前仓库的操作日志。在日志中找出回溯历史之前的哈希值，通过git reset --hard命令恢复到回溯历史前的状态。

首先执行git reflog命令，查看当前仓库执行过的操作的日志。

```bash
$ git reflog
2940e74 (HEAD -> fix-B) HEAD@{0}: commit: Fix B
4ccb5ed (master) HEAD@{1}: checkout: moving from master to fix-B
4ccb5ed (master) HEAD@{2}: reset: moving to 4ccb5edf8e815e94217e77c23caca07514d890f0
4ccb5ed (master) HEAD@{3}: reset: moving to 4ccb5edf8e815e94217e77c23caca07514d8
c9d4a7e HEAD@{4}: merge feature-A: Merge made by the 'recursive' strategy.
4ccb5ed (master) HEAD@{5}: checkout: moving from feature-A to master
cc48086 (feature-A) HEAD@{6}: checkout: moving from master to feature-A
4ccb5ed (master) HEAD@{7}: checkout: moving from feature-A to master
cc48086 (feature-A) HEAD@{8}: commit: Add feature-A
4ccb5ed (master) HEAD@{9}: checkout: moving from master to feature-A
4ccb5ed (master) HEAD@{10}: commit: Add index
dc2e251 HEAD@{11}: commit: commit
4a479ef HEAD@{12}: commit (initial): First commit
```

在日志中，我们可以看到commit、checkout、reset、merge等Git命令的执行记录。只要不进行Git的GC（Garbage Collection，垃圾回收），就可以通过日志随意调取近期的历史状态，就像给时间机器指定一个时间点，在过去未来中自由穿梭一般。即便开发者错误自信了Git操作，基本也都可以利用git reflog命令恢复到原先的状态，所以请各位务必牢记本部分。

从上面数第四行表示feature-A特性分支合并后的状态，对应哈希值为c9d4a7e。我们将HEAD、暂存区、工作树恢复到这个时间点的状态。

```bash
$ git checkout master
Switched to branch 'master'
$ git reset --hard c9d4a7e
HEAD is now at c9d4a7e Merge branch 'feature-A'
```

之前我们使用git reset --hard命令回溯了历史，这里又再次通过它恢复到了回溯前的历史状态。当前的状态如图4.7所示。

### 消除冲突

现在只要合并fix-B分支，就可以得到我们想要的状态。让我们赶快进行合并操作。

```bash
$ git merge --no-ff fix-B
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

这时，系统告诉我们README.md文件发生了冲突（Conflict）。系统在合并README.md文件时，feature-A分支更改的部分与本次想要合并的fix-B分支更改的部分发生了冲突。

不解决冲突就无法完成合并，所以我们打开README.md文件，解决这个冲突。

#### 查看冲突部分并将其解决

用编辑器打开README.md文件，就会发现其内容变成了下面这个样子。

```
# Git教程

<<<<<<< HEAD
	- feature-A
=======
	- fix-B
>>>>>>> fix-B

```

====以上的部分是以前HEAD的内容，以下的部分是要合并的fix-B分支中的内容。我们在编辑器中将其改为想要的样子。

```
# Git教程

	- feature-A
	- fix-B
```

如上所示，本次修正让feature-A与fix-B的内容并存于文件之中。但是在实际的软件开发中，往往需要删除其中之一，所以各位在处理冲突时，务必要仔细分析冲突部分的内容后再行修改。

#### 提交解决后的结果

冲突解决后，执行git add命令与git commit命令。

```bash
$ git add README.md
$ git commit -m "Fix conflict"
[master 3099c46] Fix conflict
 1 file changed, 1 insertion(+)
```

由于本次更改解决了冲突，所以提交信息记为”Fix conflict“。

### git commit –amend——修改提交信息

要修改上一条提交信息，可以使用git commit --amend命令。

我们将上一条提交信息记为了“Fix conflict”，但它其实是fix-B分支的合并，解决合并时发生的冲突只是过程之一，这样标记是在不妥。于是，我们要修改这条提交信息。

```bash
$ git commit --amend
```

执行上面的命令后，编辑器就会启动。

```bash
Fix conflict

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Wed Nov 14 17:09:02 2018 +0800
#
# On branch master
# Changes to be committed:
#       modified:   README.md
#
```

编辑器中显示的内容如上所示，其中包含之前的提交信息。请将提交信息的部分修改为Merge branch ‘fix-B’，然后保存文件，关闭编辑器。

```baash
[master 304396f] Merge branch 'fix-B'
```

随后会显示上面这条结果。现在执行git log --graph命令，可以看到提交日志中的相应内容已经被修改。

```bash
$ git log --graph
*   commit 5d14ea672844c1b7bbb07eee4be386f5d36d8124 (HEAD -> master)
|\  Merge: c9d4a7e 2940e74
| | Author: shenhuanjie <shenhuanjie@live.cn>
| | Date:   Wed Nov 14 17:22:25 2018 +0800
| |
| |     Merge branch 'fix-B'
| |
| * commit 2940e74385fd651b4c765d0b9c9a4f46ba82f928 (fix-B)
| | Author: shenhuanjie <shenhuanjie@live.cn>
| | Date:   Tue Nov 13 17:16:30 2018 +0800
| |
| |     Fix B
| |
* |   commit c9d4a7e15c88f71829537fd34fd485c48ca37806
|\ \  Merge: 4ccb5ed cc48086
| |/  Author: shenhuanjie <shenhuanjie@live.cn>
|/|   Date:   Tue Nov 13 16:19:38 2018 +0800
| |
| |       Merge branch 'feature-A'
| |
| * commit cc4808697802c95cbec76dccefbcbd7acd3fe53a (feature-A)
|/  Author: shenhuanjie <shenhuanjie@live.cn>
|   Date:   Tue Nov 13 15:43:23 2018 +0800
|
|       Add feature-A
|
* commit 4ccb5edf8e815e94217e77c23caca07514d890f0
| Author: shenhuanjie <shenhuanjie@live.cn>
| Date:   Tue Nov 13 15:13:23 2018 +0800
|
|     Add index
|
* commit dc2e2510e3a50fcb451360046c99d9b576b1a945
| Author: shenhuanjie <shenhuanjie@live.cn>
| Date:   Tue Nov 13 14:41:49 2018 +0800
|
|     commit
|
* commit 4a479effb41d58702126dce6cb3eedbe3c4ce55a
  Author: shenhuanjie <shenhuanjie@live.cn>
  Date:   Tue Nov 13 12:02:06 2018 +0800

      First commit
```

### git rebase -i——压缩历史

在合并特性分支之前，如果发现已提交的内容中有些拼写错误等，不妨提交一个修改，然后将这个修改包含到前一个提交之中，压缩成一个历史记录。这是个会经常用到的技巧，让我们来实际操作体会一下。

#### 创建feature-C分支

首先，新建一个feature-C特性分支。

```bash
$ git checkout -b feature-C
switched to a new branch 'feature-C'
```

作为feature-C的功能实现，我们在README.md文件中添加一行文字，并且故意留下拼写错误，以便之后修正。

```bash
# Git教程

	- feature-A
	- fix-B
	- faeture-C
```

提交这部分内容。这个小小的变更就没必要先执行git add命令再执行git commit命令了，我们用git commit -am命令来一次完成这两步操作。

```bash
$ git commit -am "Add feature-C"
[feature-C 725131d] Add feature-C
 1 file changed, 1 insertion(+), 1 deletion(-)
```

#### 修正拼写错误

现在来修正刚才预留的拼写错误。请各位自行修正README.md文件的内容，修正后的差别如下所示。

```bash
$ git diff
diff --git a/README.md b/README.md
index ccff532..be244f2 100644
--- a/README.md
+++ b/README.md
@@ -2,4 +2,4 @@

        - feature-A
        - fix-B
-       - faeture-C
+       - feature-C
```

然后进行提交

```bash
$ git commit -am "Fix typo"
[feature-C fdee3b6] Fix typo
 1 file changed, 1 insertion(+), 1 deletion(-)
```

错字漏字等失误称作typo，所以我们将提交信息即为“Fix typo”。

实际上，我们比希望在历史记录中看到这类提交，因为健全的历史记录并不需要它们。如果能在最初提交之前就发现并修正这些错误，也就不会出现这类提交了。

#### 更改历史

因此，我们来更改历史。将“Fix typo”修正的内容与之前一次的提交合并，在历史记录中合并为一次完美的提交。为此，我们要用到git rebase命令。

#### 合并至master分支

feature-C分支的使命告一段落，我们将它与master分支合并。

```bash
$ git checkout master
Switched to branch 'master'

$ git merge --no-ff feature-C
Merge made by the 'recursive' strategy.
 README.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

```

master分支整合了feature-C分支。开发进展顺利。

```bash
$ git log --graph
*   commit 0c192808ab41b9aac0adb423a7a5142a38fce8cb (HEAD -> master)
|\  Merge: 5d14ea6 fdee3b6
| | Author: shenhuanjie <shenhuanjie@live.cn>
| | Date:   Wed Nov 14 17:42:40 2018 +0800
| |
| |     Merge branch 'feature-C'
| |
| * commit fdee3b61b555cee1ba3079c3116e22e6300dcc9a (feature-C)
| | Author: shenhuanjie <shenhuanjie@live.cn>
| | Date:   Wed Nov 14 17:35:34 2018 +0800
| |
| |     Fix typo
| |
| * commit 725131da20e7fb40b44bd37116169a4821508681
|/  Author: shenhuanjie <shenhuanjie@live.cn>
|   Date:   Wed Nov 14 17:33:42 2018 +0800
|
|       Add feature-C
|
*   commit 5d14ea672844c1b7bbb07eee4be386f5d36d8124
|\  Merge: c9d4a7e 2940e74
| | Author: shenhuanjie <shenhuanjie@live.cn>
| | Date:   Wed Nov 14 17:22:25 2018 +0800
| |
| |     Merge branch 'fix-B'
| |
| * commit 2940e74385fd651b4c765d0b9c9a4f46ba82f928 (fix-B)
| | Author: shenhuanjie <shenhuanjie@live.cn>
| | Date:   Tue Nov 13 17:16:30 2018 +0800
| |
| |     Fix B
| |
* |   commit c9d4a7e15c88f71829537fd34fd485c48ca37806
|\ \  Merge: 4ccb5ed cc48086
| |/  Author: shenhuanjie <shenhuanjie@live.cn>
|/|   Date:   Tue Nov 13 16:19:38 2018 +0800
| |
| |       Merge branch 'feature-A'
| |
| * commit cc4808697802c95cbec76dccefbcbd7acd3fe53a (feature-A)
|/  Author: shenhuanjie <shenhuanjie@live.cn>
|   Date:   Tue Nov 13 15:43:23 2018 +0800
|
|       Add feature-A
|
* commit 4ccb5edf8e815e94217e77c23caca07514d890f0
| Author: shenhuanjie <shenhuanjie@live.cn>
| Date:   Tue Nov 13 15:13:23 2018 +0800
|
|     Add index
|
* commit dc2e2510e3a50fcb451360046c99d9b576b1a945
| Author: shenhuanjie <shenhuanjie@live.cn>
| Date:   Tue Nov 13 14:41:49 2018 +0800
|
|     commit
|
* commit 4a479effb41d58702126dce6cb3eedbe3c4ce55a
  Author: shenhuanjie <shenhuanjie@live.cn>
  Date:   Tue Nov 13 12:02:06 2018 +0800

      First commit

```

## 4.4 推送至远程仓库

Git是分散型版本管理系统，但我们前面所学习的，都是针对单一本地仓库的操作。下面，我们将开始接触远在网络另一头的远程仓库。远程仓库顾名思义，是与我们本地仓库相对独立的另一个仓库。让我们先在GitHub上创建一个仓库，并将其设置为本地仓库的远程仓库。

请参考第3章的3.2节在GitHub上新建一个仓库。为防止与其他仓库混淆，仓库名请与本地仓库保持一致，即git-tutorial。创建时请不要勾选Initialize this repository with a README.md选项（图4.8）。因为一旦勾选该选项，GitHub一侧的仓库就会自动生成README文件，从创建之初便与本地仓库失去了整合性。虽然到时也可以强制覆盖，但为防止这一情况发生还是建议不要勾选该选项，直接点击Create repository创建仓库。

### git remote add——添加到远程仓库

在GitHub上创建的仓库路径为“git@github.com:shenhuanjie/git-tutorial.git”。现在我们用git remote add命令将它设置成本地仓库的远程仓库。

```bash
$ git remote add origin git@github.com:shenhuanjie/git-tutorial.git
```

按照上述格式执行git remote add命令之后，Git会自动将git@github.com:shenhuanjie/git-tutorial.git远程仓库的名称设置为origin（标识符）。

### git push——推送至远程仓库

#### 推送至master分支

如果想将当前分支下本地仓库中的内容推送给远程仓库，需要用到git push命令。现在假定我们在master分支下进行操作。

```bash
$ git push -u origin master
Enumerating objects: 26, done.
Counting objects: 100% (26/26), done.
Delta compression using up to 4 threads
Compressing objects: 100% (10/10), done.
Writing objects: 100% (26/26), 1.93 KiB | 123.00 KiB/s, done.
Total 26 (delta 4), reused 0 (delta 0)
remote: Resolving deltas: 100% (4/4), done.
remote:
remote: Create a pull request for 'master' on GitHub by visiting:
remote:      https://github.com/shenhuanjie/git-tutorial/pull/new/master
remote:
To github.com:shenhuanjie/git-tutorial.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

像这样执行git push命令，当前分支的内容就会被推送给远程仓库origin的master分支设置为本地仓库当前分支的upstream（上游）。添加了这个参数，将来运行git pull命令从远程仓库获取内容时，本地仓库的这个分支就可以直接从origin的master分支获取内容，省去了例外添加参数的麻烦。

执行该操作后，当前本地仓库master分支的内容将会被推送到GitHub的远程仓库中。在GitHub上也可以确认远程master分支的内容和本地master分支相同。

#### 推送至master以外的分支

除了master分支之外，远程仓库也可以创建其他分支。举个例子，我们在本地仓库中创建了feature-D分支，并将它同名形式push至远程仓库。

```bash
$ git checkout -b feature-D
Switched to a new branch 'feature-D'
```

我们在本地仓库中创建了feature-D分支，现在将它push给远程仓库并保持分支名称不变。

```bash
$ git push -u origin feature-D
Total 0 (delta 0), reused 0 (delta 0)
remote:
remote: Create a pull request for 'feature-D' on GitHub by visiting:
remote:      https://github.com/shenhuanjie/git-tutorial/pull/new/feature-D
remote:
To github.com:shenhuanjie/git-tutorial.git
 * [new branch]      feature-D -> feature-D
Branch 'feature-D' set up to track remote branch 'feature-D' from 'origin'.
```

现在，在远程仓库的GitHub页面就可以查看feature-D分支了。

## 4.5从远程仓库获取

上一节中我们把在GitHub上新建的仓库设置成了远程仓库，并向这个仓库push了feature-D分支。现在，所有能够访问这个远程仓库的人都可以获取feature-D分支并加以修改。

本节中我们从实际开发者的角度出发，在另一个目录下新建一个本地仓库，学习从远程仓库获取内容的相关操作。这就相当于我们刚刚执行过push操作的目标仓库又有了另一名新开发者来共同开发。

### git clone——获取远程仓库

#### 获取远程仓库

首先我们换到其他目录下，将GitHub上的仓库clone到本地。不要与之前操作的仓库在同一目录下。

```bash
$ git clone git@github.com:shenhuanjie/git-tutorial.git
Cloning into 'git-tutorial'...
remote: Enumerating objects: 26, done.
remote: Counting objects: 100% (26/26), done.
remote: Compressing objects: 100% (6/6), done.
Receiving objects: 100% (26/26), done.
Resolving deltas: 100% (4/4), done.
remote: Total 26 (delta 4), reused 26 (delta 4), pack-reused 0
```

执行git clone命令后我们会默认处于master分支下，同时系统会自动将origin设置成该远程仓库的标识符。也就是说，当前本地仓库的master分支与GitHub端远程仓库（origin）的master分支在内容上是完全相同的。

```bash
$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/feature-D
  remotes/origin/master
```

我们用git branch -a命令查看当前分支的相关信息。添加-a参数可以同时显示本地仓库和远程仓库的分支信息。

结果显示了remotes/origin/feature-D，证明我们的远程仓库中已经有了feature-D分支。

#### 获取远程的feature-D分支

我们试着将feature-D分支获取至本地仓库。

```bash
$ git checkout -b feature-D origin/feature-D
Switched to a new branch 'feature-D'
Branch 'feature-D' set up to track remote branch 'feature-D' from 'origin'.
```

-b 参数的后面是本地仓库中新建分支的名称。为了便于理解，我们仍将其命名为feature-D，让它与远程仓库的对应分支保持同名。新建分支名称后面是获取来源的分支名称。例子中指定了origin/feature-D，就是说以名为origin的仓库（这里指GitHub端的仓库）的feature-D分支为来源，在本地仓库中创建feature-D分支。

#### 向本地的feature-D分支提交更改

现在假定我们是另一名开发者，要做一个新的提交。在README.md文件中添加一行文字，查看更改。

```bash
$ git diff
diff --git a/README.md b/README.md
index be244f2..1886982 100644
--- a/README.md
+++ b/README.md
@@ -1,5 +1,6 @@
 # Git教程

-       - feature-A
-       - fix-B
-       - feature-C
+- feature-A
+- fix-B
+- feature-C
+- feature-D
```

按照之前学过的方式提交即可。

```bash
$ git commit -am "Add feature-D"
[master 51f18bf] Add feature-D
 1 file changed, 4 insertions(+), 3 deletions(-)
```

#### 推送feature-D分支

现在来推送feature-D分支。

```bash
$ git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 276 bytes | 92.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:shenhuanjie/git-tutorial.git
   0c19280..51f18bf  master -> master
```

从远程仓库获取feature-D分支，在本地仓库中提交更改，再将feature-D分支推送回远程仓库，通过这一系列操作，就可以与其他开发者相互合作，共同培育feature-D分支，实现某些功能。

### git pull——获取最新的远程仓库分支

现在我们放下刚刚操作的目录，回到原先的那个目录下。这边的本地仓库中只创建了feature-D分支，并没有在feature-D分支中进行任何提交。然而远程仓库的feature-D分支中已经有了我们刚刚推送的提交。这时我们就可以使用git pull命令，将本地的feature-D分支更新到最新状态。当前分支为feature-D分支。

```bash
$ git pull origin feature-D
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From github.com:shenhuanjie/git-tutorial
 * branch            feature-D  -> FETCH_HEAD
   0c19280..aec5544  feature-D  -> origin/feature-D
error: Your local changes to the following files would be overwritten by merge:
        README.md
Please commit your changes or stash them before you merge.
Aborting
Updating 0c19280..aec5544
```

GitHub端远程仓库中的feature-D分支是最新状态，所以本地仓库中的feature-D分支就得到了更新。今后只需要像平常一样在本地进行提交再push给远程仓库，就可以与其他开发者同时在同一个分支中进行作业，不断给feature-D增加新功能。

如果两人同时修改了同一部分的源代码，push时就很容易发生冲突。所以多名开发者在同一个分支中进行作业时，为减少冲突情况的发生，建议更频繁地进行push和pull操作。

## 4.7 小结

本章就理解本书所需的Git操作进行了讲解。只要掌握了本章的知识，就足以应付日常开发中的大部分操作了。

遇到不常见的特殊操作时，还请各位读者查阅本书介绍的参考资料，确保操作的正确性。