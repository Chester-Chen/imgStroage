---
date: 2019/4/26 23:36:17 
title: git常用命令
categories: git
tags:
 - 随笔
 - git
---

# 1、安装完成后，git bash 执行一下两句

命令：git config --global user.name +用户名

命令：git config --global user.email +邮箱


# 2、生成公钥和私钥的命令：ssh-keygen -t rsa

后面会提示是否设置密码，如果设置了密码，每次使用到git都会用到密码（就会特别麻烦，所以直接回车为空）

果断选择回车3下，ok。

# 3、查看公匙  

命令：cat ~/.ssh/id_rsa.pub  

# 4、测试是否连接成功

命令：ssh -T git@github.com

# 5、一些个人常用的基本命令

## 5.1创建版本库

```
命令: 
mkdir learntGit         在当前目录下创建learntGit文件夹
cd  learnGit            进入learnGit文件夹中
pwd                     查看当前路径
git init                将当前目录变成Git可以管理的仓库
git add + 文件名         将文件添加到仓库，可多次提交（添加成功，没有任何显示）
git commit -m + 提交说明  把文件提交到版本库

示例: 提交3个文件到版本库中
git  add  file1.txt
git  add  file2.txt  file3.txt

我一般会用 git add . （直接提交所有更改，简单快捷）

git commit -m "此处填写此次提交的描述"
```
## 5.2查看版本状态

- 命令: 
```
git  status  查看当前版本库的状态
git  diff    查看当前相对上一次提交修改的内容
```
## 5.3版本回退

```
命令: 
git log                          显示从最近到最远的提交日志
git log   --pretty== oneline     显示log,但是不显示很多凌乱的信息
q                                显示log版本信息有很多，使用q键停止查看
git reset —hard head^         回退到上一个版本
git reset —hard head^^        回退到上上个版本
git reset —hard head~100      回退到之前100个版本
git reset —hard +commit_id    回到某个版本号的版本
```
# 6、本地仓库与远程仓库的关联

```
命令: 
git remote add origin +远程仓库地址   将本地仓库关联远程仓库
git push -u origin master           第一次推送master分支的所有内容到远程仓库
git push origin master              本地推送到远程(第一次之后）
```
# 7、从远程仓库克隆到本地

```
命令: 
git clone +远程仓库地址   克隆远程仓库到本地，相当于创建了与之关联的本地仓库

示例: 
先使用cd命令切换到某个文件夹位置然后使用如下命令:
git clone git@github.com:example.git
```
# 8、把本地分支推送到远程仓库
 git push的一般形式为 git push <远程主机名> <本地分支名>  <远程分支名> ，例如 git push origin master：refs/for/master ，
 即是将本地的master分支推送到远程主机origin上的对应master分支， origin 是远程主机名，

```
git push origin master
(如果远程分支被省略，如上则表示将本地分支推送到与之存在追踪关系的远程分支（通常两者同名），如果该远程分支不存在，则会被新建)
```
