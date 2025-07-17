## Step

```bash
# 查看用户
cat /etc/passwd


```

**字段含义**（以 `ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash` 为例）：

- `ubuntu`：用户名
- `x`：密码占位符（实际密码存储在 `/etc/shadow`，仅 root 可查看）
- `1000`：用户 ID（UID，`0` 为 root，`1-999` 通常为系统用户，`1000+` 为普通用户）
- `1000`：组 ID（GID）
- `Ubuntu`：用户描述（可选）
- `/home/ubuntu`：家目录
- `/bin/bash`：登录 shell（`/usr/sbin/nologin` 表示该用户无法登录系统）