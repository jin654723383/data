# git常用命令及相关注意事项

## 一、注意事项

1. git是分布式系统，只有本地和远程的区别，commit和push是两个动作，commit只是提交到本地；
2. 禁止在master、develop、release分支上直接添加、修改内容，只能从其它分支合并，修改pom文件版本除外；
3. 所有开发工作都要基于分支进行，不同的开发内容放在不同分支上；
4. 主干分支：master、开发分支：develop、提测分支：release、hotfix提测分支：hotfix；
5. 其它分支命名规则：feature/v2.14/特性、bugfix/bug号，bugfix分支的的bug号可以省略PSSLCC，只用后面五个数字即可，如无bug号可根据所修改内容自己命名，分支名字（不含前缀）中除bug号外其余使用驼峰规则，可使用数字（0-9）、英文大小写（a-z A-Z）、英文句点（.）、短横线（-）、斜线（/）、下划线（_），避免使用中文；
6. 集成测试分支：dev-test，如项目需要把一些分支进行集成测试，可创建一个集成测试分支，把相关分支合并上去进行集成测试，请勿把此分支向任何其它分支合并；
7. 配置显示的用户名和邮箱，如有其它外网的git项目，建议每个项目分别配置（在当前项目目录执行并去掉global参数）：

```bash
    $ git config --global user.name zhangsan
    $ git config --global user.email zhangsan@travelsky.com
```

## 二、常用命令

1. 复制远程仓库

```bash
    $ git clone git://github.com/xxxx/xxxx.git
```

2. 拉取远程分支到本地并跟踪

```bash
    $ git checkout -b local_branch_name origin/branch_name
    $ local_branch_name和远程branch_name可以不同，但建议采用相同名称便于识别
```

3. 拉取当前分支远程代码

```bash
    $ git pull
```

4. 查看本地文件状态

```bash
    $ git status
```

5. 加入本地文件及修改内容

```bash
    $ git add --all, 只在初始化时仓库时使用，在日常开发中慎用此命令，使用之前必须使用git status查看文件状态
    $ git add xxx 或 git stage xxx
```

6. 切换分支

```bash
    $ git checkout branch_name
```

7. 创建本地分支

```bash
    $ git branch branch_name
```

8. 基于develop分支创建本地分支并切换

```bash
    $ git checkout -b branch_name develop
```

9. 把本地分支推送到远程

```bash
    $ 第一次推送：git push --set-upstream origin xxx
    $ 后续推送：git push
```

10. 合并分支

```bash
    $ git merge --no-ff branch_name -m "xxx"
    $ branch_name可以是orgin/xxx，表示合并远程的某一个分支到本地的当前分支，合并代码前必须review要合并的分支
```

11. 删除本地分支

```bash
    $ git branch -d branch_name
```

12. 删除远程分支

```bash
    $ git push origin --delete branch_name
```

13. 提交修改

```bash
    $ git commit –m ”xxx”
```

14. 在当前分支上打tag

```bash
    $ git tag -a v2.11 -m 'this is version 2.11'
```

15. 推送tag到远程

```bash
    $ git push origin v2.11
```

16. rebase某个分支代码（develop）到当前分支，常用于release分支

```bash
    $ git rebase develop
```

17. 放弃本地文件的修改
```bash
    $ git checkout -- filepathname (比如： git checkout -- readme.md  ，不要忘记中间的 “--” ，不写就成了检出分支了！！)
    $ 放弃所有的文件修改可以使用 git checkout -- .  命令
```

18. 放弃暂存区代码（已经执行过git add）

```bash
    $ git reset HEAD filepathname
    $ 比如： git reset HEAD readme.md）来放弃指定文件的缓存，放弃所以的缓存可以使用 git reset HEAD . 命令
```

19. 放弃本地仓库中已提交代码（已经执行过 git commit）

```bash
    $ 回退到上一次commit的状态可以使用 git reset --hard HEAD^
    $ 回退到任意版本：git reset --hard  commitid，可以使用 git log 命令来查看git的提交历史的提交id
```

20. 本地分支与远程分支关联

```bash
    $ git branch --set-upstream-to=origin/xxx
```

21. 查看本地分支详情

```bash
    $ git branch -vv
```

22. 查看当前远程仓库信息

```bash
    $ git remote -vv
```

23. 切换到指定tag的代码

