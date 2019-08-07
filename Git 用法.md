Git 本身是一套命令行工具，建议使用命令行而不是工具。

Git主要就是各种版本，各种不同的分支上处理版本。综上，版本+分支。

有一个指针叫做master，它会指向不同的版本。

*版本 + branch  -> 不同分支版本的合并*

git鼓励大量使用分支：
```
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>
创建+切换分支：git checkout -b <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>
把XX文件在
把XX文件在工作区的修改全部撤销： git checkout — XX
```

<hr/>
问题描述：   

```
git push origin master; // 报错：error：failed to push some refs to
```

原因：本地没有update到最新版本的项目   

解决办法:
```
输入： git pull --rebase origin master
再输入：git push origin master
```
