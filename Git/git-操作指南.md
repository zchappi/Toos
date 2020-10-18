**1. 检出代码，在开发分支（develop）上开发**

	# ssh-keygen -t rsa -C "你的名称@mxrcorp.com"
	# git config --global user.name "你的名称"
	# git config --global user.email "你的名称@mxrcorp.com"
	# git clone git@192.168.0.130:bookstore/bookstore-xxx-service.git
	# git checkout -b develop
	# git branch --set-upstream-to=origin/develop develop
	# git pull

**2. 正式发布，将develop分支合并到master分支**        

	# git checkout master
	# git merge --no-ff develop
	# git tag -a 2.0（对应的tagID）
	# git push
	# git branch -va
	# git checkout develop

**3. 功能分支（feature），开发某个特定功能版本。** 

	开发前从develop分支创建功能分支，采用feature-*的形式命名
	# git checkout -b feature-x develop
	
	开发完，合并到develop分支
	# git checkout develop
	# git merge --no-ff feature-x
	
	删除功能分支
	# git branch -d feature-x

**4. 修补bug分支** 

	从master分支上创建一个修补bug分支，采用fixbug-*的形式
	# git checkout -b fixbug-0.1 master
	
	修补结束后，合并到master分支：
	# git checkout master
	# git merge --no-ff fixbug-0.1
	# git tag -a 0.1.1
	
	再合并到develop分支：
	# git checkout develop
	# git merge --no-ff fixbug-0.1
	
	删除"修补bug分支"：
	git branch -d fixbug-0.1

**5. 代码统计**
	

	git log --format='%aN' | sort -u | while read name; do echo -en "$name\t"; git log --author="$name" --pretty=tformat: --numstat --since=2017-06-01 --until=2017-06-30 | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }' -; done

**6. 回滚版本到某次提交**

```
工具：SourceTree
1. 切换到需要回滚的分支；
2. 在History中选中需要回退到的那个历史提交记录；
3. 右键-》重置当前分支到此次提交，“使用模式”选择 “强行合并--丢弃所有改动过的工作副本”
4. 再次右键-》重置当前分支到此次提交，“使用模式”选择 “软合并 – 保持所有本地改动”；
5. 在文件状态中提交并推送修改记录；
```

参考链接： https://jingyan.baidu.com/album/ab0b563057387ac15afa7dca.html?picindex=1 

**6. 回滚提交**

```
在最新的提交记录上，右键-》回滚提交
可以看到提交历史记录多了一个Revert  反向提交
这个时候，反向提交只提到了本地仓库，如需撤销远程仓库的，只需推送到远程仓库
```