# 👻 Ghost 安全模块
**基于 PowerShell 的 Windows 和 Azure 安全加固工具**

> **为 Windows 端点和 Azure 环境提供主动安全加固。** Ghost 提供基于 PowerShell 的加固功能，可通过禁用不必要的服务和协议来帮助减少常见攻击向量。

## ⚠️ 重要免责声明

**需要测试**：始终先在非生产环境中测试 Ghost。禁用服务可能影响合法的业务功能。

**无保证**：虽然 Ghost 针对常见攻击向量，但没有任何安全工具能够阻止所有攻击。这是综合安全策略的一个组成部分。

**操作影响**：某些功能可能影响系统功能。在部署前请仔细检查每项设置。

**专业评估**：对于生产环境，请咨询安全专家以确保设置符合您组织的需求。

## 📊 安全态势

勒索软件损失在 **2025 年达到 570 亿美元**，研究表明许多成功的攻击利用了基本的 Windows 服务和错误配置。常见攻击向量包括：

- **90% 的勒索软件事件**涉及 RDP 利用
- **SMBv1 漏洞**使 WannaCry 和 NotPetya 等攻击成为可能
- **文档宏**仍是恶意软件传播的主要方法
- **基于 USB 的攻击**继续针对气隙网络
- **PowerShell 滥用**在近年来显著增加

## 🛡️ Ghost 安全功能

Ghost 提供 **16 个 Windows 加固功能** 加上 **Azure 安全集成**：

### Windows 端点加固

| 功能 | 目的 | 考虑因素 |
|------|------|----------|
| `Set-RDP` | 管理远程桌面访问 | 可能影响远程管理 |
| `Set-SMBv1` | 控制传统 SMB 协议 | 非常旧的系统需要 |
| `Set-AutoRun` | 控制 AutoPlay/AutoRun | 可能影响用户便利性 |
| `Set-USBStorage` | 限制 USB 存储设备 | 可能影响合法 USB 使用 |
| `Set-Macros` | 控制 Office 宏执行 | 可能影响启用宏的文档 |
| `Set-PSRemoting` | 管理 PowerShell 远程连接 | 可能影响远程管理 |
| `Set-WinRM` | 控制 Windows 远程管理 | 可能影响远程管理 |
| `Set-LLMNR` | 管理名称解析协议 | 通常禁用是安全的 |
| `Set-NetBIOS` | 控制 TCP/IP 上的 NetBIOS | 可能影响传统应用程序 |
| `Set-AdminShares` | 管理管理共享 | 可能影响远程文件访问 |
| `Set-Telemetry` | 控制数据收集 | 可能影响诊断功能 |
| `Set-GuestAccount` | 管理来宾账户 | 通常禁用是安全的 |
| `Set-ICMP` | 控制 ping 响应 | 可能影响网络诊断 |
| `Set-RemoteAssistance` | 管理远程协助 | 可能影响帮助台操作 |
| `Set-NetworkDiscovery` | 控制网络发现 | 可能影响网络浏览 |
| `Set-Firewall` | 管理 Windows 防火墙 | 对网络安全至关重要 |

### Azure 云安全

| 功能 | 目的 | 要求 |
|------|------|------|
| `Set-AzureSecurityDefaults` | 启用基本 Azure AD 安全 | Microsoft Graph 权限 |
| `Set-AzureConditionalAccess` | 配置访问策略 | Azure AD P1/P2 许可 |
| `Set-AzurePrivilegedUsers` | 审计特权账户 | 全局管理员权限 |

### 企业部署选项

| 方法 | 用例 | 要求 |
|------|------|------|
| **直接执行** | 测试、小环境 | 本地管理员权限 |
| **组策略** | 域环境 | 域管理员、GP 管理 |
| **Microsoft Intune** | 云管理设备 | Intune 许可、Graph API |

## 🚀 快速开始

### 安全评估
```powershell
# 加载 Ghost 模块
Invoke-WebRequest 'https://raw.githubusercontent.com/jimrtyler/Ghost/main/Ghost.ps1' -OutFile .\Ghost.ps1
Get-Content .\Ghost.ps1
. .\Ghost.ps1

# 检查当前安全状态
Get-Ghost
```

