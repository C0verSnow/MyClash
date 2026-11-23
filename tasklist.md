# 本文档为4象限的任务清单，全文书写大白话

## 问题1及解决模板：YAML 里混入 JS 语法与重复 exclude-filter 键

- [1795433762] 遇到的问题：原 `2.yaml` 的 `Bybit` 策略组里写了 `"include-all": true,` 和 JS 的 `[...].join("|");` 数组拼接，还在同一组末尾重复写了第二个 `exclude-filter: '\[Home\]'`，YAML 会把后者覆盖前者或直接解析报错，且 Go 正则默认区分大小写会导致 `us`、`hk` 漏掉大写节点名；通用解决步骤：去掉多余引号与逗号，把所有排除条件和 `\[Home\]` 合并成一条带 `(?i)` 前缀的 YAML 单引号正则字符串，再用 YAML 解析器静态校验。

## 问题2及解决模板：url-test 改成 fallback 后残留专属字段

- [1795433762] 遇到的问题：把 `Bybit` 策略组从 `url-test` 改成 `fallback` 时，原来的 `tolerance: 50` 是 `url-test` 才用的测速容差字段，留在 `fallback` 里不符合 Mihomo 规范；通用解决步骤：核对 `MetaCubeX/mihomo` 的 `FallbackOption` 与 `Meta-Docs` 文档，把 `type` 改为 `fallback` 并删掉 `tolerance`。

## 问题3及解决模板：YAML 锚点定义在别名引用之后导致解析失败

- [1795433825] 遇到的问题：原 `2.yaml` 把 `proxy-providers` 里的 `<<: *p` 放在顶层 `p: &p` 之前，YAML 解析器报 `Unknown alias: p`；通用解决步骤：把 `p: &p` 模板锚点移到 `proxy-providers` 之前再做静态 YAML 解析校验。

## 想做：

- [1795433871] 完成最后一次文档更新提交（更新 `README.md`），然后合并 PR #2 并确认 Issue #1 自动关闭。
- [1795433871] 合并到 `main` 后，用户在 `config.yaml` 的 `proxy-providers` 填入订阅链接，即可直接在 Mihomo 客户端拉取 `https://raw.githubusercontent.com/C0verSnow/MyClash/main/rules/bybit.list`。

## 做完：

- [1795433762] 已克隆 `C0verSnow/MyClash` 到本地并切到 `feat/bybit-ruleset-and-config` 分支，读取 Issue #1 评论与附件 `3.yaml`、`2.yaml`。
- [1795433762] 已在 GitHub 检索开源同类项目：`MetaCubeX/mihomo`（36016 star，MIT）、`MetaCubeX/Meta-Docs`（202 star）和 `Aethersailor/Custom_OpenClash_Rules`（6882 star，CC-BY-SA-4.0），确认 `fallback` 组字段、`classical`/`text` 规则文件格式及 `rule-providers` 写法。
- [1795433825] 已整理生成 `rules/bybit.list`（25 条规则，清理末尾多余逗号与空格）与 `config.yaml`（修复锚点顺序、修复 `Bybit` 组语法并改为 `fallback`、新增 `bybit` 规则集与 `RULE-SET,bybit,Bybit`），并通过 Ruby `YAML.safe_load` 静态解析与正则校验。
- [1795433871] 已推送分支并创建 PR #2（`https://github.com/C0verSnow/MyClash/pull/2`）。

## 没做：

- [1795433871] 按仓库规范未执行本地编译、构建或运行 Mihomo 内核实测；仓库无既有 CI 工作流，未新增额外 CI 工具。

## 在做：

- [1795433871] 按四象限重新整理 `tasklist.md`（倒数第二次提交），随后更新 `README.md` 并合并 PR。
