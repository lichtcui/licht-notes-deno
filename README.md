# Deno
ts文件第一次运行需要编译，编译完成后会缓存起来，下次运行不需要再次编译

## Deno 特点
- 默认支持ES Modules
- 默认支持TypeScript
- 尽可能使用 Web 标准
- 沙箱模式更安全
- 提供标准库

## Deno vs Node
### 优势
- 沙箱模式更安全
- 没有大量 npm 模块
- 原生模块系统支持
- 兼容 Web 的运行时 APIs

### 劣势
- 生态！！！

## deno info  
查看文件所在目录（缓存文件）

## Deno 权限
-|-
-|-
--allow-net|连接网络权限
--allow-read|读取权限
--allow-write|写权限
--unstable|会存在不稳定模块‘希望使用明确版本地址’ 
> unstable：deno在大幅度迭代过程，部分runtime api还不够稳定

## 兼容 Web 标准 APIs
```
const res = await fetch('https://api.github.com')
const data = await res.json()
console.log(data)

// Deno 中的运行时 Api 默认全部使用 promise
const decoder = new TextDecoder('utf-8')
const buffer = await Deno.readFile('./temp.txt')
const contents = decoder.decode(buffer)

console.log(contents)
```

## 库
将内部‘不方便使用’的 运行时 API 封装成简单 API，会自动加载master的代码
```
import { ensureDir } from 'https://deno.land/std/fs/mod.ts'
// mkdir -p
await ensureDir('./foo/bar/baz')

import { serve } from 'https://deno.land/std/http/server.ts'

const server = serve({ port: 8000 })

console.log('http://localhost:8000/')
for await (const req of server) {
  // 每有一次请求，都会执行一轮
  req.respond({ body: 'hello world\n' })
}
```

## 第三方模块
Deno同样允许第三方模块机制，建设模块生态，单纯通过URL方式，任何服务器都可以
```
import { hello } from 'https://cdn.zce.me/demo/mod.ts'
console.log(hello('tom'))

import { Application } from 'https://deno.land/x/oak/mod.ts'
const app = new Application()
app.use(ctx => { ctx.response.body = 'hello world'})
await app.listen({ port: 8000 })
```
