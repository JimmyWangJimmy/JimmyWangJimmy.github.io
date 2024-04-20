
在项目根目录
创建db.json文件，并安装启动json-server。
```shell
 npx json-server --port 3001 --watch db.json
```
也可以在package.json中添加启动命令
```shell
 "scripts": {
    //...
    "server": "npx json-server -p3001 --watch db.json"
  },
```


安装axios，在package.json中可以看到axios成功安装。
```
npm install axios
```

```shell
  "dependencies": {
    "axios": "^1.6.8",
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
```
