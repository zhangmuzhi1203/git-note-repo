# Git

为了区分git和Linux的命令 git的命令前都加了 git

仓库中的.git目录是git仓库的重要的一个目录 如果.git目录被删除了则该目录就不是仓库目录了

1. 省略(local)：本地配置，直对本地仓库有效
2. --global：全局配置，所有仓库生效
3. --system：系统配置，对所有用户生效

## 1.初始化设置

```js
git config --global user.name 'zhangmuzhi1203'  //配置用户名
git config --global user.email 'luolan1234569@gmail.com'//配置邮箱
git config --global credential.helper store //存储配置
git config --global --list //查看git的配置信息
```

## 2.创建仓库相关命令

仓库就相当于一个文件目录 文件目录里有.git才表示git仓库 删除.git就不表示git仓库了

```js
mkdir gitStudy //创建文件夹
ls //查看当前目录有哪些文件
ls -a //列出所有文件，包括隐藏文件（以.开头的文件）。
ls -l//以长格式列出文件的详细信息，包括权限、链接数、所有者、文件大小和修改时间等。
ls -t//按照修改时间排序，最近修改的文件将显示在最前面。
ls -r//逆序排列文件列表（通常与其他选项组合使用，如 -ltr 表示按时间排序但逆序排列）。
ls -altr//组合
git init (projectname)// 再一个目录里使用这个命令 就是新建git仓库 方法一
git clone //再一个目录里使用这个命令 就是新建git仓库 方法二
rm -rf .git//强制删除文件
```

## 3.四个区域

```js
//工作区
就是你在电脑里能实际看到的目录,.git所在目录
//暂存区
暂存区也叫索引， 用来临时存放即将提交到Git仓库的修改内容， 一般在.git目录下的index中。
//本地仓库
Git在本地的版本库， 仓库信息存储在.git这个隐藏目录中 就是git init 和 git clone 创建的仓库
//远程仓库
托管在远程服务器上的仓库, 常用的有GitHub、 GitLab、 Gitee
//整个流程
先在工作区的文件里写完内容后 git add 提交到暂存区 然后再将暂存区的内容修改 git commit 提交到本地仓库
```

## 4.git文件中的状态

```js
//1.未跟踪
就是新创建还没有被git管理起来的文件
//2.未修改
就是被git已经管理起来但是文件内容没有发生变化没有修改
//3.已修改
就是被修改但是还没有放到暂存区的文件
//4.已暂存
修改后已经放到暂存区但是还没有提交的文件
//5.已提交
把暂存区的文件提交到本地仓库后的状态
```

## 5.添加和提交文件

文件只有被提交到本地仓库中才算真正的保存起来

1.当把f1.txt文件添加到暂存区后 此时工作区和暂存区都存在该文件 

2.当修改工作区f1.txt文件或者删除f1.txt文件后 此时查看仓库状态 会显示Changes not staged for commit 

3.并且工作区没了该文件 暂存区还存在  可以使用 `git rm` 命令或者 `git add` 命令 此时就告诉git要删除暂存区这个文件

4.**如果你不想删除该文件，想恢复它**：git restore f1.txt 这条命令会把 `f1.txt` 恢复到删除之前的状态（即从 Git 历史中恢复该文件）。

```js
git status //查看仓库状态 红色文件是未跟踪文件 绿色文件是已暂存文件
git add 文件名//添加到暂存区
git add *.txt//通配符方式提交到暂存区
git add .//把当前文件夹下的所有文件添加到暂存区内 这个.就是当前目录
git rm 文件名//从工作区和暂存区删除一个文件 
git rm -f 文件名//删除文件
git rm --cached 文件名//把添加到暂存区的文件重新变成未跟踪文件 从索引/暂存区中删除文件，但是本地工作区文件还在， 只是不希望这个文件被版本控制
git commit -m '提交信息'//提交到本地仓库 只会提交暂存区的文件
git commit -a -m 'xxx'//这个命令可以同时实现暂存提交两个操作 -a -m
git log //查看提交记录
git log --oneline//查看简洁的提交记录
git reflog //查看操作的历史记录
//小案例
echo '第一个文件' > file1.txt //创建一个文件且含义内容
cat file1.txt //查看文件内容
git status //可以看到红色的未跟踪文件 file.txt
git add file.txt //把 file.txt文件添加到暂存区
git status//可以看到绿色的已暂存的文件
git rm --cached file.txt
git status//可以看到红色的未跟踪文件 file.txt
mv file.txt file1.txt //修改文件名
```

