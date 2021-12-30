# 如何在 Rum 上实现种子网络 “Huoju 在 Rum 上说了啥”？

*这篇文章的预设读者是编程爱好者、开发者。Rum 普通用户无需掌握以下信息，直接[载安装 Rum APP](https://docs.prsdev.club/#/rum-app/test)就能顺畅使用啦。*

霍炬评价 “Huoju 在 Rum 上说了啥” 说：

> 这个玩法是符合 rum 预期的。和传统互联网应用不同的是，RUM 的数据完全由用户拥有，所以用户可以通过自己过滤数据，完成自己需要的功能。
> 
> 为什么传统的 SNS 需要“关注”这样的功能？因为数据都在公司手里，必须提供这个功能，用户才能过滤自己需要的信息。但是在 RUM 恰好相反，数据都在用户手里，用户可以通过自己过滤数据来完成功能。比如这就是一种过滤，用这种方式就不需要传统的”关注“了。可以把各出的数据自己汇总到一起，按照自己喜欢的方式重新组合，甚至分享给别人。如果其中有经济系统关联，还可以收费，分成。

这篇攻略我将分享如何实现 “Huoju 在 Rum 上说了啥” 这样的种子网络。只要你能实现它，你将和我一样切实体会到数据主权被自己掌握的乐趣。

别急着关掉文章，它非常简单，只需 Python 入门水平就能做到。

![图片](https://i.xue.cn/ba551229.png)

## 一、准备

### 安装 Rum APP

Rum APP 在电脑、安卓手机上都能运行，不过我们需要在电脑上编写并运行一些代码，所以请采用电脑安装。

下载地址：
https://docs.prsdev.club/#/rum-app/test

### 安装 Python 

只要是 Python 3.x 版本都可以。官网默认推荐 3.10 版本。

下载地址：
https://www.python.org/downloads

### 安装代码编辑器

你需要一个能用来写代码、运行代码的编辑器。像是 vscode、Jupyter Notebook 等都可以。编辑器只是工具，选择你熟悉或顺手的就行。

如果你之前从来没在本地安装过，建议直接用命令行来安装 Jupyter Lab。——如果你已经安装好 Python，也设置好了环境变量，那么你将可以直接使用 pip 指令。打开 terminal、cmd、或称作命令提示符之类的工具，输入命令行 ```pip install jupyterlab```，不要害怕报错，如果遇到报错，请拷贝报错信息去网上搜索。

Jupyter Lab 的启动方式稍显硬核，不是鼠标双击某个图标，而是需要输入命令行 ```jupyter lab``` 来启动它。如果你用的不顺手，去下载 vscode 也完全可行。不必换来换去，选择一个，熟悉如何熟悉地打开代码编辑器，如何编写和调试 Python 代码~

## 二、练习

### 启动 Rum APP

电脑上安装好 Rum APP 后，会生成一个图标。找到它（比如你的桌面，或是开始菜单）并双击，它就会启动。这是一个桌面应用。

启动后，你需要做几件小事：

1、创建节点，相当于创建账号。注意保管好密码。

2、加入种子网络。可以加入任意多个，当然必须要加入 Huoju 所在的`去中心微博`。

“去中心微博”的种子：

```json

{
  "genesis_block": {
    "BlockId": "7c32c425-a41b-4a0b-96ba-48e2e8816375",
    "GroupId": "3bb7a3be-d145-44af-94cf-e64b992ff8f0",
    "ProducerPubKey": "CAISIQKm+gTifqG6ga1FUb9NzXDetFIi9AosQSx/RBFH3RbGFQ==",
    "Hash": "+y7oVa69YPEOZcbZuUUOLt/i+vv5yychSHz4Xy7T7z8=",
    "Signature": "MEYCIQDlshiApdymHMDK65Qv9VqGevyspb3WW9cLcbHF0r7QagIhAMCxvEREmkQi2IReMu9OBx1rjSJvEcq510CywXYYsWHx",
    "TimeStamp": "1634699220007617449"
  },
  "group_id": "3bb7a3be-d145-44af-94cf-e64b992ff8f0",
  "group_name": "去中心微博",
  "owner_pubkey": "CAISIQKm+gTifqG6ga1FUb9NzXDetFIi9AosQSx/RBFH3RbGFQ==",
  "owner_encryptpubkey": "age1njthmyqheex4gmnl473et8lskj45ydnvt4qz73ngm9m9m42sk4fqrzdcja",
  "consensus_type": "poa",
  "encryption_type": "public",
  "cipher_key": "9d9e13ce3b77f6ae1da4e5ef15d94ff22e77e509dc4e3bdd70fa7435f3a9992b",
  "app_key": "group_timeline",
  "signature": "3045022100cb18857635cadb520a88d6f7a6e4f27133528bad775260f1b2a935af31cf5d4b02203530c40bcdd83bfaa015897bc38fd2cf4c237dc823966f521408066a99fda3cd"
}

```

你可以立即训练自己观察数据结构的能力，以后还需要反复训练。比如上述 seed 中，可以看到它的 `group_id` 是 `"3bb7a3be-d145-44af-94cf-e64b992ff8f0"`。

加入后，静静等待它自动建立连接、自动开始同步数据。在此过程中，你可以继续后续操作。

3、创建种子网络，用来测试。

创建好种子网络后，你可以查看到该种子网络的 `group_id`。在 Rum App 的客户端上，点击这个种子网络的名字，会弹出`种子网络详情`界面，上面的 ID 就是它的 `group_id`。

![图片](https://i.xue.cn/07f21229.png)

在你所创建的种子网络中，随便发布文字、图片~手动制造一点点数据。

接下来，我们可以开始写点代码并运行啦！

### 与 Rum API 交互

启动 Rum APP 并创建账号后，你就自动成为 rum system 网络中的一个节点。同时默默发生了两件事，一件是 quorum 进程被启动，它是 Rum 本地服务端，它将帮你持续地、自动地搜索 rum system 网络中的其它节点，与之建立连接、同步你所加入的种子网络的数据，并把同步到的数据存储在你的电脑上。另一件是 Rum 图形界面也被启动，它是 Rum 本地客户端，你上述创建账号、创建种子网络、加入种子网络，都是在图形界面上点击，你还能在上面查看到你所同步到的数据，比如谁在哪个种子网络发了哪些文本或图片。

Rum 本地客户端，你不需要动它，你可以用它来观察自己的操作效果。

我们要做的是采用 Python 代码来访问 Rum 本地服务端，让代码与服务端建立连接，与服务端提供的 API 交互。这种交互让我们既可以拉取数据（比如查看指定种子网络的内容），也可以推送数据（比如向指定种子网络发布内容）。

我们先试试练习这些基础能力。

#### 与服务端建立连接

代码只有几行：

```python

import requests

session = requests.Session()
session.verify =  r"C:\Users\75801\AppData\Local\Programs\prs-atm-app\resources\quorum_bin\certs\server.crt"
session.headers.update({
    "USER-AGENT": "asiagirls-py-bot",
    "Content-Type": "application/json"})

baseurl = f"https://127.0.0.1:55043/api/v1"

```

但有几处变量，你需要做一点修改：

- `session.verify` 的取值，是 Rum APP 在你本地生成的认证文件的绝对路径，你可以本地电脑上搜索`server.crt`找到它。
- `url` 中有个`55043` 是你本地 Rum 的端口，你可以在 Rum APP 右上角  `节点与网络 - 节点参数 - 端口` 找到它。

运行后，没有任何报错就是好的~

requests 是 python 中的一个 HTTP 库。如果你想要深究 requests 可以访问我的[ requests 学习笔记](https://github.com/liujuanjuan1984/common_python_code/blob/master/Python_requests_examples.ipynb)。更进一步想要深究 HTTP，推荐上野宣所著的《图解 HTTP》，少见的 HTTP 入门读物，在微信读书上有。

深究 requests 或 HTTP 的事，暂放一边。先继续往下看：

#### 向服务端发起请求

采用之前的 `session` 访问 Rum 所允许的 API ，并得到 Rum 本地服务端的反应 `response`。

注：API 是指 `url` 这个字符串，访问是指 `session.get(url)`这个关键语句。

```python

#去中心微博
group_id = "3bb7a3be-d145-44af-94cf-e64b992ff8f0"
url = f"{baseurl}/group/{group_id}/content"
response = session.get(url)
print(response.status_code)
trxs = response.json()

```

你将可以查看 `response` 的状态码 `status_code`，它用来告诉你请求是成功（200）？还是遇到了问题？如果遇到问题，在 `.json()` 中查看更多提示。

你可以用 `response.json()` 处理数据，便于查看或进一步处理。你可以完全不懂 json 模块，因为这个处理得到的就是基本的字符串、列表、字典所构建的复合数据结构。必要的能力是，掌握每个 API 所要求的参数或所返回的数据的数据结构。如何掌握？翻阅 API 文档，以及观察。

比如上方的 trxs 就是一个稍微复杂点的 list ，这个列表的每个元素，是一个复杂点的字典。入门必须的条件/循环语句，就能让你对这些数据做很多事情。——代价只不过是代码稍微多写几行，运行效率不一定最高效，但对于新手来说，也足够用。——有余力，当然也要持续精进。

### 浏览 Rum API 文档

与 Rum API 交互时，我们具体能做什么，按什么规范来做，服务端将给我们什么反应，这些都是 API 文档所描述的。你总是需要查阅文档，让自己熟悉它到底能干什么，以便在产生想法时，能预判什么事情是可以做到的。

你可以直接看我用 Python 封装的 Rum API，关键是看那些 URL，以及 session 的 get 还是 post 的动作，以及相应的参数。你可以实际操作一遍，并观察服务端返回给你的 status_code 和数据。

https://github.com/liujuanjuan1984/common_python_code/blob/master/JJLiu/Rum/API.py

这种尝试、观察对于有经验的开发者来说，是比较浪费时间的。他们会选择直接查看 Quorum 的官方文档，甚至查阅源码：

https://github.com/rumsystem/quorum/blob/main/Tutorial.md 

把 API 文档作为一种随时打开的可查询手册，不用刻意牢牢记住细节。

当你练习了几次，掌握了如何与 Rum 本地服务端交互，也懂得随时查询 API 文档，那么接下来可以考虑实现一些具体想法了。

## 三、实现

### 简单的实现

筛选某人在种子网络 A 的动态，并推送到种子网络 B 。这样的需求该如何实现呢？

基于你对 Rum Api 的了解，可拆解获知实现方式为：

1、API 请求种子网络 A 的所有数据 

`GET:*/group/group_id/content`

2、筛选出某人所产生的数据

3、把原始数据加工成推送时所要求的数据结构 

4、把数据通过 API 推送到种子网络 B 

`POST:*/group/content`

需要提醒的是，请求某个种子网络的所有数据，是在向你本地 Rum 服务端请求它已经保存下来的所有数据。如果你还在同步中，那么已经保存的数据和 Rum System 网络相比，是不全的。但同步状态本身不影响代码尝试。

你遇到的第一个困难，或许是如何知道谁是 Huoju？你在 Rum 本地客户端的“去中心微博”种子网络中看到了 Huoju 的发言，你知道这就是他！但如何让代码能识别出来呢？

观察 API 得到的数据结构是很重要的。前面我们得到的 `trxs` 是由多条 trx 构成的列表，每条 trx 都有一个 `TrxId` ，它长这样：

```python
{'TrxId': '60124f2f-32d7-4b55-96e6-89b79829adea',
 'Publisher': 'CAISIQPDnUIBt1eapHY4sF+1zkIMsXE6CR7MjQfMTsW0iZXHsA==',
 'Content': {'type': 'Note',
  'content': '铲屎的，你买的这批橘子不太新鲜，别着急吃，先观察有没有坏的',
  'inreplyto': {'trxid': 'b3d54b84-31f3-47dc-836e-a1445d01a481'}},
 'TypeUrl': 'quorum.pb.Object',
 'TimeStamp': 1640760417549993900}
```

对于 Rum 来说，每条 trx 就是一条动态。新用户加入种子网络，修改头像、昵称或钱包，用户发布一条内容，评论或回复，点赞或踩，申请成为 producer 等等所以这些行为，都被处理成 trx。而上述 trx 中的 `Publisher` 字段用来标识是谁产生了这个行为。

通过观察，我们知道 Huoju 在`去中心微博`这个种子网络中的 `Publisher` 字段取值为`"CAISIQODbcx2zjXC6AVGFNk3rzfoydQrIfXUu5FDD092fICQLA=="`，它被称作用户的 pubkey，即公钥，是用户在某个种子网络中的唯一标识。所有人都可以改昵称称自己为 Huoju 但唯一标识帮你确认了它就是他。——同一个人在不同的种子网络中的 pubkey 都是不同的。用户可以在不同种子网络拥有全新的头像、昵称和唯一标识。它就像你在微博、推特、简书等等产品中都拥有不同的身份。当然这个信息如果让你困惑，可以先略过。

和用户昵称一样，种子网络也可以重名，唯一识别的办法是 group_id。trx 也有唯一识别，有些地方被写作`trx_id`，有些的地方被写作 `TrxId`，注意观察你得到的数据。

那么，我们可以着手筛选 Huoju 所产生的数据了。

```python

huoju = "CAISIQODbcx2zjXC6AVGFNk3rzfoydQrIfXUu5FDD092fICQLA=="
huojus_data = []
for trx in trxs:
    if trx["Publisher"] == huoju:
        huojus_data.append(trx)
len(huojus_data)

```

在我写这篇攻略时，筛选出 `huojus_data` 有 179 条 trx，各种行为都有。

接下来需要把这些原始的 trx 数据构建成往 Rum 推送内容所需的数据结构，然后尝试推送。

请务必留意：已有的种子网络 `Huoju 在 Rum 上说了啥` 运行得很好，你当然不能直接往那里发你的测试数据。前面你创建的那个种子网络 B 可以用作测试，请往 B 推送。

推送一条内容的代码，长这样：

```python

#往你创建的种子网络中推送内容
#注意修改 group_id 为你所创建的种子网络
group_id = "d87b93a3-a537-473c-8445-609157f8dab0"
url = f"{baseurl}/group/content"
text = "Good Morning!\nHave a nice day."
data = {
            "type": "Add",
            "object": {
                "type": "Note",
                "content": text,
                "name": ""
            },
            "target": {
                "id": group_id,
                "type": "Group"
            }
        }
response = session.post(url,json=data)
print(response.status_code,response.json())

```

你可以观察到，上述代码有个变量 `text` 控制着你所发布的内容。

如何把 Huoju 的 trx 数据构建成 text 呢？策略就是观察 `huojus_data` 里的不同 trx，然后构建出对应的字符串。只需掌握 Python 入门知识就可以搞定的。

我的实现如下：

```python

trxtype = trx["Content"]["type"]
ts = Stime().timestamp_to_str(trx["TimeStamp"])
origin = f"\n\n 来源：{seed}".replace("'", '\"')
if trxtype == "Note":
    inote = trx["Content"]["content"]
    if "inreplyto" not in trx["Content"]:
        note = f"{name} {ts} 说：\n{inote}"
    else:
        jid = trx["Content"]["inreplyto"]["trxid"]
        jnote = self.get_trx_content(gtrxs, jid)
        note = f"{name} {ts} 回复说：\n{inote}\n\n 回复给：\n{jnote}"
if trxtype == "Like":
    inote = self.get_trx_content(gtrxs, trx["Content"]["id"])
    note = f"{name} {ts} 点赞给：\n{inote}"
 ```

### 为更复杂的想法做准备

如果你已经成功推送一条内容，你就有能力推送无数次。用循环语句就能办到。

上面只是介绍了一些基础，它们就好像一块块基础积木块。当你想要实现复杂的需求，用这些积木块不断搭建、封装、重构就好。——是的，尝到乐趣后，记得把重复使用的代码封装为函数或类，然后试试实现更宏大的想法！

很显然，只需要修改几个变量的值，你就能运行这些代码轻松追踪任何其他人的动态，甚至能筛选对方在很多个组的数据，全部归拢并推送到同一个组去。

你可以对自己做这件事：你经常在各个组发布内容，但这些数据不够集中，你想要归拢到一个组内方便自己备份、检索或查阅。大可一试！

你能轻松从本地服务端请求到数据，于是对数据做统计分析，并定时向指定种子网络发布统计结果。

你可以检索自己所加入的所有种子网络中任何人分享的种子，用正则识别出来，然后让自己自动加入。

你可以提前准备好很多条内容，或写个简单爬虫自动爬取有价值的数据，然后以某种节奏，自动推送到指定种子网络。

你甚至可以约定一种标记方式，把 group 用作待办清单管理、或是写作练习薄等等，并能自动筛选、统计。

这些都是我已经尝试通过的想法。你还有哪些奇思妙想吗？或许未来我们可以一起搞一搞。

https://github.com/liujuanjuan1984/common_python_code

最后，欢迎关注我对以上需求的 Python 实现。