### 基本加固（先测试）
```powershell
# 基本加固 - 先在实验室环境中测试
Set-Ghost -SMBv1 -AutoRun -Macros

# 审查变更
Get-Ghost
```

### 企业部署
```powershell
# 组策略部署（域环境）
Set-Ghost -SMBv1 -AutoRun -GroupPolicy

# Intune 部署（云管理设备）
Set-Ghost -SMBv1 -RDP -USBStorage -Intune
```

## 📋 安装方法

### 选项 1：直接下载（测试）
```powershell
Invoke-WebRequest 'https://raw.githubusercontent.com/jimrtyler/Ghost/main/Ghost.ps1' -OutFile .\Ghost.ps1
Get-Content .\Ghost.ps1
. .\Ghost.ps1
```

### 选项 2：模块安装
```powershell
# 从 PowerShell Gallery 安装（当可用时）
Install-Module Ghost -Scope CurrentUser
Import-Module Ghost
```

### 选项 3：企业部署
```powershell
# 复制到网络位置进行组策略部署
# 配置 Intune PowerShell 脚本进行云部署
```

## 💼 用例示例

### 小企业
```powershell
# 最小影响的基本保护
Set-Ghost -SMBv1 -AutoRun -Macros -ICMP
```

### 医疗环境
```powershell
# 以 HIPAA 为重点的加固
Set-Ghost -SMBv1 -RDP -USBStorage -AdminShares -Telemetry
```

### 金融服务
```powershell
# 高安全性配置
Set-Ghost -RDP -SMBv1 -AutoRun -USBStorage -Macros -PSRemoting -AdminShares
```

### 云优先组织
```powershell
# Intune 管理部署
Connect-IntuneGhost -Interactive
Set-Ghost -SMBv1 -RDP -AutoRun -Macros -Intune
```

## 🔍 功能详细信息

### 核心加固功能

#### 网络服务
- **RDP**：阻止远程桌面访问或随机化端口
- **SMBv1**：禁用传统文件共享协议
- **ICMP**：防止用于侦察的 ping 响应
- **LLMNR/NetBIOS**：阻止传统名称解析协议

#### 应用程序安全
- **宏**：禁用 Office 应用程序中的宏执行
- **AutoRun**：防止从可移动媒体自动执行

#### 远程管理
- **PSRemoting**：禁用 PowerShell 远程会话
- **WinRM**：停止 Windows 远程管理
- **远程协助**：阻止远程协助连接

#### 访问控制
- **管理共享**：禁用 C$、ADMIN$ 共享
- **来宾账户**：禁用来宾账户访问
- **USB 存储**：限制 USB 设备使用

### Azure 集成
```powershell
# 连接到 Azure 租户
Connect-AzureGhost -Interactive

# 启用安全默认值
Set-AzureSecurityDefaults -Enable

# 配置条件访问
Set-AzureConditionalAccess -BlockLegacyAuth -RequireMFA

# 审计特权用户
Set-AzurePrivilegedUsers -AuditOnly
```

### Intune 集成（v2 中的新功能）
```powershell
# 连接到 Intune
Connect-IntuneGhost -Interactive

# 通过 Intune 策略部署
Set-IntuneGhost -Settings @{
    RDP = $true
    SMBv1 = $true
    USBStorage = $true
    Macros = $true
}
```

## ⚠️ 重要考虑

### 测试要求
- **实验室环境**：先在隔离环境中测试所有设置
- **分阶段部署**：逐步部署以识别问题
- **回滚计划**：确保需要时可以撤销更改
- **文档记录**：记录哪些设置适用于您的环境

### 潜在影响
- **用户生产力**：某些设置可能影响日常工作流程
- **传统应用程序**：较旧的系统可能需要特定协议
- **远程访问**：考虑对合法远程管理的影响
- **业务流程**：验证设置不会破坏关键功能

### 安全限制
- **纵深防御**：Ghost 是安全的一层，不是完整解决方案
- **持续管理**：安全需要持续监控和更新
- **用户培训**：技术控制必须与安全意识相结合
- **威胁演变**：新攻击方法可能绕过当前保护

## 🎯 攻击场景示例

