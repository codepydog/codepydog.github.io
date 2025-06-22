---
layout: post
title: "[中文版] RAPTOR: Recursive Abstractive Processing for Tree-Organized Retrieval"
date: 2024-09-18 10:00:00
description: Stanford 提出的 RAPTOR 方法透過遞迴分群和總結建立階層式文本架構，有效解決傳統 RAG 系統在文本切塊時的語意資訊流失問題。該方法使用軟分群、UMAP 降維和貝葉斯資訊準則來優化檢索效果，在保留完整語意的同時提升檢索準確性。
tags: rag retrieval clustering
categories: paper chinese
thumbnail: assets/img/paper/RAPTOR_Recursive_Abstractive_Processing_for_Tree-Organized_Retrieval/raptor_figure1.png
related_posts: true
lang: zh
lang_ref: raptor-tutorial
---

<div class="language-switcher" style="text-align: right; margin-bottom: 20px; padding: 8px 0; border-bottom: 1px solid #333;">
  <span style="font-size: 14px; color: #888; margin-right: 8px;">🌐 Language:</span>
  <strong style="color: #007bff; font-weight: 600;">中文</strong>
  <span style="color: #666; margin: 0 6px;">|</span>
  <a href="/blog/2024/raptor-en/" style="color: #007bff; text-decoration: none; font-weight: 500; transition: opacity 0.2s;" onmouseover="this.style.opacity='0.7'" onmouseout="this.style.opacity='1'">English</a>
</div>

