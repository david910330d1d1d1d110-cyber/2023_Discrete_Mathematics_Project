# 有限集合上遞移關係數量的計數問題 (Transitive Relations Counting)
> **Discrete Mathematics Project | From Naive to Bit-vector Optimization**

[📄 點此閱讀完整研究報告 (PDF)](report_dm.pdf)

這是關於 **OEIS A006905** 序列的計數實作。本專案探討如何透過程式邏輯，在元素數量為 $n$ 的集合中，計算所有符合「遞移律 (Transitivity)」的關係矩陣總數，並嘗試透過位元優化克服組合爆炸帶來的運算瓶頸。

## 📖 研究動機
在離散數學中，遞移律的定義為：若 $(a, b) \in R$ 且 $(b, c) \in R$，則 $(a, c) \in R$。
雖然定義簡單，但實作計數時面臨極大的效能挑戰：

1. **理論驗證**：產出數據並與 **OEIS A006905** 序列比對，確保演算法在數學上的正確性。
2. **組合爆炸**：關係總數以 $2^{n^2}$ 指數級別成長。當 $n=6$ 時，搜尋空間高達 $2^{36} \approx 6.87 \times 10^{10}$ 種組合。
3. **效能挑戰**：觀察當 $n=6$ 時，高達 **687 億次** 的運算量如何讓普通演算法崩潰，並嘗試優化它。

## 🛠️ 演算法實作與優化

### 1. 暴力法 (Naive Approach) - $O(n^3)$
使用三層嵌套迴圈檢查所有的三元組 $(i, k, j)$。雖然邏輯直觀，但在面對大數據時效能極低：
```c
for (int i = 0; i < n; i++)
    for (int k = 0; k < n; k++)
        if (Matrix[i][k]) // 若 i -> k 存在
            for (int j = 0; j < n; j++)
                if (Matrix[k][j] && !Matrix[i][j]) return 0; // 若 k -> j 存在但 i -> j 不存在，則不符合

# 有限集合上遞移關係數量的計數問題 (Transitive Relations Counting)
> **Discrete Mathematics Project | From Naive to Bit-vector Optimization**

## 有限集合上遞移關係計數 (Transitive Relations)
### 從暴力法到位元並行優化 (From Naive to Bit-vector Optimization)

[📄 點此閱讀完整研究報告 (PDF)](report_dm.pdf)

探討如何計算元素數量為 $n$ 的集合中，所有符合「遞移律」的關係矩陣數量，並克服 $2^{n^2}$ 組合爆炸帶來的運算效能問題。

#### 🛠️ 演算法優化過程
1.  **暴力法 (Naive Approach) - $O(n^3)$**：
    使用三層迴圈檢查三元組，當 $n=6$ 時面臨高達 687 億次的運算量，效能極低。
2.  **位元並行優化 (Bit-vector Optimization) - $O(n^2)$**：
    將矩陣每一列存為 **Bit-vector**。若 $i \to k$ 成立，則利用位元交集快速判斷 $i$ 是否指向所有 $k$ 指向的元素。
    ```c
    // 關鍵優化邏輯：一次 CPU 位元運算取代內層 O(n) 迴圈
    if ((Row[k] & Row[i]) != Row[k]) return NOT_TRANSITIVE;
    ```

#### 📊 實驗數據 (OEIS A006905)
| $n$ (元素數) | $2^{n^2}$ (總關係數) | 遞移關係總數 (Output) |
| :--- | :--- | :--- |
| 4 | 65,536 | 3,994 |
| 5 | 33,554,432 | 154,303 |
| 6 | 68,719,476,736 | 9,415,189 |

#### 🧪 技術突破
* **Early Exit 策略**：發現反例立即跳出，大幅降低平均運算時間。
* **底層優化**：透過 C 語言處理記憶體映射與位元運算，壓榨硬體效能以對抗指數級成長的計算量。

---

### 📫 聯繫方式
* **GitHub**: [你的GitHub連結]
* **Email**: [你的電子郵件]
* **Status**: 資工研究所考生，專注於演算法、系統優化與數學應用。
