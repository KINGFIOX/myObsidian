## 1、Git概述

 　　Git是一个免费的、开源的分布式版本控制系统，可以快速高效地处理从小型到大型的各种项目。Git 易于学习，占地面积小，性能极快。 它具有廉价的本地库，方便的暂存区域和多个工作流分支等特性。其性能优于 Subversion、CVS、Perforce 和 ClearCase 等版本控制工具。

## 1.1 何为版本控制

版本控制是一种记录文件内容变化，以便将来查阅特定版本修订情况的系统。  
　　版本控制其实最重要的是可以记录文件修改历史记录，从而让用户能够查看历史版本，方便版本切换。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618140639960.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 1.2 为什么需要版本控制

个人开发过渡到团队协作  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618140735549.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 1.3 版本控制工具

-   集中式版本控制工具  
    　　CVS、SVN(Subversion)、VSS……  
    　　集中化的版本控制系统诸如 CVS、SVN 等，都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。多年以来，这已成为版本控制系统的标准做法。  
    　　这种做法带来了许多好处，每个人都可以在一定程度上看到项目中的其他人正在做些什么。而管理员也可以轻松掌控每个开发者的权限，并且管理一个集中化的版本控制系统，要远比在各个客户端上维护本地数据库来得轻松容易。  
    　　事分两面，有好有坏。这么做显而易见的缺点是中央服务器的单点故障。如果服务器宕机一小时，那么在这一小时内，谁都无法提交更新，也就无法协同工作。  
    　　![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618141524208.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)
-   分布式版本控制工具  
    　　Git、Mercurial、Bazaar、Darcs……  
    　　像 Git 这种分布式版本控制工具，客户端提取的不是最新版本的文件快照，而是把代码仓库完整地镜像下来（本地库）。这样任何一处协同工作用的文件发生故障，事后都可以用其他客户端的本地仓库进行恢复。因为每个客户端的每一次文件提取操作，实际上都是一次对整个文件仓库的完整备份。分布式的版本控制系统出现之后,解决了集中式版本控制系统的缺陷:
    1.  服务器断网的情况下也可以进行开发（因为版本控制是在本地进行的）
    2.  每个客户端保存的也都是整个完整的项目（包含历史记录，更加安全）  
        ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618141616733.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 1.4 Git简史

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021061814165141.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 1.5 Git工作机制

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618141715334.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 1.6 Git和代码托管中心

代码托管中心是基于网络服务器的远程代码仓库，一般我们简单称为远程库。  
➢ 局域网  
　　✓ GitLab  
➢ 互联网  
　　✓ GitHub（外网）  
　　✓ Gitee 码云（国内网站）

## 2、Git安装

官网地址： https://git-scm.com/  
1.查看 GNU 协议，可以直接点击下一步。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618141835768.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
2.选择 Git 安装位置，要求是非中文并且没有空格的目录，然后下一步。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618141858874.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
3.Git 选项配置，推荐默认设置，然后下一步。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618141922562.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
4.Git 安装目录名，不用修改，直接点击下一步。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618142013549.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
5.Git 的默认编辑器，建议使用默认的 Vim 编辑器，然后点击下一步。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618142032428.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
6.默认分支名设置，选择让 Git 决定，分支名默认为 master，下一步。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618142050477.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
7.修改 Git 的环境变量，选第一个，不修改环境变量，只在 Git Bash 里使用 Git。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618142106968.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
8.选择后台客户端连接协议，选默认值 OpenSSL，然后下一步。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618142123251.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
9.配置 Git 文件的行末换行符，Windows 使用 CRLF，Linux 使用 LF，选择第一个自动转换，然后继续下一步。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618142148124.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
10.选择 Git 终端类型，选择默认的 Git Bash 终端，然后继续下一步。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618142207559.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
10.选择 Git pull 合并的模式，选择默认，然后下一步。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618142227209.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
11.选择 Git 的凭据管理器，选择默认的跨平台的凭据管理器，然后下一步。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618142248955.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
12.其他配置，选择默认设置，然后下一步。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618142303264.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
13.实验室功能，技术还不成熟，有已知的 bug，不要勾选，然后点击右下角的 Install按钮，开始安装 Git。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618142322201.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
14.点击 Finsh 按钮，Git 安装成功！  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618142337718.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
15.右键任意位置，在右键菜单里选择 Git Bash Here 即可打开 Git Bash 命令行终端。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021061814235152.png)  
16.在 Git Bash 终端里输入 git --[version](https://so.csdn.net/so/search?q=version&spm=1001.2101.3001.7020) 查看 git 版本，如图所示，说明 Git 安装成功。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021061814240843.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 3、Git常用命令

| 命令名称 | 作用 |
| --- | --- |
| git config --global user.name | 用户名 设置用户签名 |
| git config --global user.email | 邮箱 设置用户签名 |
| **git init** | 初始化本地库 |
| **git status** | 查看本地库状态 |
| **git add** | 文件名 添加到暂存区 |
| **git commit -m** | “日志信息” 文件名 提交到本地库 |
| **git reflog** | 查看历史记录 |
| **git reset --hard** | 版本号 版本穿梭 |

## 3.1 设置用户签名

1 ）基本语法

```
git config --global user.name 用户名
git config --global user.email 邮箱
```

2 ）案例实操  
全局范围的签名设置：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619134616273.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
说明：  
　　签名的作用是区分不同操作者身份。用户的签名信息在每一个版本的提交信息中能够看到，以此确认本次提交是谁做的。Git 首次安装必须设置一下用户签名，否则无法提交代码。  
　　`※注意`：这里设置用户签名和将来登录 GitHub（或其他代码托管中心）的账号没有任  
何关系。

## 3.2 初始化本地库

1）基本语法

```
git init
```

2 ）案例实操  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619134924853.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
3 ）结果查看  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619135040286.png)

