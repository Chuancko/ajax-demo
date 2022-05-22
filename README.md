## nodemon

避免每次更新代码 node 都需要重开的情况，全局安装：

```
yarn global add nodemon // npm install -g nodemon
```

使用

```
nodemon server.js 12345
```

## 问题记录

在挑战用 AJAX 加载下一页时出现了错误，通过 log 法逐步解决。

```
  request.onreadystatechange = () => {
    if (request.readyState === 4 && request.status === 200) {
      console.log(request.response)
      })
    }
  }
```

这个`console.log(request.response)`没有被打印出来，于是我转而

```
console.log(request.readyState === 4 && request.status === 200)
```

得到了 false 值，再而我分别

```
console.log(request.readyState)
console.log(request.status)
```

返回的值竟然是 4 0，status 不应该是 1xx-5xx 吗，怎么会是 0 呢？经过一番搜寻后发现，status 的值一定会返回运行这些步骤的结果：

1. If the state is UNSENT or OPENED, return 0.（如果状态是 UNSENT 或者 OPENED，返回 0）
2. If the error flag is set, return 0.（如果错误标签被设置，返回 0）
3. Return the HTTP status code.（返回 HTTP 状态码）

ok，问题解决了，原来是我把 status 打成了 states。。