```bash
    $ git checkout tag_name
    $ tag相当于是一个快照，不能更改它的代码，如果要在tag基础上做修改，需要基于tag建一个分支：
    $ git checkout -b branch_name tag_name
```

24. 查看提交日志

```bash
    $ git log –stat ：显示提交记录及相关修改文件
    $ git log --stat –p ：显示提交记录及相关修改文件详情
    $ git log --graph：图形化显示提交记录
    $ git show 4821d18581212d22f116616ce9ba1d149f2a0afd ：查看某一次提交详情
```

25. 查看一下文件中内容由哪次提交产生

```bash
    $ git blame filename
```

26. 查看两个分支中的提交差异

```bash
    $ git log develop ^master ：查看develop分支有，而master分支中没有的提交
    $ git log master..develop，功能近似
```

27. 分支差异比较

```bash
    $ git diff branch_nameA branch_nameB ： 比较两个分支的差异
    $ 可把diff替换为difftool，使用在配置中的外部比较工具显示差异
```

28. 文件差异比较

```bash
    $ git diff， 查看工作区与暂存区之间的差异
    $ git diff filename， 查看特定文件在工作区与暂存区之间的差异
    $ git diff HEAD -- filename，可以查看工作区和版本库里面最新版本的区别，不指定filename表示所有文件（--也省略）
    $ git diff HEAD^ -- filename，不指定filename表示所有文件（--也省略）
    $ git diff --cached filename，暂存区与HEAD比较，不指定filename表示所有文件
    $ git diff commit-id filename，比较工作区与指定的commit-id的差异，不指定filename表示所有文件
    $ git diff --cached commit-id filename，比较暂存区与指定commit-id的差异

    如filename在工作区改动后，还未执行add命令的时候，$ git diff filename 和 $ git diff HEAD -- filename 两个都可以查看当前工作区和最近一次提交的版本之间的差别。
    而工作区改动后并且执行add命令后但未提交，$ git diff filename将不能再进行比较。但可以继续用关键词HEAD、HEAD^ 等进行比较。
    而工作区改动后并且已提交，$ git diff filename 和 $ git diff HEAD -- filename ，都不会输出文件的改动信息，执行 $ git diff HEAD^ -- filename 则可以查看最近两次提交版本的区别。
    $ HEAD 表示当前版本，也就是最新的提交。上一个版本就是 HEAD^ ，上上一个版本就是 HEAD^^ ，往上100个版本写100个 “ ^ ” 比较容易数不过来，所以写成 HEAD~100 。HEAD~2 相当于 HEAD^^
```

![git diff](./git-diff.png)

29. 生成补丁

```bash
    $ git diff还可以制作补丁文件，在其他机器上对应目录下使用 git apply patch 将补丁打上即可
    $ git diff > patchFile，patch的命名是随意的，不加其他参数时作用是当我们希望将我们本仓库工作区的修改拷贝一份到其他机器上使用
    $ git diff --cached > patchFile，将我们暂存区与版本库的差异做成补丁
    $ git diff --HEAD > patchFile，将工作区与版本库的差异做成补丁
    $ git diff filename > patchFile，将单个文件做成一个单独的补丁
    $ git apply patchFile，应用补丁
    $ 应用补丁之前我们可以先检验一下补丁能否应用，git apply --check patchFile如果没有任何输出，那么表示可以顺利接受这个补丁
    $ 使用git apply --reject patchFile将能打的补丁先打上，有冲突的会生成.rej文件，此时可以找到这些文件进行手动打补丁
```

30. 从仓库中删除文件

```bash
    $ git rm -r folder_path --cached，删除目录
    $ git rm file_path --cached，删除文件

    如果有些文件或目录误添加进版本库，可从仓库中删除掉，上面两个命令都不会删除本地文件，只是从仓库中删掉，执行完命令后需要commit和push；
    无需进行版本管理的文件放入.gitignore文件（忽略规则参见七），修改完.gitignore之后再删除文件，如不修改gitignore可能后续还会误加进来；
```

31. 从其它分支合并单次提交内容到当前分支

```bash
    $ git cherry-pick [<options>] commit-id...
    常用options:
        --quit                退出当前的chery-pick序列
        --continue            继续当前的chery-pick序列
        --abort               取消当前的chery-pick序列，恢复当前分支
        -n, --no-commit       不自动提交
        -e, --edit            编辑提交信息

    commit-id是每次提交生成的id，commit-id参数可以有多个id，使用空格分开；
    git cherry-pick可以理解为”挑拣”提交，它会获取某一个分支的单次提交，并作为一个新的提交引入到你当前分支上。 当我们需要在本地合入其他分支的提交时，如果我们不想对整个分支进行合并，而是只想将某一次提交合入到本地当前分支上，那么就要使用git cherry-pick。
```

