# HTTP

HTTP（Hypertext Transfer Protocol）是一种用于在互联网上传输数据的协议。它是万维网数据通信的基础。HTTP旨在促进客户端（如Web浏览器）和服务器（托管网站和Web应用程序的地方）之间的通信。

```
【请求报文】
请求行    POST   /s?ie=utf-8    HTTP/1.1
请求头	   key: value
		 key: value
		 key: value
		 key: value
		 ......
空行
请求体    uername=adimin&password=asmin

【响应报文】
响应行   HTTP/1.1  200 OK
响应头   Content-Type: text/heml;charset=utf-8
		Content-Length: 288
    	Content-encoding: gzip
    	key: value
    	......
空行
响应体   可以是html结构等
```





# Ajax

Ajax（Asynchronous JavaScript and XML）是一种用于在Web应用程序中实现异步通信的技术。它允许在不刷新整个页面的情况

## 第一个ajax请求

```javascript
// 1、创建对象
const xhr = new XMLHttpRequest();
// 2、初始化 设置请求方法和url
xhr.open("GET", "http://localhost:8000/jsonserve");
// 3、发送ajax请求
xhr.send();
// 4、设置响应处理 0 1 2 3 4 步骤
xhr.onreadystatechange = function () {
    // 判断on 当 readystate 属性 改变 change 时
    if (xhr.readyState === 4) {
        // 判断响应码 200 300 400 404 401 500
        if (xhr.status >= 200 && xhr.status < 300) {
            // 响应成功
            // 行  头  空行  体
            console.log(xhr.status); // 状态码
            console.log(xhr.statusText); // 状态字符串
            console.log(xhr.getAllResponseHeaders()); // 所有响应头
            console.log(xhr.response); // 响应体
        } 
    }
}


// POST请求
    // 1. 创见对象
    const xhr = new XMLHttpRequest();
    // 2. 初始化 设置请求类型、url、是否异步
    xhr.open('POST', 'http://localhost:8000/serve', true);
    // 0——1. 设置请求头(预定义、自定义)：可能产生options请求：使用jsonp跨域解决
    xhr.setRequestHeader('Content-Type','application/x-www-from-urlencoded')
    xhr.setRequestHeader('YZling','412')
    // 3. 发送请求：携带参数(格式自定义)
    // xhr.send('A=100&B=200');
    // xhr.send('A/100&B/200');
    xhr.send('A:100&B:200');
    // 4. 设置响应
    xhr.onreadystatechange = function () {
        // 判断状态
        if (xhr.readyState === 4) {
            // 判断请求
            if (xhr.status >= 200 && xhr.status < 300) {
                // 请求成功处理
                div.innerHTML = xhr.response;
            }
        }
    }
```

## 响应json数据

```javascript
// 1、创建对象
const xhr = new XMLHttpRequest();
// 00_1 设置响应数类型
xhr.responseType = 'json';
// 2、初始化 设置请求方法和url
xhr.open("GET", "http://localhost:8000/jsonserve");
// 3、发送ajax请求
xhr.send();
// 4、设置响应处理 0 1 2 3 4 步骤
xhr.onreadystatechange = function () {
    // 判断on 当 readystate 属性 改变 change 时
    if (xhr.readyState === 4) {
        // 判断响应码 200 300 400 404 401 500
        if (xhr.status >= 200 && xhr.status < 300) {
            // 响应成功
            // let data =JSON.parse(xhr.response);// 手都转化为json
            let data =xhr.response;
            div.innerHTML = data.name+'-'+data.hot;
        } else {
            alert('error')
        }
    }
}
```

## 请求超时、网络异常处理

```javascript
// 1、创建对象
const xhr = new XMLHttpRequest();
// 00_1 最大等待响应时时间
xhr.timeout =2000;
// 00_2 请求超时回调
xhr.ontimeout =function(){
    alert('请求超时')
}
// 00_3 网络异常回调
xhr.onerror = function(){
    alert('网络异常')
}
// 2、初始化 设置请求方法和url
xhr.open("GET", "http://localhost:8000/delay");
// 3、发送ajax请求
xhr.send();
// 4、设置响应处理 0 1 2 3 4 步骤
xhr.onreadystatechange = function () {
    // 判断on 当 readystate 属性 改变 change 时
    if (xhr.readyState === 4) {
        // 判断响应码 200 300 400 404 401 500
        if (xhr.status >= 200 && xhr.status < 300) {
            // 响应成功
            div.innerHTML = xhr.response;
        } 
    }
}
```

## 重复请求检验

