# Ansible_lamp
# LAMP Stack + Nginx

* ansible version: 2.9.27
* Operating System: CentOS Linux 7 (Core)

本项目使用 ansible 自动化部署 LAMP + Nginx 扩展架构，我们将安装 nginx 做负载均衡器轮询转发客户端访问后端 apache 服务器的流量，博客网站 typecho 文件部署在 nfs 服务器，用户数据存储在MySQL服务器中。

## 初始化设置

首先，我们通过在主机清单 hosts 文件中列出我们的主机地址，并按用途分组：

```
[all_hostname]
server2
server3
server4
server5
server6
[nginx]
server2
[apache]
server3
server4
[mariadb]
server5
[nfs]
server6
```

之后我们执行以下命令来部署站点：

```
ansible-play -i hosts site.yml
```

部署完成后，可以在 Web 浏览器中访问负载均衡器主机的 IP 地址来验证部署：http://hosts:8080
,重新加载页面可以让您访问不同的网站服务器。

## 博客部署

如需安装个人的博客网站，您可以在 nfs 文件服务器的 /nfs 文件夹下执行以下命令：

```
wget http://typecho.org/downloads/1.1-17.10.30-release.tar.gz
tar -zxvf 1.1-17.10.30-release.tar.gz
mv build/* .
```

并在数据库中添加指定库、用户等操作：

```
create database typecho;
grant all privileges on typecho.* to user@"%" identified by '123456';
flush privileges;
```

最后访问网站即可。