## 三、工作流程说明

1. 开发流程：所有开发工作都以develop分支为基础创建feature分支；
2. bugfix流程：所有bug修复都以develop（迭代周期内）、release（迭代结束到上线前）分支为基础创建bugfix分支；
3. hotfix流程：以master分支为基础创建hotfix-release分支，完成之后合并到develop和master分支；
4. 提测流程：SM先把需要提测内容合并进develop分支，然后merge进release分支或者在release分支rebase develop分支，在release分支修改版本号，20环境始终针对release分支；
5. 发布后（迭代开发结束，未上生产环境前）bug修复流程：不能rebase develop分支，以release分支为基础建立bugfix分支，修复后合并进develop和release分支，此时develop分支可以继续合并新特性，不影响上线；
6. 上线流程：上线完成后，把release分支合并进master分支，并在master分支打版本标签；

## 四、提测流程详细步骤

1. 切换到需提测的feature或bugfix分支并更新代码（如直接合并远程分支则可省略此步骤）

```bash
    $ git checkout branch_name
    $ git pull
```

2. 切换到develop分支并更新代码

```bash
    $ git checkout develop
    $ git pull
```

3. 合并feature或bugfix分支并推送

```bash
    $ git merge --no-ff branch_name -m 'xxxxxxx'
    $ git push
```
branch_name可以是origin/branch_name方式直接合并远程分支，远程分支可以无需拉取到本地

4. 清理无用的feature或bugfix分支（如是跨迭代开发分支无须执行此步骤）

```bash
    $ git branch -d branch_name
    $ git push origin --delete branch_name
```

5. 重复执行步骤1-4，确保所有分支处理完毕
6. 合并develop分支到release分支

```bash
    $ git checkout release
    $ git pull
    $ git merge --no-ff develop -m 'v2.15.0 commit'
```

6. 修改release分支pom版本号并推送

```bash
    $ 修改pom版本号
    $ git status
    $ git commit -am 'change pom v2.15.0'
    $ git push
```

## 五、上线流程详细步骤

1. 切换到release分支并更新代码

```bash
    $ git checkout release
    $ git pull
```

2. 切换到master分支并更新代码

```bash
    $ git checkout master
    $ git pull
```

3. 合并release进master分支并推送

```bash
    $ git merge --no-ff release -m 'xxxxxxx'
    $ git push
```

4. 在master分支上打版本标签

```bash
    $ git tag -a v2.14.1 -m 'version 2.14.1'
    $ git push origin v2.14.1
```

## 六、hotfix流程详细步骤

1. 切换到master分支并更新代码

```bash
    $ git checkout master
    $ git pull
```

2. 建立hotfix-release分支并推送

```bash
    $ git checkout -b hotfix-release
    $ git push --set-upstream origin hotfix-release
```

3. 上线完成后执行上线流程（参见四，把release替换为hotfix-release）
4. 把hotfix-release的代码合并进develop分支并推送

```bash
    $ git checkout develop
    $ git pull
    $ git merge --no-ff hotfix-release
    $ git push
```

5. 删除hotfix-release分支

```bash
    $ git branch -d hotfix-release
    $ git push origin --delete hotfix-release
```

## 七、忽略Git中不想提交的文件

有三种方法可以实现（第三种方式较为常用）：

1. 在git项目的设置中指定排除文件

这种方式只是临时指定该项目的行为，需要编辑当前项目下的 .git/info/exclude文件，然后将需要忽略提交的文件写入其中。需要注意的是，这种方式指定的忽略文件的根目录是项目根目录。

2. 定义git全局的 .gitignore 文件

除了可以在项目中定义 .gitignore 文件外，还可以设置全局的.gitignore文件来管理所有Git项目的行为。这种方式在不同的项目开发者之间是不共享的，是属于项目之上Git应用级别的行为。这种方式也需要创建相应的 .gitignore 文件，可以放在任意位置。然后在使用以下命令配置Git：

```bash
    $ git config --global core.excludesfile ~/.gitignore
```

3. 在git项目中定义.gitignore文件

