---
title: 初步認識 React hook：useState
date: 2025-07-10 16:42:59
tags: 
- React
- 學習
---
## 1. 什麼是 useState？  

useState 是 React hooks 的一種，以下為用法介紹：

**1. 用來在「函式型元件」中建立和管理狀態（state）**
**2. 類似資料的概念，可以定義為瞬間的狀態：**計數器、輸入框內容、開關狀態等
**3. 只要狀態改變，畫面會自動重新渲染**
**4. 每個 useState 都是獨立的：**可以在同一個元件中使用多個 useState
**5. 狀態值可以是任何型別：**數字、字串、布林、物件、陣列都可以
**6. 不能在條件判斷、迴圈中使用：**需要放在元件最外層(return之前)，通常會放在元件中最上方
**7. 初始值只會設定一次：**即使重新渲染也不會重新執行 useState(初始值)

<!-- more -->

## 2. useState 基本語法
```jsx
    const [state, setState] = useState(1);
```
- **變數宣告：**使用 `useState` Hook 宣告一個名為 `state` 的狀態變數，初始值為 1
- **解構賦值：**從 `useState` 回傳的陣列中取得兩個值：
        - `state`：目前的狀態值，初始值為 `1`
        - `setState`：用於更新 `state` 的函式,呼叫 `setState` 會觸發元件重新渲染
- **初始值設定：** `useState(1)` 中的參數 `1` 是 `state` 的初始值。這表示在元件首次渲染時，`state` 的值為 `1`
- **更新狀態：** 當需要更新 `state` 的值時，呼叫 `setState` 函式，並傳入新的值。
例如，`setState(2)` 會將 `state` 更新為 `2`，並觸發元件的重新渲染，以反映最新的狀態

我請chatGPT給我一個好記的口訣😆： <span style="color:red;font-size:20px;"> 宣告狀態 → 改變狀態 → 自動更新畫面！</span>

## 3. `useState` 的生命週期（Life Cycle）

**A. 元件初始化（Initial Render）**
- `useState`(初始值) 被執行一次
- 建立「狀態變數」與「更新函式」
- 初始值只執行一次，不會每次重新渲染時都重設初始值

**B. 狀態被更新**
- 呼叫 `setState` → 觸發元件重新渲染
- 但 `useState()` 本身不會重新執行
- 原本的狀態值會被保留下來（React 自動幫你管理）

**C. 元件卸載（Unmount）**
- 該元件從畫面中被移除時
- 對應的狀態資料也會一起被銷毀（React 自動清理）

✅ 圖解流程
`useState()` 初始值設定 → 元件 render，畫面出現 → 使用 `setState `更新狀態 → React 重新 render，畫面更新 → 狀態仍保留，直到元件被卸載

📢範例：計數器
```jsx

import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0); // 生命週期開始：初始化為 0

  return (
    <div>
      <p>目前計數：{count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button> 
    </div>
  );
}
```
解釋：
| 階段        | 行為                                     |
| --------- | -------------------------------------- |
| 初次 render | `count = 0`，畫面顯示 0                     |
| 點一次按鈕     | 呼叫 `setCount(1)` → React 幫你更新狀態並重新渲染畫面 |
| 再點一次      | `count = 2`，畫面繼續更新                     |                               |

💡 **<span style="color:red;font-size:14px;"> `setState()` 會觸發 re-render，但不會重新跑 `useState()`</span>**
可以想像成：
有一個計分板，剛開始分數是 0（這是 `useState` 的初始值）
每次加分（`setState`）分數會變，計分板會顯示新分數（re-render）
但不會因為加分分數又自動回到 0（不會重新跑 `useState` 的初始值）

## 6. 總結

1. `useState()` 初始值只在第一次執行，之後不會重跑
2. `setState()` 改變資料 → `React` 重新渲染畫面，但不會回到初始值
3. 每次渲染都是「新的畫面 + 保留的狀態」
4. 若狀態之間有依賴（像根據 A 狀態決定 B），可考慮搭配 `useEffect` 使用

