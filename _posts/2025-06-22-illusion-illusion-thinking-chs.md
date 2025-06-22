---
layout: post
title: "[中文版] The Illusion of the Illusion of Thinking: 當AI評估方法成為能力判斷的陷阱"
date: 2025-06-22 01:00:00
description: 這篇評論文章揭露了一個驚人真相：我們經常將AI評估方法的限制誤認為是AI系統能力的限制！研究發現，許多被認為是AI推理失敗的案例，實際上是評估框架設計不當造成的誤判。
tags: ai-evaluation reasoning model-limitations methodology
categories: paper chinese
thumbnail: assets/img/paper/The_Illusion_of_Thinking/cover.png
related_posts: true
lang: zh
lang_ref: illusion-illusion-thinking-tutorial
---

<div class="language-switcher" style="text-align: right; margin-bottom: 20px; padding: 8px 0; border-bottom: 1px solid #333;">
  <span style="font-size: 14px; color: #888; margin-right: 8px;">🌐 Language:</span>
  <strong style="color: #007bff; font-weight: 600;">中文</strong>
  <span style="color: #666; margin: 0 6px;">|</span>
  <a href="/blog/2025/illusion-illusion-thinking-en/" style="color: #007bff; text-decoration: none; font-weight: 500; transition: opacity 0.2s;" onmouseover="this.style.opacity='0.7'" onmouseout="this.style.opacity='1'">English</a>
</div>