```html
<body>
    <button>开始请求</button>
    <button>取消请求</button>
    <div style="width:200px; height: 50px; background-color: #e88;"></div>

    <script>
        const btns = document.querySelectorAll('button');
        const div = document.querySelector('div');
        let xhr = null; // 便于调用
        let isSending = false; // 是否重复请求

        btns[0].onclick = function () {
            // 判断是否重复请求：如果重复就取消所以才请求
            if(isSending) xhr.abort(); 
            xhr = new XMLHttpRequest(); // 1. 创建对象
            isSending = true;
            xhr.open('GET', 'http://localhost:8000/delay', true); // 2. 初始化
            xhr.send(); // 3. 发送请求
            xhr.onreadystatechange = function () { // 4. 设置响应处理
                if (xhr.readyState === 4) {
                    isSending = false;
                    if (xhr.status === 200) {
                        div.innerHTML = xhr.response;
                    }
                }
            }
        }
        btns[1].onclick = function () {
            xhr.abort();// 取消请求
        }
    </script>
</body>
```

## Jquery和Ajax

```html
 <script src="https://cdn.bootcdn.net/ajax/libs/axios/1.3.6/axios.js"></script>
<button>axios->get</button>
<button>axios->post</button>
<button>axios->other</button>
<script>
    const btns = document.querySelectorAll('button');
    axios.defaults.baseURL = 'http://localhost:8000';
    btns[0].onclick = function () {
        axios.get('/axios', {
            // url参数
            params: { a: 2, b: 1 },
            // 请求头信息
            Headers: { name: 'YZling' }
        }).then((value) => {
            // 请求回调
            console.log(value)
        })
    }
    // 参数（url、{请求体}、{其他配置}）
    // 如果第二个参数直接写一个对象没有设置请求体，那么这个对象就作为请求体
    btns[1].onclick = function () {
        axios.post('/axios', {
            // 行  头  体
            // url参数
            params: { a: 2, b: 1 },
            // 请求头信息
            Headers: { name: 'YZling' },
            // 请求体
            data: { uername: 'hhh', password: '123456' }
        }).then((value) => {
            // 请求回调
            console.log(value)
        })
    }
    // 直接一个对象
    btns[2].onclick = function () {
        axios({
            //请求报文格式：请求路径 行  头  体
            // url
            url: '/axios',
            // url参数
            params: { a: 2, b: 1 },
            // 请求头信息
            Headers: { name: 'YZling' },
            // 请求体
            data: { uername: 'hhh', password: '123456' }
        }).then((response) => {
            // 请求回调
            console.log(response)
            console.log(response.status)
            console.log(response.headers)
            console.log(response.data)
        })
    }
</script>
```

## fetch发送ajax请求

```javascript
// 参数：url|request对象、可选配置项
// 请求的 行 头 体 参数设置
fetch('http://localhost:8000/fetch?hot=412',{
    // 请求方法
    method:'POST',
    // 请求头
    headers:{
        name:'YZling',
    },
    // 请求体:可以是对象、字符串
    body:'a=100&b=200&c=200'
}).then((response)=>{
    console.log(response); // 响应体获取不清晰
    // return response.text(); 
    return response.json(); 
}).then((response)=>{
    console.log(response);
})
```

## 跨域

```html
<!-- 进入 http://localhost:9000/ 测试同源请求 -->
<h3>跨域</h3>
<button>get user info</button>
<script>
    // 同源策略：【协议、域名、端口】一致
    // 跨域：http\https  :8080\:8000
    // 同源
    const btn = document.querySelector('button');
    btn.onclick = function () {
        // 原生jsonp
        // 1、创建script标签
        const script = document.createElement('script');
        // 2、设置标签src属性
        script.src = 'http://127.0.0.1:8000/jsonp';
        // 3、将标签插入文档、
        document.body.appendChild(script);
    }
    function handle(data) {
        alert(data.name+data.hot)
    }
</script>
```

### jsonp解决

```html
<body>
    <!-- 进入 http://localhost:9000/ 测试同源请求 -->
    <h3>跨域</h3>
    <button>get user info</button>
    <div></div>
    <script>
        // 同源策略：【协议、域名、端口】一致
        // 跨域：http\https  :8080\:8000
        // 同源
        const btn = document.querySelector('button');
        const div = document.querySelector('div');
        btn.onclick = function () {
            // 1、创建对象
            const xhr = new XMLHttpRequest();
            // 2、初始化 设置请求方法和url
            xhr.open("GET", "http://localhost:8000/cros" );
            // 3、发送ajax请求
            xhr.send();
            xhr.onreadystatechange = function () {
                // 判断on 当 readystate 属性 改变 change 时
                if (xhr.readyState === 4) {
                    // 判断响应码 200 300 400 404 401 500
                    if (xhr.status >= 200 && xhr.status < 300) {
                        // 响应成功
                        div.innerHTML = xhr.response;
                    }
                }
            }
        }
    </script>
</body>
```



