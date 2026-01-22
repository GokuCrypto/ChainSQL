# AgentFlow 协议白皮书

**版本:** 1.0
**日期:** 2026年1月


## 摘要 (Abstract)

互联网建立在信息的自由交换之上，但价值的交换仍然充满摩擦。随着人工智能（AI）和自主智能体（Autonomous Agents）的兴起，这种摩擦已成为关键瓶颈。AI 智能体可以以光速处理信息，但无法便捷地为它们消耗的数据、API 和计算资源付费。

**AgentFlow 协议** 是一个开放标准，旨在 **Avalanche 网络** 上复活沉睡已久的 HTTP `402 Payment Required` 状态码。它构建了一个机器原生的经济体系，使得每一次 HTTP 请求都可以成为一笔微交易，并在链上进行即时、原子化的结算。利用 Avalanche 的亚秒级终局性（Finality），AgentFlow 为 AI 时代创造了无缝的“按需付费”（Pay-as-You-Fetch）体验。

## 1. 介绍 (Introduction)

### 1.1 网络的“原罪”
HTTP/1.1 协议预留了 `402` 状态码用于数字支付，但从未被标准化。相反，互联网依赖于信用卡、Cookie 和订阅制——这些系统是为人类设计的，而非机器。

### 1.2 AI 智能体的困境
AI 智能体是代表用户执行任务的自主软件程序。为了有效运作，它们需要访问付费 API（例如搜索、天气、金融数据）。然而：
*   智能体无法通过银行账户的 KYC 审查。
*   智能体无法物理持有信用卡。
*   订阅模式（月费）对于只需偶尔调用特定 API 的智能体来说效率低下。

### 1.3 解决方案：AgentFlow
AgentFlow 在客户端（智能体）和服务器（资源）之间建立了一个标准化的协商层。它允许服务器为特定资源要求付费，而客户端可以使用 AVAX 或 Avalanche C-Chain 上的代币即时支付。

## 2. 核心理念 (Core Philosophy)

*   **HTTP 原生:** 我们不重复造轮子。我们使用标准的 HTTP 标头和状态码。
*   **机器优先:** 没有验证码，没有重定向，无需人工确认。只有代码和加密签名。
*   **原子结算:** 每个请求都由可验证的链上交易支持。这确保了绝对的去信任化（Trustlessness），并为 AI 行为创建了不可篡改的审计跟踪。
*   **Avalanche 驱动:** 只有具备 Avalanche 的吞吐量和确认速度的链，才能支持机器对机器商业的高频特性。

## 3. 协议规范 (Protocol Specification)

### 3.1 流程 (The Flow)

1.  **请求 (Request):** 客户端（智能体）请求资源（例如 `GET /api/market-data`）。
2.  **挑战 (Challenge - 402):** 服务器拒绝请求并响应 `402 Payment Required`。
    *   **Header:** `WWW-Authenticate: AgentFlow realm="Avalanche", address="0x...", price="0.1 AVAX"`
3.  **支付 (Payment):** 客户端解析标头，在 Avalanche C-Chain 上构建交易并广播。
4.  **证明 (Proof):** 客户端重新发送请求，并在标头中附带支付证明。
    *   **Header:** `Authorization: AgentFlow tx="0x123..."`
5.  **验证 (Verification):** 服务器（或网关）在链上（或通过本地索引器）验证交易。
6.  **响应 (Response - 200):** 如果验证通过，服务器提供资源。

### 3.2 关键组件

*   **资源网关 (Resource Gateway):** 位于任何 API 前端的反向代理（中间件）。它处理定价逻辑和 402 响应。
*   **AgentFlow SDK:** 供 AI 智能体（Python/Node.js）使用的库，处理钱包管理和自动支付执行。
*   **结算合约 (Settlement Contract):** 一个 Avalanche 智能合约，管理支付路由、退款（如果支付后请求失败）和开发者收入分成。

## 4. 架构与安全 (Architecture & Security)

### 4.1 原子化链上结算 (Atomic On-Chain Settlement)
与定期结算的状态通道或 Optimistic Rollup 不同，AgentFlow 优先考虑 **原子性**。每一个有效的 API 调用都会导致 Avalanche 区块链上的状态变更。
*   **优势:** 智能体与服务器之间无需信任。
*   **优势:** 非常适合高价值、低信任环境。
*   **影响:** 这种设计利用 Avalanche 独特的共识机制来处理突发交易，将区块链转变为 API 访问的实时账本。

### 4.2 安全模型
*   **抗女巫攻击 (Sybil Resistance):** 费用（Gas + 服务费）充当天然的垃圾邮件过滤器。
*   **重放保护 (Replay Protection):** 每笔支付都绑定到特定的请求哈希（URL + 时间戳 + 随机数），防止支付证明被重复使用。

## 5. 用例 (Use Cases)

*   **AI 数据市场:** 智能体按文章或按数据集行付费。
*   **去中心化算力:** 智能体按秒租用 GPU 时间。
*   **API 货币化:** 开发者无需设置 Stripe 或用户账户即可通过开源 API 获利。
  DEMO截图
<img width="1222" height="831" alt="image" src="https://github.com/user-attachments/assets/60d86416-f2f3-41f6-8e90-d7740e051fd2" />

  

## 6. 结论 (Conclusion)
AgentFlow 不仅仅是一个支付工具；它是未来自动化互联网的经济语言。通过将语义网（HTTP）与价值网（Avalanche）相结合，我们正在释放机器经济的真正潜力。
