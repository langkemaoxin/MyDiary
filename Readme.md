
# 转Web3之路

> 个人的真实记录，且看我究竟能不能翻身，能不能成长自己，欢迎评论，点赞

## 个人背景介绍

个人背景：90年，2010计算机系，2014年进入社会，现在2016年，走过了12个年头，工作12年，三分钟热度，任何的方向，都没有超过3个月，先后报名了各种培训班。都没有坚持下去....

- 1、尼恩架构班（收藏了好多视频没看）
- 2、拉钩教育（学习了半年后面的项目没学完）
- 3、图灵教育（给了9800，没去上课了）
- 4、考架构师证（没过）
- 5、搞K8s（没坚持下去）
- 6、买了各种算法课（没去看）
- 7、买了各种极客时间的小班课（咸鱼，淘宝二手货）。
- 8、3万多英语课程（搞了半年又退货了，违约金好多。。）

最近又是感觉到迷茫，明明买了很多课程，但是今天决定这个重要学这个，明天觉得那个重要学那个。既想要一份稳定的工作，又不想要这样苟活着。

## 目前的状况

现在工作的内容都是轻车熟路了，每天都在业务里面滚磨带爬的，还是比较稳，公司应该不会把我开掉的。我的想法是年底能够找到一份远程的兼职工作，能够获取额外的收入。

最终决定的方向是：**Go+远程工作+英语**

- 远程工作不受年龄和地域限制，海外（尤其远程工作领域）对年龄包容度高。
- Go语言在远程工作、云原生领域是主流，且你已开始接触。
- 一旦走通，可以同时兼顾当前工作（苟着保底）和远程收入，实现“两条腿走路”

- **国内Java架构师这条路，对我而言已经是“高成本、低边际收益”的赛道**。我投入多年，始终无法突破面试和项目深度，说明这条路与你的能力结构、职业阶段匹配度不高。
- **远程工作是我能实现“收入跃升 + 年龄脱敏”的唯一杠杆**。而Go + 英语是远程岗位（尤其是海外远程工作、外企远程）最刚需的组合。
- **我当前有一份Java工作保底**，这是我转型的缓冲垫，不必急于辞掉。

现在活到这把年纪了，真的得好好的为自己认真的活一次了。今天立下一个Flag，就是这篇文章，我打算一直更新下去，每天都往这个里面进行记录。

### 深入研究项目
>
> 2026年的目标 高并发后端、云原生工具、Web3 基础设施。

1、高并发后端项目 - Gin  <https://github.com/gin-gonic/gin>

- 看它的 Context 是如何设计的（对比 Java 的 ThreadLocal）。
- 看它的 中间件（Middleware）链式调用 是如何实现的。
- 它的 路由树（Radix Tree） 是如何匹配的（理解 Go 对算法效率的极致追求）

2、顶级高并发与指标处理 - Prometheus  <https://github.com/prometheus/prometheus>

- 看它的 Goroutine 池 是如何管理的。
- 看它的 存储引擎（TSDB） 接口定义（学习 Go 的接口抽象高度）
- 看它如何处理 优雅停机（Graceful Shutdown）。

3、以太坊的官方实现 Go-Ethereum (Geth)  <https://github.com/ethereum/go-ethereum>

- 看它的 P2P 网络层 是怎么写的
- 看它的 并发模型 怎么处理区块链的共识。
- （看懂任何一块代码，工作任我挑选）
  
## 每日行动清单

### 2026年03月21日 周六   [**第1天**]

今日陪同家人去玩，没有学习。最近有所感悟，想改变自己，彻底摆脱一事无成的自己。于是定下了为期300天的规划。分别使用Gemini和DeepSeek在做规划。还是现在11点钟了，有点累了，明天周日再规划学习路径。

### 2026年03月22日 周日  [**第2天**]

在针对做计划的时候，先前总是想着能够非常的具体，精确到每天什么的，规定好每天的任务，但是这样一来会约束自己，在做计划的时候，任务越是明确，感觉自己的灵活度不够。所以在做计划的初期，我就粗略的定下一个目标，就是得把三个Go项目啃下来，分别是Gin，Prometheus,Geth。具体怎么做，再单独新增一篇文章来处理吧。

目前自己不太懂Go语法，如果还是使用以前的方法，从头到尾的把语法过一遍。那就太慢了。目标就是搞懂这个Gin，进行源码分析，输出文章。语法什么的都是顺手的事情。

今天主要的内容就是学习 Gin中的使用，和Geth的使用，我先是使用codex，对整个项目进行分析。然后让它来教会我应该如何学习这个项目。