# Promise

Promise 是一种用于处理异步操作的 JavaScript 对象。它提供了一种更优雅和可预测的方式来编写和组织异步代码，避免了回调地狱（callback hell）和嵌套的问题。Promise 在现代 JavaScript 中广泛用于处理网络请求、文件读写、定时器等需要等待结果的操作。

## promise的状态

promise实例对象上两个属性之一：【PromiseState】。保存当前实例的状态。
一个 Promise 可以处于以下三种状态之一：状态只可以改变一次，无论失败还是成功都有一个结果。

1. **Pending（进行中）**：初始状态，表示操作尚未完成，也未被拒绝或解决。
2. **Fulfilled（已解决）**：表示操作已成功完成，并且与 Promise 关联的值可用。
3. **Rejected（已拒绝）**：表示操作未成功完成，可能由于错误或其他原因。

promise实例对象的状态的改变：

1. resolved（）padding -> fulfilled
2. reject（） padding -> rejected
3. throws 

## promise对象的值

promise实例对象上两个属性之一：【PromiseState】。保存异步任务成功或失败的结果。

promise实例对象的结果的改变：

1. reslove
2. reject

promise工作流程：

1. **创建 Promise**：您可以通过实例化 Promise 对象来创建一个新的 Promise。Promise 构造函数接受一个执行器函数作为参数，这个函数包含异步操作的代码。
2. **异步操作**：在 Promise 的执行器函数中，您可以编写异步操作的代码，如网络请求、文件读取等。通常，异步操作会触发一个回调函数，当操作完成时，这个回调函数会被调用。
3. **处理结果**：在执行器函数中，您需要根据异步操作的结果来决定是调用 `resolve` 还是 `reject`。`resolve` 用于将 Promise 标记为成功，同时可以传递结果数据；`reject` 用于将 Promise 标记为失败，并可以传递一个错误对象。
4. **处理 Promise**：一旦 Promise 被创建，您可以使用 `.then()` 方法来定义在异步操作成功时的处理逻辑，使用 `.catch()` 方法来定义在异步操作失败时的处理逻辑。

1. **创建 Promise**：创建一个 Promise 对象，提供一个执行器函数。
2. **执行异步操作**：在执行器函数中执行异步操作，根据操作结果调用 `resolve` 或 `reject`。
3. **处理结果**：使用 `.then()` 处理异步操作成功情况，使用 `.catch()` 处理失败情况。
4. **结束处理**：使用 `.finally()` 定义最终处理逻辑，无论成功或失败都会执行。



## 第一个promis实例

```javascript
// 创建一个Promis、Ajax
const p = new Promise((resolve, reject) => {
    // 1. 创建对象
    const xhr = new XMLHttpRequest();
    // 2. 初始化
    xhr.open('GET', 'https://api.lsp.st/API/cosplay/index.php', true)
    // 3. 发送请求
    xhr.send()
    // 4. 处理响应
    xhr.onreadystatechange = function () {
        if (xhr.onreadystatechange === 4) {
            if (xhr.status >= 200 && xhr.status < 300) {
                p.resolve(xhr.response);
            } else {
                p.reject(xhr.status);
            }
        }
    }
});
// 使用 Promise 的 then 方法处理操作成功的情况
p.then(
    // 成功的回调
    value => { console.log('成功', value) },
    // 失败的回调
    reason => { console.log('失败', reason) }
)

// 封装fs模块读取文件操作
const fs = require('fs');
function mineReadFile(path) {
    return new Promise((reslove, reject) => {
        fs.readFile(path, (err, data) => {
            if (err) reject(err);
            reslove(data)
        });
    });
}
mineReadFile('./resource/context.txt')
    .then((value) => {
        console.log(value.toString());
    }, (reason) => {
        console.log(reason);
    })
```

## util.promisify风格化promise

```javascript
// fs模块读取文件
const util = require('util'); // 引入util模块
const fs = require('fs'); // 引入fs模块
let mineReadFile = util.promisify(fs.readFile)
mineReadFile('./resource/context.txt')
    .then((value) => {
        console.log(value.toString());
    }, (reason) => {
        console.log(reason);
    })
```

## 深入使用Promise

#### Promise 构造函数: Promise (excutor) {}

> (1) executor 函数: 执行器 (resolve, reject) => {}
>
> (2) resolve 函数: 内部定义成功时我们调用的函数 value => {}
>
> (3) reject 函数: 内部定义失败时我们调用的函数 reason => {}
>
> 说明: executor 会在 Promise 内部立即`同步调用`,异步操作在执行器中执行,换话说Promise支持同步也支持异步操作

