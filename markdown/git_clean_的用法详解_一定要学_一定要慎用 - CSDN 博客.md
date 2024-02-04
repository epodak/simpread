> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_44137575/article/details/108142088)

git clean 从你的工作目录中删除所有**没有 tracked，没有被管理**过的文件。

#### 太可怕，删除了就找不回了，一定要慎用。但是如果被 git add . 就不会被删除。

> **git clean 和 git reset --hard 结合使用。**
> 
> clean 影响没有被 track 过的文件（清除未被 add 或被 commit 的本地修改）
> 
> reset 影响被 track 过的文件 （回退到上一个 commit）
> 
> 所以需要 clean 来删除没有 track 过的文件，reset 删除被 track 过的文件
> 
> 结合两命令 → 让你的工作目录**完全**回到一个指定的 <commit> 的状态

用法详解
----

> **参数说明：**
> 
> **n** ：显示将要被删除的文件
> 
> d ：删除未被添加到 git 路径中的文件（将 .gitignore 文件标记的文件全部删除）
> 
> f ：强制运行
> 
> x ：删除没有被 track 的文件

```
git clean -n
// 是一次 clean 的演习, 告诉你哪些文件会被删除，不会真的删除
 
git clean -f
// 删除当前目录下所有没有 track 过的文件
// 不会删除 .gitignore 文件里面指定的文件夹和文件, 不管这些文件有没有被 track 过
 
git clean -f <path>
// 删除指定路径下的没有被 track 过的文件
 
git clean -df
 
// 删除当前目录下没有被 track 过的文件和文件夹
 
git clean -xf
 
// 删除当前目录下所有没有 track 过的文件.
// 不管是否是 .gitignore 文件里面指定的文件夹和文件
 
git clean 
// 对于刚编译过的项目也非常有用
// 如, 他能轻易删除掉编译后生成的 .o 和 .exe 等文件. 这个在打包要发布一个 release 的时候非常有用
 
git reset --hard
git clean -df
git status
// 运行后, 工作目录和缓存区回到最近一次 commit 时候一摸一样的状态。
// 此时建议运行 git status，会告诉你这是一个干净的工作目录, 又是一个新的开始了！
```

**还有一个 very important 要说**

**git add .  git commit  后一定要记得 git push ，不然你又找不回来了。**