## 6.git reset 回退版本

可以退回到之前的某一个提交状态

```js
//git reset的三种模式
git reset --soft 版本号 //soft参数表示回退到某一个版本并保留工作区和暂存区的内容
git reset --hard 版本号//hard参数表示回退到某一个版本不保留工作区和暂存区的内容
git reset --mixed 版本号//mixed参数表示回退到某一个版本保留工作区的内容不保留暂存区的内容 mixed是git reset命令的默认参数
git reset HEAD^  //表示上一个版本
git ls-files//查看暂存区和已经提交的文件
cp -rf file newfile//文件复制
```

## 7.使用git diff查看差异

1. 查看工作区，暂存区，本地仓库之间的差异
2. 查看一个文件不同版本之间的差异
3. 查看不同分支之间的差异
4. git diff 默认不加参数 是查看工作区和暂存区之间的差异 它会显示发生更改的文件以及更改的详细信息

## 8.删除文件

```js
rm file;git add file//1. 先从工作区删除文件，然后再将暂存区内容删除
git rm <file>//2.把文件从工作区和暂存区同时删除
git rm --cached <file>//3.把文件从暂存区删除，但保留在当前工作区中
git rm -r *//递归删除某个目录下的所有子目录和文件
//删除后不要忘记提交
```

## 9.SSH配置和克隆远程仓库

```js
git clone 仓库ssh地址 //克隆远程仓库到本地 这个命令直接把本地仓库和远程仓库关联 后续操作和标题10内容一样
//一般第一次的时候就会报错 这是因为我们没有配置ssh密钥 找到.ssh文件进入里面创建密钥
ssh-keygen -t rsa -b 4096//生成密钥 -t指定协议 rsa -b指定生成的大小4096
//然后会提示输入文件名字 第一次 默认回车就行 如果不是第一次指定一个名字就行
//接着会说是否输入密钥短语来保护密钥就是输入密码的意思 可以输入 可以直接回车 然后就生成成功了
id_rsa id_rsa.pub//第一次会生成这两个文件 不带.pub的是私钥文件谁都不要给 带.pub的是公钥文件
//如果不是第一次配置密钥而是额外生成第二个密钥 这个时候想用这个密钥连接github需要进行如下配置
tail - 5 config
# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/xxx

//本地仓库修改完后 不会影响远程仓库 这个时候需要使用push命令
git push <remote><branch>//推送更新内容 如果已经clone了远程项目到本地仓库 可以直接在这个项目里输入git push即可推送更新内容
git pull <remote>//拉取更新内容
```

## 10.关联本地仓库和远程仓库

如果本地已经有仓库了 怎样把它放到远程仓库里面呢

```js
//1.首先还是要在github上创建一个新仓库
//2.复制仓库ssh地址
//3.命令行进入本地仓库
//4.输入git remote add origin git@github.com:zhangmuzhi1203/firstRep.git origin 这个命令的意思是在这个本地仓库添加一个远程仓库 默认远程仓库别名一般都是这个可以改
//5.在要和远程仓库关联的那个本地仓库输入上述命令
//6.接着可以输入 git remote -v 这个命令可以查看当前仓库所对应的远程仓库的别名和地址
//7.git branch -M main 这个命令可以指定本地分支为main 如果本地分支就为main 可以省略此步骤
//8.git push -u origin main:main 最后一步把本地仓库与远程仓库关联起来 main:main就是把本地仓库main分支与远程仓库main分支关联起来。如果本地分支与远程分支名称相同就可以省略直接写一个名称就行 如 main:mian => main。-u 选项的意思是 将本地分支与远程分支进行关联。不带 -u：你每次推送时都需要明确指定远程仓库和分支。带 -u：在第一次推送时加上 -u，后续推送和拉取时，你只需要输入 git push 或 git pull，Git 会知道你想要推送或拉取的远程分支是 origin/main。 
//9.使用8这个命令一定要确保本地仓库有提交文件 不然报错
//10.接着就可以修改文件更改文件新建文件等等操作 然后git add git commit -m 最后git push
//11.如果远程仓库发生了修改要想同步到本地仓库需要 git pull <remote> 命令
//12.即git pull origin main 这个命令就是把远程仓库的main分支内容拉取到本地再进行合并 因为本地和远程都是main分支所以省略写法
```