> **來源：** [Paper](https://arxiv.org/abs/2401.18059) / [Official GitHub](https://github.com/parthsarthi03/raptor)

這是一篇來自 Stanford 的論文，全名為 **[RAPTOR: Recursive Abstractive Processing for Tree-Organized Retrieval](https://arxiv.org/abs/2401.18059)**。今年初剛出來時看到很多人在 Twitter 上分享，同時這篇也被 ICLR 接受，非常值得閱讀。

相信讀者應該都具備基礎的 RAG 知識，這邊就不特別著墨了，直接來講論文的核心內容。

## Concept

### 本文想解決的問題：Simple RAG 的缺陷

> Despite a diversity in methods, the retrieving components of models predominantly rely on standard approaches, i.e., chunking corpora and encoding with BERT-based retrievers. Although this approach is widely adopted, Nair et al. (2023) highlights a potential shortcoming: contiguous segmentation might not capture the complete semantic depth of the text.

RAG 的大致製作流程包含：將文本分塊 → 建立 embedding → query 進來時，尋找相關的文本當作 context。然而將文本分塊時，必然會產生上下文語意上的資訊流失，導致找不到相關的文本或資訊不完整。

因此這篇論文想要解決的問題就是：**如何在文本切塊的同時，盡可能地保留語意等級的資訊。**

### 隨著 LLM 可接受的 context 變長，為什麼還需要 RAG？

> Why Retrieval? … However, as Liu et al. (2023) and Sun et al. (2021) have noted, models tend to underutilize long-range context and see diminishing performance as context length increases, especially when pertinent information is embedded within a lengthy context.

當今 LLM 發展趨勢的其中一項是：可以輸入 LLM 的 token 越來越長。例如：GPT-4o (128k), Claude 3.5 Sonnet (200k)。

這時你可能會想，如果可以簡單地將所有 context 一次餵給 LLM，那還搞什麼 RAG，簡單地把所有東西塞給 LLM 就完事啦～

當然實務上，如果只是要 Demo 搞一個 Baseline 版本，這樣的做法當然沒有問題。不過如果對模型輸出的質量有所追求的話，那麼必須知道 context 過長有以下的缺點：

1. **較長的 context 模型越難以理解，輸出的質量較差。**
2. **較長的 context 經常伴隨著無關的內容，這對模型理解造成負面影響。**
3. **較長的 context 貴又慢。**

基於上面這三點，目前 RAG 還是非常適合應用在 LLM 產品中！（暫時還不會被淘汰）

## Solution

### 核心概念 (Idea)

前面提過，當前簡易的 RAG 系統最大的問題在於：分割文本時，容易丟失關鍵訊息，造成切塊的文本之間的語意不完整，導致檢索不夠精確。

那麼怎麼保留丟失的關鍵訊息呢？

本文提出的解法思路非常簡潔：**透過不斷地群聚再總結，產生一個階層式的架構來盡可能保留語意相似的資訊。**

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/paper/RAPTOR_Recursive_Abstractive_Processing_for_Tree-Organized_Retrieval/raptor_figure1.png" title="RAPTOR Tree Architecture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 1. RAPTOR Tree Architecture Overview.
</div>

這篇論文的方法是將 leaf layer（最原始分塊後的文本）先做一次分群，接著把群裡文本分別進行一次總結，從而產生出新 layer 的文本（對應圖中 6,7,8）。不斷地循環這個過程，直到達到預設的 layer 上限，最終就可以建構出一個樹的結構，每一層 layer 都涵蓋前一層 layer 相關文本的完整資訊。

簡單來說，整個流程大致如下：

1. **先將原始的文本分塊，得到第一層 layer (leaf layer)。** Ex: 一篇文章 → 5個 chunks (Figure 1. 中的 1,2,3,4,5)
2. **對該層的文本進行分群。** Ex: 5 chunks → 3個聚類 (Figure 1. 中的 layer 2 灰色區域)
3. **對每個群的文本進行 summary，得到下一層的文本。** Ex: 3 個聚類 → 3個新文本 (Figure 1. 中的 3,5 產生一個 summary 得到新層級的文本 6)
4. **重複 2~3 步驟，直到達到 Max layer。** Ex: Figure 1. 中最大的 layer 看起來是 3 層

### 分群 (Clustering)

> Clustering plays a key role in building the RAPTOR tree, organizing text segments into cohesive groups. This step groups related content together, which helps the subsequent retrieval process.

RAPTOR 這篇論文最重要的就是分群。本質上，整篇論文就是透過遞迴分群法，產生一個階層狀的文本架構，讓 RAG 系統得到更豐沛的 context。

在實作中，為了讓分群效果更好，作者做了一些特別處理：

#### Soft Clustering

> One of the unique aspects of our clustering approach is the use of soft clustering, where nodes can belong to multiple clusters without requiring a fixed number of clusters. This flexibility is essential because individual text segments often contain information relevant to various topics, thereby warranting their inclusion in multiple summaries.

現實中，一個文本當然不只一種類型。因此作者考慮用軟分群 (soft clustering) 的分群演算法，就是 **Gaussian Mixture Model (GMM)**。

如果對 GMM 不熟也沒關係，GMM 假設一個文本可能來自於多個群，只是屬於每個群的機率不同。

#### Dimension Reduction — UMAP

> The high dimensionality of vector embeddings presents a challenge for traditional GMMs, as distance metrics may behave poorly when used to measure similarity in high-dimensional spaces (Aggarwal et al., 2001). To mitigate this, we employ Uniform Manifold Approximation and Projection (UMAP), a manifold learning technique for dimensionality reduction (McInnes et al., 2018).

作者指出，GMM 在高維度空間中效果不佳，因為高維度下距離計算的準確度下降，而分群的本質就是算距離。因此這邊作者打算將 embedding 後的文本做降維，具體使用的方法為 **UMAP**。（比方文本 embedding 後原本是 768 維度，用 UMAP 降到 20 維）

那實務中，應該降到多少維度？

雖然作者提到高維度分群效果不好來自於相關論文的結果。不過該論文沒提到具體應該要多少維度，但其實驗都是在維度 20 做的，可以參考這個數值看看。

#### Global & Local Clustering

> ...it first identifies global clusters and then performs local clustering within these global clusters. This two-step clustering process captures a broad spectrum of relationships among the text data, from broad themes to specific details.

作者設計兩階段的分群方法，先對一層 layer 先做一次 global clustering，接著對每個群裡面分別再分一次群。這樣不僅能捕捉高層級的群，也能兼顧每個群裡的細部項目。

簡單來說，兩階段分群可以捕捉更多文本之間的關係。這邊大致瞭解為兩階段分群的用意就好，想知道細節可以去看實作。

#### Bayesian Information Criterion (BIC)

分群中最常見的問題就是群數要設多少？

作者直接用 **Bayesian Information Criterion** 算資訊分數來決定要分多少群。下面是計算公式：

$$BIC = -2 \ln(L) + k \ln(N)$$

其中 N 表示文本片段的數量，k 表示模型參數的數量，而 L 是模型的最大概似函數值。

### 檢索 (Retrieve)

前面的部分都是在講怎麼建立知識庫，接下來要講要怎麼檢索。

共有兩種方法，各有優勢：

#### Tree Traversal (遍歷樹)

簡單來說，給定 root 的文本，將其所有 children 都找出來當作 retrieve 的相關文本。

**優點：** 資訊顆粒度均勻、拿到的文本包含廣泛和詳細的文本，資訊豐沛且多樣。

**缺點：** 檢索複雜、需要調整的地方比較多（如每一層要拿幾個？拿多深？）

實作中，假設每一層都拿最相關的那個文本。

#### Collapsed Tree (壓縮樹)

這個更簡單，直接將樹攤平，所有文本都是同一層的情況下去挑最相關的文本。

**優點：** 檢索簡單、適合回答特定問題（顆粒度恰好）

**缺點：** 檢索顆粒度粗細度不均、需要對每個文本都做檢索，計算消耗比較大。

#### 實務上哪種檢索比較好？

作者認為**壓縮樹的表現平均來說優於遍歷樹**。因為壓縮樹考慮所有文本的資訊相關性，得到的資訊顆粒度適合用來回答一些特定問題。反觀遍歷樹，不論怎麼設定每層的檢索數目，最後拿到的文本包含粗細的比例都保持不變。

## 總結

- **這篇論文解決什麼問題？** 文本切割 → 語意不完整 → 檢索不準確
- **解法：** 遞迴分群 → 產生多層次的文本，保留完整的語意

---

## References

- [Lost in the Middle: How Language Models Use Long Contexts](https://arxiv.org/abs/2307.03172)
- [In Defense of RAG in the Era of Long-Context Language Models](https://arxiv.org/abs/2409.01666)
- [On the Surprising Behavior of Distance Metrics in High Dimensional Space](https://link.springer.com/chapter/10.1007/3-540-44503-X_27)
- [Late Chunking: Balancing Precision and Cost in Long Context Retrieval](https://arxiv.org/abs/2409.04701)
