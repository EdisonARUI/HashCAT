**HashCAT MVP需求文档 v1.0**  
**最后更新：2025-03-22**  
**核心定位：基于比特币算力风险的链上保险协议**

---

### **一、产品定位**
1. **核心创新**  
   - 将比特币矿池算力波动风险转化为链上保险产品
   - 首创动态精算定价引擎（Dynamic Actuarial Engine）
   - 基于Sui区块链的分钟级赔付验证系统
   - BTC跨链桥接机制（待开发）

2. **差异化价值**  
   ```math
   V_{HashCAT} = \frac{(算力波动率 × 保额)}{(精算模型置信度 × 质押率)}
   ```
   通过该公式实现实时保费定价，较传统保险产品效率提升300%

---

### **二、核心功能模块**
#### **MVP必需模块**
#### **模块1：保险NFT铸造（Mint Insurance）**
- **功能描述**  
  - 矿工输入：算力设备ID（通过Chainlink矿池预言机验证）
  - 参数配置：保额（1-1000 BTC）、保障周期（7-90天）
  - 输出产物：保险NFT（含精算参数元数据）
- **技术实现**
  - 使用Move语言实现NFT标准
  - 集成Chainlink矿池预言机进行算力验证
  - 实现动态元数据更新机制
  - 支持NFT质押和转让功能

#### **模块2：债券池管理**
- **功能描述**
  - 基于总投保额动态调整债券池规模
  - 仅支持sBTC购买和赎回
  - 债券池收益用于支付保险赔付
- **技术实现**
  - 实现自动再平衡机制
  - 集成AMM算法进行流动性管理
  - 支持债券质押和收益分配
  - 实现风险准备金机制

#### **模块3：动态精算引擎**
- **数据输入**  
  - 实时算力数据（30+矿池API聚合）
  - 比特币网络难度调整预测
  - 历史赔付率数据库
- **核心功能**
  - 基于GARCH模型预测未来30日波动率
  - 动态调整质押率（5分钟更新周期）
  - 实时保费定价
  - 赔付阈值计算
- **技术实现**
  - 实现链下计算+链上验证架构
  - 集成机器学习模型进行风险预测
  - 支持多维度风险评估
  - 实现自动参数优化机制

#### **模块4：自动赔付系统**
- **触发条件**（需同时满足）  
  1. 连续2小时算力下降 ≥15%
  2. BTC价格未同步下跌（防止恶意索赔）
  3. 网络难度未发生预期外调整
- **赔付流程**
  1. 触发条件验证
  2. 时间锁确认
  3. 保险NFT验证
  4. 赔付执行
- **技术实现**
  - 实现多预言机数据聚合
  - 集成智能合约自动执行机制
  - 支持赔付状态追踪
  - 实现争议解决机制

#### **延后开发模块**
#### **模块5：BTC跨链桥（V2.0）**
- **功能描述**
  - 支持bitcon链上的BTC与Sui链代币的跨链交换
  - 实现BTC在Sui上的1:1锚定为sbtc
  - 确保跨链安全性（多重签名机制）
  - 支持sui链的sbtc与sui链代币的swap
- **技术实现**
  - 采用多重签名钱包（3/5）进行跨链资产托管
  - 实现原子交换机制确保交易安全
  - 集成Chainlink预言机进行价格喂价
  - 支持跨链交易状态追踪和查询

#### **模块6：Bitcoin Mining Map（V2.0）**
- **功能描述**
  - 直观观察每个地区的算力，便于用户选择
- **技术实现**
  - 集成地理信息系统（GIS）
  - 实现实时算力数据可视化
  - 支持多维度数据筛选
  - 提供算力分布热力图

#### **模块7：流动性池（V2.0）**
- **功能描述**
  - 支持用户向流动性池中组成LP，提高整体流动性
  - 降低swap和bridge功能项目方的流动性压力
- **技术实现**
  - 实现自动做市商（AMM）算法
  - 支持流动性挖矿奖励机制
  - 集成滑点保护机制
  - 实现流动性池再平衡功能