## 3.3 查看本地库状态

1） 基本语法

```
git status
```

2）案例实操  
1、首次查看（工作区无任何文件）  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021061913531091.png)  
2、 新增文件（ hello.txt ）  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619135612631.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
3、再次查看（检测未被追踪到的文件）  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619135712295.png)

## 3.4 添加暂存区

1）基本语法

```
git add 文件名
```

2）案例实操  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619135914208.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
查看状态，检测到暂存区有新文件

## 3.5 提交到本地库

1）基本语法

```
git commit -m "日志信息" 文件名
```

2）案例实操  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619140154807.png)  
查看状态，没有文件需要提交  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619140230388.png)

## 3.6 修改文件

1、修改hello.txt  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021061914032046.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
2、查看状态，检测到工作区有文件被修改  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619140412401.png)  
3、将修改的文件再次添加到暂存区  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619140611172.png)

4、将修改的文件再次提交  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619140649686.png)

## 3.7 历史版本

**3.7.1 查看历史版本**  
1）基本语法

```
git reflog   查看版本信息
git log      查看详细版本信息
```

2）案例实操  
git reflog  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619140821151.png)  
git log  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619140847199.png)  
**3.7.2 版本穿梭**  
1）基本语法

```
git reset --hard 版本号
```

2）案例实操  
1、可以看出现在指向第二次提交的`6b1dc53`版本  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619141109175.png)  
2、切换到第一次提交的 `5af531f`版本  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619141209143.png)  
3、查看hello.txt，发现内容已经更改  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619141249386.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
4、查看日志，发现指向第一次提交的 `5af531f` 版本  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619141354146.png)

## 4、Git的分支操作

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619143517598.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 4.1 什么是分支

在版本控制过程中，同时推进多个任务，为每个任务，我们就可以创建每个任务的单独分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开来，开发自己分支的时候，不会影响主线分支的运行。对于初学者而言，分支可以简单理解为副本，一个分支就是一个单独的副本。（分支底层其实也是指针的引用）  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021061914355086.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 4.2 分支的好处

同时并行推进多个功能开发，提高开发效率。  
　　各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败  
的分支删除重新开始即可。

## 4.3 分支的操作

| 命令名称 | 作用 |
| --- | --- |
| git branch 分支名 | 创建分支 |
| git branch -v | 查看分支 |
| git checkout 分支名 | 切换分支 |
| git merge 分支名 | 把指定的分支合并到当前分支上 |

**4.3.1 查看分支**  
1）基本语法

```
git branch -v
```

2 ）案例实操  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619143928354.png)  
\*代表当前所在分区

**4.3.2 创建分支**  
1）基本语法

```
git branch 分支名
```

