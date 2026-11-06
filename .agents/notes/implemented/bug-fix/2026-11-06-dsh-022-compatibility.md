# Agent Note: 放开 MCP Skill Manager 对 DSH 0.2.2-rc.1 的写入闸

Status: implemented — 兼容白名单与 peer 已包含 0.2.2-rc.1。

## Problem

Desktop 运行时升到 `0.2.2-rc.1` 后，`@sophiadata-ai/dsh-mcp-skill-manager` fiber 仍 active，但 Settings → MCP & Skills 写入被硬闸禁用：`testedRuntimeVersions` 只有 `0.2.0-rc.2` / `0.2.1-rc.2`，`versionStatus=unverified`。

## Decision

把 `0.2.2-rc.1` 加入精确回归白名单，并把三个 optional peer 扩成 `^0.2.0-rc.2 || ^0.2.1-rc.2 || ^0.2.2-rc.1`。发布 `0.10.1` 后重装，不走 profile 级版本豁免。

## Alternatives considered

- 仅 `set_version_exemption`：可临时开写，但白名单仍旧，且豁免作用于整个 profile、带崩溃/数据风险。
- 放宽为「凡 0.2.x 一律 tested」：违背「精确白名单证明行为」的既有兼容决策。
- 只升 peer 不改正名单：安装约束松了，设置页仍只读。

## Acceptance criteria

隔离回归覆盖 `0.2.2-rc.1` 基线；未知/未测版本仍只读。desktop 安装 `0.10.1` 并重启后，`compatibility.status` 报告 `tested` 且 `writes.mcp=true`。

## Consequences

白名单仍是精确匹配；未来 0.2.2 其它 rc/正式版需再扩名单并复验。真实桌面端到端写操作验收独立于隔离回归。
