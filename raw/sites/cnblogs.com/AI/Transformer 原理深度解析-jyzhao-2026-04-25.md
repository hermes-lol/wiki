---
title: 一文搞懂 LLM 的 Transformer！看完能和别人吹一年
source_url: https://www.cnblogs.com/jyzhao/p/19275222/yi-wen-gao-dong-llm-de-transformer-kan-wan-neng-he
author: jyzhao
date: 2026-04-25
category: AI
tags: [AI, 深度学习, Transformer, 自注意力]
---

# Transformer 架构全面解析：LLM 的基石

核心论文：*Attention is All You Need*

## Encoder

1. **Input Embedding**：词→向量
2. **Positional Encoding**：sin+cos 给位置贴标签
3. **Self-Attention**：Q/K/V 机制，每个词用 Q 去其他词的 K 处打分
4. **Multi-Head Attention**：8 个注意力头并行，不同头关注不同语义关系
5. **Add & Norm**：残差连接 + LayerNorm 稳定训练
6. **FFN**：Linear→ReLU→Linear 深度加工
7. 堆叠 N 层（论文 6 层）

## Decoder

1. Output Embedding
2. **Shifted Right**：右移一位，开头加 `<start>`
3. **Masked Multi-Head Attention**：遮住未来词，防止偷看答案
4. **Encoder-Decoder Attention**：连接输入输出
5. Linear + Softmax：概率输出，选最高概率词

## 总结

| 组件 | 功能 |
|------|------|
| Self-Attention | Q/K/V 语义关系捕捉 |
| Multi-Head | 多视角并行理解 |
| FFN | 非线性深度特征表达 |
| Masked Attention | 防止偷看未来词 |

> Transformer 已成为 ChatGPT、DeepSeek 等所有大语言模型的基础架构。
