#### git学习

![](C:\Users\luonN\Documents\我的形状\111.png)





##### git操作

1.*git init* 在文件夹下初始化git 。生产.git 目录，是本地存放git的基本 信息不饿能删除

2.设置签名  标识开发人员的身份，这个签名和代码托管中心没有任何关系

 	2.1 命令 设置用户有效范围 

​			a.项目级别  **git config** user.name luo   git config user.email 1992@luo.com

​			b.系统用户级别  ***git config -- global***

​			c.两者都有时，采取就近原则，二者不能都没有，要有其一

​			d.保存在.git/config 下面的文件中

3.**git status** 

4.**git add good.txt ** 提交文件

5.**git rm --cahed good.txt** 从暂存区删除文件

6.**git commit**  提交代码

7.版本的前进和后退

​	7.1 **git log --pretty=oneline** 查看日志 git reflog

​	7.2 head 就是git指向当前版本的指针

​	7.3 基于索引值去操作  **git rest --hard** 索引值

​	7.4 使用  git reset --hard HEAD^  一个^是退一步

​	7.5 使用 git reset --hard HEAD~5  后退5步

​	7.6 git reset --soft 仅仅在本地库移动指针

​	7.7 git reset --mixed 不在工作区移动指针 

​	7.8 hard 是工作区，暂存区，本地库都会移动指针

8. 永久删除找回
   1. rm good.txt 删除文件
   2. 用git reset --hard 索引  退回到以前的版本
9. 比较文件
   1. git diff good.txt 和暂存区进行比较
   2. git diff HEAD^ 
10. 合并分支
    1. git branch -v
    2. git branch hotfix 创建分支
    3. git check out hotfix
    4. 合并代码要切换到合并的目的分支，在git merge  hotfix



