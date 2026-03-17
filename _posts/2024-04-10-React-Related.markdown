---
layout: post
title: "React 入门卡点复盘：多数问题其实来自 JavaScript 基础"
date: 2024-04-10 19:05:42 +0800
categories: [拆问题]
summary: "React 初学期最常见问题并不是框架 API，而是 JS 基础在状态管理场景下被放大，例如不可变更新、箭头函数返回值和数组操作。"
pillars:
  - 拆问题
---

我一开始也以为 React 难在框架本身。  
后来发现最常绊脚的，往往是 JavaScript 基础没有在状态场景里用扎实。

## 三个高频卡点

1. 直接修改状态对象/数组，导致视图不更新
2. `map` 使用 `{}` 却忘了 `return`
3. 更新数组时把旧值改坏，造成链式错误

## 一个典型例子

```jsx
const [items, setItems] = useState([])
setItems([...items, newItem])
```

这行的重点是“创建新数组”，而不是“修改原数组”。

## 你可以这样自检

1. 所有 `setState` 是否基于新值
2. `map/filter` 回调是否正确返回
3. 组件渲染依赖是否来自 state/props，而不是外部可变变量

## 一句话结论

React 学习曲线的本质是：框架在逼你把 JavaScript 基础真正练扎实。
