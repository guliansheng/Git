#Git基本用法

##1 什么是Git

####1.1 Git和SVN的区别
- svn是集中式的,需要有一台中央服务器

- Git是分布式的

- Git速度比svn快

- svn中的每个文件夹都有一个文件 .svn文件,git有一个单独的文件 .git文件夹

##2 Git命令行操作
####2.1 基本命令
- dir : 查看当前目录详情

- echo a > a.txt : 将a写入到a.txt中 , 这种方法会将文件中的所有内容替换成a

- echo aa >> a.txt : 将aa写到a.txt的末尾中

- type a.txt : 查看文件内容


####2.2 git命令
- git init : 初始化文件夹 , 将文件夹设置成git的工厂

- git status / git status -s : 查看当前文件状态

- git add a.txt : 将a.txt文件讲给git管理

- git add . : 将目录中的所有文件讲给git管理

- git commit -m "初始化版本" : 提交文件 

- git checkout [文件名] : 将修改的文件还原成最新版本中的状态, 如果文件已经提交到了暂存区就无法还原 

- git log : 查看文件所有版本信息

- git reflog : 得到操作的版本id信息 

- git reset --hard [版本号的前几位] : 将文件恢复到对应的版本

- git log --pretty=oneline : 在一行显示版本信息

- git reset HEAD [文件名] : 用HEAD(最新版本中的文件)替换该文件,这个文件可能已经提交到了暂存区,此时需要用该方法还原

- git rm [文件名] : 将文件删除 , 同时将暂存区中的文件也删除

- git rm --cached [文件名] : 删除暂存区中的文件

####git状态
- 工作区 : 执行git init的文件夹

- 暂存区 : 修改的文件存放的地方

- 工厂 : 存储版本的地方

- 四种状态 : 

    + untracked : 未跟踪状态

    + unmodified : 未修改
    
    + modified : 修改
    
    + staged : 阶段性

####git 组件
- blob组件 : 当执行git add . 时, git会在object文件夹中创建一个blob组件文件夹, 只是将文件内容存储起来, 和文件没有关系
    
    - git hash-object a.txt : 查看blob组件的hash值

- commit组件 : 每一次提交就会生成一个commit组件
    
    - git cat-file -p [commit组件的hash值] : 可以查看相应的tree组件hash

- tree 组件 : 存储和文件相关的属性, 包括文件名, 修改时间等信息
    
    - git cat-file -p [tree组件的hash值] : 查看文件相关信息

####git分支
- 分支命令:
    + git branch : 查看git分支

    + git branch b1 [基于什么创建的分支(master)]: 创建分支b1 (在master上创建的分支)

    + git checkout b1/master : 切换到b1分支/主干

    + git checkout -b b1 : 创建分支b1,并且切换到b1分支

    + git merge master : 将主干的内容合并到其他分支中

    + git merge b1 : 将b1分支合并到master主干

    + git branch -d b1 : 合并到主干之后要将分支删除

- 分支注意: 
    + 版本信息可能出现多个父级版本, 合并时会出现这种情况

    + 多级分支: 在分支上再创建子分支, 要先将子分支合并到分支, 再将分支合并到主干
    
    + 当主干和分支修改同一个文件时, 合并分支会出现冲突, 需要手动修改

####git 常用
- git update-ref refs/INIT 4fb0 : 给4fb0版本重命名为INIT,存放到refs文件夹中,之后可以直接使用git reset --hard INIT还原到该版本

- git show-ref : 查看引用名称

- git update-ref -d refs/INIT : 删除INIT引用名称

- git tag beta0.0.1 : 给当前版本设置一个标签, 和引用名称效果相同, 可以直接使用这个名称切换版本

- git tag -d beta0.0.1 : 删除版本标签

- git rev-parse HEAD~n : 查看最新版本的上n个版本绝对名称

- git rev-parse HEAD^^ : 用^的数量来查看最新版本上几个版本绝对名称, 两个表示上一个版本


####git远程工厂
- 现有本地工厂, 之后提交给GitHub 

- 本地工厂没有, 直接最先就在GitHub上创建, 之后克隆到本地
    
    - 如果创建的仓库初始化了, 需要先将仓库的内容pull到本地, 在将本地内容进行提交
    ```
    //git pull 拉取远程仓库内容, 并且合并分支
    git remote add origin https://github.com/guliansheng/git_test01
    git pull origin master --allow-unrelated-histories (windows中的写法)
    git pull origin master (linux中的写法)
    git push origin master (提交到GitHub)
    ```
    ```
    //git fetch 拉取远程仓库内容, 不合并分支
    git remote add origin https://github.com/guliansheng/git_test01
    git fetch
    git branch -a (查看github分支)
    git merge --allow-unrelated-histories remotes/origin/master (windows中的写法, 合并分支)
    git merge remotes/origin/master (linux中的写法)
    git push origin master
    ```

    - 如果创建的库没有进行初始化, 直接提交到GitHub
    
    ```
    git remote add origin https://github.com/guliansheng/git_test02
    git push origin master
    
    ```

- 本地没有, 直接在github上克隆一个仓库到本地
    ```
    git clone https://github.com/guliansheng/empty_01
    cd empty_01
    echo empty > empty.txt
    git add .
    empty_01 > git add .
    git push origin master
    ```


