---
layout: post
title: "刚开始学 React 时，真正反复绊住我的其实不是框架，而是 JavaScript 本身"
date: 2024-04-10 19:05:42 +0800
categories: [与AI共生]
summary: "React 入门早期最容易卡住的，经常不是 JSX 或组件概念，而是数组拷贝、`map` 返回值、状态不可变这些 JavaScript 基础没有真正吃透。"
pillars:
  - 与AI共生
---

刚开始学 React 的时候，我以为最难的是框架本身。

后来越学越觉得，真正最容易反复绊住人的，往往不是 React，而是 JavaScript 基础在组件和状态场景里突然变得不能再含糊。

## 第一步：先把项目真正跑起来

现在回头看，React 入门最好的起点还是尽快把一个最小项目跑起来。

例如用 Vite：

```shell
npm create vite@latest filename -- --template react
cd filename
npm install
npm run dev
```

这一步的价值不是“创建了一个项目”，而是让你尽快进入能改、能刷、能观察状态变化的环境。

很多理解如果只停在教程文字里，很难真正变成自己的东西。

## React 入门最早会卡住什么

### 1. 状态不是随便改的

一旦开始用 `useState`，就会很快碰到一个问题：  
数组和对象不能像以前那样随手改完再继续用。

比如状态是数组：

```jsx
const [state, setState] = useState([])
```

如果你要新增一项，更自然的写法是：

```jsx
setState([...state, newItem])
```

这里用到的不是 React 特技，而是一个更基本的原则：  
状态更新要尽量基于“新值”，而不是直接修改原值。

### 2. `map` 不是“写了就会返回”

这个坑我当时也踩过。

比如本来想把对象数组提取成名字数组，正确写法是：

```jsx
const personnames = persons.map(person => person.name)
```

但如果写成这样：

```jsx
const personnames = persons.map(person => { person.name })
```

结果就会变成一堆 `undefined`。

原因不是 React，而是 JavaScript 箭头函数在使用 `{}` 时，需要显式 `return`。

这类问题特别典型：  
看起来像是“框架没工作”，实际上是语言本身的规则还没真正内化。

### 3. 拷贝数组这件事，比想象中更重要

像下面这种写法：

```jsx
const copy = [...votes.slice(0, selected), votes[selected] + 1, ...votes.slice(selected + 1)]
```

第一次看会觉得很绕，但它背后其实是在解决一个很核心的问题：

如何在不直接修改原状态的前提下，生成一个新的状态结果。

一旦这个思路开始建立，很多 React 状态更新就会突然顺很多。

## 为什么我现在会把这些问题归到“React 入门的真正门槛”

因为 React 最容易制造一种误解：

好像问题都来自框架本身。

实际上很多早期报错、渲染异常、状态不更新，根因是下面这些基础问题：

- 数组和对象怎么拷贝
- `map` / `filter` / `slice` 到底返回什么
- 箭头函数什么时候隐式返回
- 状态更新为什么不能依赖“原地修改”

这些东西在普通 JavaScript 里可能还能模糊着用，但进了 React 之后，它们会被放大得非常明显。

## 我后来怎么理解 React 学习曲线

现在回头看，React 入门并不是“先学框架，再补基础”。  
更真实的过程其实是：

React 会不断逼你重新学习 JavaScript。

也正因为这样，它才是一个很好的训练场。  
你会在一次次组件、状态、列表渲染和事件处理里，慢慢把以前会但不稳的基础真正补牢。

所以如果刚开始学 React 总觉得自己哪里不顺，不一定说明你不适合它。  
很多时候只是框架正在逼你面对一个更真实的问题：

你对 JavaScript 的理解，究竟是“看过”，还是“真的会用”。
