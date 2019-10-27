

# 一 安装
    安装完成后 运行 git->git bash
    设置用户名与邮箱
      $ git config --global user.name "Your Name"
    $ git config --global user.email email@example.com
    设置字体风格
    $ git config --global color.ui true
    概念
    A：工作区
    B：版本库  隐藏的.git目录
    a：暂存区(stage/index)  
    b：分支（master）
# 二 常用命令
    git init 在该文件下创建一个工作区
    git add one.txt 将one.txt 添加到缓存区 添加多个文件时 git add .
    git commit -m "log" 将修改的文件提交到版本库，并注明日志
    git status 查看仓库当前的状态
    git log 查看历史记录、日志
    git log --pretty=oneline 查看历史记录、日志，只显示一行
    git reset --hard HEAD^ 回退到上一个版本 有几个^ 回退几个版本
    git reset --hard HEAD~100 回退100个版本
    git reset --hard commit_id 回退到指定版本
    git reflog 重返未来，查看回退记录
    git checkout -- one.txt 撤销未提交的文件修改 1、 未被添加到缓存区的文件修改，还原到版本库状态  在工作区删除文件，属于此类。 2、 在暂存区未被commit的文件修改，还原到暂存区状态
    git reset HEAD one.txt 将在暂存区修改的文件，回退到工作区，但保留在暂存区的修改

---
    删除文件
    在工作区中删除
    删除：git re test.txt    git commit -m “delete file”
    还原：git checkout – test.txt
    命令删除
    git rm test.txt    git commit -m “delete test.txt”
    远程仓库
    //删除远程分支origin
    git remote rm origin  (出现fatal: remote origin already exists时，用此命令) .
    //创建一个远程分支
    git remote add origin https://github.com/YLJ76/git.git
    更新远程仓库（提交到远程仓库）
    git push
    git push -u origin master  //提交到指定分支
    更新本地仓库
    git pull
    分支
    创建并切换到dev分支 
                    git checkout -b dev
                    等价于 git branch dev
                        Git checkout dev
    查看当前分支
    git branch
    切换分支
    git checkout master  //切换到master
    合并分支
    git merge dev  //将dev分支中的改变合并到当前分支
    合并时，会有一个commit
    默认是使用Fast forward方式合并
    这种模式下，合并后删除分支，就会丢掉分支信息
            git merge --no-ff -m "merge with no-ff" dev
            --on-ff使用非fast forward合并方式
            -m "merge with no-ff" commit的备注
    删除分支
    git branch -d dev  //删除分支dev
    冲突
                当当前分支与想要合并的分支都有修改数据时，当前分支合并分支就会出现错误。执行合并操作时，就会将文件数据修改为两个分支的变化，手动将文件修改，后提交数据，删除分支。
    Bug分支
    当前分支没有提交，需要先去修改bug。
    先将当前的工作储藏起来。git stach
    新建bug分支，在该分支上修改bug，
    然后切换到原来的分支上，合并，删除bug分支
    恢复分支
         A:git stash apply恢复分支，然后git stash drop 删除存储记录
         B:git stash pop 恢复分支，删除出存储记录。
            git stash list 存储列表
# Git常用命令

    git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

    git init	初始化一个Git仓库

    git add <file>
    git commit	添加文件到Git仓库

    git status	掌握工作区的状态
    git diff <file>		查看修改内容

    git log		显示从最近到最远的提交日志
        --pretty=oneline

    git reset --hard commit_id	回滚到某个版本
    git log						查看提交历史，确定回滚版本号
    git reflog					要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

    git checkout -- file		可以丢弃工作区的修改,让这个文件回到最近一次git commit或git add时的状态
    git reset HEAD file			可以把暂存区的修改撤销掉（unstage），重新放回工作区
    场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
    场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。
    场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

    git rm file					从版本库中删除文件

    git remote add origin git@github.com:michaelliao/learngit.git	把上面的michaelliao替换成你自己的GitHub账户名.关联某个github账户
    git push -u origin master										初次讲本地库Push到github多一个-u
    git push origin master	

    git clone git@github.com:michaelliao/gitskills.git				 克隆远程仓库

    git checkout -b dev					创建dev分支，然后切换到dev分支.相当于以下两条命令:$git branch dev	$git checkout dev
    git branch							查看当前分支
    git merge <分支>					用于合并指定分支到当前分支
    git branch <name>					创建分支
    git checkout <name>					切换分支
    git branch -d <name>				删除分支

    git log --graph						可以看到分支合并图
    git log --graph --pretty=oneline --abbrev-commit

    git merge --no-ff -m "merge with no-ff" dev		--no-ff参数，表示禁用Fast forward,合并后的历史有分支，能看出来曾经做过合并

    ----------------bug分支-----
    git stash	暂时保存未完成的分支现场
    git stash pop	修改Bug后回到现场

    git branch -D <name>	如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

----
# 多人协作的工作模式通常是这样：

    首先，可以试图用git push origin branch-name推送自己的修改；

    如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

    如果合并有冲突，则解决冲突，并在本地提交；

    没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

    如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。

    git remote -v	查看远程库信息
    在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；


git  查看 版本号及提交记录
git log --pretty=oneline 

git  回退到某个版本
git reset --hard HEAD~1


git rebase
把一个分支中的修改整合到另一个分支的办法有两种： merge 和 rebase 。 rebase 可以把在一个分支里提交的改变移到另一个分支里重放一遍。
rebase 的操作不当，可能导致原分支被修改等后果。作为一个危险操作，需要慎重。
变基操作解决冲突后应执行 git add . 和 git rebase --continue ，而不是 git commit 命令
A分支切出来一个B分支，当前在B分支，已经做了两个提交，切回A分支也做了两个提交，再切回B分支
# B分支合并A分支的提交记录
git rebase A
# 处理完冲突继续合并
git add .
git rebase –continue
# 跳过
git rebase –skip
# 取消合并
git rebase –abort
# 最后再切回A分支，A merge B，这样A中的提交记录就是整洁的了（没有merge记录）

我们的话就是先自己分支开发好，commit后，切回master，fetch，然后pull新的master下来。切回自己分支，git rebase master（把master的commit转移到自己分支），然后提交自己的分支上去，然后在仓库合并到master

git commit
git fetch
git rebase origin/master
git push







    