#### Promise.prototype.then 方法: (onResolved, onRejected) => {}

> (1) onResolved 函数: 成功的回调函数 (value) => {}
>
> (2) onRejected 函数: 失败的回调函数 (reason) => {}
>
> 说明: 指定用于得到成功 value 的成功回调和用于得到失败 reason 的失败回调 返回一个新的 promise 对象

#### Promise.prototype.catch 方法: (onRejected) => {}

> (1) onRejected 函数: 失败的回调函数 (reason) => {}
>
> 说明: then()的语法糖, 相当于: then(undefined, onRejected)
>
> (2) 异常穿透使用:当运行到最后,没被处理的所有异常错误都会进入这个方法的回调函数中

#### Promise.resolve 方法: (value) => {}

> (1) value: 成功的数据或 promise 对象
>
> 说明: 返回一个成功/失败的 promise 对象,直接改变promise状态
>
> ```
>   let p3 = Promise.reject(new Promise((resolve, reject) => {  resolve('OK'); }));      
>   console.log(p3);
> ```

#### Promise.reject 方法: (reason) => {}

> (1) reason: 失败的原因
>
> 说明: 返回一个失败的 promise 对象,直接改变promise状态,`代码示例同上`

#### Promise.all 方法: (promises) => {}

> promises: 包含 n 个 promise 的数组
>
> 说明: 返回一个新的 promise, 只有所有的 promise `都成功才成功`, 只要有一 个失败了就直接失败
>
> ```
>  let p1 = new Promise((resolve, reject) => { resolve('成功');  })
>     let p2 = Promise.reject('错误错误错误');
>     let p3 = Promise.resolve('也是成功')
>     const result = Promise.all([p1, p2, p3]);
>  console.log(result);
> ```

#### Promise.race 方法: (promises) => {}

> (1) promises: 包含 n 个 promise 的数组
>
> 说明: 返回一个新的 promise, `第一个完成`的 promise 的结果状态就是最终的结果状态,
>
> 如p1延时,开启了异步,内部正常是同步进行,所以`p2>p3>p1`,结果是`P2`
>
> ```
>  let p1 = new Promise((resolve, reject) => {
>      setTimeout(() => {
>        resolve('OK');
>      }, 1000);
>    })
>    let p2 = Promise.resolve('Success');
>    let p3 = Promise.resolve('Oh Yeah');
>    //调用
>    const result = Promise.race([p1, p2, p3]);
>    console.log(result);
> ```

## 手写Promise

```javascript

```

## asycn/await

------

### 说明

##### Promise异步

##### await异步转同步

1. await 可以理解为是 async wait 的简写。await 必须出现在 async 函数内部，不能单独使用。
2. await 后面可以跟任何的JS 表达式。虽然说 await 可以等很多类型的东西，但是它最主要的意图是用来等待 Promise 对象的状态被 resolved。如果await的是 promise对象会造成异步函数停止执行并且等待 promise 的解决,如果等的是正常的表达式则立即执行

##### async同步转异步

1. 方法体内部的某个表达式使用await修饰，那么这个方法体所属方法必须要用async修饰所以使用awit方法会自动升级为异步方法

------

### async函数

1. 函数的返回值为 promise 对象
2. promise 对象的结果由 async 函数执行的返回值决定

### await表达式

1. await 右侧的表达式一般为 promise 对象, 但也可以是其它的值
2. 如果表达式是 promise 对象, await 返回的是 promise 成功的值
3. 如果表达式是其它值, 直接将此值作为 await 的返回值

------

### 注意事项

1. await 必须写在 async 函数中, 但 async 函数中可以没有 await
2. 如果 await 的 promise 失败了, 就会抛出异常, 需要通过 try...catch 捕获处理

------

# Axios

## axios配置项

ps：只有**url**是必需的；如果没有指定`method`，则请求将默认使用`GET`方法。

