title:  LearnGit
date: 2018年7月16日18:06:46
categories: Git
tags: 

	 - Git命令
cover_picture: /images/common.png
---

##### 流程

1. 初始化：git init
2. 添加所有：git add .
3. 本地提交信息：git commit -m'***'
4. 远程仓库添加：git remote add origin git@gitee.com:****
5. 远程仓库更换：git remote set-url origin git@gitee.com:****
6. 远程提交：git push origin master
7. 查看本地分支：git branch 
8. 查看远程分支 ：git  branch  -a 
9. 拉取远程分支并在本地创建：git  checkout  -b  xxx  origin  origin/xxx
10. 切换本地分支：git  checkout  xxx 
11.  查看本地Log：git log
12. 查看远程Log：git log origin/master
13. Log信息一行：git log --pretty=oneline
14. 合并
    1. 将****分支合并到当前分支:git merge  *** 



##### 错误

1. fatal: refusing to merge unrelated histories
   - git pull origin master --allow-unrelated-histories 