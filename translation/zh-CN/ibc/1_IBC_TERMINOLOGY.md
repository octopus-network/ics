# 1: IBC Terminology

**This is an overview of terms used in IBC specifications.**

**For an architectural overview, see [here](./2_IBC_ARCHITECTURE.md).**

**For a broad set of protocol design principles, see [here](./3_IBC_DESIGN_PRINCIPLES.md).**

**For a set of example use cases, see [here](./4_IBC_USECASES.md).**

**For a discussion of design patterns, see [here](./5_IBC_DESIGN_PATTERNS.md).**

This document provides definitions in plain English of key terms used throughout the IBC specification set.

## Abstraction definitions

### 参与者

*角色*或*用户*是在跨链通信中进行交互的实体。一个参与者可以是一个终端用户，在区块链上运行的模块或智能合约，或者是能够签署交易的链下中继器进程。

### 机器 / 链 / 账本

*机器*，*链*，*块链*或*账本*（可互换使用）是实现IBC规范的一部分或全部的状态机（并不需要严格意义上的区块链，可以是分布式账本或者链）。

### 中继进程

“*中继进程*”是一个链下进程，负责通过扫描两个或更多机器的状态并提交交易来在两台或多台机器之间中继 IBC 数据包和元数据。

### 状态机

特定链的*状态机*定义状态的结构以及一组规则，这些规则确定有效的交易，这些交易基于链的共识算法所认同的当前状态来触发状态转换。

### 共识

*共识*算法是一组操作分布式账本的进程使用的协议，通常在存在一定数量的拜占庭式故障的情况下就同一状态达成协议。

### 共识状态

*共识状态*是关于共识算法状态的一组信息，该状态信息用于验证关于该共识算法输出的证明。

### 承诺

加密*承诺*是一种廉价地验证键/值对映射中的成员是否存在的方法，其中可以使用较短的见证字符串来落实映射。

### 区块头

*区块头*是对特定区块链共识状态的更新，包括对当前状态的承诺，可以通过“轻客户端''算法以明确定义的方式进行验证。

### 承诺证明

*承诺证明*是一种证明结构，用于证明特定键是否映射到承诺集中的特定值。

### 处理模块

IBC *处理模块*是状态机中对 [ICS 25](../spec/ics-025-handler-module) 子协议的实现，负责管理客户端、连接和通道，以及对证明进行验证并对数据包的承诺进行适当地存储。

### 路由模块

IBC*路由模块*是状态机中对 [ICS 26](../spec/ics-026-routing-module) 子协议的实现，在处理模块以及状态机上使用了路由模块外部接口的其他模块之间将数据包进行分发。

### 数据报

*数据报*是指通过某些物理网络传输的非明文字节串，由账本的状态机中实现的 IBC 路由模块进行处理。 在某些实现中，包可以是特定于账本的交易或消息数据结构中的一个字段，其中还包含其他信息（例如，垃圾邮件预防费用，防止重现随机数，路由到 IBC 处理程序的类型标识符等） 。 所有 IBC 子协议（例如，建立连接、创建通道、发送数据包等）都是根据包的集合和协议来定义的，以便通过路由模块处理它们。

### 连接

*连接*是两条链上包含了所连接的账本中的共识状态信息的持久化数据集。意味着，更改所连接的两条链其中一条链上的共识状态会改变另一条链的连接对象的状态。

### 通道

*通道*是两条链上包含了用于促进数据包排序、“有且只有一次”发送和预防重放的元数据。通过通道发送的数据包会更改其内部状态。通道与连接之间是多对一的关系——也就是说一个连接可以拥有任意数量的相关联的通道，并且所有通道必须所属于某个连接，且连接必须先与通道建立。

### 数据包

*数据包*是一种特殊的数据结构，具有与序列相关的元数据（由IBC规范定义）和一个不透明的值字段，称为数据包*数据*（具有由应用层定义的语义，例如通证的名称和数量）。 数据包通过特定的通道发送（以及特定的连接）。

### 模块

*模块*是特定区块链状态机的子组件，可以与 IBC 的处理逻辑进行交互并根据发送或接收的特定 IBC 数据包的*数据*字段更改账本状态（例如，铸币或销毁币）。

### 握手

*握手{/em1}是一类涉及多个包的子协议，通常用于初始化两个相关链上的某些公共状态，例如彼此共识算法的可信状态等。*

### 子协议

子协议定义了一系列必须实现的包类型和功能，应用 IBC 进行跨链的区块链必须有对应的 IBC 处理模块来实现这些协议。

包必须通过外部中继器程序在链之间中继。 假定该中继器过程以任意方式运行-安全特性不受其行为的影响，尽管进步通常取决于至少一个正确的中继器过程的存在。

IBC 子协议被认定为是两条链`A`和`B`之间的相互作用——这两条链之间没有先验区别，并且假定它们正在执行相同的正确的 IBC 协议。 按照惯例，`A` 只是在子协议中排在第一位的链，而 `B` 则在第二条协议中排在第一位。 协议定义通常应避免在变量名中包含`A`和`B`以避免混淆（因为链本身不知道它们在协议中是`A`还是`B`）。

### 身份验证

*身份验证*是确保包实际上是由特定链以 IBC 处理程序定义的方式发送的属性。

## 属性定义

### 最终确定性

*最终确定性*是指在不考虑有关验证者集合行为的某些假设的情况下，某个特定的区块被最终确认并且不可再更改，由共识算法提供的可量化保证。IBC 协议需要最终确定性，尽管它不一定是绝对的（例如，针对中本聪共识算法的确定性阈值将提供确定性，但要遵守有关矿工行为的经济假设）。

### 恶意行为

*恶意行为*是由共识算法定义的一类共识错误，可由该共识算法的轻客户端检测。

### 存疑行为

*存疑行为*是由一个或多个验证者犯下的一类特定的共识错误，它们以无效的方式对不同的区块提案进行投票。 所有的存疑行为都是恶意行为。

### 数据可用性

*数据可用性*是指链下中继处理器在一定时间范围内从世界状态中提取数据的能力。

### 数据保密性

*数据机密性*是状态机节点在不损害 IBC 协议功能的情况下拒绝向特定方提供特定数据的能力。

### 不可否认性

*不可否认性*是指状态极无法成功地质疑已发送的特定数据包或已经提交的特定状态。 IBC 是具备不可否认性的协议，状态机对模块数据的选择是保密的。

### 共识活跃度

*共识活跃度*是特定节点的共识算法对出块的持续性参与度。

### 交易活跃度

*交易活跃度*是指由特定节点的共识算法持续性地对传入的交易进行确认的参与度（哪些交易应根据上下文明确）。 交易活跃度需要共识活跃度，但是共识活跃度不一定提供交易活跃度。 交易活跃度意味着对交易的审查阻力。

### 有界共识活跃度

*有届共识活跃度*是指在特定界限内的共识活跃度。

### 有届交易活跃度

*有届交易活跃度*是指在特定界限内的交易活跃度。

### 有且只有一次安全性

*有且只有一次安全性*是指一个数据包不会被多次确认。

### 抵达或超时安全性

*抵达或超时安全性*是指数据包要么是抵达并执行要么超时这两种可能的结果回执给发送方的属性。

### 复杂度常量

当提到空间或时间复杂度时，代表 `O(1)`.

### Succinct

*Succinct*, when referring to space or time complexity, means `O(poly(log n))` or better.