```javascript
{
    url: '/user', // 请求的服务器地址 URL        
    method: 'GET', // 请求方式，默认值 GET
    baseURL: 'https://some-domain.com/api/', // 如果 url 不是绝对地址，则会发送请求时在 url 前方加上 baseURL
    headers: {'X-Requested-With': 'XMLHttpRequest'}, // 自定义请求头
    params: { ID: 12345 }, // 与请求一起发送的 URL 参数
    data: { firstName: 'Fred' },  // 作为请求体被发送的数据，仅适用 'PUT', 'POST', 'DELETE 和 'PATCH' 请求方法
    timeout: 1000, // 请求超时的毫秒数，如果请求时间超过 `timeout` 的值，则请求会被中断，默认值是 `0` (永不超时)，
    responseType: 'json', // 期望服务器返回的数据类型，选项包括: 'arraybuffer', 'document', 'json', 'text', 'stream'， 浏览器专属：'blob'，默认值 json
    // 允许在向服务器发送前，修改请求数据，它只能用于 'PUT', 'POST' 和 'PATCH' 这几个请求方法
    transformRequest: [function (data, headers) {   
        return data; // 对发送的 data 进行任意转换处理
    }],
    // 在传递给 then/catch 前，允许修改响应数据
    transformResponse: [function (data) {
    	return data; // 对接收的 data 进行任意转换处理
    }]
}
```

## 发送请求

### axios(*config*)

```javascript
axios({
    method: 'POST', // 请求方式
    url: '/example-url/……', // 请求地址
    // …… 其他配置 ……
})

axios({
    method: 'GET', // 请求方式，可省略不写
    url: '/example-url/……', // 请求地址
    // …… 其他配置 ……
})

```

### axios(url[, config])

```javascript
// Post请求
axios(
    '/example-url/……', // 请求地址
    {
        method: 'POST', // 请求方式
        // …… 其他配置 ……
    }
)

// Get请求
axios(
     '/example-url/……', // 请求地址
    {
        method: 'GET', // 请求方式，可省略不写
        // …… 其他配置 ……
    }
)
```

## 请求方式别名

为了方便起见，已经为所有支持的请求方法提供了别名，在使用别名方法时，`url`、`method`、`data 这些属性都不必在`**config**`中指定

1. `axios.request(config*)`
2. `axios.get(url[, config])`
3. `axios.delete(url[, config])`
4. `axios.head(url[, config])`
5. `axios.options(url[, config])`
6. `axios.post(url[, data[, config]])`
7. `axios.put(url[, data[, config]])`
8. `axios.patch**(url[, data[, config]])`

## 自定义axios实例请求

`axios.create([config])`：调用`create`函数传入自定义配置，来创建自定义`axios`实例

```javascript
import axios from 'axios'

const request = axios.create({
    baseURL: 'https://some-domain.com/api/',
    timeout: 1000,
    headers: {'X-Custom-Header': 'foobar'}
})

export default request

//=========== 使用自定义请求实例 ============
import request from '@/request/axiosInstance.js'
request({
    method: 'POST', // 请求方式
    url: '/example-url/……', // 请求地址
    // …… 其他配置 ……
})

request('/example-url/……', // 请求地址
    {
        method: 'POST', // 请求方式
        // …… 其他配置 ……
    }
)

request.post(
    '/example-url/……', // 请求地址
    { /* 请求体中的参数 */ },
    {/* …… 其他配置 …… */}
)
```

------

## 响应数据

发送请求后通过`.then(*response* => {})`来获取服务器响应的数据

`response`响应式结构：

1. `data`：服务器提供的响应【最重要】
2. `status`：来自服务器响应的 HTTP 状态码，成功为`200`，请求地址不存在为`404`，服务器异常为`500`，请求方式错误为`405`……
3. `statusText`：来自服务器响应的 HTTP 状态信息
4. `headers`：服务器响应头
5. `config`： 请求的配置信息
6. `request`：生成此响应的请求，在`node.js`中它是最后一个`ClientRequest`实例，在浏览器中则是`XMLHttpRequest`实例

```javascript
// ====== 普通请求 ====
axios({
    method: 'GET',
    url: '/example-url/……'
    // …… 其他配置 ……
}).then(response => {
    console.log(response.data) // 获取服务器传递来的数据
})

// ====== 使用别名 ====
axios.get(
    url: '/example-url/……'
    { /* …… 其他配置 …… */ }
)
.then(response => {
    console.log(response.data) // 获取服务器传递来的数据
})
```

## 请求错误处理

发送请求后，使用`.catch ( error=> {} )`来处理此次请求异常，请求成功发出且服务器也响应了状态码，但状态代码超出了`2xx`的范围

```javascript
axios({
    method: 'GET', // 请求方式
    url: '/example-url/……', // 请求地址
}).catch(error => {
    console.log('请求失败！')
})
```

------

# vue跨域

1. 跨域：指的是浏览器不能执行其他网站的脚本；它是由浏览器的同源策略造成的，是浏览器对`javascript`施加的安全限制
2. 同源策略：是指协议，域名，端口都要相同，其中有一个不同都会产生跨域
3. 浏览器为了安全问题一般都限制了跨域访问，也就是不允许跨域请求资源，如果未处理跨域访问则会在请求时控制台出现`**Access-Control-Allow-Origin**……`的报错信息
4. 如何处理跨域问题，可在`vite`项目的`vite.config.js`文件中添加`proxy`代理

```javascript
import { fileURLToPath, URL } from 'node:url'
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
    plugins: [vue()],
    resolve: {
        alias: {
            '@': fileURLToPath(new URL('./src', import.meta.url))
        }
    },
    // 服务
    server: {
        // 代理
        proxy: {
            '/apisapce': {
                target: 'https://eolink.o.apispace.com/', // 代理后台服务器地址
                changeOrigin: true, //允许跨域               
                rewrite:(path) => path.replace(/^\/apisapce/,'') // 将请求地址中的 /apisapce 替换成空
            }
        }
    }
})
```



------

# node.js

## 1-express.js

```javascript
// 1. 引入express
const express = require('express');
// 2. 创建应用对象
const app = express();
// 3. 创建路由规则 request | response : 请求|响应报文的封装

