##git 命令

- 查看分支： git branch/git branch -r

- 切换分支  git checkout [name]

- 新建分支  git checkout -b [name]
- 推送分支  git push origin [name]
- 关联远程分支 git branch --set-upstream-to=origin/[name]

-# 新建一个分支，指向某个tag  git checkout -b [branch] [tag]

- 拉去代码 git pull
- #合并指定分支到当前分支  git merge [branch]   (git checkout [commit] [file])

-- 提交代码 1. git add . (#或者 git add [name])  2. git commit -m [message] （#添加记录） 3. git push （#推送到分支）

-- 拉去克隆项目  git clone [url]

-- # 删除本地分支 git branch -d [branch-name]

-- #删除远程分支 git push origin --delete [branch-name]