2 ）案例实操  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619144106871.png)  
**4.3.3 修改分支**  
1、在`master`分支上做修改  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619144415453.png)  
2、添加至暂存区、提交到本地库  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021061914450355.png)  
3、hot-fix 分支并未做任何改变、master 分支已更新为最新一次提交的版本  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619144525968.png)  
**4.3.4 切换分支**

1）基本语法

```
git checkout 分支名
```

2 ）案例实操  
1、切换到hot-fix分支  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619144813407.png)  
2、查看 hot-fix 分支上的文件内容发现与 master 分支上的内容不同  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619144844572.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
3、在 hot-fix 分支上做修改  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021061914505933.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
4、添加到暂存区、提交到工作区  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619145159932.png)  
**4.3.5 合并分支**

1）基本语法

```
git merge 分支名
```

2 ）案例实操 在 master 分支上合并 hot-fix 分支  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619145351865.png)  
**4.3.6 产生冲突**

冲突产生的表现：后面状态为 `MERGING`  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619145504234.png)  
查看冲突文件hello.txt  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619145539543.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
冲突产生的原因：  
　　合并分支时，两个分支在`同一个文件的同一个位置`有两套完全不同的修改。Git 无法替  
我们决定使用哪一个。必须`人为决定`新代码内容。  
　　查看状态（检测到有文件有两处修改）  
　　![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619145709700.png)  
**4.3.7 解决冲突**  
1）编辑有冲突的文件，删除特殊符号，决定要使用的内容  
特殊符号：`<<<<<<< HEAD` 当前分支的代码 `=======` 合并过来的代码 `>>>>>>> hot-fix`  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619145921240.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
2）添加到暂存区

3）执行提交（注意：此时使用 git commit 命令时不能带文件名）  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619150202392.png)  
发现后面 MERGING 消失，变为正常

## 5、Git团队协作机制

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619150740490.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021061915080981.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 6、GitHub操作

GitHub 网址：https://github.com/

## 6.1 创建远程仓库

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062001001012.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620010025363.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 6.2 远程仓库操作

| 命令名称 | 作用 |
| --- | --- |
| git remote -v | 查看当前所有远程地址别名 |
| git remote add 别名 远程地址 | 起别名 |
| git push 别名 分支 | 推送本地分支上的内容到远程仓库 |
| git clone 远程地址 | 将远程仓库的内容克隆到本地 |
| git pull 远程库地址别名 远程分支名 | 将远程仓库对于分支最新内容拉下来后与当前本地分支直接合并 |

**6.2.1 创建远程仓库别名**  
1） 基本语法

```
git remote -v 查看当前所有远程地址别名
git remote add 别名 远程地址
```

2）实操案例  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620010304869.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620010317440.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
**6.2.2 推送本地分支到远程仓库**  
1） 基本语法

```
git push 别名 分支
```

2）实操案例  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620010410114.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
**6.2.3 克隆远程仓库到本地**  
1） 基本语法

```
git clone 远程地址
```

2）实操案例  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620010445766.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
小结：clone 会做如下操作。1、拉取代码。2、初始化本地仓库。3、创建别名

**6.2.4 邀请加入团队**  
1） 选择邀请合作者  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620010540403.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
2）填入想要合作的人  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620010602742.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
3） 复 制 地 址 并 通 过 微 信 钉 钉 等 方 式 发 送 给 该 用 户 ， 复 制 内 容 如 下 ：  
https://github.com/atguiguyueyue/git-shTest/invitations  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620010630534.png)  
4）在 atguigulinghuchong 这个账号中的地址栏复制收到邀请的链接 ，点击接受邀请  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620010703229.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
5）在成功之后可以在 atguigulinghuchong 这个账号上看到 git-Test 的远程仓库。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062001072715.png)  
6 ）令狐冲可以修改内容并 push 到远程仓库。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620010807573.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620010818665.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
7）回到 atguiguyueyue的GitHub 远程仓库中可以看到，最后一次是 lhc 提交的  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620010849352.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
**6.2.5 拉取远程库内容**  
1） 基本语法

```
git pull 远程库地址别名 远程分支名
```

2）实操案例  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620010937540.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 6.3 跨团队协作

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620011132769.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620011145517.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620011154922.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620011207656.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062001121892.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620011231522.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620011247223.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620011257637.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620011311419.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620011326388.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 6.4 SSH免密登录

