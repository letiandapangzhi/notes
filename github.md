## Step

```bash
git init
git add .
git commit -m "xx"

git remote add origin https://github.com/your-username/new-repo-name.git
# 或使用 SSH: git remote add origin git@github.com:your-username/new-repo-name.git

# 分支关联
git branch --set-upstream-to=origin/<branch> main

# 远程仓库非空
git pull origin main --rebase
git push origin main

# ssh key
ssh-keygen -t ed25519 -C "your_email@example.com"
cat ~/.ssh/id_ed25519.pub # set to github

# log 定向查看
git log --author="xxx" --oneline branch_name

# 仅下载最新版本的代码文件，忽略历史提
git clone --depth 1 https://github.com/username/repo.git
# 结合 --filter=blob:none 进一步减少数据量
git clone --filter=blob:none --depth 1 https://github.com/username/repo.git


```

