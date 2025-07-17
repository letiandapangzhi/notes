## Step

### 安装卸载

```bash
# 安装
apt-get update
apt-get install nginx

# 卸载
systemctl stop nginx
# 保留配置文件卸载
apt-get remove nginx nginx-common
# 彻底卸载 删除所有文件
apt-get purge nginx nginx-common
# 删除不再需要的依赖包
apt-get autoremove
# 删除 Nginx 创建的网站目录
rm -rf /var/www/html
```

## Cli

```bash
nginx version: nginx/1.24.0 (Ubuntu)
Usage: nginx [-?hvVtTq] [-s signal] [-p prefix]
             [-e filename] [-c filename] [-g directives]

Options:
  -?,-h         : this help
  -v            : show version and exit
  -V            : show version and configure options then exit
  -t            : test configuration and exit
  -T            : test configuration, dump it and exit
  -q            : suppress non-error messages during configuration testing
  -s signal     : send signal to a master process: stop, quit, reopen, reload
  -p prefix     : set prefix path (default: /usr/share/nginx/)
  -e filename   : set error log file (default: stderr)
  -c filename   : set configuration file (default: /etc/nginx/nginx.conf)
  -g directives : set global directives out of configuration file

# 常用
nginx -t  # 测试配置
nginx -s reload  # 重新加载配置
nginx -T
nginx -V

# 测试
curl -H "Host: b.com" http://localhost
```

- **-V**：显示版本号及编译配置选项（如启用的模块、安装路径等）

- -t：测试配置文件语法是否正确，不启动 Nginx
  - 若配置正确，显示：`configuration file /etc/nginx/nginx.conf test is successful`
  - 若有错误，显示具体错误位置和原因

- **-T**：测试配置并输出合并后的完整配置（用于调试）

- **-s stop**：快速停止 Nginx（强制终止所有进程）

- **-s quit**：优雅停止 Nginx（等待现有连接处理完毕）

- -s reopen：重新打开日志文件（用于日志分割）
  - 例如：`nginx -s reopen` 可在不重启服务的情况下切换日志文件

- -s reload：重新加载配置文件（平滑重启）
  - Nginx 会启动新工作进程，逐步关闭旧进程