> **來源：** [Paper](https://arxiv.org/pdf/2506.09250)

這是一篇由 A. Lawson 撰寫的重要評論文章，全名為 **[The Illusion of the Illusion of Thinking: A Comment on Shojaee et al. (2025)](https://arxiv.org/pdf/2506.09250)**。這篇論文是針對 Shojaee et al. (2025) 關於大型推理模型限制研究的回應，而 Shojaee et al. 的研究正是我們之前分析過的 Apple 論文的後續討論。如果對這個背景感興趣，可以參考之前的筆記：[The Illusion of Thinking: Apple論文解析](/blog/2025/illusion-thinking-chs/)。

本評論文章提出了一個深刻的觀點：**我們經常將評估方法的限制誤認為是AI系統能力的限制**。

## 核心問題：評估方法的陷阱

Shojaee et al. (2025) 聲稱發現了大型推理模型的基本限制，通過系統性評估規劃謎題，發現模型準確度在超過某些複雜度閾值後會"崩潰"至零。

> _"Shojaee et al. (2025) claim to have identified fundamental limitations in Large Reasoning Models through systematic evaluation on planning puzzles. Their central finding—that model accuracy 'collapses' to zero beyond certain complexity thresholds—has significant implications for AI reasoning research."_

然而，本評論文章指出這些表面上的失敗實際上源於實驗設計的限制，而非模型本身的推理能力缺陷。

> _"However, our analysis reveals that these apparent failures stem from experimental design limitations rather than fundamental reasoning failures."_

## 三大關鍵問題

### 問題一：模型主動識別輸出限制

**最重要的發現：** 模型實際上能夠主動識別何時接近輸出限制，並做出合理的停止決策。

論文引用了 @scaling01 在 Twitter 上的複製實驗，捕捉到模型輸出明確表示：

> _"The pattern continues, but to avoid making this too long, I'll stop here"_

這證明模型理解解決方案的模式，但選擇因實際約束而截斷輸出。

> _"This demonstrates that models understand the solution pattern but choose to truncate output due to practical constraints."_

**關鍵洞察：** 將模型的這種行為誤判為"推理崩潰"反映了自動評估系統的根本問題，無法區分模型的意識和決策能力。

### 問題二：Token限制的數學分析

論文提供了詳細的數學分析，說明為什麼河內塔問題會遇到token限制：

**線性增長模式（僅輸出最終序列）：**

```
T_final(N) ≈ 10(2^N - 1) + C
```

**最大可解決規模：**

```
N_max ≈ log₂(L_max/10)
```

對於不同的token限制：

- L_max = 64,000：N_max ≈ 12-13
- L_max = 100,000：N_max ≈ 13

**關鍵發現：** 報告的"崩潰"發生在 N = 9 之前，遠早於這些理論限制。

> _"Interestingly, the reported 'collapse' before N = 9 for most models occurs well before these theoretical limits. This suggests that models are making a decision to terminate output before actually reaching their context window limits."_

### 問題三：不可能謎題的陷阱

在河流渡河實驗中，評估問題被戲劇性地放大：

> _"The evaluation issues are compounded dramatically in the River Crossing experiments. Shojaee et al. test instances with N ≥ 6 actors/agents using boat capacity b = 3. However, it is a well-established result that the Missionaries-Cannibals puzzle (and its variants) has no solution for N > 5 when b = 3."_

**問題本質：** 評估框架包含了數學上不可能解決的問題，然後將模型無法解決這些問題歸咎於推理能力不足。

## 實驗驗證：替代表示法恢復性能

為了測試失敗是否反映推理限制或格式約束，作者進行了初步測試：

**測試提示：**

```
"Solve Tower of Hanoi with 15 disks. Output a Lua function that prints the solution when called."
```

**驚人結果：**

- 多個先進模型（Claude-3.7-Sonnet, Claude Opus 4, OpenAI o3, Google Gemini 2.5）獲得了非常高的準確率
- 在5,000個tokens內完成
- 生成的解決方案正確實現了遞歸算法

> _"The generated solutions correctly implement the recursive algorithm, demonstrating intact reasoning capabilities when freed from exhaustive enumeration requirements."_

## 複雜度聲稱的重新評估

論文指出原始研究使用"組合深度"（最小移動數）作為複雜度指標的問題：

| 謎題     | 解決方案長度 | 分支因子 | 計算複雜度                      |
| -------- | ------------ | -------- | ------------------------------- |
| 河內塔   | 2^N - 1      | 1        | O(1) 每步                       |
| 積木世界 | O(N)         | O(N²)    | 線性（近最優）/ NP-hard（最優） |

**關鍵洞察：** 河內塔儘管需要指數級的移動數，但每步的決策過程是微不足道的 O(1)。積木世界則困難得多。

### 最優化問題的區別

積木世界的提示明確要求最優化：

> _"Find the minimum sequence of moves to transform the initial state into the goal state."_

雖然解決方案檢查器只驗證正確性而非最優性，但模型會嘗試按照指示尋找最優解，這增加了計算負擔。

## 深層思考：評估設計的重要性

### 工程決策 vs 推理失敗

論文最創新的洞察是區分了"工程決策"和"推理失敗"：

> _"This distinction further emphasizes the importance of evaluation design. Scoring models as 'failures' for making reasonable engineering decisions about output length mischaracterizes their actual capabilities."_

**核心觀點：** 當模型選擇不輸出冗長的中間步驟時，這可能是合理的效率考量，而非能力缺陷。

### 模型的自我認知能力

研究發現模型具有某種自我校準能力：

> _"This behavior indicates that models may be poorly calibrated about their own context length capabilities, choosing to stop prematurely."_

**重要發現：** 模型展現出對自身能力的某種認知，會根據情況做出停止決策。

## 對AI評估的深遠影響

這項研究提醒我們需要重新思考AI評估的方法學：

**未來工作應該：**

1. **設計能區分推理能力和輸出約束的評估**
2. **在評估模型性能前驗證謎題的可解性**
3. **使用反映計算難度而非僅僅解決方案長度的複雜度指標**
4. **考慮多種解決方案表示法，以分離算法理解和執行**

### 測量問題的哲學

這讓我聯想到測量問題的哲學：我們的測量工具和方法本身會影響我們對被測量對象的理解。

**核心啟示：** 在AI評估中，我們需要更加謹慎地設計評估框架，避免將方法論的限制誤認為是系統能力的邊界。

## 與人類認知的類比

### 效率優先的智能行為

這項研究讓我想到人類在面對複雜問題時的認知模式：

- **實用主義** - 人類也會採用啟發式方法，而非暴力枚舉
- **認知負荷管理** - 人類會根據問題複雜度調整思考策略
- **自我監控** - 人類會意識到自己的認知限制並相應調整

**深層思考：** AI模型的這種行為是否反映了某種智能的特徵？

## 批判性思考

### 平衡的重要性

雖然這篇評論文章提出了有價值的觀點，但我們也需要注意：

**避免極端化：** 不要因為批評原始研究就完全忽視AI系統的真實限制

**保持客觀：** 需要建立更加平衡和細緻的評估框架，既能識別真正的能力邊界，又不會因為方法論問題而產生誤判

## 結論

這篇論文最終提醒我們一個重要道理：

> _"The question isn't whether LRMs can reason, but whether our evaluations can distinguish reasoning from typing."_

**核心啟示：**

- 評估方法的設計會直接影響我們對AI能力的判斷
- 需要區分真正的能力限制和評估框架的限制
- AI系統的"失敗"可能實際上是合理的工程決策
- 模型展現出的自我認知和適應能力值得重視

**在AI能力評估中，"如何問問題"往往和"問什麼問題"一樣重要。**

這項研究不僅為當前的AI評估提供了重要反思，也為未來構建更可靠的AI能力評估體系指明了方向。我們需要透過"評估的幻象"，看到AI系統的真實能力。

---

## 參考文獻

- [The Illusion of the Illusion of Thinking: A Comment on Shojaee et al. (2025)](https://arxiv.org/pdf/2506.09250)
- [The Illusion of Thinking: Understanding the Strengths and Limitations of Reasoning Models via the Lens of Problem Complexity](https://arxiv.org/pdf/2506.06941)
