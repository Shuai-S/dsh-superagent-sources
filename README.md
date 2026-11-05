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

## 互斥（重要）

SourceKind 注册是引擎级全局单例（重复注册抛错）。**同一个源不允许统一包与独立包并存**：
从独立安装迁移到统一安装，先 `remove` 独立包再 `add` 本包；引擎 settings 中保留的实例
配置会在重启后按 kind 自动拾回，无需重新配置。

## 单源启停

统一安装后不想启用某个源，在 profile 的 cordis.patch.yml 追加一条 override（注释保留）：

```yaml
- id: dsh-superagent-sources-qq
  disabled: true
```

## 版本策略

dependencies 按源包的 npm 版本区间声明（当前 ^0.1.0）。各源仓独立发版，本包发版时同步
收窄区间；本包不携带任何源代码，因此不存在代码漂移，只有区间约束。