#### **模块8：治理系统（V2.0）**
- **功能描述**
  - 实现去中心化治理机制
  - 支持提案投票和参数调整
- **技术实现**
  - 基于HCT代币的投票权重
  - 实现提案生命周期管理
  - 支持多级治理结构
  - 集成治理奖励机制

#### **模块9：风险监控系统（V2.0）**
- **功能描述**
  - 实时监控系统风险指标
  - 提供风险预警机制
- **技术实现**
  - 实现多维度风险指标计算
  - 集成预警通知系统
  - 支持风险报告生成
  - 提供风险控制建议

---

### **四、技术架构**
```plaintext
Layer 1: Sui区块链（核心合约）
├── 智能合约组
│   ├── ActuarialEngine.move
│   ├── InsuranceNFT.move
│   ├── BondPool.move
│   └── PayoutOracle.move

Layer 2: 混合预言机网络
├── Chainlink矿池数据流
├-> 去中心化节点验证（30+矿池）
└-> 抗女巫攻击机制（PoRA共识）

Layer 3: 前端交互层
├── React dApp界面
├-> 三维算力波动可视化仪表盘
└-> MetaMask/Suiet钱包集成
```

### **四-1、前端需求规范**
#### **1. 用户界面架构**
- **技术栈**
  - React 18 + TypeScript
  - TailwindCSS + ShadcnUI
  - Vite构建工具
  - Sui SDK集成

- **页面结构**
  ```plaintext
  ├── 布局组件
  │   ├── Header（导航栏）
  │   ├── Sidebar（侧边栏）
  │   └── Footer（页脚）
  ├── 核心页面
  │   ├── Dashboard（数据总览）
  │   ├── Insurance（保险产品）
  │   ├── BondPool（债券池）
  │   └── Profile（个人中心）
  └── 功能组件
      ├── 钱包连接器
      ├── 交易确认弹窗
      └── 通知系统
  ```

#### **2. 核心功能模块**
- **Dashboard模块**
  - 实时算力波动图表（使用ECharts）
  - 保险产品概览卡片
  - 债券池状态指标
  - 最近交易记录

- **保险产品模块**
  - 保险产品列表展示
  - 投保流程向导
  - 保单详情页面
  - 理赔申请界面

- **债券池模块**
  - 债券池状态展示
  - 购买/赎回操作界面
  - 收益计算器
  - 历史交易记录

- **个人中心模块**
  - 钱包管理
  - 保单管理
  - 债券持仓
  - 交易历史

#### **3. 交互设计规范**
- **响应式设计**
  - 移动端优先
  - 断点设计：sm(640px), md(768px), lg(1024px), xl(1280px)
  - 自适应布局

- **用户体验**
  - 页面加载时间 < 2s
  - 操作反馈时间 < 300ms
  - 表单验证即时反馈
  - 错误提示友好化

- **可访问性**
  - WCAG 2.1 AA标准
  - 键盘导航支持
  - 屏幕阅读器兼容
  - 高对比度模式

#### **4. 数据可视化要求**
- **实时数据展示**
  - WebSocket实时更新
  - 数据缓存策略
  - 离线数据支持

- **图表组件**
  - 算力波动趋势图
  - 保费定价曲线
  - 债券池流动性图表
  - 交易量热力图

#### **5. 安全要求**
- **钱包安全**
  - 私钥本地加密存储
  - 交易签名确认机制
  - 多钱包支持

- **数据安全**
  - HTTPS加密传输
  - 敏感数据脱敏
  - 防XSS/CSRF攻击

#### **6. 性能指标**
- **加载性能**
  - 首屏加载 < 2s
  - 路由切换 < 300ms
  - 资源按需加载

- **运行性能**
  - 60fps动画流畅度
  - 内存占用 < 100MB
  - 后台任务优化

---

### **五、经济模型设计**
1. **代币体系**  
   - $HCT（HashCAT Token）：治理+费用代币
   - 债券池收益分配：
     ```math
     Revenue_{pool} = 保费收入 × 85% - 赔付支出
     ```