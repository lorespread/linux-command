查看日志发现日志如下

```shell
2024/04/12 19:45:45 [crit] 7749#7749: *18 connect() to 127.0.0.1:8080 failed (13: Permission denied) while connecting to upstream, client: 192.168.1.110, server: local-oa-test.ruiangeo.com, request: "GET /admin.do HTTP/1.1", upstream: "http://127.0.0.1:8080/admin.do", host: "local-oa-test.ruiangeo.com"
2024/04/12 19:45:45 [crit] 7749#7749: *18 connect() to 127.0.0.1:8080 failed (13: Permission denied) while connecting to upstream, client: 192.168.1.110, server: local-oa-test.ruiangeo.com, request: "GET /favicon.ico HTTP/1.1", upstream: "http://127.0.0.1:8080/favicon.ico", host: "local-oa-test.ruiangeo.com", referrer: "http://local-oa-test.ruiangeo.com/admin.do"
2024/04/12 20:53:10 [crit] 7749#7749: *21 connect() to 127.0.0.1:8080 failed (13: Permission denied) while connecting to upstream, client: 192.168.1.110, server: local-oa-test.ruiangeo.com, request: "GET /admin.do HTTP/1.1", upstream: "http://127.0.0.1:8080/admin.do", host: "local-oa-test.ruiangeo.com"
2024/04/12 20:53:10 [crit] 7749#7749: *21 connect() to 127.0.0.1:8080 failed (13: Permission denied) while connecting to upstream, client: 192.168.1.110, server: local-oa-test.ruiangeo.com, request: "GET /favicon.ico HTTP/1.1", upstream: "http://127.0.0.1:8080/favicon.ico", host: "local-oa-test.ruiangeo.com", referrer: "http://local-oa-test.ruiangeo.com/admin.do"
```

可以看出重点是 `13: Permission denied`

当你看到 Nginx 日志中出现如 `connect() to 127.0.0.1:8080 failed (13: Permission denied)` 的错误时，这通常表示 Nginx 进程由于权限问题而无法连接到本地的 8080 端口。这种情况最常见的原因之一是 SELinux（Security-Enhanced Linux）的安全策略阻止了 Nginx 访问该端口。

### 解决步骤

#### 1. 检查 SELinux 状态

首先，你可以检查 SELinux 的当前状态以确定它是否在影响 Nginx 的连接：

```bash
sestatus
```

如果显示为 `enabled` 并且当前的策略可能限制了 HTTP 网络连接，你可能需要调整 SELinux 的策略。

#### 2. 更改 SELinux 策略（暂时）

作为测试，你可以暂时设置 SELinux 为宽松模式（permissive mode），这样它会记录违反策略的行为，但不会阻止它们。这可以帮助确认 SELinux 是否是问题的原因：

```bash
sudo setenforce 0
```

然后再次尝试启动或重新加载 Nginx，并检查问题是否解决。如果这解决了问题，你知道是 SELinux 配置需要调整。

#### 3. 调整 SELinux 策略（持久）

如果你确认 SELinux 是问题所在，你可以更改 SELinux 策略以允许 Nginx 与本地应用程序通信。使用以下命令可以让 Nginx 有权访问网络：

```bash
sudo setsebool -P httpd_can_network_connect 1
```

这个命令会永久更改 SELinux 策略，允许 httpd（在 SELinux 策略中 Nginx 通常归类为 httpd）进行网络连接。

#### 4. 恢复 SELinux 状态

一旦问题解决，不要忘记将 SELinux 状态恢复到 `enforcing`：

```bash
sudo setenforce 1
```

#### 5. 重新加载 Nginx

确保再次加载 Nginx 配置，以应用任何更改：

```bash
sudo nginx -t
sudo systemctl reload nginx
```

### 注意事项

-   **安全考虑**：在更改 SELinux 策略之前，请确保了解这些更改的安全影响。宽松 SELinux 策略会减少系统的安全性。
-   **日志审查**：如果 SELinux 是问题的原因，你也可以通过查看 `/var/log/audit/audit.log` 文件来获取更多信息，了解哪些操作被拒绝。

通过这些步骤，你应该能够解决 Nginx 无法连接到本地端口的问题，同时保持系统的安全性。

### 3 实际操作

```shell
[root@ra-local-oa nginx]# sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      31
[root@ra-local-oa nginx]# setenforce 0
[root@ra-local-oa nginx]# systemctl reload nginx
[root@ra-local-oa nginx]# setsebool -P httpd_can_network_connect 1
[root@ra-local-oa nginx]# vi nginx.conf
[root@ra-local-oa nginx]# systemctl reload nginx
```

按照上面的操作指引，解决了问题
