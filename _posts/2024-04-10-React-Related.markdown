---
layout: post
title:  "React Related"
date:   2024-04-10 19:05:42 +0800
categories: jekyll update
---

- [Egghead Beginner Guide To React](https://egghead.io/courses/the-beginner-s-guide-to-react) 
the new functional ones.

#### 创建React应用程序
```shell
npm create vite@latest filename -- --template react
```
```shell
cd filename
```
```shell
npm install
```
```shell
npm run dev
```

移除`App.css` `Index.css`文件 `assets`文件夹

修改`App.jsx`文件
```jsx
import { useState } from 'react'
const App = () => {
  const [count, setCount] = useState(0)
  const handleClick = () => {
    setCount(count + 1)
  }
  return (
    <div>
      <button onClick={handleClick}>Hello World{count}</button>
    </div>
  )
}
```

修改`Main.jsx`文件
```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```


###### 使用 Spread Syntax方法

·[MDN Spread_syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
<br>
什么时候用到Spread Syntax方法？
```text
在使用conditional state的时候，将一个数组作为状态，比如
const [state, setState] = useState([])
此时如果使用setState去更新状态，就会涉及到对原state数组的更新，但是由于const的限制，我们无法直接对state进行修改，所以我们需要使用Spread Syntax方法，将原state数组展开，然后再进行更新。
具体的setState方法如下：
setState([...state, newItem])
```

举个例子：
我的初始状态是一个空数组
```jsx
const [votes, setVotes] = useState(anecdotes.map(() => 0))
```

这是创建一个copy数组的方法，使用Spread Syntax方法和slice方法

```jsx
const copy = [...votes.slice(0, selected), votes[selected]+1,  ...votes.slice(selected+1)]
```

###### 创建数组
Array(anecdotes.length).fill(0)

anecdotes.map(() => 0)


###### 运行`npm i`解决 `npm run dev`报错:`'vite' 不是内部或外部命令，也不是可运行的程序或批处理文件。`


###### '{}'的使用注意
我的初始状态设置中有一个数组
```jsx
  const [persons, setPersons] = useState([
    { name: 'Arto Hellas' },{ name: 'Ada Lovelace' },{ name: 'Dan Abramov'}
  ]) 
```
我想要使用map函数将其中的name属性单独构建一个数组，正确的做法如下：
```jsx
const personnames = persons.map(person => person.name)
```
浏览器使用console.log(personnames)输出结果为：
```jsx
(3) ['Arto Hellas', 'Ada Lovelace', 'Dan Abramov']
```

如果不小心加上了一个可恶的'{}'，就会出现：
```jsx
const personnames = persons.map(person => {person.name})
```
浏览器使用console.log(personnames)输出结果为：
```jsx
(3) [undefined, undefined, undefined]
```

原因在于，当我们使用{}时，我们需要在{}中使用return关键字，否则会返回undefined。
更深层的原因是：