对于经常使用Git的朋友来说，.gitignore配置一定不会陌生。这种方式通过在项目的某个文件夹下定义.gitignore文件，在该文件中定义相应的忽略规则，来管理当前文件夹下的文件的Git提交行为。.gitignore 文件是可以提交到公有仓库中，这就为该项目下的所有开发者都共享一套定义好的忽略规则。在.gitingore 文件中，遵循相应的语法，在每一行指定一个忽略规则。

```bash
    #              表示此为注释,将被Git忽略
    *.a             表示忽略所有 .a 结尾的文件
    !lib.a          表示但lib.a除外
    /TODO           表示仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
    build/          表示忽略 build/目录下的所有文件，过滤整个build文件夹；
    doc/*.txt       表示会忽略doc/notes.txt但不包括 doc/server/arch.txt

    bin/:           表示忽略当前路径下的bin文件夹，该文件夹下的所有内容都会被忽略，不忽略 bin 文件
    /bin:           表示忽略根目录下的bin文件
    /*.c:           表示忽略cat.c，不忽略 build/cat.c
    debug/*.obj:    表示忽略debug/io.obj，不忽略 debug/common/io.obj和tools/debug/io.obj
    **/foo:         表示忽略/foo,a/foo,a/b/foo等
    a/**/b:         表示忽略a/b, a/x/b,a/x/y/b等
    !/bin/run.sh    表示不忽略bin目录下的run.sh文件
    *.log:          表示忽略所有 .log 文件
    config.php:     表示忽略当前路径的 config.php 文件

    /mtk/           表示过滤整个文件夹
    *.zip           表示过滤所有.zip文件
    /mtk/do.c       表示过滤某个具体文件

    被过滤掉的文件就不会出现在git仓库中（gitlab或github）了，当然本地库中还有，只是push的时候不会上传。

    需要注意的是，gitignore还可以指定要将哪些文件添加到版本管理中，如下：
    !*.zip
    !/mtk/one.txt

    唯一的区别就是规则开头多了一个感叹号，Git会将满足这类规则的文件添加到版本管理中。为什么要有两种规则呢？
    想象一个场景：假如我们只需要管理/mtk/目录中的one.txt文件，这个目录中的其他文件都不需要管理，那么.gitignore规则应写为：：
    /mtk/*
    !/mtk/one.txt

    假设我们只有过滤规则，而没有添加规则，那么我们就需要把/mtk/目录下除了one.txt以外的所有文件都写出来！
    注意上面的/mtk/*不能写为/mtk/，否则父目录被前面的规则排除掉了，one.txt文件虽然加了!过滤规则，也不会生效！

    还有一些规则如下：
    fd1/*
    说明：忽略目录 fd1 下的全部内容；注意，不管是根目录下的 /fd1/ 目录，还是某个子目录 /child/fd1/ 目录，都会被忽略；

    /fd1/*
    说明：忽略根目录下的 /fd1/ 目录的全部内容；
```

## 八、FAQ

1. 拉取远程分支与本地创建分支的区别

- 拉取远程分支方法：git checkout -b local_branch_name origin/branch_name，
- 本地创建分支：git checkout -b branch_name develop
- 本地创建的分支推送到远程：git push --set-upstream origin branch_name

2. 当前分支有未提交文件切换的问题

- 在老版本的git上，当前工作区有未提交文件时切换分支会提示不能切换，在目前git版本上处理方式有所调整，未提交文件会直接带到新的分支上，并且可以在新的分支上直接提交这些修改，这个特性有利有弊，好处是在修改了文件之后发现分支不对需要切换时比较方便，弊端是如果不希望在新分支上提交这些内容时会容易误操作，因此建议在切换分支之前执行 git status 查看工作区状态，在切换分支时保持工作区干净是一个良好的工作习惯。

3. 推送分支到远程时提示：更新被拒绝，因为远程版本库包含您本地尚不存在的提交

- 原因是远程分支比本地分支更新一些，解决方式是在当前分支上执行git pull更新一下本地分支代码，此时如本地分支已经执行过和其它分支的合并操作，合并内容会保留，如果从远程拉取分支时有些文件会与本地修改的文件合并时会打开一个vi的输入窗口，提示输入合并的备注信息，如无须输入可使用先按ESC键、然后输入冒号（:）、然后输入q退出即可。

4. 工作区、暂存区、本地仓库的关系

![git](./git-repo.png)
