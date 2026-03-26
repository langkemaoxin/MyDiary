# 如何彻底掌握  `go-ethereum` 源码 ？

> 先说结论：你现在**不要一上来硬啃 `go-ethereum` 源码**。如果前置几乎为 0，直接看这个仓库，大概率只会被 `core`、`p2p`、`trie`、`vm` 这些模块劝退。更好的路线是：

1. 先补 Go 和计算机基础。
2. 再建立“Ethereum 到底在做什么”的整体图。
3. 再回到 `go-ethereum`，按模块拆着看。
4. 最后用小项目和改测试，把理解变成能力。

截至 **2026-03-26**，Ethereum 早已不是单客户端 PoW 时代了；官方文档明确把节点描述为 **execution client + consensus client** 的组合，而 `go-ethereum` 主要是 **执行层客户端**。2014 年白皮书可以看思想，但官方也明确说它**已经不能代表今天的 Ethereum**。这是你学习顺序必须调整的原因。

**建议路线**
1. 第 1-3 周：补基础，只学和 `geth` 强相关的。
   - Go：语法、`struct/interface`、error、包管理、测试、goroutine、channel。
   - 计算机基础：哈希、树、堆、Map、TCP/IP、HTTP、进程/线程、数据库基本概念。
   - 资料：[`A Tour of Go`](https://go.dev/tour/), [`Go Tutorials`](https://go.dev/doc/tutorial/), [`Effective Go`](https://go.dev/doc/effective_go.html)

2. 第 4-5 周：建立 Ethereum 全景图。
   - 搞懂：账户、交易、区块、gas、EVM、节点、执行层/共识层分工。
   - 先不要钻公式，先把“交易怎样变成状态变化”讲清楚。
   - 资料：[`Intro to Ethereum`](https://ethereum.org/developers/docs/intro-to-ethereum/), [`Nodes and clients`](https://ethereum.org/en/developers/docs/nodes-and-clients/), [`Blocks`](https://ethereum.org/en/developers/docs/blocks/), [`Transactions`](https://ethereum.org/en/developers/docs/transactions/), [`EVM`](https://ethereum.org/developers/docs/evm/)

3. 第 6-7 周：补执行层底层数据结构和接口。
   - 学：RLP、Merkle Patricia Trie、JSON-RPC、devp2p。
   - 这一步是你看 `rlp/`、`trie/`、`p2p/` 源码前的门槛。
   - 资料：[`RLP`](https://ethereum.org/developers/docs/data-structures-and-encoding/rlp), [`Merkle Patricia Trie`](https://ethereum.org/developers/docs/data-structures-and-encoding/patricia-merkle-trie/), [`Geth JSON-RPC`](https://geth.ethereum.org/docs/interacting-with-geth/rpc), [`devp2p`](https://geth.ethereum.org/docs/tools/devp2p)

4. 第 8-10 周：开始动 `geth`，但只在本地开发模式里玩。
   - 先跑 `--dev`，别急着同步公网链。
   - 目标：能自己发交易、查区块、看日志、理解一次状态变更。
   - 资料：[`Getting started with Geth`](https://geth.ethereum.org/docs/getting-started), [`Developer mode`](https://geth.ethereum.org/docs/developers/dapp-developer/dev-mode), [`Connecting to consensus clients`](https://geth.ethereum.org/docs/getting-started/consensus-clients)

5. 第 11-12 周：开始读规范和协议升级。
   - 先读和源码强相关的 EIP：1559、2718、4844。
   - 再看执行层规范；如果以后想碰共识层，再看 consensus-specs。
   - 资料：[`EIP-1559`](https://eips.ethereum.org/EIPS/eip-1559), [`EIP-2718`](https://eips.ethereum.org/EIPS/eip-2718), [`EIP-4844`](https://eips.ethereum.org/EIPS/eip-4844), [`execution-specs`](https://github.com/ethereum/execution-specs), [`consensus-specs`](https://github.com/ethereum/consensus-specs), [`Ethereum Whitepaper`](https://ethereum.org/whitepaper/)

**这个仓库怎么读**
别按目录从上到下扫，按“问题驱动”读：

- 入口和启动流程：[README.md](E:/迅雷下载/go-ethereum-master/go-ethereum-master/README.md), [cmd/geth/main.go](E:/迅雷下载/go-ethereum-master/go-ethereum-master/cmd/geth/main.go)
- 节点和服务装配：[eth/backend.go](E:/迅雷下载/go-ethereum-master/go-ethereum-master/eth/backend.go)
- 交易如何改状态：[core/state_transition.go](E:/迅雷下载/go-ethereum-master/go-ethereum-master/core/state_transition.go)
- 区块链主流程：[core/blockchain.go](E:/迅雷下载/go-ethereum-master/go-ethereum-master/core/blockchain.go)
- EVM 执行：[core/vm](E:/迅雷下载/go-ethereum-master/go-ethereum-master/core/vm)
- 世界状态结构：[trie/trie.go](E:/迅雷下载/go-ethereum-master/go-ethereum-master/trie/trie.go)
- P2P 网络：[p2p/server.go](E:/迅雷下载/go-ethereum-master/go-ethereum-master/p2p/server.go)
- RPC 心智模型：[rpc/doc.go](E:/迅雷下载/go-ethereum-master/go-ethereum-master/rpc/doc.go)

**你现阶段最该做的 3 个练习**
1. 用 `geth --dev` 跑一个本地链，自己发送一笔交易，再用 RPC 查它。
2. 写一个 100 行以内的 Go 小程序，用 `ethclient` 读区块头、余额、交易回执。
3. 读懂 `core/state_transition.go` 里 gas 和执行结果是怎么出来的，然后用自己的话复述一遍。

**一个很重要的提醒**
如果你的目标是“能读懂并参与这个项目”，你走的是**客户端/协议工程师**路线，不是先学 Solidity 的路线。Solidity 可以后补，但它不是你现在的第一优先级。

如果你愿意，我下一条可以直接给你做一份**按周拆开的 12 周学习计划**，每天学什么、看哪篇资料、对应看仓库哪个文件。






**目标定位**

如果你是因为 `go-ethereum` 才想入门，我建议你先走这条路线：

**区块链协议/客户端工程师**，也就是偏底层、节点、执行引擎、P2P、状态存储这条线。  
这和“写 Solidity 合约”是两条路，后者可以后补，但不是你现在的主线。

下面这份计划按“**零基础 -> 能读 geth -> 能做小改动 -> 能尝试贡献**”来设计，周期 **6 个月**，每天按 **2-3 小时**算。

---

**总路线**

1. 第 1 阶段：补编程和计算机基础
2. 第 2 阶段：补区块链和 Ethereum 基础
3. 第 3 阶段：上手 Geth 和 JSON-RPC
4. 第 4 阶段：开始读 `go-ethereum` 核心模块
5. 第 5 阶段：做小项目和源码实验
6. 第 6 阶段：尝试读 issue、写测试、做小贡献

---

**第 1 个月：先把地基补起来**

目标：看到 Go 代码不慌，知道程序、网络、数据结构在干什么。

第 1-2 周：
- 学 Go 语法、函数、结构体、接口、指针、错误处理
- 学包管理、模块、测试
- 资料：
  - [A Tour of Go](https://go.dev/tour/)
  - [Go Tutorials](https://go.dev/doc/tutorial/)
  - [Effective Go](https://go.dev/doc/effective_go.html)

第 3-4 周：
- 学并发基础：goroutine、channel、context、mutex
- 补计算机基础：哈希、树、堆、TCP/IP、HTTP、进程线程、编码
- 练习：
  - 写一个 HTTP 客户端
  - 写一个并发爬虫/任务池
  - 写几个 Go 单元测试

这阶段结束标准：
- 你能独立写 300-500 行 Go 小程序
- 你能看懂大部分普通 Go 项目的结构

---

**第 2 个月：建立区块链和 Ethereum 心智模型**

目标：先知道“系统在干什么”，再去看实现。

第 5-6 周：
- 区块链基础：区块、交易、状态、哈希、Merkle 树、签名、共识
- 明白 UTXO 和 Account Model 的差别
- 明白“去中心化账本”到底记录的是什么

第 7-8 周：
- Ethereum 基础：
  - 账户
  - 交易
  - Gas
  - 区块
  - EVM
  - 节点和客户端
  - 执行层与共识层
- 官方资料：
  - [Intro to Ethereum](https://ethereum.org/developers/docs/intro-to-ethereum/)
  - [Nodes and clients](https://ethereum.org/en/developers/docs/nodes-and-clients/)
  - [Blocks](https://ethereum.org/en/developers/docs/blocks/)
  - [Transactions](https://ethereum.org/en/developers/docs/transactions/)
  - [EVM](https://ethereum.org/developers/docs/evm/)

这阶段结束标准：
- 你能讲清楚“一笔交易进入节点后，为什么会导致状态变化”
- 你知道 `geth` 是执行层客户端，不是整个 Ethereum 的全部

---

**第 3 个月：开始真正上手 Ethereum 开发环境**

目标：别只看文章，要自己跑起来。

第 9-10 周：
- 安装并运行 Geth
- 用开发模式跑本地链
- 学 RPC 基本调用
- 学会查区块、交易、余额、日志
- 官方资料：
  - [Geth Getting Started](https://geth.ethereum.org/docs/getting-started)
  - [Install Geth](https://geth.ethereum.org/docs/getting-started/installing-geth)
  - [Geth RPC](https://geth.ethereum.org/docs/interacting-with-geth/rpc)
  - [Ethereum JSON-RPC](https://ethereum.org/en/developers/docs/apis/json-rpc/)

第 11-12 周：
- 写一个 Go 小程序，用 `ethclient`：
  - 连本地节点
  - 查最新区块
  - 查账户余额
  - 查交易回执
- 本地先看这些文件：
  - [README.md](E:/迅雷下载/go-ethereum-master/go-ethereum-master/README.md)
  - [cmd/geth/main.go](E:/迅雷下载/go-ethereum-master/go-ethereum-master/cmd/geth/main.go)

这阶段结束标准：
- 你能把节点跑起来
- 你能用 RPC 和节点交互
- 你知道 geth 是怎么启动的

---

**第 4 个月：开始系统读 `go-ethereum`**

目标：从“会用”进入“看懂主要模块”。

推荐顺序：

1. 启动入口
- [cmd/geth/main.go](E:/迅雷下载/go-ethereum-master/go-ethereum-master/cmd/geth/main.go)

2. 节点装配
- `node/`
- [eth/backend.go](E:/迅雷下载/go-ethereum-master/go-ethereum-master/eth/backend.go)

3. 区块链主流程
- [core/blockchain.go](E:/迅雷下载/go-ethereum-master/go-ethereum-master/core/blockchain.go)

4. 交易执行与状态变化
- [core/state_transition.go](E:/迅雷下载/go-ethereum-master/go-ethereum-master/core/state_transition.go)
- `core/vm/`

5. 状态存储
- [trie/trie.go](E:/迅雷下载/go-ethereum-master/go-ethereum-master/trie/trie.go)
- `core/state/`
- `triedb/`

6. 网络层
- [p2p/server.go](E:/迅雷下载/go-ethereum-master/go-ethereum-master/p2p/server.go)
- `eth/protocols/`

7. RPC
- `rpc/`

读法不要从头逐行啃，按这三个问题读：
- 这个模块负责什么
- 输入是什么
- 输出是什么

这阶段结束标准：
- 你能说出 `cmd/geth -> node -> eth -> core/vm/trie/p2p/rpc` 的关系
- 你能找到“一笔交易执行”的主路径

---

**第 5 个月：补规范和关键协议升级**

目标：开始把代码和规范对应起来。

优先读这几个：
- [EIP-1559](https://eips.ethereum.org/EIPS/eip-1559)
- [EIP-2718](https://eips.ethereum.org/EIPS/eip-2718)
- [EIP-4844](https://eips.ethereum.org/EIPS/eip-4844)

再看：
- [RLP](https://ethereum.org/developers/docs/data-structures-and-encoding/rlp)
- [Merkle Patricia Trie](https://ethereum.org/developers/docs/data-structures-and-encoding/patricia-merkle-trie/)
- [execution-specs](https://github.com/ethereum/execution-specs)

这一阶段你要做两个输出：
- 写一篇你自己的笔记：EIP-1559 如何影响交易费用
- 写一篇你自己的笔记：状态树为什么需要 trie

---

**第 6 个月：开始像工程师一样练习**

目标：从“会看”变成“会做”。

你要做 3 件事：

1. 做一个小项目
- Go 写一个区块链浏览器后端
- 或者写一个轻量 RPC 工具
- 或者写一个交易/区块监听器

2. 改测试，不急着改功能
- 先给某个模块补测试
- 比如 `rpc/`、`rlp/`、`trie/` 中较小的单元测试

3. 尝试读 issue 和 PR
- 看别人为什么改
- 看测试怎么写
- 看协议升级如何落代码

---

**每周学习节奏**

建议固定成这样：

- 周一到周五：每天 2 小时
  - 1 小时看资料
  - 1 小时写代码/做实验
- 周六：3 小时
  - 整理笔记
  - 复盘本周不懂的概念
- 周日：2 小时
  - 只做一个小练习，不开新坑

---

**你现在最该先学的内容**

按优先级排：

1. Go
2. 数据结构和网络基础
3. Ethereum 基础概念
4. Geth 使用和 RPC
5. `go-ethereum` 源码
6. EIP 和执行层规范
7. 合约开发基础

---

**不建议你现在做的事**

- 不建议现在就看 Yellow Paper
- 不建议现在就学太多密码学证明
- 不建议一开始就同步主网节点
- 不建议先学 Solidity 再回来看 geth
- 不建议逐行硬啃 `core/` 全目录

---

**一个现实判断**

如果你能连续 6 个月执行下来，达到的水平通常会是：
- 能看懂 `go-ethereum` 重要模块的大意
- 能自己写 Go 工具和链交互程序
- 能理解常见 EIP 对客户端的影响
- 能开始做小型源码贡献
 