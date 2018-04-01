## Git Merge 
Git merge  （在master中进行）。

该操作只是将head指针指向**，也就是fast-forward
![image](https://github.com/pointwind/Git/blob/master/git_tip_merge.png?raw=true)


## Git rebase

#### 变基的风险

>变基操作的实质是丢弃一些现有的提交，然后相应地新建一些内容一样但实际上不同的提交。如果你已经将提交推送至某个仓库，而其他人也已经从该仓库拉取提交并进行了后续工作，此时，如果你用 git     rebase命今重新整理了提交并再次推送，你的同伴因此将不得不再次将他们手头的工作与你的提交进行整合，如果接下来你还要拉取并整合他们改过的提交，事情就会变得一团糟。

#### 操作注意
>只要你把变基命令当作是在推送前清理提交使之整洁的工具，并且只在苁未推送至共用仓库的提交上执行变基命令，你就不会有事。假如你在那些已经被推送至共用仓库的提交上执行变基命令，并因此去弃了一些别人的升岌所基于的提交，那你就有大麻颃了。

#### 使用建议：
###### 在push到远程仓库前使用变基，变基的作用是使提交整洁，已提交至远程仓库的代码，不能（极其不推荐）使用变基。


>Git rebase branch

(将某分支移至branch线，以branch为基)    （会移动全部分支）

>Git rebase –onto branch1 branch2

branch3(branch1为被移动到的分支，branch2为被忽略的分支，branch3为移动过去的)
![image](https://github.com/pointwind/Git/blob/master/git_tips_rebase_1.png?raw=true)
![image](https://github.com/pointwind/Git/blob/master/git_tips_rebase_2.png?raw=true)
