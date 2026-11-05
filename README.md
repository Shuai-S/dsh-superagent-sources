# dsh-superagent-sources

统一源插件：一次安装挂载全部 dsh-superagent Source（dingtalk / mail / qq / wechat / wecom）。
本包是**纯清单聚合包**——零代码、零构建：五个 Source 以 npm 依赖拉取，各入口直接加载
各源包自己的模块，kind 字符串、配置 schema 与独立安装完全一致。

## 安装

```sh
# 统一安装（全部源）
dsh plugin --profile superagent add dsh-superagent-sources

# 指定安装（只要一个源：装它自己的包，依赖按需）
dsh plugin --profile superagent add dsh-superagent-mail

# 安装后重启引擎生效（或在 Superagent 控制台执行重启）
```

## 与独立安装并存（自动互斥让位）

SourceKind 注册是引擎级全局单例（同 kind 二次注册会使后注册的条目启动失败）。本包每个
入口自带动态 disabled 表达式：**检测到同 kind 的独立包已安装在 profile 上下文中时自动
让位，独立包被卸载后自动恢复**。因此：

- 独立包与统一包可以随意安装、随意卸载，无需手工互斥或改配置；
- 同 kind 的实例配置按 kind 存于引擎 settings，提供方切换后自动拾回，零重配；
- 已验证引擎版本：dsh 0.2.1-rc.4。若未来引擎不再求值该表达式，入口退化为常开，
  最坏情况回到"重复条目启动失败被容错"的行为，不会更差。

## 单源启停

统一安装后不想启用某个源，在 profile 的 cordis.patch.yml 追加一条 override
（用户层覆盖优先于本包的表达式）：

```yaml
- id: dsh-superagent-sources-qq
  disabled: true
```

## 版本策略

dependencies 按源包的 npm 版本区间声明（当前 ^0.1.0）。各源仓独立发版，本包发版时同步
收窄区间；本包不携带任何源代码，因此不存在代码漂移，只有区间约束。
