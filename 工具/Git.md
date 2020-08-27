# 命令

**git init [--bare仓库名]**：以当前文件夹或指定文件夹创建指定名字的本地仓库。

**git clone 远程仓库地址**：从远程仓库中clone一份代码到当前本地仓库。clone时候当前目录必须为空。

**git status**：记录工作区和暂存区之间发生的变化。

**git log**：记录commit信息

**git add [./文件名/通配符]**：将工作区中的全部/指定变化提交到暂存区。

**git commit [-m 版本号]**：将暂存区中的变化提交到本地仓库中，可生成版本号等相关信息。

**git push**：将本地仓库的变化push更新到远程仓库。

**git pull**：拉取远程仓库中最新版的代码

**git checkout 文件名**：撤销工作区的变化，不可逆。

**git reset [--hard 版本号]**：本地仓库的版本回退

**git rm 文件名**：删除指定文件

**git branch -a**：查看全部分支

**git checkout [-b] 分支名**：本地仓库创建/切换分支，添加-b可在本地仓库创建远程仓库中没有的分支，并切换到该分支，但push时需在远程仓库中创建该分支。

**git merge 源分支**：把指定分支的内容合并到当前分支。

# .gitignore

**touch .gitignore**：创建.gitignore文件，.gitignore中的文件将不会被git管理。当想忽略的文件在远程仓库中已被管理，需先在本地仓库中删除该文件，在.gitignore中添加该文件名，再add、commit、push到远程仓库中。

# 关联远程仓库

1. 创建本地仓库（以当前文件夹）

    ```
    git init
    ```

2. 关联远程仓库

    ```
    git remote add origin 仓库的的SSH
    ```

3. push前先将远程repository的修改pull下来，因为我们是init的，不是clone下来的。

    ```
    git pull origin master --allow-unrelated-histories
    ```

    不加`--allow-unrelated-histories`会报`fatal: refusing to merge unrelated histories`

4. 提交到分支

    - 将本地master分支上的内容推送到远程master分支：

        ```
        git push -u origin master
        ```

        -u参数可以在推送的同时，将origin 仓库的master 分支设置为本地仓库当前分支的upstream（上游）。添加了这个参数，将来运行git pull命令从远程仓库获取内容时，本地仓库的这个分支就可以直接从origin 的master 分支获取内容，省去了另外添加参数的麻烦。这个参数也只用在第一次push时加上，以后直接运行git push命令即可。

    - 提交到其他分支

        ```
        git checkout mybranch
        git push -u origin mybranch
        ```

        先切换到mybranch分支，然后执行git push命令，参数含义和之前的一样，这里我们创建的远程仓库的分支名也为mybranch（当然也可以取任何名字，但是为了不混淆，最好取一致的名字）。这两条命令执行成功之后，此时在网页中我们就可以看到已经有多个分支了。