我们可以看到远程仓库中还有一个 SSH 的地址，因此我们也可以使用 SSH 进行访问。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620012006183.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
具体操作如下：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062001202592.png)![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620012047310.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
复制 id\_rsa.pub 文件内容，登录 GitHub，点击用户头像→Settings→SSH and GPG keys  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620012120984.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620012130183.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620012141603.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
接下来再往远程仓库 push 东西的时候使用 SSH 连接就不需要登录了。

## 7、IDEA集成Git

## 7.1 配置Git忽略文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620134636715.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620134645989.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
问题 1: 为什么要忽略他们？  
答：与项目的实际功能无关，不参与服务器上部署运行。把它们忽略掉能够屏蔽 IDE工具之间的差异。

问题 2 ：怎么忽略？  
1）创建忽略规则文件 xxxx.ignore（前缀名随便起，建议是 `git.ignore`）  
这个文件的存放位置原则上在哪里都可以，为了便于让~/.gitconfig 文件引用，建议也放在用户家目录下  
git.ignore 文件模版内容如下：

```
# Compiled class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

# virtual machine crash logs, seehttp://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*

.classpath
.project
.settings
target
.idea
*.iml
```

2）在.gitconfig 文件中引用忽略配置文件（此文件在 Windows 的家目录中）

```
[user]
name = Layne
email = Layne@atguigu.com
[core]
excludesfile = C:/Users/asus/git.ignore
注意：这里要使用“正斜线（/）”，不要使用“反斜线（\）”
```

## 7.2 定位Git程序

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620135354223.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 7.3 初始化本地库

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620135414716.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
选择要创建 Git 本地仓库的工程。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620135434121.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 7.4 添加到暂存区

右键点击项目选择 Git -> Add 将项目添加到暂存区。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620135814464.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 7.5 提交到本地库

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062013584853.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 7.6 切换版本

在 IDEA 的左下角，点击 Version Control，然后点击 Log 查看版本  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620135959203.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
右键选择要切换的版本，然后在菜单里点击 Checkout Revision。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620140013250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 7.7 创建分支

选择 Git，在 Repository 里面，点击 Branches 按钮。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620140043939.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
在弹出的 Git Branches 框里，点击 New Branch 按钮。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620140057711.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)填写分支名称，创建 hot-fix 分支。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620140112246.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
然后再 IDEA 的右下角看到 hot-fix，说明分支创建成功，并且当前已经切换成 hot-fix 分支  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620140127150.png)

## 7.8 切换分支

在 IDEA 窗口的右下角，切换到 master 分支。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620140744489.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
然后在 IDEA 窗口的右下角看到了 master，说明 master 分支切换成功。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620140757386.png)

## 7.9 合并分支

在 IDEA 窗口的右下角，将 hot-fix 分支合并到当前 master 分支。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620140813628.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
如果代码没有冲突，分支直接合并成功，分支合并成功以后，代码自动提交，无需手动提交本地库。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620140832970.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
如果代码没有冲突，分支直接合并成功，分支合并成功以后，代码自动提交，无需手动提交本地库。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141124567.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 7.10 解决冲突

如图所示，如果 master 分支和 hot-fix 分支都修改了代码，在合并分支的时候就会发生冲突。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141149213.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141158850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
我们现在站在 master 分支上合并 hot-fix 分支，就会发生代码冲突。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141214128.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
点击 Conflicts 框里的 Merge 按钮，进行手动合并代码。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062014122518.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
手动合并完代码以后，点击右下角的 Apply 按钮。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141236264.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
代码冲突解决，自动提交本地库。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/202106201412472.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 8、IDEA集成GitHub

## 8.1 设置 GitHub 账号

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141539235.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
如果出现 401 等情况连接不上的，是因为网络原因，可以使用以下方式连接：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141602888.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
然后去 GitHub 账户上设置 token。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141625318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141634247.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141645548.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
点击生成 token。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141658632.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
复制红框中的字符串到 idea 中。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141709769.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
点击登录。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141721773.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 8.2 分享工程到 GitHub

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141740761.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141748481.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141803223.png)  
来到 GitHub 中发现已经帮我们创建好了 gitTest 的远程仓库。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141826558.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 8.3 push 推送本地库到远程库