虽然 Ghost 针对常见攻击向量，但具体预防取决于适当的实施和测试：

### WannaCry 风格攻击
- **缓解**：`Set-Ghost -SMBv1` 禁用易受攻击的协议
- **考虑**：确保没有传统系统需要 SMBv1

### 基于 RDP 的勒索软件
- **缓解**：`Set-Ghost -RDP` 阻止远程桌面访问
- **考虑**：可能需要替代远程访问方法

### 基于文档的恶意软件
- **缓解**：`Set-Ghost -Macros` 禁用宏执行
- **考虑**：可能影响合法启用宏的文档

### USB 传递的威胁
- **缓解**：`Set-Ghost -USBStorage -AutoRun` 限制 USB 功能
- **考虑**：可能影响合法 USB 设备使用

## 🏢 企业功能

### 组策略支持
```powershell
# 通过组策略注册表应用设置
Set-Ghost -SMBv1 -RDP -AutoRun -GroupPolicy

# GP 刷新后设置在域范围内应用
gpupdate /force
```

### Microsoft Intune 集成
```powershell
# 为 Ghost 设置创建 Intune 策略
Set-IntuneGhost -Settings $GhostSettings -Interactive

# 策略自动部署到受管设备
```

### 合规性报告
```powershell
# 生成安全评估报告
Get-Ghost | Export-Csv -Path "SecurityAudit-$(Get-Date -Format 'yyyy-MM-dd').csv"

# Azure 安全态势报告
Get-AzureGhost | Out-File "AzureSecurityReport.txt"
```

## 📚 最佳实践

### 部署前
1. **记录当前状态**：在更改前运行 `Get-Ghost`
2. **彻底测试**：在非生产环境中验证
3. **计划回滚**：了解如何撤销每个设置
4. **利益相关者审查**：确保业务部门批准更改

### 部署期间
1. **分阶段方法**：首先部署到试点组
2. **监控影响**：关注用户投诉或系统问题
3. **记录问题**：记录任何问题以供将来参考
4. **沟通变更**：向用户通报安全改进

### 部署后
1. **定期评估**：定期运行 `Get-Ghost` 以验证设置
2. **更新文档**：保持安全配置为最新
3. **审查有效性**：监控安全事件
4. **持续改进**：根据威胁态势调整设置

## 🔧 故障排除

### 常见问题
- **权限错误**：确保 PowerShell 会话已提升
- **服务依赖**：某些服务可能有依赖关系
- **应用程序兼容性**：与业务应用程序一起测试
- **网络连接**：验证远程访问仍然工作

### 恢复选项
```powershell
# 需要时重新启用特定服务
Set-RDP -Enable
Set-SMBv1 -Enable
Set-AutoRun -Enable
Set-Macros -Enable
```

## 👨‍💻 关于作者

**Jim Tyler** - PowerShell Microsoft MVP
- **YouTube**：[@PowerShellEngineer](https://youtube.com/@PowerShellEngineer)（10,000+ 订阅者）
- **通讯**：[PowerShell.News](https://powershell.news) - 每周安全情报
- **作者**："PowerShell for Systems Engineers"
- **经验**：数十年 PowerShell 自动化和 Windows 安全经验

## 📄 许可证和免责声明

### MIT 许可证
Ghost 在 MIT 许可证下提供，可免费使用、修改和分发。

### 安全免责声明
- **无保证**：Ghost 按"原样"提供，不提供任何形式的保证
- **需要测试**：始终先在非生产环境中测试
- **专业指导**：为生产部署咨询安全专家
- **操作影响**：作者不对任何操作中断负责
- **综合安全**：Ghost 是完整安全策略的一个组成部分

### 支持
- **GitHub 问题**：[报告错误或请求功能](https://github.com/jimrtyler/Ghost/issues)
- **文档**：使用 `Get-Help <function> -Full` 获取详细帮助
- **社区**：PowerShell 和安全社区论坛

---

**🔐 使用 Ghost 加强您的安全态势 - 但始终先测试。**

```powershell
# 从评估开始，而不是假设
Get-Ghost
```

**⭐ 如果 Ghost 帮助改善您的安全态势，请为此存储库加星！**