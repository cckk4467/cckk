#对一些git命令的总结，不是tutorial哦

首先，脑袋里一定要清楚三个概念的联系：工作区->缓存区->仓库

*** git add -A  ***添加修改到缓冲区
*** git commit -m "describe"  ***移动缓冲区文件到仓库
*** git reset --hard HEAD^  ***回退到上一个版本HEAD^^  HEAD~100
*** git checkout -- *.*  ***把暂存区清空并把工作区的修改还原为最新版本库

###分支
####性质：从原分支分出去会继承原分支的工作区的缓冲区，原分支的工作区的缓冲区会丢失

*** git branch *** 查看分支，带*号的是当前分支
*** git branch *** xxx 创建xxx分支
*** git checkout *** xxx 切换到xxx分支
以上两句等同于
*** git checkout -b *** xxx创建并切换到
*** git merge xxx ***  融入xxx分支到当前分支
*** git branch -d *** xxx 删除分支(d大写是强制删除)
 
*** 先创建新的分支，切换并在其上面工作，最后记得commit提交内容到分支
切换回master，合并分支到master，然后删除分支，搞定 ***
当然你也可以很暴力地直接在master上操作=。=

###暂存区
####性质：在原分支上申请一个暂存区，把原分支的工作区移动进去，并清除原分支的缓存区，达到清空原分支工作区的缓冲区的效果，还原时，只能还原暂存区保存的工作区内容了

*** git stash *** 在当前分支上，丢失当前缓冲区的内容，保存工作区的内容
*** git stash pop *** 在当前分支上，还原之前工作区的所有内容




>因此，多人协作的工作模式通常是这样：

>首先，可以试图用git push origin branch-name推送自己的修改；

>如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

>如果合并有冲突，则解决冲突，并在本地提交；

>没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

>如果git pull提示“no tracking >information”，则说明本地分支和远程分支的链接关系没有创建，用命令git >branch --set-upstream-to=origin/<branch> <branch>

>这就是多人协作的工作模式，一旦熟悉了，就非常简单。


##附：git的fetch和pull用法

>git fetch origin master:cc >获取远程分支合并到本地cc分支上，若cc不存在则创建它
>git diff cc
>>(这里用的diff语法是:git diff branchName filepath 
>>当前分支的文件与branchName 分支的文件进行比较)

>git merge cc     (这里会自动融合文件中不同的地方就是用什么>>><<<===来分隔)
>git branch -d cc

>以上效果等同于
>git pull orgin master:master

###疑问：
fetch后的内容是在cc分支上的缓冲区还是commit后的仓库
###猜测：
因为diff若在缓冲区有内容时比较工作区和缓冲区的差别，否者比较工作区和已commit的仓库
所以fetch数据去到的是cc分支的缓冲区？或者是其仓库

###实验：解决上述疑问
试试git pull orgin master:cc
看看cc内容分布情况

###实验结果：
*** git fetch cc后会直接放到cc的仓库，然后再merge合并 ***
git  pull orgin master:cc 看不懂什么情况，反正怎么样都会直接合并冲突到master里2333