## 11.分支Branch

分支可以看成代码库中的不同版本 可以独立存在 并且有自己的提交记录

不同开发人员可以在自己的分支上工作 不影响其他分支的工作 最后在合并到主线代码库中

```js
//这些命令前提是当前仓库要有提交否则会报错
git branch //查看当前仓库中有多少分支
git branch 分支名//创建一个新的分支需要注意的是只是创建并没有切换到这个分支上
git checkout 分支名//切换到指定的分支上去 需要注意的是这个命令有潜在的问题
//潜在的问题：
//这个命令还可以恢复文件 比如我们意外修改某个文件 这个时候我们就可以使用这个命令恢复到修改之前的状态 如果这个时候分支名称和文件名称相同的话就会出现歧义 git checkout 这个命令会默认切换分支而不是恢复文件 为了避免歧义 git新推出了一个新命令 git switch 这个命令专门用来切换分支
//假设现在有两个分支 main主分支 和dev分支 再main分支上提交了一个f1.txt文件 切换到dev分支在这个分支上提交了f2.txt文件 那么可以在dev分支上查看到main分支和dev分支所有文件 如果切换到main分支只能查看到main分支文件 那是因为 dev分支提交内容还没有合并到main分支上
git merge 要被合并的分支名//这个命令就是合并分支 比如我们要把dev分支合并到main分支就要先切换到main分支 在输入git merge dev
git log --graph --oneline --decorate --all//这个命令是用来查看分支图的
//合并后再git branch 还是可以看到dev分支的存在的 合并后分支不会被删除掉 还是存在 除非手动删除
git branch -d dev //如果一个分支不需要了话 -d参数就表示 可以用这个命令删除合并后的分支
git branch -D dev //这个命令就表示删除没有合并的分支 这个参数-D表示强制删除分支
```

## 12.解决合并冲突

```js
/*
如果两个分支修改了一份文件的不同代码那么合并的时候就不会发生冲突 
如果两个分支修改了同一份文件的相同部分代码 那么git就不知道该保存哪个分支修改后的代码 那么合并的时候就会发生冲突 这个时候需要手动解决冲突 比如main分支有一个f1.txt文件dev分支对f1.文件进行修改 然后提交 切换到main分支对f1.txt文件进行同一位置修改后提交接着把dev分支合并到main此时会发生冲突错误 把冲突文件的内容修改后 再重新进行提交就行了 提交之后自动完成合并操作
git merge --about 终止这次合并
*/
```

## 13.回退和rebase

```js
/*
合并分支的另一种命令 git rebase name
rebase命令可以在任意分支上操作 merge只能在主分支上操作
现在还是两个分支 main和dev 此时在dev分支上用 git rebase main命令 此时就会找到main和dev分支的共同节点就是开启dev分支的那次操作记录 然后再找到main分支上最新的提交记录接着把dev整个的移动到main分支的最新提交记录的后面
如果在main分支上执行 git rebase name 此时就会找到main和dev分支的共同节点就是开启dev分支的那次操作记录 然后把main分支上从这个共同节点这个地方到main分支最新的提交记录这一部分全部移到dev的最新提交后面
*/
//alias 可以设置命令别名
alias graph = 'git log --graph --oneline --decorate --all'
graph//直接就可以查看分支图了
git reset --hard 命令编码// 将仓库回退到某一个时间点
/*
git merge 
优点：不会破坏原分支的提交历史，方便回溯和查看
缺点：会产生额外的提交节点，分支图比较复杂
git rebase
优点：不会新增额外的提交记录，方便回溯和查看
缺点：会改变提交历史，改变了当前分支branch out的节点 避免在共享分支使用
*/
```

