# git

### 初始
#### 设置用户信息
- 使用下面命令设置　文件的信息
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
- 如果个别文件需要某个特别的信息
```
$ git config user.name "Your Name"
$ git config user.email "email@example.com"
```

#### 仓库
- 仓库需要初始化
```
git init
```
- 文件添加到仓库中
```
git add "filenames"
(or) git add --all
```
- 文件提交到仓库
```
git commit -m "说明"
```
(以上每次提交都需要两部`git add`和`git commit`)
- 查询仓库状态
```
git status
```
- `git diff`查看代码更改，能看到的是从上次提交到目前的更改,`git diff HEAD -- readme.txt`可以查看工作区里的文件与版本库里的此文件的区别。（后者就是两个文件对比，关于`HEAD`下面就说）
### 关于版本的还原与还原
- 每次`commit`都有一个说明，使用`git log`可以查看每次的`commit`
`git log`输出信息如下面格式，如果很占地方，可以在后边加`--pretty=oneline`参数
输出结果如
```
commit 11f9e2e5dfebf1ed18e51f525f80fcacd3aaa4e9
Author: fanzzz <izjfan@163.com>
Date:   Sun Sep 23 20:00:07 2018 +0800

    第一次提交
```
- 版本还原
还原到之前的版本，`HEAD`表示当前版本，上一个就是`HEAD^`，上上一个是`HEAD^^`，类推，但是当太靠前时，可用`HEAD~100`表示。
```
git reset --hard HEAD^
```
- 版本再向前
如果退回之前的，又后悔了，可以通过之前说的的`commit`后面的一串数字来还原（不需要全部,５个即可）,这里的号码可以通过`history`指令来查看。最后若是关了电脑什么的，可以通过指令`git reflog`来查看以往版本变更的commit id.
git的版本回退特别快，因为是通过`HEAD`指针来实现的。
***
Git 跟踪的不是文件，是修改，从`commit status`可以看到修改
***
### 工作区　版本库　分区
工作区是当前工作目录
隐藏目录`.git`是版本库，`.git`里有很多东西，其中有一个stage(或者index)的暂存区，还有Git为我们自动创建的第一个分之`master`，以及指向`master`的一个指针`HEAD`.

- `git add`把修改提交到暂存区（Stage）,`git commit`把暂存区的修改提交到分支。**注意`git commit`只会把暂存区的修改提交到版本库的分支**
- 使用`git checkout -- filename`撤销提交到暂存区的修改　　似乎使用`git reset HEAD filename`也可以。

下面引用廖雪峰的话
> 命令`git checkout -- readme.txt`意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
> 一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
> 总之，就是让这个文件回到最近一次git commit或git add时的状态。

`git reset`：可以回退版本，又可以把暂存区的修改删除。然后使用上面的`checkout`回退到版本库版本

### 删除操作
- 一般删除本地工作区删除使用`rm filename`，删除版本库使用同样方法`git rm filename`，但是要紧接着`git commit`

- 如果删除错误，使用`git checkout -- filename`


## 远程仓库
第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
```
$ ssh-keygen -t rsa -C "youremail@example.com"
```
你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。
如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。
第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：
然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容。然后点击"Add Key"

- 在github上创建仓库后，仓库是空的，我们可以从这个仓库克隆出新的仓库(到本地？)，也可以将其与一个已有本地仓库关联，然后就可以把本地仓库推送到GitHub上。

**在创建仓库后给了下面几个选择**

```
Quick setup — if you’ve done this kind of thing before
We recommend every repository include a README, LICENSE, and .gitignore.

…or create a new repository on the command line
echo "# gitlearn" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:fanzzz/gitlearn.git
git push -u origin master

…or push an existing repository from the command line
git remote add origin git@github.com:fanzzz/gitlearn.git
git push -u origin master

…or import code from another repository
You can initialize this repository with code from a Subversion, Mercurial, or TFS project.
```
所以使用下面的指令先关联
`git remote add origin git@github.com:fanzzz/gitlearn.git`
然后推送到GitHub
`git push -u origin master`
由于远程库是空的，第一次推送加了参数`-u`，Git不但会把本地的`master`分支的内容推送到远程新的`master`分支，还会把本地的`master`分支的内容与远程新的`master`**关联起来**，以后推送拉取是可以简化命令。简化后的为`git push origin master`。
> #### SSH警告
当你第一次使用Git的clone或者push命令连接GitHub时，会得到一个警告：
```
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
```
这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入yes回车即可。
Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：
`Warning: Permanently added 'github.com' (RSA) to the list of known hosts.`
这个警告只会出现一次，后面的操作就不会有任何警告了。
如果你实在担心有人冒充GitHub服务器，输入yes前可以对照GitHub的RSA Key的指纹信息是否与SSH连接给出的一致。

#### 从远程仓库克隆
```
git clone SSH
```
也可以用`https:`但是除了慢之后，每次都需要输入口令。

## 分支
分支是两个不同的目录?区域?　　　可以做不同的更改，最后合并，然后每个分支的实现的功能就可以合在一起。
****
每个分支可以看做一条时间线。在Git里，有主分支`master`。然后`HEAD`指向`master`，最后`master`指向提交的内容。
***
- 创建与合并
当创建一个新的分支，例如`dev`，Git创建一个指针叫`dev`，指向`master`相同的提交。随后把`HEAD`指向`dev`，那么当前就在`dev`分支上。
随后更改哪个分支就会移动哪个分支(指针的移动，指向新的提交)，
```
git checkout -b dev //创建并切换到
//相当于下面两条指令
git branch dev
git checkout dev
//下面指令查看当前分支，*支出当前工作分支
git branch
//切换分支
git checkout master
//删除分支
git branch -d \<name\>
```
合并分支`git merge dev`要在*主分支*下进行此操作，根据更改不同，合并方式不同。

### 冲突
冲突是两个分支更改了同一个内容，比如原来是1234,现在一个改成1224,另一个改成1334,这时候Git无法正常合并，所以只能手动修改冲突，可以适当使用`git add`，会在有冲突的文件里做出标记。<====和====>
最后可以使用`git log --graoh`查看分支合并图

- 当只有一个分支(fu)有更改时,会使用`Fast forward`，这可能会造成不好的影响。可以在合并时使用
```
git merge --no-ff -m"merge without ff" dev
```
这次合并会创建一个新的commit.
***`master`分支应该作为稳定分支，平时作业不应该在`master`上，应选择创建其他分支来工作***

- 当不得不停下手头工作去更改本项目其他地方临时出现的紧急问题，但是手头工作又无法提交，此时可以使用`git stash`暂时储存。然后切换到需要工作的分支，去处理紧急问题，当处理完成后，回到暂停分区，然后`git stash list`查看工作储存内容。一种使用`git stash apply`来回复，但是回复后，stash内容并不被删除，需要使用`git stash drop`来删除。另一种方法是`git stash pop`，恢复的同时删除stash；之上可以使用多次`git stash`，恢复时先使用`git stash list`查看（会给出号码）。然后使用`git stash apply stash@{0}`恢复


September 23, 2018 10:26 PM

