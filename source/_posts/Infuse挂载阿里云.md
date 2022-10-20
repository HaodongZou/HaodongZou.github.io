---
title: Infuse挂载阿里云盘
date: 2022-10-20 14:05:28
categories: 好玩
tags: 
    - 影音
    - 自动化
---




在服务器上部署Alist以便Infuse可以挂载阿里云盘WEBDav，实现多个设备随处观看影音内容，并且多设备同步观看进度。
<!--more-->
# 服务器搭建AList

[Guide](https://alist.nn.ci/guide/)

## 使用Docker安装

```bash
docker run -d --restart=always -v /etc/alist:/opt/alist/data -p 5244:5244 --name="alist" xhofe/alist:latest
```

查看默认的用户名密码：

```bash
docker exec -it alist ./alist admin
```

## 一键安装脚本

```bash
# 安装
curl -fsSL "https://alist.nn.ci/v3.sh" | bash -s install

# 更新
curl -fsSL "https://alist.nn.ci/v3.sh" | bash -s update

# 卸载
curl -fsSL "https://alist.nn.ci/v3.sh" | bash -s uninstall
```

默认安装在`opt/ist`中。要自定义安装路径，请将安装路径添加为第二个参数，该参数必须是绝对路径（如果路径以alist结尾，则直接安装到给定路径，否则将安装到给定路径下alist目录中），例如安装到root：

```bash
# Install
curl -fsSL "https://alist.nn.ci/v3.sh" | bash -s install /root
# update
curl -fsSL "https://alist.nn.ci/v3.sh" | bash -s update /root
# Uninstall
curl -fsSL "https://alist.nn.ci/v3.sh" | bash -s uninstall /root
```

## 检查安装是否成功

`AList`默认启动端口为`5244`，在浏览器中访问`http://xx.xx.xx.xx:5244`查看是否出现`AList`主页，如下图：（用户名密码参照安装步骤中获取）

![Untitled](images/Infuse挂载阿里云/Untitled.png)

登录后点击“管理”进入管理页面

## AList挂载阿里云盘

[阿里云盘](https://alist.nn.ci/zh/guide/drivers/aliyundrive.html)

在AList控制面板点击“存储”，添加驱动方式为阿里云盘的新存储。

- 挂载路径：唯一标识，即要挂载到的位置，如果要挂载到根目录，就是 `/`。这个是在AList上的目录，建议按照功能进行自定义。
- 顺序：当有多个账户时，用于排序，越小越靠前。
- 缓存过期：目录结构的缓存时间。
- Web代理：网页预览、下载和直接链接是否通过中转。如果你打开此项，建议你设置**[Api 地址](https://alist.nn.ci/zh/config/site.html#api%E5%9C%B0%E5%9D%80)**，以帮助alist更好的工作。
- WebDAV策略：
    - 302 重定向：重定向到真实链接（在服务器部署一般选择这一项）
    - 使用代理 URL：重定向到代理 URL
    - 本机代理：直接通过本地中转返回数据（最佳兼容性）
- 下载代理 URL：开启代理时不填写此字段，默认使用本机进行传输。提供了两种代理方式：[https://alist.nn.ci/zh/guide/drivers/common.html#下载代理-url](https://alist.nn.ci/zh/guide/drivers/common.html#%E4%B8%8B%E8%BD%BD%E4%BB%A3%E7%90%86-url)
- 提取文件夹：
    - 提取到前面：排序时将所有文件夹放在前面
    - 提取到后面：排序时将所有文件夹放在后面
- 排序：
    - 排序方式：按什么排序
    - 排序方向：排序方向是升序还是降序

### 获取刷新令牌

[阿里云盘](https://alist.nn.ci/zh/guide/drivers/aliyundrive.html#%E5%88%B7%E6%96%B0%E4%BB%A4%E7%89%8C)

### 根文件夹ID

打开阿里云驱动官网，点击进入要设置的文件夹时点击 URL 后面的字符串

如 **[https://www.aliyundrive.com/drive/folder/5fe01e1830601baf774e4827a9fb8fb2b5bf7940open in new window](https://www.aliyundrive.com/drive/folder/5fe01e1830601baf774e4827a9fb8fb2b5bf7940)**

这个文件夹的 file_id 即为 `5fe01e1830601baf774e4827a9fb8fb2b5bf7940`：

![Untitled](images/Infuse挂载阿里云/Untitled%201.png)

# Infuse挂载WebDAV

[WebDAV](https://alist.nn.ci/zh/guide/webdav.html#webdav-%E9%85%8D%E7%BD%AE)

打开Infuse，在“文件”中选择“新增文件来源”，“添加WebDAV”。

- 名称：自定义，如“影视”。
- 通讯协定：如果网站开启了反向代理且使用SSL，选择WebDAV(HTTPS)，否则选择WebDAV。
- 位址：`http[s]://domain`
- 用户名：与网页端用户名一致，无特殊需求使用admin即可。
- 密码：与网页端密码一致。
- 路径：`/dav`根目录，`/dav/影视`自定义目录。
- 端口：AList运行的端口，默认为`5244`

成功挂载WebDAV之后，请将文件夹添加到“我的最爱”（即点击文件夹后面的星号），这样infuse主屏幕才会出现云盘中的影视内容，并自动匹配元数据。