今日学习总结：

1、在Gin这个项目中，主要的学习了最简单的Web启动流程中的，这段代码中，了解了

``` go

func main() {
 // Create a Gin router with default middleware (logger and recovery)
 r := gin.Default()

 // Define a simple GET endpoint
 r.GET("/ping", func(c *gin.Context) {
  // Return JSON response
  c.JSON(http.StatusOK, gin.H{
   "message": "pong",
  })
 })

 // Start server on port 8080 (default)
 // Server will listen on 0.0.0.0:8080 (localhost:8080 on Windows)
 if err := r.Run(); err != nil {
  log.Fatalf("failed to run server: %v", err)
 }
}

```

r.GET("/ping", handler) 是向 Gin 的 Engine 注册一条规则：当收到 GET /ping 请求时，使用这个 handler 处理。真正收到 HTTP 请求后，请求会先进入 ServeHTTP，再进入 handleHTTPRequest。Gin 会根据请求的 Method 和 Path 找到之前注册的 handler，然后把当前请求的 Context 传给它执行。通过 c.Next() 按顺序执行这一组函数。

```go
r.GET("/ping", m1, m2, handler)
```

这是注册一条 GET /ping 规则
这条规则绑定了一组函数：m1、m2、handler
前面的通常是中间件，最后一个通常是真正业务 handler
请求来了以后，Gin 找到这条规则
然后通过 c.Next() 启动这组函数的执行
中间件里如果不调用 c.Next()，后面通常不会继续执行
中间件里 c.Next() 前后的代码，分别表示“后续流程前”和“后续流程后”
所以执行顺序会像这样：

```
m1 before
m2 before
handler
m2 after
m1 after
```

2、go-ethereum项目中的学习情况：

今天再次的学习这个项目时，我没有一上来就学习里面的源码，我让Codex告诉我这个项目能够干什么，是否能够启动起来。

``` sh
go run .\cmd\geth --dev console
```

这会启动一个本地开发用的 Geth 节点，并进入交互控制台

通过这种方式启动时会：

- 1、自动给你一个测试账户
- 2、默认可挖矿
- 3、不连外部网络
- 4、适合学习和实验

```
运行后节点已经启动成功，而且当前开发链的区块高度是 0。

Using developer account address=...
意思：Geth 自动给你准备了一个测试账户。

network=1337
意思：你现在跑的不是以太坊主网，而是一条本地开发链。

Writing custom genesis block
意思：程序正在写入这条开发链的创世块，也就是第一个区块。

Loaded most recent local block number=0
意思：当前链上只有创世块，所以高度是 0。

You are running Geth in --dev mode
意思：这是开发模式，不是正式网络环境。

Networking is disabled
意思：这个节点不会去连接外部以太坊节点，只在你本机自己玩。
```

今天太晚了，就搞到这里吧。

### 2026年03月23日 周一  [**第3天**]

今天在上班的路上，听英语的博客，两人对话的那种，发现自己很多都是听不懂，因为我看了一下词汇量部分，快接近一万了那种，想要真正的去听懂这些，词汇量必须得上才行。B站上面有个恋恋有词的，我在想是不是可以去听这些单词讲解类的，起码能够快速的提升自己的英语听力和应用能力水平。

上午使用Go语言做了一个Excel导出Sheet的工具，建了Github仓库上传了。里面具体的代码细节还没有彻底搞明白，晚上再深入的研究每一行代码看看怎么回事。

午睡后醒来后，发现脉脉发了一条消息，50K-80K 15薪的公司，要求要有架构经验，Java后端框架，SpringBoot,微服务，数据库，Nosql,消息系统（RabbitMq,Kafaka，RockMq等）还需要有LLM相关项目的架构开发设计经验，还要有英语作为工作语言，全球化的跨时区工作。

英语真的是必备一门技能。目前每天投入的比重还是很少，词汇量不够，应该如何办呢？

晚饭时间，看了B站上面的恋恋有词，朱伟老师的的单词讲解，感觉能够学到东西。他的例句都是从报纸中获取的，感觉很有难度，说是有7000词什么的。不过这个背单词的话，肯定得重复几遍才行的。光靠看一次，是不够的。后面要么买一本对应的单词书。

然后又网上搜索了一下，他们觉得这个恋恋有词，废话太多了，推荐庞肖狄词汇。还没看Mark一下。

