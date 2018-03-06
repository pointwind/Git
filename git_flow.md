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