// cors解决跨域问题
const cors = require("cors");
app.use(cors()); //使用cors中间件


app.get('/', (requset, response) => {
    response.send('HELLO WORLD!')
})
app.post('/serve', (requset, response, next) => {
    response.setHeader('Access-Control-Allow-Origin', '*'); // 允许跨域
    response.setHeader('Access-Control-Allow-Headers', '*'); // 允许自定义请求头
    response.setHeader('Access-Control-Max-Age', '600');
    //10秒:options请求缓存
    // response.header() 
    // 设置响应体
    response.send('HELLO AJAX')
})
app.get('/serve', (requset, response, next) => {
    response.setHeader('Access-Control-Allow-Origin', '*'); // 允许跨域
    response.send('HELLO AJAX'); // 设置响应体
})
// 响应json数据
app.get('/jsonserve', (requset, response, next) => {
    response.setHeader('Access-Control-Allow-Origin', '*'); // 允许跨域
    // 设置响应体
    const data = {
        name: 'YZLing',
        hot: '412'
    }
    // 响应一个json数据
    let jsonData = JSON.stringify(data);
    response.send(jsonData)
})
// 延时响应
app.get('/delay', (requset, response, next) => {
    response.setHeader('Access-Control-Allow-Origin', '*'); // 允许跨域
    setTimeout(() => {
        response.send('TIME OUT'); // 设置响应体
    }, 3000)

})
// JQUERY
app.all('/jquery', (requset, response, next) => {
    response.setHeader('Access-Control-Allow-Origin', '*'); // 允许跨域
    const data = {
        name: 'YZling',
        hot:412
    }
    response.send(JSON.stringify(data)); // 设置响应体
})
// AXIOS
app.all('/axios', (requset, response, next) => {
    response.setHeader('Access-Control-Allow-Origin', '*'); // 允许跨域
    const data = {
        name: 'YZling',
        hot:412
    }
    response.send(JSON.stringify(data)); // 设置响应体
})

// AXIOS
app.all('/fetch', (requset, response, next) => {
    response.setHeader('Access-Control-Allow-Origin', '*'); // 允许跨域
    const data = {
        name: 'fetch',
    }
    response.send(JSON.stringify(data)); // 设置响应体
})

// 4. 监听端口启动服务 http://localhost:8000/
app.listen(8000, () => {
    console.log("serve is runing,8000端口监听开始>>>");
})
```

## 8-express.js

```javascript
// 1. 引入express
const express = require('express');
// 2. 创建应用对象
const app = express();
// 3. 创建路由规则 request | response : 请求|响应报文的封装

// cors解决跨域问题
const cors = require("cors");
app.use(cors()); //使用cors中间件

app.get('/', (requset, response) => {
    response.sendFile(__dirname + '/8-跨域.html')
})
app.get('/data', (requset, response) => {
    response.send('用户数据')
})
// 
app.all('/cros', (requset, response) => {
    response.send('cros-用户数据')
})


app.get('/jsonp', (requset, response) => {
    // response.send('console.log("hello jsonp")')
    const data = {
        name: 'YZling',
        hot:412
    }
    let jsonDdata = JSON.stringify(data);
    response.end(`handle(${jsonDdata})`)
})
// 4. 监听端口启动服务 http://localhost:9000/
app.listen(8000, () => {
    console.log("serve is runing,8000端口监听开始>>>");
})
```



# 请求实例

## 智能天气实况

### 选项式

```html
<script>
import axios from 'axios'
  
