# MyClash

Mihomo (Clash.Meta) 个人配置与自定义分流规则集合仓库。

## 文件说明

- `config.yaml`：Mihomo 主配置模板，包含 DNS、Sniffer、TUN、策略组（`Select`、`Default`、`Bybit`）、`rule-providers` 与 `rules`。
- `rules/bybit.list`：Bybit 相关域名与 IP-CIDR 规则列表（`classical` / `text` 格式，共 25 条规则）。

## 使用方法

### 1. 直接订阅 Bybit 规则集

在 Mihomo 配置的 `rule-providers` 中引用 Raw 地址：

```yaml
rule-providers:
  bybit:
    type: http
    behavior: classical
    format: text
    url: https://raw.githubusercontent.com/C0verSnow/MyClash/main/rules/bybit.list
    path: ./ruleset/bybit.list
    interval: 86400
```

在 `rules` 中添加分流规则（位于 `cn` / `cn-ip` 与 `MATCH` 之前）：

```yaml
rules:
  - RULE-SET,bybit,Bybit
```

### 2. 使用完整配置 `config.yaml`

1. 在 `config.yaml` 的 `proxy-providers.provider1.url` 和 `provider2.url` 中填入实际订阅链接。
2. `Bybit` 策略组采用 `fallback` 模式，按节点顺序故障转移，并通过 `exclude-filter` 排除受限地区、直连与 `[Home]` 节点。

## 参考项目

- [MetaCubeX/mihomo](https://github.com/MetaCubeX/mihomo)
- [MetaCubeX/Meta-Docs](https://github.com/MetaCubeX/Meta-Docs)
- [Aethersailor/Custom_OpenClash_Rules](https://github.com/Aethersailor/Custom_OpenClash_Rules)
