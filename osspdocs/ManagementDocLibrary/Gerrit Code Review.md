Gerrit Code Review流程



		git config --global user.name "用户名"
		git config --global user.email "邮箱"
		git config --global pull.rebase true

		git config –global alias.st status
		git config –global alias.ci commit
		git config –global alias.co checkout
		git config –global alias.br branch


# 应用代码clone

> Clone 代码使用commit-msg hook方式

		git clone ssh://chengyangyang.hx@12.168.3.18:29418/ylh-micro-mall && scp -p -P 29418 chengyangyang.hx@12.168.3.18:hooks/commit-msg ylh-micro-mall/.git/hooks/

# 本地开发分支Git操作

> 创建开发分支(Task || Bug)

		# 存在tag的情况(推荐使用)
		git checkout -b user/task3926 1.0.0
		# 从DEV分支切出本地分支
		git checkout -b user/task3926 origin/dev

		# 使用本地创建远程分支
		git push origin user/task3926

> 代码提交

		git add . -A
		git commit

		# 可以创建远程分支提交到远程,或者不创建远程保留在本地
		git push

# 创建Change

> 创建 review分支

		# 存在tag的情况(推荐使用)
		git checkout -b review/task3926 1.0.0
		# 从DEV分支切出本地分支
		git checkout -b review/task3926 origin/dev

> merge 开发分支到 review分支

		git merge --squash user/task3926
		git commit

		# 在review分支创建Gerrit change
		git push origin HEAD:refs/for/dev
		# 如果是紧急发布 在master分支上创建 change
		git push origin HEAD:refs/for/master


> Sonarlint & checkstyle检查存在问题等问题需要修改代码 

		在Gerrit 控制台中 Download -> checkout  
		# 1 复制到本地执行修改
		git fetch ssh://chengyangyang.hx@12.168.3.18:29418/ylh-micro-mall refs/changes/45/145/2 && git checkout FETCH_HEAD
		# 2 添加更新
		git add . -A
		# 3 修改提交信息
		git commit --amend
		# 4 Push更新到 changes
		git push origin HEAD:refs/for/dev
		git push origin HEAD:refs/for/master		

> Push Change 时冲突

 		# 1 fetch
		git fetch
		# 2 Download checkout
		git fetch ssh://wangjingjing@10.168.3.10:29418/jsh-service-user-api refs/changes/79/79/1 && git checkout FETCH_HEAD
		# 3 rebase
		git rebase origin/dev
		# 4 解决冲突后提交
		git add . -A
		git rebase --continue
		git push origin HEAD:refs/for/dev

# 在Gerrit Console上添加Changes面板视图中 添加Code Reviewer

		# 目前只能添加 王靖靖 于海青, 添加完成后即可