export default {
    data: () => ({
        cityCode: null, // 城市编号
        cityWeather: null // 城市天气状况
    }),
    methods: {
        // 获取城市天气状况
        getCityWeather() {
            // 发送请求
            axios({
                url: '/apisapce/456456/weather/v001/now', // 请求地址（已处理跨域）
                method: 'GET', // 请求方式
                // 请求头
                headers: {
                    'X-APISpace-Token': 'p6cz2g80pcplxtituz1mz3ccgkgaaxl6',
                    'Authorization-Type': 'apikey'
                },
                // 请求 URL 参数
                params: { 'areacode': this.cityCode }
            }).then(response => {
                const responseData = response.data // 获取服务器响应的数据
                console.log(responseData)
                if (responseData.status === 0) {
                    // 请求成功
                    this.cityWeather = responseData.result
                } else {
                    // 请求失败
                    alert(responseData.message)
                }
            }).catch(error => {
                alert('服务器异常')
            })
        }
    }
}
</script>

<template>
    城市编号：<input type="text" v-model="cityCode">
    <button @click="getCityWeather">获取城市天气状况</button>

    <hr>

    <ul v-if="cityWeather">
        <li>城市：{{ cityWeather.location.path }}</li>
        <li>天气现象：{{ cityWeather.realtime.text }}</li>
        <li>气温：{{ cityWeather.realtime.temp }}</li>
        <li>风速：{{ cityWeather.realtime.wind_speed }}</li>
        <li>风向：{{ cityWeather.realtime.wind_dir }}</li>
        <li>数据更新时间：{{ cityWeather.last_update }}</li>
    </ul>
</template>
```

### 组合式

```html
<template>
    城市编号：<input type="text" v-model="cityCode" placeholder="请输入城市ID">
    <button @click="getCityWeather">获取城市天气状况</button>
    <hr>
    <p>惠阳ID：101280303</p>
    <p>北京ID：101010100</p>
    <hr>
    <ul v-if="cityWeather">
        <li>城市：{{ cityWeather.location.path }}</li>
        <li>天气现象：{{ cityWeather.realtime.text }}</li>
        <li>气温：{{ cityWeather.realtime.temp }}</li>
        <li>风速：{{ cityWeather.realtime.wind_speed }}</li>
        <li>风向：{{ cityWeather.realtime.wind_dir }}</li>
        <li>数据更新时间：{{ cityWeather.last_update }}</li>
    </ul>

</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

// 城市编号:101280303(惠阳)
const cityCode = ref(null)

// 城市天气状况
const cityWeather = ref(null)

// 获取城市天气状况
function getCityWeather() {
    // 发送请求
    axios({
        url: `/apisapce/456456/weather/v001/now`, // 请求地址（已处理跨域）
        method: 'GET', // 请求方式
        // 请求头
        headers: {
            'X-APISpace-Token': 'jn291dt0raxtgutp64oanm6t30i5qquh',
            'Authorization-Type': 'apikey'
        },
        // 请求 URL 参数
        params: { 'areacode': cityCode.value }
    }).then(response => {
        
        const responseData = response.data // 获取服务器响应的数据
        console.log('response.data',response.data)
        if (responseData.status === 0) {
            // 请求成功
            cityWeather.value = responseData.result
        } else {
            // 请求失败
            alert(responseData.message)
        }
    })
    .catch(error => {
        console.log(error)
        alert('服务器异常')
    })
}
</script>
```

## 手机归属地查询

### 选项式

```html
<script>
import axios from 'axios'
export default {
    data: () => ({
        phone: null, // 手机号
        phoneInfo: null, // 手机号信息
    }),
    methods: {
      	// 查询手机归属地
        getPhoneInfo() {
            // 发送请求
            axios.post(
                '/apisapce/teladress/teladress', // 请求地址（已处理跨域）
                { 'mobile': this.phone }, // 请求体中的参数
              	// 配置项
                {
                    headers: {
                        'X-APISpace-Token': 'p6cz2g80pcplxtituz1mz3ccgkgaaxl6',
                        'Authorization-Type': 'apikey',
                        'Content-Type': 'application/x-www-form-urlencoded'
                    }
                }
            ).then(response => {
                const responseData = response.data // 获取服务器响应的数据
                console.log(responseData)
                if (responseData.code === '200000') {
                    // 请求成功
                    this.phoneInfo = responseData.data
                } else {
                    // 请求失败
                    alert(responseData.message)
                }
            }).catch(error => {
                alert('服务器异常')
            })
        }
    }
}
</script>

<template>
    手机号：<input type="number" v-model="phone">
    <button @click="getPhoneInfo">获取手机归属地</button>

    <hr>

    <ul v-if="phoneInfo">
        <li>手机号：{{ phoneInfo.mobile }}</li>
        <li>运营商：{{ phoneInfo.isp }}</li>
        <li>运营商：{{ phoneInfo.isp }}</li>
        <li>省份：{{ phoneInfo.province }}</li>
        <li>城市：{{ phoneInfo.city }}</li>
    </ul>
