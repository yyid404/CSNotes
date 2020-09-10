## Git

### 版本控制

​		版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。 除了项目源代码，可以对任何类型的文件进行版本控制。

​		版本控制使得我们可以将某个文件回溯到之前的状态，甚至将整个项目都回退到过去某个时间点的状态，可以比较文件的变化细节，查出最后是谁修改了哪个地方，从而找出导致怪异问题出现的原因，又是谁在何时报告了某个功能缺陷等等。

### 版本控制系统

#### 集中化的版本控制系统

​		Centralized Version Control Systems，简称 CVCS。有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。使得在不同系统上的开发者协同工作。但存在下面的问题：

- 单点故障： 中央服务器宕机，则其他人无法使用；如果中心数据库磁盘损坏有没有进行备份，你将丢失所有数据。
- 必须联网才能工作： 受网络状况、带宽影响。

#### 分布式版本控制系统

​		Distributed Version Control System，简称 DVCS。客户端并不只提取最新版本的文件快照，而是把代码仓库完整地镜像下来。 这么一来，任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。 因为每一次的克隆操作，实际上都是一次对代码仓库的完整备份。

​		分布式版本控制系统可以不用联网就可以工作，因为每个人的电脑上都是完整的版本库，当你修改了某个文件后，你只需要将自己的修改推送给别人就可以了。但是，在实际使用分布式版本控制系统的时候，很少会直接进行推送修改，而是使用一台充当“中央服务器”的东西。这个服务器的作用仅仅是用来方便“交换”大家的修改，没有它大家也一样干活，只是交换修改不方便而已。

​		Git 就是一个典型的分布式版本控制系统，还具有极其强大的分支管理等功能。

### Git

​		对待数据的方式上，大部分版本控制系统（CVS、Subversion、Perforce、Bazaar 等等）都是以文件变更列表的方式存储信息，这类系统将它们保存的信息看作是一组基本文件和每个文件随时间逐步累积的差异。

​		Git采用的是直接记录快照的方式，而非差异比较。更像是把数据看作是对小型文件系统的一组快照。 每次你提交更新，或在 Git 中保存项目状态时，它主要对当时的全部文件制作一个快照并保存这个快照的索引。 为了高效，如果文件没有修改，Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件。 Git 对待数据更像是一个快照流。

### 状态

已提交（committed）：数据已经安全的保存在本地数据库中。

已修改（modified）：已修改表示修改了文件，但还没保存到数据库中。

已暂存（staged）：表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。

### 工作区

Git 仓库（.git directoty）

工作目录（Working Directory）

暂存区域（Staging Area）

### 工作流程

1. 在工作目录中修改文件。
2. 暂存文件，将文件的快照放入暂存区域。
3. 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录。

### 分支

​		分支用来将特性开发绝缘开来。创建仓库时，*master* 是默认的分支。通常在其他分支上进行开发，完成后再将它们合并到主分支上。我们通常在开发新功能、修复一个紧急 bug 等等时候会选择创建分支。单分支开发好还是多分支开发好，还是要看具体场景来说。

### .gitignore

**touch .gitignore**：创建.gitignore文件，.gitignore中的文件将不会被git管理，称为忽略文件。当想忽略的文件在远程仓库中已被管理，需先在本地仓库中删除该文件，在.gitignore中添加该文件名，再add、commit、push到远程仓库中。

### FETCH_HEAD

​		指定某个branch在服务器上最新状态。

> 我们切到 dev分支 上，git fetch一下，然后看看FETCH_HEAD内容：
>
> ```shell
> $cat .git/FETCH_HEAD
> //版本号
> 01e8809a7861a55f7a403981f2f1bcd68603e33a branch 'dev' of https://github.com/Moonergfp/learngit
> //当前FETCH_HEAD是否将要合并
> ff47932aba92e0eaec6c75ce8112d2f24d890dab not-for-merge
> branch 'develop' of https://github.com/Moonergfp/learngit
> //git版本库路径
> ff47932aba92e0eaec6c75ce8112d2f24d890dab not-for-merge
> branch 'master' of https://github.com/Moonergfp/learngit
> ```
>
> 如上3行中，dev就是要默认指定merge的分支。直接git merge就可以把origin/dev分支merge到dev分支上。

### 命令

**git init [--bare仓库名]**：以当前文件夹或指定文件夹创建指定名字的本地仓库。该命令将创建一个名为 `.git` 的子目录。

**git clone 远程仓库地址**：从远程仓库中clone一份代码到当前本地仓库。clone时候当前目录必须为空。

**git status**：记录工作区和暂存区之间发生的变化。检测当前文件状态。

**git log**：记录commit历史信息，会按提交时间列出所有的更新，最近的更新排在最上面。

**git log --author=作者名**：只看某个人的提交记录。git log 可添加一些参数来查看自己希望看到的内容。

**git add [./文件名/通配符]**：将工作区中的全部/指定文件的变化提交到暂存区。

