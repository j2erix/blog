## Git中的撤销操作：checkout、revert、reset和clean
*2015年6月5日下午2:37 by j2erix*

Git提供了一系列命令用于进行撤销相关的动作，包括检出过往提交、撤销或重置回某次提交状态、清除未跟踪的文件等。

### git checkout - 检出
git checkout有三种功能:

1. **git checkout <branch>**  
检出某个分支的最新版本

2. **git checkout <commit>**  
检出当前分支在某次提交时候的版本

3. **git checkout <commit> <file>**  
检出当前分支某个文件在某次提交中的版本

1和2类似，都属于检出某次提交。检出提交属于只读操作，并不会影响资源的状态，只是将HEAD指向了之前的某次提交。  
此时的HEAD的状态被称作“detached head”，可以通过下图更好的理解：

![Checking out a previous commit](https://www.atlassian.com/git/images/tutorials/getting-started/undoing-changes/01.svg)

与之相反，检出某个文件并非只读，而是会对资源产生影响。你可以重新提交检出的文件，就好像你手动对文件进行了编辑一样。  
所以，检出文件可以更好的理解为将文件回滚都以前某个版本：

![Checking out a previous version of a file](https://www.atlassian.com/git/images/tutorials/getting-started/undoing-changes/02.svg)

### git revert - 恢复

1. **git revert <commit>**  
恢复某次提交所做的改动

为了保证历史的完整，git revert命令不会删除某次提交，而是计算出怎样能够恢复，并将计算结果放入Staging Area，可以用于提交。  
这样，一份undo操作，就变成了一份do操作，既能达到恢复的效果，又能保证历史的完整。

![](https://www.atlassian.com/git/images/tutorials/getting-started/undoing-changes/03.svg)

### git reset - 重置
如果说git revert是一种安全的撤销操作，git reset就是一种危险的操作。

git reset也有多种用法:

1. **git reset <file>**  
将Staging Area的某个文件移出，不改变Working Directory。等同于在Tower中的unstage操作。

2. **git reset**  
将Staging Area置回最近一次提交，不改变Working Directory。等同于Tower中的stage all操作。

3. **git reset --hard**  
将Staging Area和Working Directory都重置回某次提交的版本。危险：这将会丢失本地目录中的改动。

4. **git reset <commit>**  
将当前分支回退到指定版本，不改变Working Directory，当对之前的提交不满意的时候，这时候可以重新提交。

5. **git reset --hard <commit>**  
将当前分支回退到指定版本，Staging Area和Working Directory都被回退，所以属于危险操作！

下图展示了reset与revert的不同：

![Reverting & Reseting](https://www.atlassian.com/git/images/tutorials/getting-started/undoing-changes/06.svg)

### git clean - 清除
git clean操作用于将未追踪——从未add过——的文件清除，它有以下几个用法：

1. **git clean -n**  
显示将会被清除的文件

2. **git clean -f**  
强制清除未追踪的文件，ps：-f 被要求是为了保险。

3. **git clean -df**  
清除未追踪的文件及目录。

4. **git clean -xf**  
清除为追踪的文件以及git ignore忽略的文件。

git clean和git reset经常一起使用，用于讲分支彻底重置会某个版本，如下：

1. **git reset –hard**  
将对已追踪的文件的修改置回最近一次提交的版本。
2. **git clean -df**  
将未追踪的文件和目录清除出Staging Area和Working Directory。

【参考】：[https://www.atlassian.com/git/tutorials/undoing-changes](https://www.atlassian.com/git/tutorials/undoing-changes)