</template>
```

### 组合式

```html
<script setup>
import axios from 'axios'
import { ref } from 'vue'
const phone = ref(null) // 手机号
const phoneInfo = ref(null) // 手机号信息44
// 查询手机归属地
function getPhoneInfo() {
    // 发送请求
    axios.post(
        '/apisapce/teladress/teladress', // 请求地址（已处理跨域）
        { 'mobile': phone.value }, // 请求体中的参数
      	// 配置项
        {           
            headers: {
                'X-APISpace-Token': 'p6cz2g80pcplxtituz1mz3ccgkgaaxl6',
                'Authorization-Type': 'apikey',
                'Content-Type': 'application/x-www-form-urlencoded'
            }
        }
    ).then(response => {
        const responseData = response.data // 获取服务器响应的数据
        console.log(responseData)
        if (responseData.code === '200000') {
            // 请求成功
            phoneInfo.value = responseData.data
        } else {
            // 请求失败
            alert(responseData.message)
        }
    }).catch(error => {
        alert('服务器异常')
    })
}
</script>


<template>
    手机号：<input type="number" v-model="phone">
    <button @click="getPhoneInfo">获取手机归属地</button>

    <hr>

    <ul v-if="phoneInfo">
        <li>手机号：{{ phoneInfo.mobile }}</li>
        <li>运营商：{{ phoneInfo.isp }}</li>
        <li>运营商：{{ phoneInfo.isp }}</li>
        <li>省份：{{ phoneInfo.province }}</li>
        <li>城市：{{ phoneInfo.city }}</li>
    </ul>
</template>
```

# webpack

基本使用

```javascript
// 全局安装
npm install webpack webpack-cli --global
// 本地安装
npm install webpack webpack-cli --save-dev
// npm内置的npx在当前目录查找到本地的webpack
npx webpack
// 指定入口文件
npx webpack --entry ./src/index.js --mode production
```

------

# git

***工作流：***：你的本地仓库由 git 维护的三棵“树”组成。
第一个是你的 **`工作目录`**，它持有实际文件；
第二个是 **`暂存区（Index）`**，它像个缓存区域，临时保存你的改动；
最后是 **`HEAD`**，它指向你最后一次提交的结果。

## 基本操作

```javascript
// 初始化仓库
git init
// 克隆仓库（检出仓库）
git clone /path/to/repository  //（克隆本地仓库）
git clone username@host:/path/to/repository  //（克隆远程仓库）
// 添加修改的文件到暂缓区
git add <fliename>
git add *
// 提交（至HEAD区）
git commit -m "描述"
// 推送更改（到远程仓库master分支）
git push origin master
git remote add origin <server>  //（没有克隆现有仓库，并欲将你的仓库连接到某个远程服务器）
```

## 分支

```javascript
/*
	什么是分支？ 分支是用来将特性开发绝缘开来的。在你创建仓库的时候，master 是“默认的”分支。在其他分支上进行开发，完成后再将它们合并到主分支上。
*/
// 创建一个“feature_x”分支并切换进去
git checkout -b feature_x
// 切换回主分支 
git checkout master
// 删除“feature_x”分支
git branch -d feature_x
// 分支推送到远端仓库，不然该分支就是 不为他人所见的
git push origin <branch>
```

## 更新与合并

```javascript
// 在合并改动之前，你可以使用如下命令预览差异
git diff <source_branch> <target_branch>
// 更新本地仓库至最新改动，以在你的工作目录中 获取（fetch） 并 合并（merge） 远端的改动
git pull
// 合并其他分支到你的当前分支
git merge <branch>
// 以上两种方法git都会尝试去自动合并改动，但可能需要手动修改文件，解决冲突（conflicts）
// 改完之后，将它们标记为合并成功
git add <filename>
```

## 其它操作

### 标签

```javascript
// 创建一个叫做 1.0.0 的标签，1b2e1d63ff 是你想要标记的提交 ID 的前 10 位字符
// 可以使用少一点的提交 ID 前几位，只要它的指向具有唯一性。
git tag 1.0.0 1b2e1d63ff
// 获取提交 ID
git log
```

### 替换本地改动

```javascript
// 会使用 HEAD 中的最新内容替换掉你的工作目录中的文件。
// 已添加到暂存区的改动以及新文件都不会受到影响。
git checkout -- <filename>
// 假如你想丢弃你在本地的所有改动与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它
git fetch origin
git reset --hard origin/master
```















------

# ......