**git commit [-m "版本号"]**：将暂存区中的变化提交到本地仓库中，可生成版本号等相关信息。每次准备提交前，先用 `git status` 看下，是不是都已暂存起来了， 然后再运行提交命令 `git commit`。版本号应该尽量的清晰和简洁，方便相关的 Git 日志查看工具显示和其他人的阅读，如描述和解释你的这次提交，加入更多的细节来解释提交，如加入一些相关的背景或者解释这个提交能修复和解决什么问题。

**git commit -a -m "版本号"**：加上 -a 选项，Git 会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 `git add` 步骤，跳过使用暂存区域更新。

**git commit --amend**：有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 加上 `--amend` 尝试重新提交。

**git push**：将本地仓库的变化push更新到远程仓库。

**git push 分支名**：将分支推送到远端仓库，推送成功后其他人可见。

**git pull**：拉取远程仓库中最新版的代码

**git checkout 文件名**：撤销工作区文件的变化，不可逆。

**git reset [--hard 版本号]**：本地仓库的版本回退

**git reset filename**：取消暂存的文件

**git fetch**：从远程分支拉取代码，fetch常结合merge一起用，git fetch + git merge == git pull。git pull会将代码直接合并，造成冲突等无法知道，fetch代码下来要git diff orgin/xx来看一下差异然后再合并。git fetch 会拉取当前项目的所有分支的commit。

**git fetch origin**：手动指定要fetch的remote。

> 假如你想丢弃你在本地的所有改动与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它：
>
> ```shell
> git fetch origin
> git reset --hard origin/master
> ```

**git fetch origin branch1**：设定当前分支的 FETCH_HEAD 为远程服务器的branch1分支。该命令不会在本地创建本地远程分支。该命令可以用来测试远程主机的远程分支branch1是否存在，如果存在，返回0，如果不存在，返回128，抛出一个异常。

**git fetch origin branch1:branch2**：

1. 首先执行上面的fetch操作。
2. 使用远程branch1分支在本地创建branch2，但不会切换到该分支。
3. 如果本地不存在branch2分支，则会自动创建一个新的branch2分支。
4. 如果本地存在branch2分支，并且是fast forward，则自动合并两个分支，否则会阻止以上操作。

**git fetch origin :branch2**：等价于 `git fetch origin master:branch2`

**git branch 分支名**：创建一个指定名称的分支

**git branch -a**：查看全部分支

**git branch -d 分支名**：把新建的指定分支删掉

**git checkout [-b] 分支名**：本地仓库创建/切换分支，添加-b可在本地仓库创建远程仓库中没有的分支，并切换到该分支，但push时需在远程仓库中创建该分支。当你切换分支的时候，Git 会重置你的工作目录，使其看起来像回到了你在那个分支上最后一次提交的样子。 Git 会自动添加、删除、修改文件以确保此时你的工作目录和这个分支最后一次提交时的样子一模一样。

**git merge 源分支**：把指定分支的内容合并到当前分支。

**git rm filename**：从暂存区域移除文件，然后提交。

**git mv 原文件名 新文件名**：对文件重命名。

> 例如：`git mv README.md README`这个命令相当于`mv README.md README`、`git rm README.md`、`git add README` 这三条命令的集合。

**git remote rename 原远程仓库名 新远程仓库名**：重命名远程仓库。

**git remote rm 远程仓库名**：移除远程仓库。

### 关联远程仓库

1. 创建本地仓库（以当前文件夹）

    ```shell
    git init
    ```

2. 关联远程仓库

    ```shell
    git remote add origin 仓库的的SSH
    ```

3. push前先将远程repository的修改pull下来，因为我们是init的，不是clone下来的。将这些改动提交到远端仓库，可以把 *master* 换成你想要推送的任何分支。

    ```shell
    git pull origin master --allow-unrelated-histories
    ```

    不加`--allow-unrelated-histories`会报`fatal: refusing to merge unrelated histories`

4. 提交到分支

    - 将本地master分支上的内容推送到远程master分支：

        ```shell
        git push -u origin master
        ```

        -u参数可以在推送的同时，将origin 仓库的master 分支设置为本地仓库当前分支的upstream（上游）。添加了这个参数，将来运行git pull命令从远程仓库获取内容时，本地仓库的这个分支就可以直接从origin 的master 分支获取内容，省去了另外添加参数的麻烦。这个参数也只用在第一次push时加上，以后直接运行git push命令即可。

    - 提交到其他分支

        ```shell
        git checkout mybranch
        git push -u origin mybranch
        ```

        先切换到mybranch分支，然后执行git push命令，参数含义和之前的一样，这里我们创建的远程仓库的分支名也为mybranch（当然也可以取任何名字，但是为了不混淆，最好取一致的名字）。这两条命令执行成功之后，此时在网页中我们就可以看到已经有多个分支了。
