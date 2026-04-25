---
title: 从Prompt工程到Skill工程：Agent Skills开放标准彻底改变了AI协作方式
source_url: https://www.cnblogs.com/zlt2000/p/19577443
author: zlt2000
date: 2026-04-25
category: AI
tags: [AI编程, Agent Skill, Claude Code, 技能工程]
---

# 从Prompt工程到Skill工程

## 为什么突然火了？

AI 专业能力无法沉淀。Anthropic 2025年10月推出 Agent Skill，被 OpenAI、Cursor、Trae 跟进。

## 技术本质

标准化文件夹：SKILL.md（必须）+ scripts/ + references/ + assets/

## 三层渐进式加载

| 层级 | 内容 | 时机 |
|------|------|------|
| L1 | 元数据 | Agent 启动时 |
| L2 | 说明文档 | 匹配需求时 |
| L3 | 脚本/模板 | 执行中按需 |

## 对比

| 维度 | Prompt | Skill |
|------|--------|-------|
| 性质 | 临时指令 | 标准化流程 |
| 加载 | 全量输入 | 按需渐进 |
| 稳定性 | 依赖模型记忆 | 固化检查点 |

> Prompt 是口头交代，Skills 是书面 SOP + 工具箱。

## Skill vs MCP

MCP 提供"工具"，Skill 提供"使用指南"——两者互补。
