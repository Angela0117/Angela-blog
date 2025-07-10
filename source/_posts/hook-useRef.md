---
title: 初步認識 React hook：useRef
date: 2025-07-10 16:42:59
tags: 
- React
- 學習
---

## 1. 什麼是 useRef？  

useRef 是 React hooks 的一種，主要的功能為：

**1. 透過 `useRef()` 抓取 DOM 元素（類似 JS 的 querySelector）**
**2. 不受 `useState()` 的生命週期影響，透過 `useRef()` 保存特定值，但不影響畫面 re-render**
**3. 所有用 `useRef()` 建的變數，都要 `.current` 才能拿到 DOM**

<!-- more -->

## 2. useRef 基本語法
```javaScript
const xxxRef = useRef(null);     // 1️⃣ 建立 ref
return <div ref={xxxRef}></div>  // 2️⃣ 綁定元素
xxxRef.current                   // 3️⃣ 拿到 DOM
```

我請chatGPT給我一個好記的口訣😆： <span style="color:red;font-size:20px;"> 抓元素 → 用 ref → 拿 current！</span>

## 3. 什麼是 useRef().current？

範例一：抓取 DOM 元素
```jsx
import { useRef, useEffect } from 'react';

function MyComponent() {
  
  const inputRef = useRef(null);  //記住 input 這個 DOM 元素，初始值是 null

  useEffect(() => {
    inputRef.current && inputRef.current.focus();  // 確保 current 有東西再操作
  }, []);
 
  return <input ref={inputRef} />;  //綁定到 input 元素上
} 
```
可以想像`inputRef`是個物件，有一個`current`屬性
```jsx
const inputRef = {
  current: null 
}
```
‼️<span style="color:red;font-size:14px;"> 注意：
因為初始值為`null`，`inputRef.current` 只有在該元素已經 mount 後才會有值，通常需搭配 useEffect 使用，避免在尚未渲染時操作。`React` 會在畫面渲染後，自動把這個 DOM 元素塞進 .current，使得該 input 自動 focus</span>

```jsx
useEffect(() => {
    inputRef.current && inputRef.current.focus();
  }, []);
```

範例二：記住資料
```javaScript
const timerRef = useRef(null);
useEffect(() => {
  timerRef.current = setInterval(() => {
    // do something
  }, 1000);

  return () => clearInterval(timerRef.current); // 清除 timer
}, []);

```
#### 範例解釋
- useRef ：可以保存可變的值（mutable value），用來保存 timer ID，不會因為 re-render 而消失
- setInterval ：每 1000ms 執行一次 callback
- 清除副作用：在 useEffect 的 return 中用 clearInterval 清除定時器，避免 memory leak
- 依賴陣列為空：確保只在元件掛載與卸載時執行一次


以上簡單範例就像是： **(useRef)這隻手已經抓住 input 元素，可以做事(進行後續動作)了。**

## 4. current 是必須的嗎？
是，<span style="color:red;">因為 useRef() 回傳的是物件（不是 DOM 元素本身），真正的 DOM 或資料都在 .current 屬性裡。</span>

## 5. React vs JS 的差別
| React useRef   | JS querySelector |
| -------------- | ---------------- |
| React Hook 語法  | 原生 JS            |
| `.current` 拿元素 | 直接找 DOM          |
| 自動隨 React 更新   | 可能找不到最新 DOM（React 還沒畫好時）      |


## 6. 總結
1. `useRef()` 回傳的是一個物件 `{ current: ... }`
2. `.current` .current 才是我們真正要操作的 DOM 元素
3. `.current` 會在畫面渲染後由 React 自動塞入

