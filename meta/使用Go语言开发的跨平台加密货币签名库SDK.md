# 使用Go语言开发的跨平台加密货币签名库SDK

## 概述

wallet-core是由Go语言实现的加密货币签名库SDK，主要面向移动端应用场景。该库支持Android和iOS平台，可通过Gomobile技术生成.aar和.framework二进制文件。核心功能涵盖BTC、Omni（USDT）和ETH等主流加密货币的交易签名，未来将持续扩展更多币种支持。

**核心特性**：
- 支持冷热钱包与多重签名场景
- 模块化设计降低集成体积
- 跨平台兼容移动端开发框架

👉 [加密货币钱包开发工具包下载](https://bit.ly/okx_welcome)

## 主要功能特性

### 标准协议支持
| 协议类型 | 功能实现 |
|---------|---------|
| BIP39   | 助记词生成与解析 |
| BIP44   | 多币种私钥推导 |

### 加密货币支持
#### 比特币（BTC）
- 原始交易构造
- 交易签名（不支持隔离见证地址）
- 多重签名支持

#### Omni协议（USDT）
- 继承BTC全部功能
- USDT发行与转账支持

#### 以太坊（ETH）
- 原始交易构造
- 交易签名
- 简单多重签名（智能合约实现）
- ERC20代币兼容

## 开发者指南

### 跨平台集成方案
- **Android**：生成.aar文件，支持armeabi-v7a/x86等架构
- **iOS**：打包.framework框架，包含完整API文档
- **跨平台框架**：支持React Native和Flutter集成（参考[dabankio/flutter-wallet-core](https://github.com/dabankio/flutter-wallet-core)）

### 模块化打包策略
根据实际需求灵活组合以下模块：
- `bip39`：助记词功能
- `bip44`：钱包结构规范
- `eth`：以太坊相关功能
- `btc`：比特币核心功能
- `btc+omni`：BTC与USDT联合支持

**体积对比（armeabi-v7a架构）**：
| 模块组合 | 打包体积 |
|---------|---------|
| 全功能包 | 4MB     |
| BTC专用 | 3MB     |
| ETH专用 | 2.2MB   |

### 开发注意事项
1. **私钥管理**：推荐使用Android KeyStore或iOS KeyChain进行安全存储
2. **网络交互**：需自行实现以下功能：
   - 余额查询
   - UTXO获取（BTC）
   - Nonce获取（ETH）
   - 交易广播
3. **测试环境**：
   - BTC测试需配置`BITCOIN_BIN_DIR`
   - Omni测试依赖`OMNI_BIN_PATH`
   - ETH测试需要ganache-cli

👉 [区块链开发者工具包获取](https://bit.ly/okx_welcome)

## 常见问题解答

### Q1：如何精简SDK体积？
A：可通过Makefile配置指定架构打包，推荐使用`flutter build apk --target-platform android-arm --split-per-abi`实现架构精简。

### Q2：iOS打包有哪些特殊要求？
A：建议避免使用byte/int8/uint8类型，改用int64等兼容类型以确保稳定性。

### Q3：如何进行多币种集成测试？
A：执行`go test -tags=integration`并参考`qa/`目录下的测试用例，各币种测试环境配置要求不同。

### Q4：gomobile有哪些已知限制？
A：存在类型导出限制，需遵循[gobind类型规范](https://godoc.org/golang.org/x/mobile/cmd/gobind#hdr-Type_restrictions)，且不支持多个SDK并行加载。

### Q5：如何获取最新版本？
A：通过GitHub仓库持续更新，重大版本变更会通过[support@dabank.io](mailto:support@dabank.io)通知。

## 技术架构解析

### 分层设计模型
```go
// 核心层
import (
    "github.com/dabankio/wallet-core/bip39"
    "github.com/dabankio/wallet-core/bip44"
)

// 链支持层
import (
    "github.com/dabankio/wallet-core/btc"
    "github.com/dabankio/wallet-core/eth"
)

// 业务逻辑层
type Wallet struct {
    mnemonic string
    privateKey []byte
}
```

### 典型使用场景
1. **冷钱包生成**
```go
mnemonic := bip39.GenerateMnemonic(256)
seed := bip39.NewSeed(mnemonic, "")
```

2. **BTC交易签名**
```go
tx := btc.NewTransaction()
signedTx, err := tx.Sign(privateKey)
```

3. **ETH合约调用**
```go
contract := eth.NewContract("erc20.abi")
tx := contract.Call("transfer", to, amount)
```

👉 [区块链技术前沿动态](https://bit.ly/okx_welcome)

## 贡献指南

### 开发流程
1. 提交Issue讨论新需求
2. Fork项目并创建功能分支
3. 编写单元测试（覆盖核心逻辑）
4. 提交Pull Request并等待审核

### 测试规范
- 单元测试：`go test`
- 集成测试：`go test -tags=integration`
- 测试覆盖率要求：核心模块≥85%

## 协议与支持

本项目采用BSD-3-Clause开源协议，商业使用需遵守以下条款：
1. 不得用于非法交易活动
2. 修改代码需保留原始版权信息
3. 不提供任何形式的担保

如需商业支持，请通过[support@dabank.io](mailto:support@dabank.io)联系技术团队。

## 未来规划

### 开发路线图
| 时间节点 | 目标 |
|---------|-----|
| 2024-Q4 | 支持隔离见证地址 |
| 2025-Q1 | 集成DeFi协议交互 |
| 2025-Q2 | 增加Cosmos链支持 |
| 2025-Q3 | 完善多签智能合约 |

### 社区计划
- 建立开发者论坛
- 发布月度技术报告
- 举办区块链黑客马拉松

通过持续的技术迭代和社区建设，wallet-core致力于打造最可靠的移动端加密货币开发框架。