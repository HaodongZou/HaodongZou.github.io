---
title: Clash
date: 2022-10-20 14:47:28
categories: 软件
tags: 
    - 科学上网
    - 网络
---

关于Clash在Centos服务器上的部署、Subconverter（订阅转换工具）部署等

<!--more-->


# 在CentOS安装clash

[clash as a daemon · Dreamacro/clash Wiki](https://github.com/Dreamacro/clash/wiki/clash-as-a-daemon#preface)

# 部署SubConverter

- **备份配置文件⬇️ (for 迁移）**
    
    [pref.toml](https://www.notion.so/pref-toml-fe362c89e46d4c79ab259444dc589876)
    
    [groups.toml](https://www.notion.so/groups-toml-a1e3036eaee247f78a51ab6fa0d50621)
    
    [rulesets.toml](https://www.notion.so/rulesets-toml-d07e8e7e5ce7413fa36ddf25e7170875)
    

## 安装

仓库地址：

[https://github.com/tindy2013/subconverter](https://github.com/tindy2013/subconverter)

在服务器上安装：

```bash
wget https://github.com/tindy2013/subconverter/releases/download/v0.7.2/subconverter_linux64.tar.gz
tar -xvzf subconverter_linux64.tar.gz subconverter && chmod +x subconverter/subconverter
```

## 修改配置文件

1. 默认主配置文件为`pref.toml`，修改`[common]`字段下的`default_url`添加多个订阅地址（不需要URL Encode）

```toml
version = 1
[common]
# API mode, set to true to prevent loading local subscriptions or serving local files directly
api_mode = false

# Access token used for performing critical action through Web interface
api_access_token = "zouhaodong"

# 默认订阅链接, 如有多个连接需要使用"|"进行分割，用以在请求未提供订阅链接时处理订阅，可以为文件或者链接
default_url = ["https://xxxx.xxxx/ssss"]
```

1. 开启Emoji

```toml
[emojis]
# 默认为false关闭，设置为true打开
add_emoji = true
remove_old_emoji = true

[[emojis.emoji]]
#match = '(流量|时间|应急)'
#emoji = '🏳️‍🌈'
import = "snippets/emoji.toml"
```

1. 修改`snippets/groups.toml` 自定义分流规则。下面的例子是在默认规则的基础上将香港、美国等不同区域的节点聚合起来使用`url-test` 进行自动切换。匹配节点的正则表达式可以在`snippets/emoji.toml` 中找到。
    
    [groups示例](https://www.notion.so/groups-d5fc19ed41474329b5c4f8008cdf385e)
    
2. 如果需要，可以修改`snippets/rulesets.toml`指向GitHub中的地址，如
    
    [ACL4SSR/Clash at master · ACL4SSR/ACL4SSR](https://github.com/ACL4SSR/ACL4SSR/tree/master/Clash)
    
    不过默认的本地文件就已经够用了，唯一缺点就是无法更新
    

## 开机自启动

```bash
# 编辑服务配置文件
vi /etc/systemd/system/subconverter.service

# 写入服务信息
[Unit]
Description=A API For Subscription Convert
After=network.target

[Service]
Type=simple
#subconverter 文件路径
ExecStart=/root/subconverter/subconverter
#subconverter 所在目录
WorkingDirectory=/root/subconverter
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target

systemctl enable subconverter #启用服务
systemctl start subconverter #启动服务
```

## 修改配置文件模版——以开启IPV6为例

修改`base/all_base.tpl`

```
external-controller: :9090
// 添加下面这一行
ipv6: true
```

## 添加自定义规则

- 创建`rules/Costom.list`
- 修改`snippets/rulesets.toml`

```toml
# 添加如下规则集
[[rulesets]]
group = "🔰 节点选择"
ruleset = "rules/Costom.list"
```

- 添加自定义规则（以添加byr.pt为例）

```toml
# 在rules/Costom.list中添加
DOMAIN-SUFFIX,byr.pt
```

# 🙅白嫖订阅节点

[2022 免费白嫖节点方法 | ahhhhfs - A姐分享](https://www.ahhhhfs.com/29583/)

1. 打开[https://www.zoomeye.org](https://www.zoomeye.org/)，登录并搜索`title:”Openwrt”`
2. 使用查找结果中的IP地址➕Openwrt的端口号访问
3. 尝试密码`password`
4. 如果能进入管理界面，在服务中一般能找到`PassWall`或者`Clash`、`V2Ray`相关内容并进去复制订阅链接
5. 使用订阅转换工具转换订阅