# Git workflow
## 前言
如果你不能理解本文的内容，照着做就是了；如果你连照着做也做不到，就不要用 Git。


## 修改全局设置
在你能好好地使用 Git 管理你的代码之前，把这几个设置改一下：


> $ git config --global color.status auto       # 使 git status -s 命令的输出带有颜色。

> $ git config --global color.diff auto             # 使 git diff -s 命令的输出带有颜色。

> $ git config --global color.branch auto           # 使 git branch -a 命令的输出带有颜色。

> $ git config --global color.interactive true      # 使 git add -i 命令的输出带有颜色。

> $ git config --global core.editor /bin/nano       # 请不要理会那个竟然敢自称编辑器的叫 vi 的智障。

> $ git config --global core.pager "less -x1,5"     # 设置好 diff 的对齐。 [1]

> $ git config --global core.autocrlf input         # 别把 CR LF 提交到服务器上。

> $ git config --global push.default simple         # 仅 push 当前分支。

> $ git config --global pull.ff only                # 禁用非 --ff-only 的 pull 操作。

> $ git config --global merge.ff only               # 禁用非 --ff-only 的 merge 操作。

开发者手册
《Git 工作流程》
http://www.ruanyifeng.com/blog/2015/12/git-workflow.html
仅参考其中的 Gitlab flow。 master 分支（debug/unstable）仅作开发，产品分支（release/stable）仅以 cherry-pick 更新。

《常用 Git 命令清单》
http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html

《Git 使用规范流程》
http://www.ruanyifeng.com/blog/2015/08/git-use-process.html

禁止事项
禁止直接在 master 分支上开发。
禁止直接向 master 和 release/stable 分支 push。
禁止 merge， --ff-only 除外。
禁止 pull， --ff-only 和 --rebase 除外。
文本文件禁止使用宽字符编码，非手动编辑的文件除外。
开发流程

一般开发流程
获取 最新代码：
$ git checkout master
$ git fetch
$ git reset --hard origin/master
创建 用于开发的 本地分支 （此处假定用于开发的分支名为 working）：
$ git checkout -b working
在 working 分支上 进行修改 。
提交修改 到 working 分支：
$ git add 第一个文件
$ git add 第二个文件
$ git commit
将本地分支 working 推送 到服务器上，创建一个新的 远端分支 origin/working：
$ git push origin working
在 GitLab 上查看推送的分支， 创建 到 master 分支的 Merge Request 。
查看新建的 Merge Request，如果没有冲突，点击 合并 。
再次 获取 最新的代码：
$ git checkout master
$ git pull --ff-only
删除本地的 开发 分支 ：
$ git branch -d working
删除服务器上的 开发 分支 ：
$ git push origin :working
如何解决冲突：
获取 最新代码：
$ git fetch
如果本地有没提交的修改， 提交 之，否则无法进行变基操作。
执行 变基 操作，基于新的远端分支 origin/master：
$ git rebase origin/master
如果提示 CONFLICT， 查看 当前变基 状态 ：
$ git status -s
M  暂存区　　变基并且没有冲突的文件
 M 工作区　　修改但是没有提交的文件
?? 非追踪　　没有加入版本控制的文件
UU 冲突　　　变基出现冲突的文件
解决冲突：
进行编辑，查找 <<<<<< （6 个）标记并解决，然后标记为 已解决 ：
$ git add 变基出现冲突的文件
丢弃本地的修改 ，使用服务器的文件（对于变基操作 --ours 是 基点 ）：
$ git checkout --ours 变基出现冲突的文件
$ git add 变基出现冲突的文件
丢弃服务器的修改 ，使用本地的文件（对于变基操作 --theirs 是 本地 ）：
$ git checkout --theirs 变基出现冲突的文件
$ git add 变基出现冲突的文件
重复 3~5 直到解决所有冲突，然后 继续变基操作 ：
$ git rebase --continue
重复 3~6 直到变基完成。
用本地的 working 分支 覆盖服务器上的 working 分支 ：
$ git push origin +working
重新尝试合并 出现冲突的 Merge Request。
如果变基到一半不想变基了怎么办
取消变基操作 ，本地已提交的修改不会丢失：
$ git rebase --abort
切记
切换分支之前把本地需要提交的修改全部提交。
如果涉及到合并或重置代码，创建新的分支做。原有分支不会受到影响。  
没事不要 merge。
没事不要 merge。
没事不要 merge。
对于已 fork 的仓库怎么合并上游修改、发布补丁
（以下假定下游远端为 origin，上游远端为 upstream，双方的主分支都是 master。）

获取上游仓库：
$ git fetch upstream
以当前分支为原本，创建一个新的分支用于变基：
$ git checkout -b rebasing
对本地修改进行变基：
$ git rebase upstream/master
如果遇到冲突，解决冲突。
变基结束后，测试新的分支。
如果测试没有问题，用新的分支 替换 原来的分支：
$ git checkout -B master
用本地的 master 分支 覆盖服务器上的 master 分支 ：
$ git push origin +master
将本地相对于上游新增的补丁导出成 .patch 文件（可以使用电子邮件发送给上游）：
$ git format-patch upstream/master
合并下游补丁
应用补丁文件：

$ git am 第一个补丁文件 第二个补丁文件
推送到远端分支

$ git push
从其他分支挑拣单个提交
挑拣单个提交：

$ git cherry-pick 单个提交散列标识
推送到远端分支

$ git push