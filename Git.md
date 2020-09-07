# Git基础

## Git配置

```
// 配置姓名
git config --global user.name 提交者姓名
// 配置邮箱
git config --global user.email 提交者邮箱
// 查看配置信息
git config --list
```

## 提交步骤

1. `git init`  初始化git仓库
2. `git status`  查看文件状态
3. `git add 文件列表`  追踪文件 添加到暂存区
4. `git commit -m 提交信息`  向仓库中提交代码
5. `git log`  查看提交记录

## 撤销

- `git checkout 文件`  用暂存区的文件覆盖工作目录的文件
- `git rm --cached 文件`  将文件从暂存区删除
- `git reset --hard commitID`  将git仓库指定的更新记录恢复出来，并且覆盖暂存区和工作目录

## 分支

### 分支细分

- 主分支(master)：第一次向git仓库提交更新记录自动产生的分支
- 开发分支(develop)：作为开发的分支，基于master分支创建
- 功能分支(feature)：作为开发具体功能的分支，基于开发分支创建

### 分支命令

- `git branch`  查看分支
- `git branch 分支名称`  创建分支
- `git checkout 分支名称`  切换分支
- `git merge 来源分支`  合并分支
- `git branch -d 分支名称`  删除分支（分支被合并后才允许删除，`-D` 强制删除）

## 暂时保存更改

- `git stash`  存储临时改动
- `git stash pop`  恢复改动

# Github

**推送到远程仓库**

```
// git push 远程仓库地址 分支名称
git push https://github.com/motherloveyou/git-demo.git master

// git remote add 远程仓库地址别名 远程仓库地址
git remote add origin https://github.com/motherloveyou/git-demo.git
// git push -u 远程仓库地址别名 分支名称	-u:记住推送地址及分支，下次推送只需要输入git push
git push -u origin master
```

**克隆仓库**

```
git clone 远程仓库地址
```

**拉取远程仓库最新版本**

```
git pull 远程仓库地址 分支名称
```

**ssh**

```
// 生成密钥
ssh-keygen
```

**git忽略清单**

- git忽略清单文件名称：`.gitignore`
- 将工作目录的全部文件添加到暂存区 `git add .`