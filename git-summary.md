---
title: 《GitHub入门与实践》读书笔记
date: 2018-01-21 18:34:55
update:
tags:
    - 笔记
    - 基础知识
    - GitHub
categories:
    - 笔记
---

## 写在之前的话
- 本文是杜若学习GitHub的笔记整理,主要参考了以下书籍,博客:
    1. [《GitHub入门与实践》][1]
    2. [廖雪峰先生的git教程][10]
    3. [Por Git][50] (推荐,官方教程)
- 由于我的开发环境是`Windows`,所以主要记录这上面的使用方法
    - PS: 为了添加操作对应的图片,基本重写了整个笔记,真是够了
- 在笔记的末尾,总结了笔记示例中用到的[常用Git命令](#git_command),供以查询
    - 笔记只涉及Git和Github的概念性介绍,和简单的Git命令
    - 更多厉害的功能,我也还不会,待以后扩充吧 

呐~正文开始啦~🐱‍👤

<div align="left">![github_portal][2]</div>

---

## Git简介:

- Git是一种分布式版本控制系统
- 作用是可以将文件的改动进行记录和保存
- 还能与多人共享同一项目,同步每个人改动的记录,方便的实现多人开发
- 区别于`Subversion`集中式的管理,Git使用的分布式可以实现每个成员都拥有完整的版本库
    - 多人协作时只将修改部分同步给对方
    - 这样就算某个成员数据丢失,耶不会影响整个项目的数据
- Git的开发者是大名鼎鼎的Linus,就是开发Linux的那位大神
- GitHub,就是一个使用Git系统的网络仓库,开发者可以将自己的代码提交上去,并使用Git进行方便的管理操作 

### [Git暂存区](#staged)
<!-- <span id="staged">Git暂存区</span> -->
- Git的暂存区是一个很优秀的设计,在这里大概介绍一下
- 使用Git进行版本控制,同步的实际上是文件的改动
- 在默认状况下,所有文件的改动会先存放到一特殊区域`暂存区`
- 在需要回滚之前的版本时,通过`HEAD`指针指向需要的版本
    - 可以迅速的提取准确出需要的版本,而且并不暂用大量空间
- 在Win系统中,仓库文件夹中有隐藏文件`.git`
    - `.git`文件夹下存放着暂存区和版本库分支
    - 文件的变化,首先要纳入暂存区中,再通过`commit`提交到版本库分支
- 注意事项:
    - Git版本控制系统,只能识别文本文件的具体改动,如txt文件,代码等
    - 其他类型文件,照片,视频等,是二进制文件,可以知道文件大小的变化,却无法识别具体改动
        - 特别是word也是二进制格式的,所以无法跟踪word文件的改动
        - GitHub使用是免费的,人家维护也很不容易,不要把照片和视频这样的大文件放在GitHub上面啊喂,这不是网盘
        - GitHub上的代码是公开的,别想把银行卡账号密码也放上去啊喂
        - 使用的文件编码,最好统一为`UTF-8`避免冲突  

### Git中文件的三种状态: 
在Git仓库中,文件拥有三种状态,这里大概介绍下: 
1. `commit` : 已提交
    - 表示数据已经安全的保存在本地仓库中了
2. `modified` : 已修改
    - 表示数据发生变化,但还未保存到本地仓库中
3. `staged` : 已暂存
    - 表示数据的变化存入了[暂存区](#staged),但还未保存到仓库中

---

## GitHub简介:
- Github相当于一个Git的远程公开服务器
    - 可以把自己的代码上传到Github上,并进行版本管理
    - 当然,GitHub的能力远不止管理代码版本
        - 可以使用私有仓库,要给GitHub钱
        - 私有仓库也可以自己租服务器,架Git仓库
- GitHub的标志很萌,叫`octocat`--> `octopus`+`cat`
<div align="left">![octocat][3]</div>

- GitHub上绝大多数都是公开的代码,这影响了人们的开发方式
    - GitHub提出了`Social Coding`,社会化编程的概念
    - 任何人可以都拷贝别人的代码进行审核或学习
    - 也可以直接像开发者指出错误,和提出改进方案
    - 甚至可以直接将自己改好的代码提交给开发者,并由开发者合并到源码中
    - Github还提供了`Issue`功能,用于相互讨论
        - GitHub的评论支持Markdown语法,这一定程度上推进了MD的普及
    - 提供了`WiKi`功能,用于开发者描述项目
    - `Pull Request`功能,用于向别人仓库提出申请,将自己修改的代码合并到别人项目中
    - 总之,GitHub让全世界程序员联系在了一起,成为了程序员的乐园 

---

## Git在Windows下的安装和初始化
### Git安装
- `Git`下载地址:
    - `https://git-scm.com/downloads`
    - 按照安装提示进行安装即可
    - 如果已安装,需要更新`Git`版本,就在`Git Bash`中输入
        - `git clone https://github.com/git/git`
- 安装好后,会出现一个`Git Bash`应用
    - 这是一个命令提示符界面,类似CMD
    - 所有的Git命令都在这个应用中使用 
    <div align="left">![Git_Bash][4]</div>

### Git初始设置
- 设置Git用户名和邮箱
    - `git config --global user.name "Firstname Lastname`
    - `git config --global user.email "your_email@example.com`
    - `--global`表示该配置对这台电脑所有有文件夹都有效
    - 设置完成,会在`C:\user\当前用户\.gitconfig`下生成本地的配置文件
        - 该配置文件中记录了当前用户的仓库路径等参数

### GitHub的注册
- 官网在此: `https://github.com/`
    - 进去注册就行啦 

### SSH Key的作用

#### SSH Key简介
- Git是支持SSH协议的,GitHub通过`SSH Key`识别推送人,所以需要进行设置
 - 简言之,`SSH Key`是连接GitHub仓库时认证用的 

#### SSH Key生成
- 在本地电脑的`Git Bash`软件中执行:
    - `ssh-keygen -t rsa -C "youremail@example.com"`
        - 邮箱改成自己的
        - 后面步骤一路回车就是了
        - 可以不用设置密码
- 执行完毕后,在当前用户目录下,会生成一个`.ssh`目录
    - 点进去可以看到`id_rsa`和`id_rsa.pub`两个文件
    - `id_rsa.pub`是公开密钥;`id_rsa`是私有密钥 

#### 向GitHub添加公钥用于连接
- 登陆GitHub,找到`setting`-->`SSH Keys and GPG Keys`
    <div align="left">![git_SSH_Key13][13]</div>
- 选`Add SSH Key`
    - title填你喜欢的
    - Key填刚才生成的公有密钥`id_rsa.pub`中的值
- 点`ADD Key`进行添加
- 注意:
    - 这个Key对应的是这台电脑和你的账号
    - 如果有多台电脑使用这个账号,需要生成多个Key,并添加到GitHub中 

---

## Git常用命令
- 这些命令都是基于Linux的,有Linux命令行基础会更好
- 下面是正文: 

### 创建本地版本库 : `init`
- 具体用法
    1. 选中一个文件夹,用`Git Bash`输入`git init`
        - 后面凡是命令输入都是在`Git Bash`中使用,不再赘述
        - 可以在选中文件夹使用鼠标右键,选中`Git Bash`再输入命令
        - 也可以打开`Git Bash`应用,使用`cd`命令进入到选中的文件夹中
    2. 使用`git status`查看该仓库状态,是否创建成功
- 说明:
    - 本地版本库是一个文件夹
    - 创建完成后,该文件夹会多出一个`.git`隐藏子目录
    - Git就是利用这个子目录来跟踪和管理版本库的,所以不要去动它

### 向本地仓库添加&提交文件 : `add`&`commit`
- 具体用法
    1. 在本地仓库(就是`init`的那个文件夹)中,创建一个简单文本文件`hello_world.php`,文件里面写上:
    ```
    <?php
    echo "Hello World";
    ?>
    ```
    2. 使用命令: `git add hello_world.php`
        - 类似事务管理,这样做只是缓存了`add`这个动作,还没有正式执行
        - 所以,可以执行多次`add`之后,再统一进行`commit`
    3. 使用命令: `git commit -m "..."`
        - commit执行后,之前的`add`命令才被执行
        - `-m`后面的语句是对执行这次`commit`动作的说明,用于解释本次操作的作用
            - 说明是非常重要的,可以通过`git log`查看,多人协作中,别人也会容易的知道你执行了什么 

### 版本控制基本用法1 : 修改文件
- 具体用法:
    1. 向之前的`hello_world.php`添加一句注释,`<!-- 添加注释 -->`
        - 此时这个文件中的数据就已经改变了
        - 可以通过`git status`命令查看文件状态,会显示`modified` 
    <div align="left">![git_diff][5]</div>
    2. 执行`git diff 文件名`命令
        - diff就是different的简写,注意后面要加文件名
        - 执行后会显示该文件的未提交的具体变化 
        - 添加的注释是绿色的,前面有个`+`
    <div align="left">![git_status][6]</div>
    3. 执行`git add 文件名`命令
        - 将修改后的结果提交到仓库中
        - 注意每次修改了,必须要执行`add`,再`commit`才能纳入仓库
    4. 执行`git commit -m "add annotation"`命令
        - 让修改生效,纳入到仓库中
        - 这样原文件和文件的修改都纳入仓库中,后面需要就可以再退回修改前的原文件,达到了版本控制的效果
    5. 执行`git status`查看当前文件状态
        - 会看到之前没有需要提交的修改了
        - `working three is clean`
    <div align="left">![github7][7]</div>
    6. 建议多次尝试修改,进行练习 

### 版本控制基本用法2 : 退回之前版本
- 具体用法:
    1. 执行`git log`命令
        - 查看仓库日志,就是`commit`的执行和说明
            - 日志中`commit`后面的一串字符是系统生成的id
            - 用来标识那次改动的版本
        <div align="left">![git_log9][9]</div>
        - 多次提交`commit`Git会把执行存成一条时间线
        - 通过`Git Gui`可以查看提交历史的时间线
            - 不建议使用这个软件,因为我们主要会在`github`上看啦
        <div align="left">![github_GitGui8][8]</div>
        - 在`Git Bash`中查看完日志后的退出
            - 方式1: 输入`q`再回车
            - 方式2: 输入`ctrl c`
                - 方式2,我写了后有bug,界面不显示我输入的字了,要重启`Git Bash`
                - 还是用方式1吧
            - 这是常见的`Linux`命令,有时间再去整理`Linux`的笔记
    2. 执行`git reset --hard HEAD^`
        - 退回上一版本
            - Git中,HEAD用于表示当前版本,上一版本用`HEAD^`表示
            - 同理,上上一版本写成`HEAD^^`,上一百个版本写成`HEAD~100`
            - 也可以通过`log`查看`commit`后面的id号,用id号替代`HEAD^`进行精确的退回
            - `--hard`:
                - Git有三种恢复等级`soft`,`mixed`(默认),`hard`
                - 其中`hard`是一种不可逆的操作
        - 退回后,再去打开之前的`hello_world.php`文件看看
            - 是不是退回到修改之前的文本了
    3. 在此恢复到修改之后的操作
        - 只要找到当时修改操作的`commit`后面的id号,就能恢复
        - 找id号,可以执行`git log`找对应的id号
        - 也可以执行,`git reflog`,找到需要退回到哪次`HEAD`上
            - 这个命令可以找到之前的每一次`commit`记录
        - 注意哈:
            - 找`commit`的id,是通过你每次提交时写的提交信息来找的
            - 所以,提交信息非常重要,要写清楚,方便自己找
        - 找到对应的id号后,执行
            - `git reset --hard 版本号`
            - 或者`git reset --hard HEAD^` 退回几次就用几个`^`符号
- 注意事项:
    - 对文件的修改,需要执行`add`命令才算纳入了暂存区
    - 所以,要注意以下描述中的错误操作
        1. 对文件进行修改(修改1)
        2. 使用`add`命令将此次修改纳入到暂存区
        3. 再对文件进行修改(修改2)
        4. 使用`commit`命令将修改提交到版本库中
    - 结果,(修改2)不会被提交到版本库中,而且通过`status`命令,会看到该文件有一次修改(modified)未被提交
    - 原因是(修改2)没有通过`add`纳入修改到暂存区
    - 结论:
        - 要确保每次修改生效,就要记得纳入暂存区
        - 最好所有修改都执行了再操作Git,防止变化没有提交版本库 
- 注意事项:
    - 现在还是在本地使用Git,还没有联网接入Github
    - 联网接入后,推送到远程库里后,再撤销,由于分支的问题,别人也会看到撤销
    - 所以提交更改要小心啦

### 版本控制基本用法3: commit前,撤销修改
- 情景1: 只修改了文件,未执行git命令前,撤销具体方法:
    1. 向之前的`hello_world.php`添加一句注释,`<!-- stupid boss -->`
        - 此时,未执行`add`命令,使用`git status`查看,会发现该文件有改动(modified)待提交,我们要做的是,撤销这次改动,不向版本库提交
    2. 执行`git checkout -- hello_world.php`
        - 这句话的作用是将文件的当前修改撤销
        - 文件如果`add`过一次修改,在未`commit`前,又修改了一次文件,使用此命令依旧有效,只要没有`commit`到版本库中就行
        - 注意1:
            -  `git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“创建⼀一个新分⽀支”的命令，后面的分⽀支管理中会再次遇到`git checkout`命令
            - 关于`分支`的介绍[后面][#branch]会讲
        - 注意2:
            - 如果修改已经`add`纳入暂存区了,使用这句命令就无效了
- 情形2: 文件的修改使用`add`纳入到了暂存区,撤销的具体方法:
    1. 向之前的`hello_world.php`添加一句注释,`<!-- stupid boss -->`
    2. 执行了`git add hello_world.php`命令
        - 此时查看`status`,会发现修改已添加到暂存区了
        - 此时需要撤销修改
    3. 执行`git reset HEAD hello_world.php`
        - 看这里`reset`命令也可以撤销暂存区中的修改哈
        - 现在看`status`,修改显示为未纳入暂存区的红色状态,那就用情形1的方法搞定吧
    <div align="left">![git_reset2][11]</div>
    4.  执行`git checkout -- hello_world.php`

### 版本控制基本用法3: 删除文件
- 具体用法:
    1. 在版本库目录下新建文件`hello_git.txt`,写一个简单的`Hello`在里面吧
    2. 执行`git add hello_git.txt`
    3. 执行`git commit -m "add hello_git.txt"`
        - 此时,新文件以纳入版本库
    4. 在本地删除该文件,执行`git status`
        - 信息说名该文件被删除了
        - 但本地删除了,版本库里还有,需要删除版本库中的执行`5.`操作
     <div align="left">![git_delete12][12]</div>
     5. 执行`git rm hello_git.txt`
        - 删除版本库中的该文件,`rm --> remove`
        - 此时查看`status`,可见该删除待`commit`
    6. 执行`git commit -m "delete hello_git.txt`
- 注意: 在未执行`commit`时,想撤回删除,恢复文件,使用如下操作
    1. 执行`git checkout -- hello_git.txt`
        - 这样的操作实际是从版本库里取出文件进行替换
        - 如果有未提交的更改未纳入版本库中,此方式会有数据的损失

---

## GitHub远程仓库的使用

### 添加远程库&与本地仓库关联
- 具体做法:
    1. 在GitHub上创建一个仓库
        - 点页面右上角的`New Repository`
        - 仓库名随便起,为了统一,都叫`nostalgic`吧
            - 听[这首歌](http://music.163.com/m/song?id=801824&userid=9567158)时候取的名字...
        - 选`Create repository`确认创建
    <div align="left">![git_new_repo14][14]</div>
    2. 将本地仓库与GitHub上新建的仓库连接
        - 进入本地仓库,在`Git Bash`中输入:
        - `git remote add origin git@github.com:你的帐户名/你的仓库名.git`
        - 这个操作,看GitHub上都有提醒的,复制代码来运行即可
        - 操作中的`origin`,是远程库的名字,默认使用这个名字,这样看到就知道是远程库了
    <div align="left">![git_new_repo15][15]</div>
    3. 将本地库内容推送到GitHub远程库上
        - 执行:`git push -u origin master`
            - `-u` : 将本地master分支和远程master分支关联的参数
                - 关联一次后,后面推送就不需要这个参数了
            - `origin` : 说明推送到的远程仓库的名字
            - `master` : 主分支,关于分支,[后面][#branch]会详细讲
    <div align="left">![git_push16][16]</div>
    4. 打开GitHub上的`noastalgic`项目,本地仓库的内容就已经推送到了
        - 随时将本地项目`push`到GitHub上,就能防止代码丢失等情况
    <div align="left">![github_push17][17]</div>
- 附 : GitHub创建仓库的名词解释
    - 创建`Private`是要收费的,免费的都是公开的
    - `Description` 是用于描述项目的,相当于简介,项目生成后在项目名下的小字
    - `ReadMe` 一般用于标明代码概要,使用流程,许可协议等,会显示在项目首页
    - `.gitignord` 添加后,里面可以放不受版本管理的文件
    - `license` 选择项目使用的开源协议 

### 从远程仓库克隆
- 具体做法:
    1. 在GitHub上创建一个新仓库,`ClonePractice`
        - 勾选添加`READNME`文件
    2. 选择一个本地文件夹,打开`Git Bash`
        - 文件夹不要和原有仓库文件夹一样,会出错
        - 执行:`git clone git@github.com:duruonanni/ClonePractice.git`
            - 换成自己的仓库地址
    <div align="left">![git_clone18][18]</div>
    <div align="left">![git_clone19][19]</div>
    3. 完成后,打开该文件夹,会看到一个`README.md`的文件,说明该仓库成功从Github上克隆到本地了 

---

## <span id="branch">GitHub分支管理</span>

### 分支管理概述:
- 分支管理是GitHub学习最重要的概念
- 回顾一下: 
    - 我们知道,Git对版本的控制,是通过时间线记录文件内容的变化来操作的
    - 每次对文件执行操作(增删改),再commit,就会在文件的版本生成一个新的时间点
    - 每个时间点有自己唯一的id号,通过`HEAD`指针,可以回到任意某个时间点,从而达到对版本的控制
- 在多端操作中:
    - 由于Git的同步方式是分布式的,这意味着,多台电脑上可以维护着同一个项目
    - 每台电脑都拥有该项目的全部代码
    - 那么,在不同电脑修改同意文件后进行提交,就会产生冲突
    - 分支的引入就是解决这个冲突的
- 分支:
    - Git上存在一个主分支`master`(之前的操作都是在主分支上进行的)
    - 我们可以创建其他分支,在其他分支中对代码进行修改
    - 当新分支中的工作结束后,我们可以将新分支中的工作,合并(merge)入主分支中
    - 在GitHub上,你可以随意的去从别人公开的代码中创建属于自己的新分支,并对上面的代码进行操作
        - Github上代码的合并需要主分支的所有者同意(pull request)
        - 所以,你可以对新分支进行任意操作,而不影响主分支的项目 
    - Git中鼓励使用分支 

### 分支管理的使用1 : 新分支的创建,合并与删除
- 具体做法:
    1. 在刚clone的项目`ClonePractice`本地仓库中,创建新分支:
        - 执行`git checkout -b dev`
            - 意思是创建一个新分支`dev`,并切换到它
            - 相当于: `git branch dev`加`git checkout dev`
    2. 查看当前项目的本地分支:
        - 执行: `git branch`
            - `*`表示当前所在分支
    <div align="left">![git_branch20][20]</div>
        - 执行: `git status`
            - 查看项目状态,注意目前所在分支
    3. 对该项目中的文件`README.MD`进行修改并提交
        - 文件末尾添加一句:  `This is dev branch did`
        - 执行: `git add README.MD`
        - 执行: `git commit -m "branch test"`
    4. 切换回master主分支
        - 执行: `git checkout master`
        - 查看`README.MD`内容,是否有改变
            - 当然是没改变,在主分支并未生效
    5. 合并`dev`分支到主分支
        - 执行: `git merge dev`
            - 执行的是将`dev`分支合并到当前分支,当前分支这里是`master`主分支
            - 执行后显示的`Fast-forward`意思是
                - 这次合并是"快进模式",也就是直接把master指向dev的当前提交,所以合并速度非常快
                - 后面还会讲到其他合并模式
    <div align="left">![git_merge21][21]</div>
    6. 删除`dev`分支
        - 执行: `git branch -d dev`
        - git建议使用完成某个分支后,就删除掉 
- 分支的强制删除: 
    - 默认情况下,分支中的内容只有被merge到其他分支后,该分支才能被顺利删除
    - 如果想要抛弃某个分支中的内容,将其强制删除,使用如下命令:
    - `git branch -D dev`
    - 注意:
        - 平时要慎重使用此删除分支的方式

### 分支管理的使用2: 解决文件冲突
- 操作概述:
    - 同一个文件,分支1修改了,分支2也进行了修改
    - 如何解决文件被修改的冲突
- 具体做法:
    1. 创建并切换到新分支`feature1`
        - 执行: `git checkout -b feature1`
    2. 修改`README.MD`文件
        - 最后一句改为: `This is feature1 did`
    3. 提交
        - 执行: `git add README.MD`
        - 执行: `git commit -m "feature1 modified"`
    4. 切换到master分支
        - 执行: `git checkout master`
            - 显示: `Your branch is ahead of 'origin/master' by 1 commit.`
                - 说明本地的`master`分支比远程的`origin/maste`超前一个版本(`commit`)
    <div align="left">![git_master22][22]</div>
    5. 修改`README.MD`文件
        - 最后一句改成: `This is master did"`
    6. 提交
        - 执行: `git add README.MD`
        - 执行: `git commit -m "master modified"`
    7. 尝试合并feature1分支
        - 执行: `git merge feature1`
        - 显示合并失败,文件冲突`CONFILCT`
        - 也开始在尝试合并后,使用`status`查看状态,系统有给出解决冲突的建议
    <div align="left">![git_master23][23]</div>
    8. 解决文件冲突
        - 打开文件看看,文件现在变成这样了
            - `cat` 是linux命令,查看文件内容
            - 也可以在编辑器中打开文件,一样的效果
        - Git⽤用`<<<<<<<`,`=======`,`>>>>>>>`标记出不同分支的内容
        - 我们可以根据需要手动对内容进行修改并保存
            - 我们只留下master的修改
            - 当然在更智能的编辑器上,可以更方便的修改冲突
            - VSCODE上集成了Git,它的冲突修改界面入下图2
    <div align="left">![git_merge_config24][24]</div>
    <div align="left">![git_merge_config_vscode25][25]</div>
    9. 提交解决冲突后的版本
        - 执行: `git add README.MD`
        - 执行: `git commit -m "confict fixed"`
    10. 删除无用分支feature1
        - 执行: `git branch -d feature1`
    11. 用log查看分支情况
        - 执行: `git log --graph`
        - 可以看到分支启用和合并的情况
    <div align="left">![git_log_graph26][26]</div>

### 分支管理的使用3: 分支管理策略
- 操作概述: 
    - 之前有讲,默认情况下,Git使用`merge`合并分支,结果是使用的`Fast forword`模式
    - 这个模式,会将独立分支中的操作,归为合并后的分支,缺点就是丢掉分支信息,丢掉了之前操作是分支执行的记录了
    - 可以通过禁止`Fast forword`的方式,保留分支信息
    - 具体操作是再执行`merge`时,添加`-no-ff`参数
    - 下面是示例: 
- 具体做法: 
    1. 创建并切换到,`dev`分支
        - `git checkout -b dev`
    2. 修改`README.MD`文件
        - 最后一句改为: `This is dev branch did, to test merge no-ff model`
    3. 为当前修改提交`add`和`commit`
        - `git add README.MD`
        - `git commit -m "update README.md`
    4. 切换回master分支
        - `git checkout master`
    5. 合并`dev`分支,注意`-no-ff` 参数
        - `git merge --no-ff -m "merge dev with no-ff" dev`
    <div align="left">![git_merge_no-ff27][27]</div>
    6. 使用`log`查看分支历史
        - `git log --graph --pretty=oneline --abbrev-commit"`
            - `--pretty=oneline` : 让log的显示格式为简略1行
            - `--abbrev-commit` : id值只显示前面几个字符
            - `log`的更多查看方式,见[下面](#log)
     <div align="left">![git_log_no-ff28][28]</div>
- 总结: 
    - 采用`no-ff`形式,分支执行的操作历史也得以保存,不会被合并
    - 这样的做法,适合团队协作开发项目时使用

### 分支管理的使用4: 暂存当前工作
- 操作概述: 
    - git中,可以将当前未完成的工作暂存起来,以便对之前的版本进行临时修改
    - 新的修改结束后,取出暂存的数据,继续进行操作
    - 这样的操作,通常用于`Bug处理`时
    - 暂存的关键词是`stash`
    - 下面来模拟操作一下: 
- 具体做法: 
    1. 切换到dev分支,在仓库中创建一个`working.txt`文件
        - `git checkout dev`
        - 创建`working.txt`文件,随便写点什么
        - `git add working.txt`
    2. 这时候master分支有bug需要立即处理,使用`stash`将当前工作"储藏"起来
        - `git stash`
    <div align="left">![git_stash29][29]</div>
    3. 切换到master分支,修复bug
        - `git checkout master`
        - 从master分支,创建修复bug的临时分支`issue-01`
            - 不要直接在master上面改呀
            - `git chekout -b issue-01`
        - 在`README.md`末尾添加一句: `bug fixed`,并提交
            - `git add README.md`
            - `git commit -m "bug fixed"`
    4. 切换回master分支,与issue-01分支合并,删除issue-01分支
        - `git checkout master`
        - `git merge --no-ff -m "merged bug fix 01" issue-01`
        - `git branch -d issue-01`
    5. 回到刚才的dev分支
        - `git checkout dev`
            - 此时查看`status`,显示没有暂存的项目,文件夹里的`working.txt`也没有
            - 这时候就需要将之前`stash`存储的工作恢复回来
    6. 恢复到之前的工作
        - 有两种恢复方式
            1. `git stash apply`
                - 恢复后,stash中的内容并不删除
                - 可使用 `git stash drop`进行删除
            2. `git stash pop`
                - 恢复stash中的内容,并删除
        - 可使用`git stash list`命令查看保存的stash
     <div align="left">![git_stash_pop30][30]</div>
      
---

## Git标签(tag)管理

### Git标签概述
- 在开发和维护程序时,通常需要对程序的版本情况进行描述
- 之前的操作中,我们都使用commit操作完成后,系统提供的id号作为对版本的说明
- 此外,Git还提供了`标签`,用来更方便的描述版本号
- 比如我们通常将初次完成的项目所在的版本,用标签命名为`v1.0`
- 使用标签号,我们可以方便的对版本进行说明,也可以使用标签名找到对应时刻的历史版本
- Git中的标签是和分支上的某个版本紧密相连的 

### Git标签操作1: 标签的创建和删除
- 操作概述: 
    - 标签操作前一定要注意查看所在的分支
    - 可以对当前commit打标签,也可以对历史的commit打标签
- 具体做法: 
    1. 查看当前分支,切换到`master`分支
        - `git branch`
        - `git checkout master`
    2. 对当前commit(也就是最新的)打上tag
        - `git tag v1.0`
    3. 查看所有标签
        - `git tag`
    4. 对历史commit打上tag
        - `git tag v0.9 4744420`
        - 后面写的是commit的版本号
    5. 删除标签
        - `git tag -d v1.0`

### Git标签操作2: 远程标签操作
- 操作概述:
    - 默认情况下,标签信息是不会随着push推送到远程端的
    - 远程端的标签信息推送需要额外的命令
- 具体做法:
    1. 把一个标签推送到远程
        - `git push origin v1.0`
    <span align="left">![git_tag_remote_add][31]</span>
    2. 把本地所有标签推送到远程
        - `git push origin --tags`
    3. 删除远程的标签
        - `git push origin :refs/tags/v1.0`
    <span align="left">![git_tag_remote_delete][32]</span>
    
---

## Git命令优化: 配置别名

### 配置别名概述:
- 之前的Git命令操作都是敲的命令全名
- 其实,Git可以配置自己专有的别名,更快捷的进行操作
- 别名配置的关键词是`alias`
- 别名配置的命令是:
    - `git config --global alias.[other name] [command]`
- 个人觉得别名更重要的作用是,可以配置一些自己适用的独特的log日志形式
- 配置后,仍然可以用全名的方式进行调用

### 别名配置示例:

- 示例1: status --> st
    - `git config --global alias.st status`
- 示例2: commit --> ct
    - `git config --global alias.ct commit`
- 示例3: branch --> br
    - `git config --global alias.br branch`
- 示例4: 快速撤销
    - `git config --global alias.unstage 'reset HEAD`
    - 需要退回上一版本,就用: `git unstage [file name]`
- 示例5:显示最新日志
    - `git conifg --global alias.last 'log -1`
    - 需要使用时,就用: `git last`

#### 别名配置专属 log
- 可以把log配置成自己想要显示的格式,然后用一个别名进行保存
- `git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"`
- 配置完成后,输入`git lg`,显示效果如下: 
<span align="left">![git_lg][33]</span>


---

## <span id="git_command">[常用git命令总结</span>

### 创建版本库
- `git init [project-name]`
     - 创建本地仓库 

### 修改和提交
- `git status`
    - 查看仓库状态,包括
        - 在哪个分支
        - 仓库中文件的变化
- `git add [file]`
    - 添加文件到就绪状态
    - 就是添加文件到暂存区,使文件纳入Git管理
- `git add .`
    - 将所有改动过的文件纳入git管理
- `git diff [file]`
    - 查看文件的修改内容
    - 增加的内容会用`+`,标示出来;删除的内容会用`-`标示出来
- `git diff --staged`
    - 查看暂存区中的文件(还未commit)和最新文件的不同
- `git commit -m "[description message]"`
    - 让文件的修改添加等命令执行,将文件变化纳入git仓库的管理
    - 就是将暂存区执行的操作提交到当前分支
    - 执行后就像游戏存盘了,可以方便的将文件改回某刻修改的状态 

### 删除和重命名
- `git rm [file]`
    - 删除本地文件和版本库中的文件
    - 这样删除了的文件,还是可以使用`checkout`找回
- `git rm --cached [file]`
    - 删除版本库中的文件,但是本地仍然保留有该文件
    - 可以用`add`命令存回版本库中
- `git mv [file-old-name] [file-new-name]`
    - 重命名文件
    - 注意
        - 不要随便在文件本身随便重命名,git会以为是删除了旧文件新建了这个新文件

### 撤销操作
- `git reset 撤回版本id或使用HEAD^方式`
    - 撤回某次操作的版本
    - 只是纳入到暂存区,未commit的修改,使用reset依然能撤回
- `git checkout -- 文件名`
    - 撤销未提交commit的修改
    - 也可以恢复未提交commit的删除
    - 注意`--`不能少,少了就变成[切换分支](#change_branch)了

### 查看提交历史
- `git log`
    - 显示`Git Bash`操作日志详情,包括每次提交的id号
    - 有多种形式的显示方式,详情见[下面](#log)
- `git log --graph`
    - 查看分支情况
- `git reflog`
    - 以`HEAD^`形式显示操作日志,不包括id号 
- `git show [commit id]`
    - 查看某次commit的超详情,包括:
        - 操作人,时间,提交信息,操作文件的详情(包括文件内容) 

#### <span id="log">log输出的常用选项</span>
说明: 这些输出命名效果可以叠加的,可以在一句log命令中使用多个,生成符合自己要求的日志信息: 
1. 末尾加文件名
    - 只显示某个文件名相关的日志信息
2. -2
    - 仅显示最近两次提交,换其他数字同理
3. -p
    - 日志中显示提交内容的差异
4. --stat
    - 显示相关文件的统计信息(修改几次,插入几次等信息)
5. --shortstat
    - --stat的简单形式
6. --name-only
    - 仅显示修改的文件信息
7. --abbrev-commit
    - id号(SHA-1),仅显示前几位
8. --relative-date
    - 时间格式显示成,几小时前,几天前的样式
9. --graph
    - 用ASCII图表形式显示.能显示分支合并的历史
10. --pretty
    - 使用其他格式:
        - `--pretty=oneline` : 每条信息显示成一行
        - `--pretty=short` : 只显示创作者和-m的信息
        - `--pretty=full` : 显示创作者和提交人和-m的信息
        - `--pretty=fuller` : 除了full的信息,还显示提交时间 

#### GitHub远程操作命令
- `git clone [url]`
    - 从Github将远程库克隆到本地
    - 会将远程库中所有内容,包括里面的版本控制信息一起克隆到本地
    - clone操作完成后,本地仓库和远程仓库就具备了联系,后面可以直接通过pull和push进行同步操作;
- `git fetch [remote]`
    - 从指定的远程库中获取代码
    - 这样操作,并未将远程库中的代码`merge`到本地库对应的分支上
        - 远程库通常有个`origin`标记(就是这里的`[remote]`])
    - 通常需要对比本地与远程库日志时这么操作,再进行合并
- `git merge [remote]/[branch]`
    - 将远程库和本地库上对应的分支合并
- `git pull [remote] [branch]`
    - 将远程库中信息下载并快速合并到本地对应的分支
    - 相当于`git fetch [remote]`+`git merge [remote]/[branch]`
- `git push [remote] [branch]`
    - 将本地的版本控制情况上传到远程,并合并到对应分支

### 分支操作
- `git branch`
    - 查看当前项目的本地分支
     - `*`表示当前所在分支
- `git branch [branch-name]`
    - 创建新分支
- <span id="change_branch">`git checkout [branch-name]`</span>
    - 切换到`分支名`的分支
    - 加`-b`参数表示创建该分支并切换到它
- `git merge [branch-name]`
    - 执行的是将`分支名`分支合并到当前分支
- `git branch -d [branch-name]`
    - 删除`分支名`分支
    - 注意
        - 用完某个分支就建议删掉
        - 无法删除当前所在的分支,需要切换到其他分支
        - master主分支无法删
- `git branch -D [branch-name]`
    - 强制删除分支
    - 默认情况下,分支中的内容只有被merge到其他分支后,该分支才能被顺利删除
    - 使用此命令可以强制删除
    - 请慎重使用此命令

### 隐藏储存区操作
- `git stash`
    - 将当前所有操作放入隐藏储存区中
- `git stash apply`
    - 从隐藏储存区中取回之前存入的操作
    - 并不删除隐藏储存区中的内容
- `git stash drop`
    - 删除隐藏存储区中的内容,这样就不能使用`apply`找回
- `git stash pop`
    - 从隐藏存储区中找回之前操作,并删除存储区中的内容
- `git stash list`
    - 列出隐藏存储区中的内容

### 标签管理
- `git tag`
    - 查看所有标签
- `git tag [tag name]`
    - 为当前版本创建标签
- `git tag [tag name] [commit id]`
    - 给指定commit id的版本添加标签
- `git tag -a [tag name] -m "[message]" [commit id]`
    - 创建带message信息的标签
    - message可以用`git show [tag name]`查看
- `git show [tag name]`
    - 查看某个标签版本的详情
- `git push [remote] [tage name]`
    - 将某个本地标签推送到远程
- `git push [remote] --tags`
    - 将本地的所有标签都推送到远程
- `git push [remote] :refs/tags/[tag name]`
    - 删除远程的某个标签 


至此,完...... 

<div align="center">![Album_Cover_清风二式_西皮士][34]</div>



<!-- 参考文献 -->
[1]: http://www.ituring.com.cn/book/1581
[2]: http://storage.live.com/items/AEE68C12565C1619!199203:/github_portal.png?authkey=AJoh90nl3u6Wj4U
[3]: http://storage.live.com/items/AEE68C12565C1619!199201:/octocat.png?authkey=AJoh90nl3u6Wj4U
[4]: http://storage.live.com/items/AEE68C12565C1619!199205:/Git_Bash.png?authkey=AJoh90nl3u6Wj4U
[5]: http://storage.live.com/items/AEE68C12565C1619!201522:/git_diff.png?authkey=AJoh90nl3u6Wj4U
[6]: http://storage.live.com/items/AEE68C12565C1619!201523:/git_status.png?authkey=AJoh90nl3u6Wj4U
[7]: http://storage.live.com/items/AEE68C12565C1619!201525:/github7.png?authkey=AJoh90nl3u6Wj4U
[8]: http://storage.live.com/items/AEE68C12565C1619!201526:/github_GitGui8.png?authkey=AJoh90nl3u6Wj4U
[9]: http://storage.live.com/items/AEE68C12565C1619!201527:/git_log9.png?authkey=AJoh90nl3u6Wj4U
[10]: https://liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000 
[11]: http://storage.live.com/items/AEE68C12565C1619!201528:/git_reset2.png?authkey=AJoh90nl3u6Wj4U
[12]: http://storage.live.com/items/AEE68C12565C1619!201529:/git_delete12.png?authkey=AJoh90nl3u6Wj4U
[13]: http://storage.live.com/items/AEE68C12565C1619!201530:/git_SSH_Key13.png?authkey=AJoh90nl3u6Wj4U
[14]: http://storage.live.com/items/AEE68C12565C1619!201531:/git_new_repo14.png?authkey=AJoh90nl3u6Wj4U
[15]: http://storage.live.com/items/AEE68C12565C1619!201532:/git_new_repo15.png?authkey=AJoh90nl3u6Wj4U
[16]: http://storage.live.com/items/AEE68C12565C1619!201533:/git_push16.png?authkey=AJoh90nl3u6Wj4U
[17]: http://storage.live.com/items/AEE68C12565C1619!201534:/github_push17.png?authkey=AJoh90nl3u6Wj4U
[18]: http://storage.live.com/items/AEE68C12565C1619!201535:/git_clone18.jpg?authkey=AJoh90nl3u6Wj4U
[19]: http://storage.live.com/items/AEE68C12565C1619!201536:/git_clone19.png?authkey=AJoh90nl3u6Wj4U
[20]: http://storage.live.com/items/AEE68C12565C1619!201537:/git_branch20.png?authkey=AJoh90nl3u6Wj4U
[20]: http://storage.live.com/items/AEE68C12565C1619!201539:/git_master22.png?authkey=AJoh90nl3u6Wj4U
[21]: http://storage.live.com/items/AEE68C12565C1619!201538:/git_merge21.png?authkey=AJoh90nl3u6Wj4U
[22]: http://storage.live.com/items/AEE68C12565C1619!201539:/git_master22.png?authkey=AJoh90nl3u6Wj4U
[23]: http://storage.live.com/items/AEE68C12565C1619!201540:/git_master23.png?authkey=AJoh90nl3u6Wj4U
[24]: http://storage.live.com/items/AEE68C12565C1619!201542:/git_merge_config24.png?authkey=AJoh90nl3u6Wj4U
[25]: http://storage.live.com/items/AEE68C12565C1619!201543:/git_merge_config_vscode25.png?authkey=AJoh90nl3u6Wj4U
[26]: http://storage.live.com/items/AEE68C12565C1619!201544:/git_log_graph26.png?authkey=AJoh90nl3u6Wj4U
[27]: http://storage.live.com/items/AEE68C12565C1619!203845:/git_merge_no-ff27.png?authkey=AJoh90nl3u6Wj4U
[28]: http://storage.live.com/items/AEE68C12565C1619!203846:/git_log_no-ff28.png?authkey=AJoh90nl3u6Wj4U
[29]: http://storage.live.com/items/AEE68C12565C1619!203847:/git_stash29.png?authkey=AJoh90nl3u6Wj4U
[30]: http://storage.live.com/items/AEE68C12565C1619!203848:/git_stash_pop30.png?authkey=AJoh90nl3u6Wj4U
[31]: http://storage.live.com/items/AEE68C12565C1619!206351:/git_tag_remote_add.png?authkey=AJoh90nl3u6Wj4U
[32]: http://storage.live.com/items/AEE68C12565C1619!206352:/git_tag_remote_delete.png?authkey=AJoh90nl3u6Wj4U
[33]: http://storage.live.com/items/AEE68C12565C1619!206354:/git_lg.png?authkey=AJoh90nl3u6Wj4U
[34]: http://storage.live.com/items/AEE68C12565C1619!199202:/Album_Cover_清风二式_西皮士.jpg?authkey=AJoh90nl3u6Wj4U


[50]: https://git-scm.com/book/zh/v2 "Por Git 官方教程"