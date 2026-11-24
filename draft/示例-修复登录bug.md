---
id: ""            # 留空。Agent 领取时自动分配
title: "示例：修复登录接口缺少账号锁定机制"
status: pending   # 仅作展示，真实状态以所在文件夹为准
priority: ""      # 留空。Agent 领取时根据正文「优先级」章节的勾选自动识别填写（此处勾选了 P1）
created: ""       # 留空。Agent 以入 pending/ 的日期自动填充
project: ""       # 填目标项目绝对路径；留空则用 config/配置.md 的 default_project
started_at: ""
finished_at: ""
blocked_reason: ""
depends_on: []
tags: [auth, security]
---

# 示例：修复登录接口缺少账号锁定机制

## 优先级

<!-- 勾选一项且只勾一项。Agent 领取任务时读取此处勾选，自动识别并填写上方 frontmatter 的 priority 字段。 -->

- [ ] P0 — 紧急，阻塞线上（最高）
- [x] P1 — 高，本迭代必须
- [ ] P2 — 中，有空就做
- [ ] P3 — 低，锦上添花（最低）

## 问题描述

当前登录接口在密码连续错误多次后没有任何限制，存在暴力破解风险。需要在登录流程中增加失败计数，连续 5 次密码错误后锁定账号 30 分钟。

## 关联文件

<!-- 以下仅是某个 Java 项目的路径示意。真实使用时按目标项目实际情况改写；相对路径相对 frontmatter.project 解析。 -->

- `backend/ddyy-auth/src/main/java/com/cetc29/rx/auth/service/SysLoginService.java`
- `backend/ddyy-auth/src/main/java/com/cetc29/rx/auth/controller/TokenController.java`
- `backend/ddyy-common/ddyy-common-redis/` （Redis 工具类，用于存失败计数）

## 验收标准

- [ ] 密码连续错误 5 次后，后续登录返回「账号已锁定，请 30 分钟后重试」
- [ ] 锁定状态持续 30 分钟，期间无论密码对错都拒绝登录
- [ ] 30 分钟后自动解锁，失败计数清零
- [ ] 正确密码登录成功时，失败计数清零
- [ ] 编译通过：`cd backend && mvn -B -DskipTests compile`
- [ ] 不影响已有正常登录流程（正确密码立即登录成功）

## 备注

- 用 Redis 存失败计数，key 格式：`login:fail:count:{username}`，TTL 30 分钟
- 锁定状态也用 Redis 存，key：`login:locked:{username}`，TTL 30 分钟
- 不要在数据库里加字段，避免表结构变更
- 参考 `ddyy-common-redis` 中的 `RedisUtils` 工具类

## 执行日志

- _（待 Agent 领取后自动填写）_

## 总结

- **做了什么**：_（一句话概述任务目标与最终做了什么）_
- **完成了什么**：_（逐条对照「验收标准」列出已达成的结果/产出）_
- **改动了什么**：_（修改/新增的文件及各自要点）_
- **后续使用**：_（成果如何使用：新接口调用方式、新配置项、新命令等；无则写「无」）_
- **注意事项**：_（遗留问题、风险、后续建议）_
