# 使用码云(Gitee)+typora+git做笔记

### 笔记神器: 码云+typora+git同步

### 码云相比于github的优势:

- 免费用户也可以创建私有目录
- 5G存储空间

### 令人惊喜的地方:

github目录可以直接转移到码云.

本地目录可以同时添加GitHub和Gitee远程仓库.



具体步骤:

1. 添加git生成的ssh key到码云:

   ​	windows下, ssk key在C:\Users\sky_73\ .ssh\id_rsa.pub

2. 从GitHub下载到本地的目录,再同步到码云: 
   1. 先给目录添加码云的远程代码库, 参考step1
   2. git remote add gitee git@gitee.com:hongbozhang/reponame.git
   3. 推送到码云, 参考step2

3. 本地目录同步到码云:

   1. 在码云上新建和本地目录local_directory同名的目录
   2. cd local_directory
   3. git init
   4. git add -A
   5. git commit -m "your comment"
   6. git push gitee master



step1.给目录添加远程代码仓库, 关联码云:

```
cd 到git目录
git init
git remote add gitee git@gitee.com:hongbozhang/目录名.git
git remote -v   //查看远程库信息:
```

或者新建一个空目录:

```
mkdir folder_name
cd folder_name
git init
git add file.xxx  (或者用git add -A添加所有(有变化)文件)
git commit -m "your comment"
```



step2. 推送到码云:
1. 在码云上建立相同目录名的目录
2. 推送:

```
git push gitee master
```



这样一来，我们的本地库就可以同时与多个远程库互相同步： 

```
┌─────────┐ ┌─────────┐
│ GitHub  │ │  Gitee  │
└─────────┘ └─────────┘
     ▲           ▲
     └─────┬─────┘
           │
    ┌─────────────┐
    │ Local Repo  │
    └─────────────┘
```



参考:

[廖雪峰使用码云](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00150154460073692d151e784de4d718c67ce836f72c7c4000)

[廖雪峰git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)