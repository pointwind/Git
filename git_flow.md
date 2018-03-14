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

## 禁止事项
1. 禁止直接在 master 分支上开发。  
2. 禁止直接向 master 和 release/stable 分支 push。  
3. 禁止 merge，--ff-only 除外。  
4. 禁止 pull， --ff-only 和 --rebase 除外  。  
5. 文本文件禁止使用宽字符编码，非手动编辑的文件除外。
![image](https://github.com/lhmouse/git-workflow-zh/raw/master/workflow.jpg)

## 开发流程
### 一般开发流程
##### 获取 最新代码：  


> $ git checkout master  
> $ git fetch  
> $ git reset --hard origin/master  

##### 创建 用于开发的 本地分支   （此处假定用于开发的分支名为 working）：  
>  $ git checkout -b working    

##### 在 working 分支上 进行修改 。  提交修改 到 working 分支：  
> $ git add 第一个文件  
> $ git add 第二个文件  
> $ git commit  

##### 将本地分支 working 推送 到服务器上，创建一个新的 远端分支 origin/working：  
> $ git push origin working  

##### 在 GitLab 上查看推送的分支， 创建 到 master 分支的 Merge Request 。
##### 查看新建的 Merge Request，如果没有冲突，点击 合并 。
##### 再次 获取 最新的代码：  
> $ git checkout master  
> $ git pull --ff-only  

##### 删除本地的 开发 分支 ：  
> $ git branch -d working 

##### 删除服务器上的 开发 分支 ：  
> $ git push origin :working  


