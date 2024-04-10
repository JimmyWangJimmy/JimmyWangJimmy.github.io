---
layout: post
title:  "React Related"
date:   2024-04-10 19:05:42 +0800
categories: jekyll update
---

- [Egghead Beginner Guide To React](https://egghead.io/courses/the-beginner-s-guide-to-react) 
the new functional ones.

#### ����ReactӦ�ó���
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

�Ƴ�`App.css` `Index.css`�ļ� `assets`�ļ���

�޸�`App.jsx`�ļ�
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

�޸�`Main.jsx`�ļ�
```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```
