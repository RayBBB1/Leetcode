## Review of Sliding Window

## Sliding Window 技巧心得總結

在演算法解題中，我認為最關鍵的環節在於：

1. **判斷適用問題的解法**
2. **根據解法解決問題**
3. **追求效率優化**

本篇心得將著重於前兩點，特別是 **何時使用** 以及 **如何實作** 滑動窗口技巧，希望能幫助讀者快速掌握這個常用的解題策略。

### 1. 何時使用滑動窗口 (When to Use Sliding Window)?
根據目前撰寫LeetCode 75 Sliding window相關題型。
初步總結，當你發現題目有以下特徵時，可以考慮使用滑動窗口：

* **暴力解法 (Brute Force) 的潛在需求**:  題目乍看之下，可能會傾向使用多重迴圈來窮舉所有可能的子陣列或子字串。
* **子陣列/子字串 (Subarray/Substring) 問題**: 題目明確要求處理連續的陣列元素或字串片段。
* **特定條件的搜尋**:  題目通常會要求在子陣列或子字串中尋找符合特定條件的解，例如：
    *  最長/最短的子陣列/子字串 (Longest/Shortest Subarray/Substring)
    *  總和、平均值、乘積滿足特定條件的子陣列/子字串 (Subarray/Substring with Sum, Average, Product Condition)
    *  包含特定元素或模式的子陣列/子字串 (Subarray/Substring with Specific Elements or Patterns)
    *  計數符合條件的子陣列/子字串數量 (Count of Subarrays/Substrings Meeting Criteria)

**核心概念：將原本可能的雙重迴圈窮舉，轉化為基於窗口滑動的單迴圈操作**  
它透過維護一個窗口，並在滑動的過程中僅需局部更新窗口內的資訊，避免了完全重複的計算，從而降低時間複雜度。  相較於暴力窮舉所有子陣列 (通常是 O(n^2) 或更高)，滑動窗口通常能將時間複雜度降至 O(n)。

**簡而言之，當你在思考暴力解法，且題目涉及連續子陣列或子字串的相關問題時，滑動窗口就是一個值得考慮的優化方向。**

### 2. 如何制定滑動窗口解題策略 (How to Implement Sliding Window)?

滑動窗口的核心思想是將原本可能的雙重迴圈窮舉，轉化為基於窗口滑動的單迴圈操作。

**關鍵：透過左右指針 (left/right pointers) 來控制窗口的大小和位置。**

#### Method 1 (Original - 雙重 While 迴圈)

```c++
// Method 1 (Original - 雙重 While 迴圈)
// (不建議使用，邏輯複雜，容易出錯)
int slidingWindow_method1(std::vector<int>& nums, int target) { // 範例，假設 nums 是輸入陣列，target 是目標值
    int left = 0;
    int right = 0;
    int result = 0; // 初始化結果

    while (right < nums.size()) {
        while (/* 擴展 right 指針的條件，根據題目調整 */) {
            // 移動 right 指針，擴展窗口
            right++;
        }

        while (/* 收縮 left 指針的條件，根據題目調整 */ && left < right) {
            // 移動 left 指針，收縮窗口
            left++;
        }
        // 在這裡檢查當前窗口 (從 left 到 right-1) 並更新結果
        // ... 更新 result ...
    }
    return result;
}
```

**Method 1 的問題：**

* **邏輯複雜**: 兩個 `while` 迴圈的條件容易互相糾纏，使得邏輯判斷變得複雜且容易出錯。 需要仔細設計兩個迴圈的跳出條件和指針移動策略。
* **不易理解**:  程式碼結構相對不直觀，較難以理解窗口的滑動過程。
* **效率可能較差**:  雖然在某些情況下也能運作，但雙重 `while` 迴圈的結構可能不如單迴圈方法效率高。

**不建議使用 Method 1，僅作為理解滑動窗口概念的起點。**

#### Method 2 (Optimized - 單一 Right 指針迭代)
現在我們每次只移動一次right，window大小完全通過控制left pointer來完成
核心思想:
  - right--> window position
  - left --> window size
每次的思考要改為，在當前這個right，這個position，left要怎麼移動才回符合題目設計
當right 在0時 left要移動到哪?
當right 在1時 left要移動到哪?
仿照這個思路
```c++

int slidingWindowTemplate(vector<int>& nums, int target) { // 範例：假設 nums 是輸入陣列，target 是目標值
    int left = 0;          // 左指針，窗口的起始位置
    int right = 0;         // 右指針，窗口的結束位置
    int result = 0;        // 初始化結果 (根據題目調整)

    while (right < nums.size()) {
        // 1. 擴展窗口 
        // 可以在此處紀錄一些題目相關的資訊 (比如收縮窗口的條件)，方便之後計算
        right++;  // 移動 right 指針，擴展窗口

        // 2. 檢查窗口條件 & 收縮窗口 
        while (/* 窗口需要收縮的條件 */) { 
            // 3. 收縮窗口: 移動 left 指針，縮小窗口
            // 可以在此處紀錄一些題目相關的資訊 (比如收縮窗口的條件)，方便之後計算
            left++; // 移動 left 指針，收縮窗口
        }

        // 4. 更新結果 
        // 根據題目需求，在每次窗口滑動後更新 result
        // 例如： result = max(result, right - left); // 更新最大窗口長度
    }

    return result; 
}
```



### 3. 滑動窗口模板 (Sliding Window Template) - C++

```c++

int slidingWindowTemplate(vector<int>& nums, int target) { // 範例：假設 nums 是輸入陣列，target 是目標值
    int left = 0;          // 左指針，窗口的起始位置
    int right = 0;         // 右指針，窗口的結束位置
    int result = 0;        // 初始化結果 (根據題目調整)

    while (right < nums.size()) {
        // 1. 擴展窗口 
        // 可以在此處紀錄一些題目相關的資訊 (比如收縮窗口的條件)，方便之後計算
        right++;  // 移動 right 指針，擴展窗口

        // 2. 檢查窗口條件 & 收縮窗口 
        while (/* 窗口需要收縮的條件 */) { 
            // 3. 收縮窗口: 移動 left 指針，縮小窗口
            // 可以在此處紀錄一些題目相關的資訊 (比如收縮窗口的條件)，方便之後計算
            left++; // 移動 left 指針，收縮窗口
        }

        // 4. 更新結果 
        // 根據題目需求，在每次窗口滑動後更新 result
        // 例如： result = max(result, right - left); // 更新最大窗口長度
    }

    return result; 
}
```