晚上看了一下 gin的说明文档，打算深入学习这个框架了，但是在学习框架之前，必须得要知道当前这个项目能够做什么？先把文档看看
<https://gin-gonic.com/en/docs/introduction/>

[Go Examples](https://github.com/gin-gonic/examples) 这个项目是go语言的案例，可以先看看这个项目。快速对整个Gin能做什么有一个快速的了解。

比较了下，还是恋恋有词的好一些。后面就跟着这个课程走吧，在上班通勤的路上。

今晚在公司的时候，看了关于gin的官方文档，有一个系列教程的。还有一个example项目。目前是使用codex进行项目分析，然后制定学习路径进行学习。

### 2026年03月24日 周二  [**第4天**]

早上在想，要么整一个项目，在实现功能需求的时候，把相关的知识点全部串起来，这样学习的方式最快。不过也得看看Go适合做什么，它不是号称性能很高么，底层究竟是什么原理呢？
晚点github搜下实用的项目？

算了，还是先把那个gin example的项目，全部吃透了再说。

早上上班通勤的路上，学习了朱伟老师恋恋有词的第二课，还没有学完，晚上回家路上继续。

现在铺天盖地的都是小龙虾，小龙虾，Agent什么的，等过阵子，比如说把 Gin完全吃透了，再整个极客时间的课程来深入学习下。

工欲善其事必先利其器，所以，今天我得好好看看，我所感兴趣的技术路线图，究竟包含了什么内容。

<https://roadmap.sh/system-design>
系统设计：这个是一个比较高纬度的视角去看待一个系统中的非功能性需求，考虑一个系统的稳定性，可靠性，负载均衡，缓存策略，性能反模式，数据库技术选型，监控，云设计模式等，如何设计一个弹性的系统。这是一个架构师必备的路线图

<https://roadmap.sh/design-system>
设计系统，感觉更像偏前端的一个路线图，UI元素什么的

<https://roadmap.sh/software-design-architecture>
如何构建一个好的系统，代码应该如何写，如何组织，一些设计原则，设计模式，系统架构模式（领域去懂你，MVC，微服务，Serverless,CQRS）

<https://roadmap.sh/software-architect>
系统架构设计中，如何和其他的组件进行交互，从技术选项，架构模式，工具，大数据，数据分析，网络，移动端，更像一个技术员应该掌握的技能

<https://roadmap.sh/blockchain>
区块链机构，共识算法，EVM以太坊，UXTO，TVM，Solana,智能合约，Ide,单元测试，安全性，dApp,前端知识，客户端就节点，应用Defi,NFTS,扩展构建

<https://roadmap.sh/claude-code>
基本组件，ClaudeCode命令，工作流，Skills,子代理，上下文管理，MCP，插件，安全性

<https://roadmap.sh/golang>
数据类型，变量，数组，Maps，结构，方法，指针，泛型，接口，代码组织，错误处理机，并发，测试，网络,ORM,日志，GO 命令，代码分析，内存管理

<https://roadmap.sh/java>
基本概念类型，面向对象变成，错误处理，依赖注入，文件操心，集合，并发，功能性编程，构建工具，Web框架SpringBoot,数据库访问，日志，测试

<https://roadmap.sh/backend>
语言，网络相关概念，版本控制，数据库，APi（类型，授权方式，网络安全）缓存，WebServers服务，AI相关（LLM，RAG等），编程助手，提示工厂，MCP，Agent,代码重构，文档生成，AI 提供上，集成模式，CI/CD,测试，容器化（Docker，K8s），消息队列，搜索引擎（ES,Solr），实时数据，可扩展数据库，NoSql数据库，NoSql数据库集合，可观测性

<https://roadmap.sh/devops>
脚本，监控，操作系统，VCS，容器，网络协议，网络应用，云提供者，Serverless,CICD，隐私管理，可观测性，云设计模式

<https://roadmap.sh/ai-agents>
LLM基础，模式，提示工厂，工具，MCP，创建MCP服务，代理记忆，代理架构，构建代理，Function,Calling,框架（LangChain,LangGraph）,可观测性工具（LangFuse,LangSimis），安全性

<https://roadmap.sh/ai-engineer>
LLM是如何工作的，LLM因素，Token，Context,提示工程，上下文工程，AI模型，选择正确的模型(Gemimi,OpenAI,Claude)，平台（Hagging Face）,向量化，Embeddings，Rag,MCP,安全与道德，开发工具

<https://roadmap.sh/vibe-coding>
使用氛围编程的一些有用的建议

<https://roadmap.sh/aspnet-core>
数据库部分，核心概念，数据映射，缓存，依赖注入，数据库类型，日志框架，API客户端，实时通讯，对象映射，人呢无调度，测试用例框架集合，微服务相关（消息队列，消息总线，网关，其他框架，MediatR,Polly等）

<https://roadmap.sh/spring-boot>
安全，自动装配 ，其他的关键组件

晚上回家的地铁路上，听了恋恋有词的视频，学习了一些东西。

### 2026年03月25日 周三  [**第5天**]

今天听到一个比较惊悚的真相，现在整个世界都在进行一次程序员的大屠杀，掌握Ai的人会把那些不会Ai的人全部Kill掉。所以究竟是被人杀，还是拿起刀去杀别人，是一件值得深思的事情。这个世界已经容不得不会Ai的人了。

在目前的工作中，其他同事在搞AI相关的，而我目前的工作内容并不包含。但是我不包含，并不意味着自己就可以不懂这个技术，相反，要比其他人得更懂这个技术才行。

因为：在这个不是杀人就是被人杀的时代，得要有自保能力才行啊。 -- 我只想活着

为了保持时刻的技术灵活性，应该跟着极客时间，他们出什么课程，就要去买什么课程去学习才对，这样才能跟得上技术前沿。

所以要么时间再分配一下，早上多关注AI相关的，LLM相关的，下午主力输出技术，晚上再搞Web3，Go相关的？

学习了一下  [Langsmith](https://docs.langchain.com/langsmith/observability),简单的来说，就是针对这些大模型的调用，增加一个Log日志，监控日志，有点像Skywalking一样的东西。然后配合一个 Langsmith UI 可以可视化的看一些细节

``` py
from openai import OpenAI
from langsmith.wrappers import wrap_openai  # traces openai calls

def retriever(query: str):
    return ["Harrison worked at Kensho"]

client = wrap_openai(OpenAI())  # log traces by wrapping the model calls

def rag(question: str) -> str:
    docs = retriever(question)
    system_message = (
        "Answer the user's question using only the provided information below:\n"
        + "\n".join(docs)
    )
    resp = client.chat.completions.create(
        model="gpt-4.1-mini",
        messages=[
            {"role": "system", "content": system_message},
            {"role": "user", "content": question},
        ],
    )
    return resp.choices[0].message.content

if __name__ == "__main__":
    print(rag("Where did Harrison work?"))
```

目前Claude Code 我还没有深入使用过，这块得花时间补上才行，因为现在铺天盖地的的都是他们，但是自己都没有掌握，闭着眼睛，等别人宰么？

还有各种Skills，MCP什么的，自己的没有了解，真的是睁眼瞎了。


今日总结：今天上午主要是扩展自己的知识面，看看前沿的技术都有什么，AI相关的，下午所有的时间都在AI上面了，虽然也是使用Codex进行了代码工作。但是对于ClaudeCode和Ai Agent节点流程编辑，还是不懂

如何打造一个虚拟团队，是迫在眉睫的事情。

今天已经列举了很多的Github仓库连接。得每个仓库都要花时间去学习，起码能够跑起来，看看是什么效果才行。

不过，今天有些点还是没有做好。就是，通勤时间没有完全利用起来，应该全部用来学习英语单词的。Go语言今天没有任何进步，区块链也是没有任何的进步。今天打个差评。工作时间太长了，得缩短一些才行。要有更多的时间用来学习新的知识。看新的网站和github仓库。

[2026-03-25-整理的资料集合](./2026-03-25-整理的资料集合.md)


### 2026年03月26日 周四  [**第6天**]

昨晚一夜没睡，今天上班通勤没有听英语
之前有个项目，我使用Codex做了一个Web页面，昨天晚上和今天早上在学习相关的React语法

今天正式重新规划了从4月份到12月份的一个学习计划，精确到每个周，需要产出什么。

早上还搭建了一下自己的博客  <https://langkemaoxin.github.io/> 后面就把自己想要发表的，想要说的内容就通过博客给记录下来

晚上的时候，看了一下，使用Java如何搭建一个MCP服务器
是公共@MCPTool去暴露接口，然后Agent或者Ide直接使用这个 MCP地址。

关于MCP的相关概念得系统学习下

<https://modelcontextprotocol.io/specification/2025-11-25/basic>

<https://github.com/langchain-ai/langchain-mcp-adapters>

在目前的工作当中，用到了 DeepAgent也带好好研究一下究竟是什么东西

<https://github.com/langchain-ai/deepagents>

<https://docs.langchain.com/oss/python/deepagents/overview>

关于Codex Skill，一直想要去好好的看看怎么使用的。

### 2026年03月27日 周五  [**第7天**]

早上上班途中看了恋恋有词的视频，还是得做笔记才行，不过也可以就这样。自己所有的闲暇时间，都用来学习英语。
通过他们分析例句，提高自己的英语水平。

目前自己想要学习的东西太多了，但是Go和Web3是一定要去学习的，Ai，Agent，要求熟悉和掌握，自己可以不去开发，但是一定得看得懂，知道他们在干什么。今天第7天了，关于GO和Web3都没怎么动，是有问题的。

工作中，分为本职工作，必须要写的代码，还有AI Agent相关的，这个也是工作内容，只不过是别人的工作内容而已。

最近收集了很多的仓库的github。但是很多仓库都不明白是什么意思，也不知道是怎么用的。

对于每个仓库，起码得知道，他都能够做什么。并且能够写下日记来记录一下。

关于AI Agent的很多概念，得跟着RoadMap快速的学习和了解一下。

我刷到一个仓库，可以使用很多免费的API，到目前为止，我都没怎么好好的使用API Key，去尝试这些新的技术点。

关于这个ClaudCode 很多Agent都需要 API Key,先解决这个 API Key免费问题。

还有一个，能够让我们去使用页面登录，然后就可以免费使用API了，这两个仓库先好好的看看。

[2026-03-27-整理的资料集合](./2026-03-27-整理的资料集合.md)


## 2026年03月28日 周六 [**第8天**]

今日工作安排

从今天开始

设计一个名字加做《闻啼鸟》的APP，用户录制一段鸟叫声，然后AI分析出是什么鸟，有可能AI会分析出多个，每种鸟都有对应的置信度。整体设计要美观大气清新。

今天学习了很多的东西，比较杂乱，现在整理了一下

# 在这个页面中 OpenCode+全新工具链，开发识别鸟叫声手机APP

<https://www.youtube.com/watch?v=bQxVeJmJTO4>

> 我用一套全新工具链，VibeCoding了一个好玩的手机APP----《闻啼鸟》。只需要录制一段小鸟的叫声，AI就能分析出鸟的品种，并且弹出图文知识卡片。  我使用OpenCode实现了一个Python后台服务，调用本地离线AI模型识别鸟类音频。 前端部分则是先使用Google Stich生成设计稿，然后导入AI Studio 使用Gemini3模型生成对应的前端代码，使用OpenCode完成本地调试，最后使用Capacitor打包成安卓或者iOS APP

使用 stitch 生成一个网页的图片
<https://stitch.withgoogle.com/projects/12889535102414467041?pli=1>

然后使用 AI Stdio生成能够运行的网页
<https://aistudio.google.com/apps/f02ca462-1c06-43ce-ae63-85e9e96e59bb?showAssistant=true&showPreview=true>

最后生成真正的IOS应用类的App

## 项目的分析的可视化

使用这个项目，把一个比较庞大的系统，通过可视化的方式来展示出来

<https://github.com/abhigyanpatwari/GitNexus?tab=readme-ov-file>

```
git clone https://github.com/abhigyanpatwari/gitnexus.git
cd gitnexus/gitnexus-web
npm install
npm run dev
```

## MCP服务的安装

Playwright MCP，安装了之后，还没有怎么使用，貌似就是指挥浏览器能够干一些事情，打开微薄，回复评论一类的。是不是也可以做一些功能测试？自动化测试。需要消耗token不？

## 晚上学习了一下 Go方面的基本概念，觉得效果不佳，好慢啊

## 2026年03月29日 周日 [**第9天**]

这些天，就像一个屯屯鼠一样，收集了很多的Github仓库资料，但是就是在转发，没有深入去了解。
得把自己看过的的仓库进行深入的了解和研究

这些天，总想着使用OpenCode去完成任务，但是总是会提示区域限制问题。今天终于解决了这个问题，就是用 Tun（虚拟网卡的形式）
这种形式的话，就是强制走区域流量。

今天的任务是学习OpenCode

Yutub的视频链接
<https://www.youtube.com/watch?v=JYVTUU9ClUA&t=866s>

<https://github.com/anomalyco/opencode>

可以使用 gemini-3-pro 和 claude-opus-4-5-thinking 模型的插件
<https://github.com/NoeFabris/opencode-antigravity-auth>

安装的方式: 把这个脚本放入到 codex命令行中

``` sh
Install the opencode-antigravity-auth plugin and add the Antigravity model definitions to ~/.config/opencode/opencode.json by following: https://raw.githubusercontent.com/NoeFabris/opencode-antigravity-auth/dev/README.md

```

然后自己就接入进来了。

使用/Connect 可以接入任何模型

比如：使用OpenRouter进行接入

/session  会话列表

/share

### Oh my opencode  里面集中了一个智能体团队
>
> 预设工具 + 预设MCP + 预设Agent

#### 预设MCP

websearch 实施搜索
Context7 获取代码的最新官方文档
grep_app 在github仓库中进行快速的代码搜索

#### 安装

```
Install and configure oh-my-opencode by following the instructions here:
https://raw.githubusercontent.com/code-yeongyu/oh-my-openagent/refs/heads/dev/docs/guide/installation.md
```

#### 使用方式1

使用@的方式，挑选一个智能体给我们干活

#### ulw 魔法词，可以开启一个模式，设置好一个目标之后，计算机会尽可能得实现一个目标

Ultraworker

设计一个宠物店的网站

#### /ralph-loop循环 可以长时间的做一个任务

例子： `/ralph-loop 使用springboot4最新标准重构整个项目，直到所有测试用例通过`

可以运行好几个小时，直到任务完成

#### 常用命令

/init  创建一个Agents.md 帮助AI快速了解项目
/compact 把之前的对话提炼成一个简单的对话

#### 自定义命令

在C:\Users\CGY\.config\opencode\command 下面 可以自定义命令

比如：run_test

C:\Users\CGY\.config\opencode\command\run_test.md

我就发起了一个指令，让你系统自动安装好 mcp
<https://github.com/github/github-mcp-server>

#### 用于生成PPT，但是得消耗比较多的Token

<https://github.com/Anionex/banana-slides>

#### 网络抓取工具

<https://github.com/D4Vinci/Scrapling>

#### 多Agent的分工合作

<https://github.com/cft0808/edict>

#### 晚上做的事情

我向Codex发送了一个请求：

```
我的需求是想要你把我的这个文件夹推到我的Github仓库区，那么当我发出指令开始到最后成功了，中间究竟是经历了什么步骤？
你是如何解决的？到后面为什么要使用这个 gh? 这个究竟是什么？ 关于这个opencode究竟是如何和github mcp进行交互的，要说明清楚
 
请你作为一个名资深的写作专家，把上述的内容，写一个一篇博客。

放在 D:\Github\langkemaoxin.github.io\_posts下

要求需要符合我的博客格式。

完成后，D:\Github\langkemaoxin.github.io 是一个我的博客github项目，把他给提交，并且推送出去
```

然后我让opencode 把我刚刚的行为固化下来，形成一个Skill，后续在处理完问题后，就可以直接解决了什么问题，记录到一个我的github的个人站点上了

## 2026年03月30日 周一 [**第10天**]

今天回到公司的第一件事情，就是把我公司的电脑，设置好相关环境，安装相关的项目

1、安装 opencode
2、安装 oh my opencode 搭建起来一个高效能的AI团队
3、安装好了 gh, Github Cli,并且进行授权验证登录（因为在tun模式下面，无法使用SSH验证的呢牢固）
4、安装好了昨天在家里些的Skill，提供了一套地址，再把这个地址，安装到我的电脑中，这个已经跑通了，每次用的时候，最好加上一句话，就是说使用SKILL。这样就可以确保他使用Skill了，因为我让他写一个博客的时候，他没有Get到我的意思

## 2026年03月31日 周二 [**第11天**]

进行得完成的是，就是安装一下Clade Code，但是现在无法安装，有问题

今天已经是第11天了，感觉对于我的正事，还是没有什么推进，如何获取一份远程工作，PartTime那种，完成任务就行。
今天去招聘的网站上去看看，究竟有什么岗位可以做。

搜索了网络中，有关于区块链相关的仓库和书籍，因为我看到的资料，都是很老的资料，难道这个领域真的不行了么？
所以我现在得先知道，关于区块链，以太坊现在最新的咨询和信息才行。

[2026-03-31-整理的资料集合](./2026-03-31-整理的资料集合.md)

最近收集了很多的项目，总算是对整个网络情况，网络社会情况有了一定的了解。所以现在需要做的就是制定好学习的计划。因为每个人的时间，经历都是有限的，不能想屯屯鼠一样，收集了很多，但是从未开始。

收集资料的时候，是一种，只要收集了，就能学到位了的感觉。但是我这次的感觉是，自己能够了解，网上有什么资料，可以让我去学习。
