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

```

