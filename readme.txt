window 系统
//为机器命名
	git config --global user.name "Your Name"
	git config --global user.email "email@example.com"
//安装版本库	
	git  init 
//查看当前工作区状态 
	git status 
//提交变化
	git add fileName
	git add .  所有变化提交到暂存区,包括文件内容修改以及新文件,但不包括被删除的文件
	git add -u 所有变化提交到暂存区,包括文件内容修改以及删除的文件,但不会提交新文件
	git add -A 所有变化提交到暂存区
//提交git仓库
	git commit -m "注释"  
//查看文件具体修改了什么内容
	git diff fileName  
//可以查看工作区和版本库里面最新版本的区别
	git diff HEAD -- fileName
//命令显示从最近到最远的提交日志  主要查看commit id 以作版本回退
	git log --oneline 
//版本回退
	git reset --hard 版本(HEAD[当前版本],HEAD^[上个版本],HEAD^^[上上个版本],HEAD~1000[上1000个版本])或者 commit  id
//用来查看记录你的每一次commit命令  关掉命令行终端后,上次git log 记录就没了 可以用这个查看
	git reflog
//可以撤销工作区的修改  工作区的修改全部撤销  修改文件后没用 git add的时候(没有被放到暂存区)
	git checkout -- fileName
//可以把暂存区的修改撤销掉,重新放回工作区(在git commit之前 git add之后)
	git reset HEAD file
//假设改错了东西，还从暂存区提交到了版本库 (git commit之后) 
	可以使用上面的版本回退    git reset --hard 版本或者commit名
一旦提交推送到远程版本库 就完了

删除文件
	rm fileName  //与git rm不同,这个只删除文件 不删除git的修改,直接用git rm 就可以删除了
//文件就从版本库中被删除
	git rm fileName     然后才用git  commit  提交版本库

远程仓库
本地Git仓库和GitHub仓库之间的传输是通过SSH加密的,需要一点设置
https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374385852170d9c7adf13c30429b9660d0eb689dd43a000

//关联远程仓库
	git remote add origin(远程仓库的默认名称是origin)  SSH或者HTTPS URL
//查看远程库的信息
	git remote //用git remote -v显示更详细的信息 

//把本地库的内容推送到远程 (把当前分支master推送到远程)
//第一次推送master分支时 加上了-u参数Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来,之后不需要加-u参数
	git push -u origin(远程仓库的默认名称是origin)  master(推送分支名)
//推送分支,就是把该分支上的所有本地提交推送到远程库
	git push origin(远程仓库的默认名称是origin) master(推送分支名)

//从远程库克隆
	git clone  远程库地址

//取回远程主机某个分支的更新，再与本地的指定分支合并
	git pull 远程仓库 远程分支名:本地分支名
	
分支
//我们创建分支，然后切换到分支：
	git checkout -b 分支   ===  git branch 分支 (创建分支) && git checkout 分支 (然后切换到分支)
//创建远程仓库的分支到本地
	git checkout -b   分支   远程仓库/分支

//查看分支
	git  branch  
//关联远程分支
	git branch --set-upstream 分支 origin(远程仓库)/分支
//合并分支
	git merge 分支
//合并dev分支，请注意--no-ff参数，表示禁用Fast forward(快速模式 合并后分支合并图看不出合并过)   
	git merge --on-ff -m "注释"  分支
//删除分支
	git branch -d 分支  
	git branch -D 分支   //强行删除分支
//查看分支合并图
	git log --graph --oneline --abbrev-commit
//变基  可以保持一个漂亮而干净的历史提交记录。 
//在当前分支上使用git rebase变基操作目标基底分支(master)
	git rebase 目标基底分支
//当前工作现场“储藏”起来，等以后恢复现场后继续工作
	git stash    //在开发时突然有别的事情做,开启另外debugger 分支是用,或者停止工作时
//可以查看Git把stash内容存在某个地方
	git stash list
//恢复git stash  
	git stash  apply   stash@{0}  //恢复后，stash内容记录并不删除  
	git stash drop //需要用git stash drop来删除；
	git stash pop //恢复的同时把stash内容记录也删

创建标签 不写commit_id 默认当前版本
//打开标签 
	git tag  标签名(v0.1) 
//创建带有说明的标签，用-a指定标签名，-m指定说明文字 
	git tag -a 标签名 -m  "说明文字"" commit_id?
//可以通过-s用私钥签名一个标签 必须首先安装gpg（GnuPG）
	git tag -s 标签名 -m "说明文字" commit_id?
//给特定的commit版本打标签
	git tag 标签名  commit_id
//查看所有标签
	git tag
//查看标签信息
	git show 标签名
//删除标签
	git tag -d 标签名
//推送某个标签到远程
	git push  origin(远程库)  标签名
//一次性推送所有标签名
	git push origin(远程库) --tags
//删除远程标签  ,先删除本地标签
	git push origin(远程库)  :refs/tags/标签名
//Git显示颜色，会让命令输出看起来更醒目
	git config --global color.ui true
//忽略特殊文件
	创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去,Git就会自动忽略,
	git add   在.gitignore文件里的被忽略的文件时不能操作,可以使用强制添加
	git add -f 在.gitignore文件里的被忽略的文件
//可以用git check-ignore命令检查文件被那个规则忽略
	git check-ignore -v  filename

配置命令行别名 --global参数是全局参数 对当前用户起作用的，如果不加，那只针对当前的仓库起作用
//告诉Git，以后st就表示status
	git config --global alias.st status
//命令git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区
//配置一个unstage别名
	git config --global alias.unstage 'reset HEAD'  
搭建Git服务器
	https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000