右键点击项目，可以将当前分支的内容 push 到 GitHub 的远程仓库中。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141848364.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141857182.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141905907.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141918559.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620141933273.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
注意：push 是将本地库代码推送到远程库，如果本地库代码跟远程库代码版本不一致，push 的操作是会被拒绝的。也就是说，要想 push 成功，一定要保证本地库的版本要比远程库的版本高！因此一个成熟的程序员在动手改本地代码之前，一定会先检查下远程库跟本地代码的区别！如果本地的代码版本已经落后，切记要先 pull 拉取一下远程库的代码，将本地代码更新到最新以后，然后再修改，提交，推送！

## 8.4 pull 拉取远程库到本地库

右键点击项目，可以将远程仓库的内容 pull 到本地仓库。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620142014789.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/202106201420269.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
注意：pull 是拉取远端仓库代码到本地，如果远程库代码和本地库代码不一致，会自动合并，如果自动合并失败，还会涉及到手动解决冲突的问题。

## 8.5 clone 克隆远程库到本地

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062014205788.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620142106711.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
为 clone 下来的项目创建一个工程，然后点击 Next。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620142132669.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620142142166.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620142155589.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620142205550.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 9、国内代码托管中心-码云

## 9.1 简介

众所周知，GitHub 服务器在国外，使用 GitHub 作为项目托管网站，如果网速不好的话，严重影响使用体验，甚至会出现登录不上的情况。针对这个情况，大家也可以使用国内的项目托管网站-码云。  
码云是开源中国推出的基于 Git 的代码托管服务中心，网址是 https://gitee.com/ ，使用方式跟 GitHub 一样，而且它还是一个中文网站，如果你英文不是很好它是最好的选择。

## 9.2 码云帐号注册和登录

进入码云官网地址：https://gitee.com/，点击注册 Gitee，输入个人信息，进行注册即可。

## 9.3 码云创建远程库

点击首页右上角的加号，选择下面的新建仓库  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620153343390.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620153352445.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620153400660.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620153411645.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620153420452.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 9.4 IDEA 集成码云

**9.4.1 IDEA 安装码云插件**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620153447248.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620153456483.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
Idea 链接码云和链接 GitHub 几乎一样，安装成功后，重启 Idea。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620153513939.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
Idea 重启以后在 Version Control 设置里面看到 Gitee，说明码云插件安装成功。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620153527681.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
然后在码云插件里面添加码云帐号，我们就可以用 Idea 连接码云了。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620153539672.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
**9.4.2 IDEA 连接码云**

Idea 连接码云和连接 GitHub 几乎一样，首先在 Idea 里面创建一个工程，初始化 git 工程，然后将代码添加到暂存区，提交到本地库，这些步骤上面已经讲过，此处不再赘述。

