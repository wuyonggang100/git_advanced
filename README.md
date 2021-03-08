# git_advanced

git 进阶
参考 Git 工作流程最佳实践--git flow https://www.jianshu.com/p/433190786ea8

1.  ## git flow 走一遍

    1. #### 立项的 master 分支，也即最后的生产分支。
       > 只用于打 tag 和发布上线,只能通过 Pull Request 从其他分支进行合并，而不能直接 push 修改；
       ```sh
       git clone xxxx.git  # 拉取master 到本地
       git checkout --track origin/develop # 如果有远程 develop 分支还需要拉取 develop ；
       ```
    2. #### 贯穿开发的 develop 分支。
       > 所有开发都在此分支上进行，此分支将长期存在,不需要合并回 master 分支，这个分支的代码会进行每日的代码持续集成用来测试;
    3. #### 开发新功能的 feature 分支。
       > 当有一个新功能需要开发时，就从 develop 分支上检出一个 feature 分支，此分支只存在于开发者本地，不能被提交到远程仓库；开发完成后，需要合并回 develop 分支或者废弃；

    ```sh
    git checkout -b myfeature develop   # 从develop 分支上检出一个 feature 分支进行开发
    git checkout develop  # 开发完成后切回到 develop 分支上
    git merge --no-ff myfeature # 将 feature 分支合并回 develop 分支
    git branch -d myfeature  # 此时feature 分支已经完成使命, 删除 feature 分支, -D 是强制删除；
    git push origin develop  # 将 develop 分支推送到远程；
    ```

    4. #### 用来发布的 release 分支。
       > 当 develop 分支上的代码已经做好反而不准备时(通过了测试)，就可以从 develop 分支检出一个 release 分支用来准备发布，在这个分支上的代码允许做小的缺陷修正、准备发布版本所需的各项说明信息（版本号、编译时间等等）,这个 release 分支可能会存在一段时间，直到发布可能被推出。 在此期间，可以在此分支做一些小的错误修复（而不是开发分支）, 而不能添加大的新功能。release 分支可以让 develop 分支空闲出来以接受新的 feature 分支上的代码提交。

    ```sh
    git checkout -b release-1.2 develop # 从 develop 分支上检出 release 分支；
    git commit -a -m "Bumped version number to 1.2" # 标上版本说明等信息
    ```

    5. #### 准备发布。
       > 当准备好正式上架或者发布到生产环境时，release 分支合并到 master 分支和 develop 分支，并且在 master 分支上添加 tag 版本号，最后删除 release 分支。
       - ##### 将 release 合并回 master 分支, 并给 master 打上 tag;
       ```sh
       git checkout master  # 切回到 master 分支
       git merge --no-ff release-1.2 # 合并回 master
       git tag -a 1.2
       ```
       - ##### 将 release 合并回 develop 分支, 如有冲突,解决完冲突后提交;无冲突直接删除 release 分支;
       ```sh
       git checkout develop
       git merge --no-ff release-1.2 # 合并回 develop
       git branch -d release-1.2 # 删除release
       ```
    6. #### 修复线上 bug 的 hotfix 分支。

       > 发布后，当线上发现了 bug ,需要修复时，就要从 master 检出 hotfix 分支来修复, 此分支最后会合并回 develop 和 master 分支，然后删除。此时和 release 分支完全相似。如果此时存在 release 分支时，就需要将 hotfix 分支合并到 release 分支，而不是 develop 分支,最后会随着 ralease 被合并到 develop, （如果在 develop 分支的工作立即需要修正这个错误，而不能等到 release 分支完成，也可以将后 hotfix 分支合并到 develop 分支。）

       ```sh
        git checkout -b hotfix-1.2.1 master # 从 master 分支上检出
        # 修复操作
        git commit -a -m "Bumped version number to 1.2.1" # 修复后提交
        git checkout master
        git merge --no-ff hotfix-1.2.1 #合并回 master
        git tag -a 1.2.1  # 给 master 打上 tag
        git checkout develop
        git merge --no-ff hotfix-1.2.1 # 合并回 develop 分支，可能有冲突，需要提交
        git branch -d hotfix-1.2.1 # 删除 hotfix
       ```

2.  ## git flow 命名规范:
    -
3.  ## sourceTree 中的变基（rebase）
4.  ## sourceTree 中的遴选（xxx）

```

```
