# 有限集合上遞移關係數量的計數問題 (Transitive Relations Counting)
> **Discrete Mathematics Project | From Naive to Bit-vector Optimization**

[📄 點此閱讀完整研究報告 (PDF)](report.pdf)

這是編號 **A006905 (OEIS)** 序列的計算實作。本專案旨在探討如何透過程式邏輯，在元素數量為 $n$ 的集合中，計算所有符合「遞移律 (Transitivity)」的關係矩陣總數，並嘗試克服組合爆炸帶來的運算瓶頸。

## 📖 研究動機
在離散數學中，遞移律的定義為：若 $(a, b) \in R$ 且 $(b, c) \in R$，則 $(a, c) \in R$。
雖然定義簡單，但實作計數時面臨極大挑戰：
* **組合爆炸**：關係總數以 $2^{n^2}$ 指數級別成長（當 $n=6$ 時，高達 $2^{36} \approx 6.87 \times 10^{10}$ 種組合）。
* **效能瓶頸**：傳統 $O(n^3)$ 演算法在 $n \ge 6$ 時會產生數百億次的運算，導致運算時間過長。

## 🛠️ 演算法實作與優化

### 1. 暴力法 (Naive Approach) - $O(n^3)$
使用三層嵌套迴圈檢查所有的三元組 $(i, k, j)$：
```c
for (int i = 0; i < n; i++)
    for (int k = 0; k < n; k++)
        if (Matrix[i][k]) // 若 i -> k 存在
            for (int j = 0; j < n; j++)
                if (Matrix[k][j] && !Matrix[i][j]) return 0; // 若 k -> j 存在但 i -> j 不存在，則不符合