➢ 将本地代码 push 到码云远程库  
![在这里插入图片描述](https://img-blog.csdnimg.cn/202106201536174.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
自定义远程库链接。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062015362682.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
给远程库链接定义个 name，然后再 URL 里面填入码云远程库的 HTTPS 链接即可。码云服务器在国内，用 HTTPS 链接即可，没必要用 SSH 免密链接。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062015365060.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
然后选择定义好的远程链接，点击 Push 即可。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620153701506.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
看到提示就说明 Push 远程库成功。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620153715338.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
去码云远程库查看代码。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062015372777.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
只要码云远程库链接定义好以后，对码云远程库进行 pull 和 clone 的操作和 Github 一致，此处不再赘述。

## 9.5 码云复制 GitHub 项目

码云提供了直接复制 GitHub 项目的功能，方便我们做项目的迁移和下载。  
具体操作如下：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620153755226.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620153803655.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
如果 GitHub 项目更新了以后，在码云项目端可以手动重新同步，进行更新！  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062015381734.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620153825566.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062015383622.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 10、自建代码托管平台-GitLab

## 10.1 GitLab 简介

GitLab 是由 GitLabInc.开发，使用 MIT 许可证的基于网络的 Git 仓库管理工具，且具有wiki 和 issue 跟踪功能。使用 Git 作为代码管理工具，并在此基础上搭建起来的 web 服务。GitLab 由乌克兰程序员 DmitriyZaporozhets 和 ValerySizov 开发，它使用 Ruby 语言写成。后来，一些部分用 Go 语言重写。截止 2018 年 5 月，该公司约有 290 名团队成员，以及 2000 多名开源贡献者。GitLab 被 IBM，Sony，JülichResearchCenter，NASA，Alibaba，Invincea，O’ReillyMedia，Leibniz-Rechenzentrum(LRZ)，CERN，SpaceX 等组织使用。

## 10.2 GitLab 官网地址

官网地址：https://about.gitlab.com/  
安装说明：https://about.gitlab.com/installation/

## 10.3 GitLab 安装

**10.3.1 服务器准备**  
准备一个系统为 CentOS7 以上版本的服务器，要求内存 4G，磁盘 50G。关闭防火墙，并且配置好主机名和 IP，保证服务器可以上网。此教程使用虚拟机：主机名：gitlab-server IP 地址：192.168.6.200  
**10.3.2 安装包准备**  
Yum 在线安装 gitlab- ce 时，需要下载几百 M 的安装文件，非常耗时，所以最好提前把所需 RPM 包下载到本地，然后使用离线 rpm 的方式安装。  
下载地址：

```
https://packages.gitlab.com/gitlab/gitlab-ce/packages/el/7/gitlab-ce-13.10.2-ce.0.el7.x86_64.rpm
```

**10.3.3 编写安装脚本**  
安装 gitlab 步骤比较繁琐，因此我们可以参考官网编写 gitlab 的安装脚本。

```
[root@gitlab-server module]# vim gitlab-install.sh
sudo rpm -ivh /opt/module/gitlab-ce-13.10.2-ce.0.el7.x86_64.rpm

sudo yum install -y curl policycoreutils-python openssh-server cronie

sudo lokkit -s http -s ssh

sudo yum install -y postfix

sudo service postfix start

sudo chkconfig postfix on

curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash

sudo EXTERNAL_URL="http://gitlab.example.com" yum -y install gitlab-ce
```

给脚本增加执行权限  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154135558.png)  
然后执行该脚本，开始安装 gitlab-ce。注意一定要保证服务器可以上网。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154149238.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
**10.3.4 初始化 GitLab 服务**  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154206427.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
**10.3.5 启动 GitLab 服务**  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154238341.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154246487.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
**10.3.6 使用浏览器访问 GitLab**  
使用主机名或者 IP地址即可访问GitLab服务。需要提前配一下windows 的hosts 文件。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154310692.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154318746.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154328416.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154343548.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
**10.3.7 GitLab 创建远程库**  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154401108.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154409556.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
**10.3.8 IDEA 集成 GitLab**  
➢ 1）安装 GitLab 插件  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062015443129.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
➢ 2）设置 GitLab 插件

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154441633.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154450748.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154459183.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
➢ 3）push 本地代码到 GitLab 远程库

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154516127.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
自定义远程连接  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154528393.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154541620.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154551823.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062015460089.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620154612767.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDE5MDY2NQ==,size_16,color_FFFFFF,t_70)

## 11、工作中正确使用git

1、 先把master主分支上的代码与远程仓库同步到最新版本。直接从远程仓库pull下来  
2、 在本地创建并切换到工作分支，创建完成后会自动切换到该分支（这时候master/work分支的内容是相同的。）  
3、 编写代码。  
4、 完成后，add commit到work分支  
5、 将主分支从远程仓库pull最新的版本下来。  
6、 将工作分支合并到主分支

-   先切换到主分支master
-   选中要合并的分支work，点击merge
-   如果有冲突，则需要手动解决冲突

7、 这时候所有在work分支上做得修改就会合并到主分支上。  
8、 最后就可以push到远程仓库上了。

`关于分支切换的两个注意点`

-   分支切换一定要先add/commit当前分支上的修改，然后如果在修改完代码后没有提交，就想切换，idea会提示是否进行smart checkou，如果你点击yes你就完完了，idea会把当代分支上的修改，保存到你要切换的另一个分支上。这样一样就乱套了。
-   如果当前工作分支上有很多bug不想提交，那么你可以先隐藏当前工作分支上的修改，stash（隐藏），然后切换到另一个分支上，那么下次你又切换回工作分支的时候，你可以通过unstash把修改的代码重新显示出来。