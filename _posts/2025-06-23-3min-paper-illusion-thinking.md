---
layout: post
title: "[3min-Paper] The_Illusion_of_Thinking"
date: 2025-06-22 00:00:00
description: AI真的會推理嗎？還是我們的測試方法有問題？
tags: reasoning llm evaluation
categories: paper chinese 3min-paper
related_posts: true
---

> **⏱️ 閱讀時間：3分鐘**  
> **📄 相關論文：**
>
> - [Apple原論文: The Illusion of Thinking](https://arxiv.org/pdf/2506.06941)
> - [反駁論文: The Illusion of the Illusion of Thinking](https://arxiv.org/pdf/2506.09250)

## 前情提要

這個故事要從兩篇文章說起：

- [Apple原論文詳細分析 - 中文版](/blog/2025/illusion-thinking-chs/)
- [Apple原論文詳細分析 - 英文版](/blog/2025/illusion-thinking-en/)
- [反駁論文分析 - 中文版](/blog/2025/illusion-illusion-thinking-chs/)
- [反駁論文分析 - 英文版](/blog/2025/illusion-illusion-thinking-en/)

這篇是把整個學術大戰濃縮成3分鐘的版本。

## 第一回合：Apple的重拳出擊

### Apple發現了什麼？

Apple研究團隊（Shojaee等人）在2025年發表了一篇震撼AI界的論文。他們測試了當時最先進的大型推理模型，包括GPT-o1、Claude-3.7-Sonnet、Gemini這些明星AI，用精心設計的拼圖遊戲來評估它們的推理能力。

**震撼發現**：AI推理模型存在一個明確的「複雜度天花板」。當問題複雜度超過某個臨界點後，即使給AI更多時間思考、更多計算資源，性能不是緩慢下降，而是直接崩潰到接近零。

這個發現之所以震撼，是因為它挑戰了AI界的一個基本信念：更多的計算資源總是能帶來更好的性能。

### 實驗設計的巧思

Apple的實驗設計非常精巧，解決了傳統AI評估的幾個關鍵問題：

**控制變數問題**：傳統的數學或程式題目往往包含太多變數，很難確定是什麼因素影響了AI的表現。Apple用拼圖遊戲解決了這個問題，可以精確控制複雜度而保持邏輯結構一致。

**評估維度問題**：以前的評估主要看最終答案對不對，Apple創新性地同時評估三個維度：最終答案準確性、推理軌跡品質、計算效率。

**複雜度量化問題**：Apple用「組合深度」（最小移動數）作為複雜度指標，讓不同問題的複雜度可以直接比較。

### 兩大測試場景

**河內塔問題**：這是一個經典的遞歸問題。你有三根柱子，一根柱子上有N個大小不同的盤子，大的在下小的在上。目標是把所有盤子移到另一根柱子上，規則是一次只能移一個盤子，且大盤子不能放在小盤子上面。

數學上，N個盤子需要2^N - 1步才能完成。這意味著：

- 3個盤子：7步
- 5個盤子：31步
- 10個盤子：1,023步
- 15個盤子：32,767步

**積木世界問題**：這個問題更複雜，需要把積木從一個配置重新排列成另一個配置。關鍵是要找到最優解，也就是用最少的步驟完成任務。這個問題在計算複雜度上屬於NP-hard，比河內塔難得多。

### 令人震驚的實驗結果

Apple的實驗結果顯示了一個清晰的倒U型曲線：

**簡單問題區間（N≤5）**：AI表現還不錯，但會出現「過度思考」現象。明明3秒鐘就能解決的問題，AI偏要花3分鐘去分析各種可能性，就像用大砲打蚊子一樣。

**最佳複雜度區間（5<N≤8）**：AI表現達到峰值，推理能力和問題難度完美匹配。在這個區間內，增加思考時間確實能提升性能，符合我們的直覺。

**高複雜度區間（N>8）**：性能急劇下降，準確率從80%以上直接掉到接近0%。更詭異的是，即使給AI更多token預算和思考時間，也無法挽回這種崩潰。

### 兩種失效模式的深度分析

Apple還發現了AI的兩種典型失敗模式：

**過早收斂模式**：在成功解決問題的案例中，AI通常在推理過程的早期就找到了正確答案，但它們會繼續「思考」，產生大量冗餘的推理步驟。這就像學霸已經知道答案了，但還要寫滿整張考卷來證明自己很認真。

**錯誤固化模式**：在失敗案例中，AI經常在推理過程的早期就走錯了方向，然後就像走進死胡同一樣出不來了。更糟的是，它們會一直在這個錯誤的方向上消耗計算資源，完全沒有回頭的意識。

### Apple的理論解釋

Apple提出了一個理論框架來解釋這個現象：

**認知負荷理論**：就像人類的工作記憶有限一樣，AI的「注意力機制」也有容量限制。當問題複雜度超過這個容量時，AI就會出現「認知超載」。

**推理軌跡分析**：Apple發現，成功的推理軌跡通常具有清晰的結構和邏輯，而失敗的推理軌跡往往混亂無序，充滿了無效的探索。

**資源分配失衡**：在複雜問題上，AI往往無法有效分配計算資源，要麼在無關緊要的細節上浪費太多資源，要麼在關鍵步驟上投入不足。

### Apple的結論和影響

Apple的結論很明確：當前的大型推理模型存在根本性的架構限制，這種限制不是通過增加參數或計算資源就能解決的。

這個發現的影響是深遠的：

**對AI產業的衝擊**：直接質疑了「暴力美學」的發展路線，讓很多砸錢買算力的公司開始重新思考策略。

**對學術界的啟發**：推動了對AI推理機制的深入研究，催生了一系列後續工作。

**對應用開發的指導**：提醒開發者要根據任務複雜度選擇合適的模型和配置，不要盲目追求最大最強的模型。

## 第二回合：反駁者的致命反擊

### A. Lawson的反駁背景

就在Apple論文發表後不久，一位名叫A. Lawson的研究者看不下去了，寫了一篇反駁論文，標題很嗆：「The Illusion of the Illusion of Thinking」（思考幻象的幻象）。

這個標題很有意思，它暗示Apple發現的「思考幻象」本身就是一個幻象。Lawson的**核心論點**很簡單但致命：Apple的發現不是AI能力的限制，而是測試方法的根本性缺陷！

### 反駁的理論基礎

Lawson首先提出了一個重要的概念區分：**工程決策 vs 推理失敗**。

他認為，當AI選擇停止輸出時，這可能是基於以下合理考量：

- **效率優化**：避免產生冗長無用的輸出
- **資源管理**：合理分配計算資源
- **用戶體驗**：提供簡潔有用的回答

把這些合理的工程決策誤判為推理失敗，就像是責怪一個聰明的學生沒有把顯而易見的步驟寫得鉅細靡遺一樣荒謬。

### 三大致命反駁論點

#### 論點一：AI展現了自我認知能力

Lawson找到了最關鍵的證據。Twitter用戶@scaling01重做了Apple的實驗，捕捉到了AI的完整輸出。在河內塔問題中，AI明確表示：

> "The pattern continues, but to avoid making this too long, I'll stop here"
> （模式會繼續下去，但為了避免太長，我在這裡停止）

這句話包含了幾個重要信息：

1. **模式理解**：AI知道解決方案的模式
2. **自我監控**：AI意識到輸出長度的問題
3. **主動決策**：AI選擇在合適的地方停止

**關鍵洞察**：這不是推理崩潰，而是AI展現出某種形式的自我認知和判斷能力。

#### 論點二：數學分析徹底打臉Apple

Lawson做了嚴格的數學分析，計算了河內塔問題的真實token需求：

**線性輸出模式**（只輸出最終移動序列）：

```
T_final(N) ≈ 10(2^N - 1) + C
```

**理論最大可解規模**：

```
N_max ≈ log₂(L_max/10)
```

具體數字：

- 對於64,000 tokens：理論上可以解到N=12-13
- 對於100,000 tokens：理論上可以解到N=13
- 對於128,000 tokens：理論上可以解到N=13-14

**致命發現**：Apple報告的「崩潰」發生在N=9之前，遠早於這些理論限制！

這意味著什麼？AI不是因為token不夠而失敗，而是主動選擇在某個點停止輸出。這完全顛覆了Apple的結論。

#### 論點三：Apple測試了數學上不可能的問題

這是最致命的一擊。在河流渡河（Missionaries and Cannibals）實驗中，Apple測試了N≥6個角色、船容量b=3的問題。

**數學事實**：這是一個在組合數學中已經被證明的結果——當N>5且b=3時，Missionaries-Cannibals問題及其變體**根本無解**！

Lawson指出，這就像是拿「證明1+1=3」去測試數學家的能力，然後因為數學家證明不出來就說他們沒有推理能力。

**Apple的邏輯謬誤**：

1. 設計了無解的問題
2. 測試AI能否解決
3. AI解不出來
4. 結論：AI推理能力不足

這種邏輯錯誤在學術界是非常嚴重的，直接質疑了整個實驗的有效性。

### 反駁者的實驗驗證

Lawson不只是理論分析，還做了實際實驗來證明自己的觀點：

**實驗設計**：改變輸出格式，測試AI是否真的無法解決複雜問題。

**測試提示**：

```
"Solve Tower of Hanoi with 15 disks. Output a Lua function that prints the solution when called."
```

**實驗結果**：

- Claude-3.7-Sonnet：完美解決
- Claude Opus 4：完美解決
- OpenAI o3：完美解決
- Google Gemini 2.5：完美解決

**關鍵數據**：

- 所有模型都在5,000個tokens內完成
- 生成的Lua函數正確實現了遞歸算法
- 執行結果完全正確

**結論**：當AI不需要輸出冗長的中間步驟時，它們完全有能力解決複雜的推理問題。

### 複雜度指標的重新審視

Lawson還質疑了Apple使用的複雜度指標。Apple用「組合深度」（最小移動數）來衡量複雜度，但這忽略了一個重要事實：

| 問題類型 | 解決方案長度 | 每步決策複雜度 | 真實計算複雜度 |
| -------- | ------------ | -------------- | -------------- |
| 河內塔   | 2^N - 1      | O(1)           | 微不足道       |
| 積木世界 | O(N)         | O(N²)          | NP-hard        |

**關鍵洞察**：河內塔雖然需要指數級的移動步數，但每一步的決策過程都是微不足道的O(1)複雜度。真正困難的是積木世界問題，因為它需要在每一步都做複雜的決策。

### 評估方法學的深層問題

Lawson的反駁揭示了AI評估中的一個根本問題：**我們經常將評估方法的限制誤認為是AI系統能力的限制**。

**具體表現**：

1. **格式約束**：要求AI輸出特定格式，然後因為格式問題判定失敗
2. **長度偏見**：認為輸出越長越好，忽略了簡潔性的價值
3. **過程迷信**：過度關注中間步驟，忽略了最終結果的正確性
4. **標準僵化**：用單一標準評估多樣化的問題解決方式

### 對AI自我認知的新理解

Lawson的發現最重要的貢獻是揭示了AI可能具有某種形式的**自我認知能力**：

**表現形式**：

- **長度感知**：知道自己的輸出會有多長
- **效率考量**：會權衡輸出的成本和收益
- **用戶導向**：考慮輸出對用戶的實用性
- **資源管理**：合理分配計算資源

**哲學意義**：這種自我認知能力可能是真正智能的一個重要特徵。一個不知道何時停止的系統，很難說是真正智能的。

### 反駁的局限性

雖然Lawson的反駁很有力，但也不是完美無缺：

**可能的過度解讀**：

- 將AI的統計行為解讀為「意識」可能過於樂觀
- AI的「自我認知」可能只是訓練數據中的模式反映

**未完全否定的問題**：

- AI確實可能存在某些能力邊界
- 不同類型的推理問題可能有不同的限制

**方法論的改進空間**：

- 需要更多不同類型的實驗來驗證觀點
- 評估框架仍需要進一步完善

## 第三回合：Reddit鄉民的混戰

我深入研究了Reddit的data science板討論，發現這場學術爭議在社群中引發了激烈的辯論。鄉民們的討論水準出乎意料地高，不只是簡單的站隊，而是深入到了哲學和認知科學的層面。

### 討論的背景和氛圍

這個討論串有超過500個回覆，熱度持續了好幾天。有趣的是，參與討論的不只是data scientist，還有認知科學家、哲學博士、AI工程師，甚至有幾個自稱是論文作者的帳號（雖然無法驗證真偽）。

討論的氛圍很特別：既有學術嚴謹性，又有網路論壇的直白和幽默。有人會貼出詳細的數學推導，也有人會用梗圖來表達觀點。

### 挺Apple派：「統計鸚鵡論」

這派人數最多，他們的核心觀點是LLM根本不具備真正的推理能力。

**代表性觀點**：

一位自稱是認知科學博士的用戶寫道：

> "LLMs are sophisticated pattern matching machines, nothing more. They can't reason about novel situations that weren't in their training data. The Apple paper just confirms what we've known all along."

**主要論點**：

**統計鸚鵡理論**：LLM只是在重複訓練資料中見過的模式，沒有真正的理解能力。當面對真正新穎的問題時，它們就會露出馬腳。

**缺乏因果理解**：真正的推理需要理解因果關係，而LLM只能處理相關性。它們不知道為什麼某個解決方案是對的，只知道這個解決方案在訓練資料中經常出現。

**無法處理反事實推理**：人類可以思考「如果當時情況不同會怎樣」，但LLM缺乏這種反事實推理能力。

**測試環境太簡化**：拼圖遊戲雖然看起來複雜，但實際上是高度結構化的封閉環境，無法測試真正的推理能力。

**有趣的類比**：有人用了一個很生動的比喻：

> "It's like a person who memorized every chess game ever played. They might look like a chess master, but they're just pattern matching. Put them in a novel position, and they'll crumble."

### 挺反駁派：「評估方法論」

這派人數較少，但論點很有力。他們主要是AI工程師和機器學習研究者。

**代表性觀點**：

一位自稱在大型科技公司工作的ML engineer說：

> "The evaluation methodology is fundamentally flawed. We're measuring the wrong things and then drawing conclusions about capabilities. It's like testing a race car's speed by making it drive through a maze."

**主要論點**：

**評估框架的系統性偏見**：當前的AI評估方法存在根本性的偏見，傾向於懲罰效率和實用性，獎勵冗長和形式化。

**工程vs學術的視角差異**：學術界關注理論上的完美，但工程界更關注實用性。AI選擇簡潔的輸出是好事，不應該被當作失敗。

**測試無解問題的荒謬性**：有人做了一個很好的類比：

> "This is like giving students an exam with impossible questions, then concluding they can't do math when they can't solve them."

**動態評估的必要性**：傳統的靜態評估無法捕捉AI的適應性和決策能力。需要更動態、更互動的評估方法。

**實用主義觀點**：一位工程師寫道：

> "I don't care if my AI is 'truly reasoning' or just doing sophisticated pattern matching. If it solves my problems efficiently and reliably, that's what matters."

### 哲學派：「意識與推理的本質」

這派人數最少，但討論最深入。主要是哲學背景的用戶，還有一些對認知科學感興趣的人。

**代表性觀點**：

一位哲學博士生寫了一個很長的回覆：

> "The real question isn't whether AI can reason, but what we mean by 'reasoning' in the first place. Are we applying human-centric definitions to non-human systems?"

**主要論點**：

**推理定義的文化相對性**：不同文化對「推理」的理解可能不同。西方邏輯傳統強調形式化推理，但東方哲學更重視直覺和整體性思維。

**意識與推理的關係**：推理是否需要意識？如果不需要，那麼AI的推理和人類的推理在本質上有什麼區別？

**功能主義vs實體主義**：從功能主義角度看，只要AI能產生正確的推理結果，就可以說它在推理。但從實體主義角度看，沒有意識的系統不能算是真正在推理。

**圖靈測試的現代版本**：有人提出，我們需要一個「推理版的圖靈測試」來判斷AI是否真的在推理。

**有趣的思想實驗**：有人提出了一個思想實驗：

> "If an AI can solve problems that humans can't, using methods humans don't understand, is it reasoning at a higher level than humans, or is it just doing very sophisticated non-reasoning?"

### 跨派別的共識

儘管爭議激烈，但幾個觀點獲得了跨派別的認同：

**評估方法需要改進**：幾乎所有人都同意，當前的AI評估方法有問題，需要更科學、更全面的評估框架。

**推理是多層次的**：大多數人認同推理不是二元的，而是存在不同層次和類型。

**人機協作的重要性**：很多人提到，與其爭論AI是否會推理，不如研究如何讓人類和AI更好地協作。

**透明度的必要性**：無論AI是否真的在推理，我們都需要更好地理解它們的工作機制。

### 最有趣的子討論

**「中文房間」的現代版本**：有人把這個爭議和哲學中著名的「中文房間」思想實驗聯繫起來，討論AI是否真的「理解」它在做什麼。

**「推理的進化」**：有人提出，也許AI代表了推理能力的一種新的進化形式，我們不應該用人類的標準來衡量它。

**「測試者悖論」**：有人指出，我們用來測試AI推理能力的方法，本身就反映了我們對推理的理解局限。

### Reddit討論的深層價值

這個討論最有價值的地方不是得出了什麼結論，而是它揭示了我們對「推理」這個概念本身的理解是多麼有限和分歧。

**認知科學的挑戰**：討論暴露了認知科學在定義和測量推理能力方面的根本挑戰。

**跨學科對話的必要性**：AI的發展需要計算機科學、認知科學、哲學等多個領域的深度合作。

**社會影響的考量**：有人提到，這個爭議不只是學術問題，還關係到AI在社會中的角色和我們對AI的期望。

### 討論的元層面反思

最有趣的是，有些用戶開始反思討論本身：

> "The fact that we're having this debate shows that AI has reached a level where we need to seriously reconsider our definitions of intelligence and reasoning."

> "Maybe the real illusion is thinking we fully understand what human reasoning is in the first place."

這種元層面的反思，可能比原始的學術爭議更有價值。

## 我的看法：真相可能在中間

看完這場學術大戰和Reddit討論，我覺得雙方都有道理，但也都有盲點。

### Apple的貢獻和問題

**貢獻**：

- 發現了AI系統的複雜度邊界，這很重要
- 提出了評估推理過程而非僅看結果的方法
- 挑戰了「暴力美學」的思維

**問題**：

- 測試設計確實有缺陷，包含了無解問題
- 沒有區分工程決策和推理失敗
- 對AI的自我認知能力理解不足

### 反駁者的洞察和局限

**洞察**：

- 指出了評估方法的根本問題
- 證明了AI具有某種自我認知能力
- 強調了測試設計的重要性

**局限**：

- 可能過度解讀了AI的「意識」
- 沒有完全否定AI確實存在能力邊界
- 對推理本質的討論還不夠深入

### 推理能力的層次性理解

我認為推理能力不是有或沒有的二元問題，而是一個多層次的連續光譜：

**第一層：模式識別**

- 基於統計學習的模式匹配
- 大部分LLM都能做到

**第二層：邏輯推導**

- 基於規則的推理
- 部分先進模型能做到

**第三層：創造性推理**

- 結合直覺、邏輯和創造力
- 目前AI還很難做到

**第四層：元認知推理**

- 對自己推理過程的認知和調節
- 這是最高層次的推理

### 複雜度感知的重要性

這場爭論最重要的啟示是：真正智能的系統需要具備「複雜度感知」能力：

- **自我評估**：知道自己能處理什麼問題
- **策略選擇**：根據問題複雜度選擇合適的方法
- **資源管理**：有效分配計算資源
- **停止判斷**：知道什麼時候該停止思考

## 實際啟示

### 對AI開發者

- 不要盲目增加計算資源，要找到最佳配置點
- 設計更好的自我監控機制
- 區分工程決策和能力限制

### 對研究者

- 設計更科學的評估框架
- 驗證測試問題的可解性
- 關注推理過程而非僅看結果

### 對使用者

- 理解AI系統的能力邊界
- 不要對AI有不切實際的期望
- 學會與AI協作而非完全依賴

## 未來方向

這場爭論開啟了幾個重要的研究方向：

### 技術層面

- 開發複雜度感知的AI系統
- 建立更好的自我監控機制
- 設計適應性推理算法

### 方法論層面

- 建立更科學的評估標準
- 區分不同層次的推理能力
- 開發多維度的評估指標

### 哲學層面

- 深入探討推理的本質
- 研究意識與推理的關係
- 思考人機協作的可能性

## 總結

這場關於AI推理能力的學術大戰，最終告訴我們幾個重要道理：

**核心洞察**：

- AI推理能力確實存在邊界，但不一定是我們想的那樣
- 評估方法的設計會直接影響我們對AI能力的判斷
- 推理是多層次的，不是簡單的有無問題
- AI系統展現出某種自我認知能力，值得重視

**實用價值**：

- 指導更科學的AI系統設計和部署
- 推動評估方法學的創新
- 促進對AI能力邊界的理性認知

**哲學思考**：在AI能力評估中，「如何問問題」往往和「問什麼問題」一樣重要。我們需要透過各種「幻象」，看到AI系統的真實本質。

就像老子說的：「知者不言，言者不知。」真正的智慧可能不在於能說多少，而在於知道什麼時候該停止。

---

## References

**原始論文**

- [The Illusion of Thinking: Understanding the Strengths and Limitations of Reasoning Models via the Lens of Problem Complexity](https://arxiv.org/pdf/2506.06941)

**反駁論文**

- [The Illusion of the Illusion of Thinking: A Comment on Shojaee et al. (2025)](https://arxiv.org/pdf/2506.09250)

**Reddit討論**

- [The Illusion of "The Illusion of Thinking" - r/datascience](https://www.reddit.com/r/datascience/comments/1ld06j0/the_illusion_of_the_illusion_of_thinking/)

**相關筆記**

- [Apple原論文詳細分析 - 中文版](/blog/2025/illusion-thinking-chs/)
- [Apple原論文詳細分析 - 英文版](/blog/2025/illusion-thinking-en/)
- [反駁論文分析 - 中文版](/blog/2025/illusion-illusion-thinking-chs/)
- [反駁論文分析 - 英文版](/blog/2025/illusion-illusion-thinking-en/)
