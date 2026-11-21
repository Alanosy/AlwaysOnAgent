---
id: ""            # 留空。Agent 领取时自动分配
title: "示例：修复登录接口缺少账号锁定机制"
status: pending   # 仅作展示，真实状态以所在文件夹为准
priority: P1
created: ""       # 留空。Agent 以入 pending/ 的日期自动填充
project: ""       # 留空，默认 /Users/alan/Documents/Code/projects/ddyy-data-platform
started_at: ""
finished_at: ""
blocked_reason: ""
depends_on: []
tags: [auth, security]
---

# 示例：修复登录接口缺少账号锁定机制

## 问题描述

当前登录接口在密码连续错误多次后没有任何限制，存在暴力破解风险。需要在登录流程中增加失败计数，连续 5 次密码错误后锁定账号 30 分钟。

## 关联文件

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

## 回执

- **完成摘要**：_（一句话说明做了什么）_
- **改动位置**：_（修改/新增的文件及各自要点）_
- **后续使用**：_（成果如何使用：新接口调用方式、新配置项、新命令等）_
- **注意事项**：_（遗留问题、风险、后续建议）_
