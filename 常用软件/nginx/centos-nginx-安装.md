在 CentOS 7.8 上安装 Nginx 的过程相对简单，可以通过 CentOS 的默认软件仓库或 Nginx 的官方仓库来完成。下面是通过 Nginx 官方仓库安装 Nginx 的步骤，这通常能确保你安装的是最新版本的 Nginx。

### 通过 Nginx 官方仓库安装

1. **添加 Nginx 仓库**首先，你需要在你的系统上添加 Nginx 的官方 Yum 仓库。创建一个新的 repo 文件：

    ```bash
    sudo vi /etc/yum.repos.d/nginx.repo
    ```

    然后，将以下内容添加到该文件中：

    ```ini
    [nginx]
    name=nginx repo
    baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
    gpgcheck=1
    enabled=1
    gpgkey=https://nginx.org/keys/nginx_signing.key
    module_hotfixes=true
    ```

2. **导入 Nginx 公钥**为了验证包的完整性，你需要导入 Nginx 的 GPG 密钥：

    ```bash
    sudo rpm --import https://nginx.org/keys/nginx_signing.key
    ```

3. **安装 Nginx**添加仓库后，使用以下命令安装 Nginx：

    ```bash
    sudo yum install nginx
    ```

4. **启动 Nginx 服务**安装完成后，启动 Nginx 服务：

    ```bash
    sudo systemctl start nginx
    ```

5. **设置 Nginx 开机自启**如果你希望在系统启动时自动启动 Nginx，可以使用以下命令设置开机自启：

    ```bash
    sudo systemctl enable nginx
    ```

6. **检查 Nginx 状态**检查 Nginx 服务状态确保一切正常：

    ```bash
    sudo systemctl status nginx
    ```

7. **调整防火墙设置**
   如果你的系统运行的是 `firewalld`，你需要允许 HTTP 和 HTTPS 通信：

    ```bash
    sudo firewall-cmd --permanent --zone=public --add-service=http
    sudo firewall-cmd --permanent --zone=public --add-service=https
    sudo firewall-cmd --reload
    ```

### 访问 Nginx

完成安装和配置后，你可以在浏览器中输入服务器的 IP 地址来访问 Nginx 的默认欢迎页面，以确认它正在正常运行。

通过以上步骤，你可以在 CentOS 7.8 上安装和配置最新版的 Nginx。这种方法优于直接使用 CentOS 默认的软件仓库，因为官方 Nginx 仓库通常会提供更及时的更新和补丁。
