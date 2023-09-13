# 代码风格

## 组合式API

## 选项式API

------

## 命名风格

### 组件命名

**PascalCase（帕斯卡命名法）：** 推荐使用帕斯卡命名法，其中每个单词的首字母大写，没有分隔符。这种风格在Vue社区中较为普遍，能够清楚地区分组件和普通HTML标签。

### props 命名风格

1. **camelCase（驼峰命名法）：** 在 Vue 2 中，Props 的命名通常采用 camelCase 风格，即第一个单词的首字母小写，后面的单词首字母大写。

   示例：`userName`, `postTitle`, `commentCount`

2. **kebab-case（短横线分隔）：** 在 Vue 3 的 Composition API 中，Props 的命名可以采用 kebab-case 风格，即单词之间使用短横线分隔。

   示例：`user-name`, `post-title`, `comment-count`

3. **Props 名称前缀：** 可以为 Props 名称添加一个前缀，以明确指示它是一个 Props。这对于在模板中区分 Props 和普通的数据变量很有帮助。

   示例：`propUserName`, `propPostTitle`, `propCommentCount`

如果一个 prop 的名字很长，应使用 camelCase 形式

### 路由命名：

1. name命名：使用小驼峰、短横线分隔
2. path命名：使用小驼峰

------

# 创建vue3工程

## 1. 使用vue-cli创建

1. 查看，vue-cli要版本4.5.0及以上。（vue -V）

2. 创建方法一

   ```javascript
   // 桌面
   cd C:\Users\ASUS\Desktop
   // 
   vue create xxx
   // 拉取 2.x 模板 (旧版本)
   npm install -g @vue/cli-init //可以全局安装一个桥接工具
   vue init webpack my-project  // 不推荐
   vue ui
   ```
   
   
   
3. vue creat xxx）（ cd C:\Users\ASUS\Desktop ）

   

## 2. 官网推荐

vue-cli处于维护阶段、推荐使用vue-create创建。`npm create vue@latest`这一指令将会安装并执行 [create-vue](https://github.com/vuejs/create-vue)，它是 Vue 官方的项目脚手架工具。你将会看到一些诸如 Project name、 TypeScript 和测试支持之类的可选功能提示。

```
npm create vue@latest
npm create vue@legacy //如果你需要支持 IE11，你可以创建一个 Vue 2 项目
```

请注意，标记名称（或）不得省略，否则可能会解析为包的缓存和过时版本。`@latest``@legacy``npm`

## 3.使用vite创建

```
pnpm create vite
```

pnpm i eslint -D

npx eslint --init

pnpm install -D  eslint-plugin-prettier prettier eslint-config-prettier

```javascript
//======== .eslintrc.cjs 配置文件

module.exports = {
    "env": {
        "browser": true,
        "es2021": true,
        "node": true,
        "jest": true
    },
    "extends": [
        "eslint:recommended",
        "plugin:@typescript-eslint/recommended",
        "plugin:vue/vue3-essential"
    ],
    "overrides": [
        {
            "env": {
                "node": true
            },
            "files": [
                ".eslintrc.{js,cjs}"
            ],
            "parserOptions": {
                "sourceType": "script"
            }
        }
    ],
    "parserOptions": {
        "ecmaVersion": "latest",
        "parser": "@typescript-eslint/parser",
        "sourceType": "module",
    },
    "plugins": [
        "@typescript-eslint",
        "vue"
    ],
    "rules": {
        // eslint
        'no-var': 'error',  // 禁止var
        'no-multiple-empty-lines': ['warn', { max: 1 }], // 不允许多个空行
        'no-useless-escape': 'off', // 关闭禁止不必要的转义字符

        // typeScript
        '@typescript-eslint/no-unused-vars': 'error',
        '@typescript-eslint/no-explicit-any': 'off',
        '@typescript-eslint/semi': 'off',

        // eslint-plugin-vue
        'vue/multi-word-component-names': 'off',  // 关闭名称只能-连接
        'vue/no-mutating-props': 'off',  // 关闭组件prop不能改变
        'vue/attribute-hyphenation': 'off', // 关闭对模板中自定义组件强制执行属性命名样式
    }
}


// ============== .prettierrc.json 配置文件
{
    "singleQuote": true,
    "semi": false,
    "bracketSpacing": true,
    "htmlWhitespaceSensitivity": "ignore",
    "endOfLine": "auto",
    "trailingComma": "all",
    "tabWidth": 2
}
```

```javascript
//============== .eslintignore 忽略文件

dist
node_modules

// ============ .prettierignore 忽略文件

/dist/*
/html/*
.local
/node_modules/**
**/*.svg
**/*.sh
/public/*
```



------

# setup（选项式）

1. vue3新配置项，值为一个函数。
2. **是所有composition API的容器。**
3. **组件中的数据、方法、计算属性等等都要配置到steup。**
4. 有两种返回值。（1）返回一个对象，模板中可以直接使用。（2）返回一个可自定义的渲染函数。

```html
<template>
  <button @click="singASong">sing</button>
</template>

<script>
import { h } from 'vue'
export default {
  name: 'HelloWorld',
  setup() {
    // 数据
    let song = '小飞飞'
    // 函数
    function singASong() {
      alert(`sing a ${song}`)
    }
    // 返回一个对象
    return {
      song,
      singASong
    }
    // 返回一个渲染函数
    // return () => h('h1', '标签内容')
  }
}
</script>

<style></style>
```

------

## vue2和vue3的数据交互

1. vue2的配置可以读取vue3的setup中的数据和方法。
2. vue3的setup中不能读取vue2的数据和方法。
3. 重名数据方法，setup优先。
4. setup不能是一个async函数，因为返回值不再是return对象，而是promise，模板看不到return的数据。（使用后面的Suspense组件可以实现）

------

## setup执行时期

- 在beforeCreate之前执行一次，this是undefined，。

## setup的参数

- props：值为对象，包含：组件外部传递过来，且组件内部声明接收了的属性。
- context：上下文对象
  - attrs: 值为对象，包含：组件外部传递过来，但没有在props配置中声明的属性, 相当于 ```this.$attrs```。
  - slots: 收到的插槽内容, 相当于 ```this.$slots```。
  - emit: 分发自定义事件的函数, 相当于 ```this.$emit```。

#### HelloWorld.vue

```html
<template>
  <div class="helloworld">
    <h2>{{ data.helloInfo.msg }}</h2>
    <button @click="allWithMe">都是我的</button>
  </div>
</template>

<script>
import { reactive } from 'vue'
export default {
  name: 'HelloWorld',
  props: ['singerName', 'songName'],
  emits: ['giveMe'],
  setup(props, context) {
    console.log('--setup--', props)
    console.log('--setup-attrs--', context.attrs)// 相当于vue2中的$attrs
    console.log('--setup-emit--', context.emit)// 触发自定义事件
    console.log('--setup-slots--', context.slots)// 触发自定义事件

    // 数据源
    let data = reactive({
      helloInfo: {
        msg: 'Hllo World!',
        myValue: '金色传说'
      }
    })

    //方法(1) 测试context的emit。
    function allWithMe() {
      context.emit('giveMe', data.helloInfo.myValue)
    }

    // 返回一个对象
    return {
      data, allWithMe
    }
  }
}
</script>

<style scoped>
.helloworld {
  background-color: #ee8888;
  padding: 20px;
}
</style>
```

#### App.vue

```html
<template>
  <div class="app">
    <h1>App component</h1>
    <HelloWorld singerName="张杰" songName="逆战" @giveMe="giveMeAValue">
      <!-- 填坑 -->
      <template v-slot:hh>
        <div>
          <h3>什么玩意？</h3>
        </div>
      </template>
    </HelloWorld>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
export default {
  name: 'App',
  components: { HelloWorld },

  setup() {
    function giveMeAValue(value) {
      alert(`给我拿来[${value}]`)
    }

    return { giveMeAValue }
  }
}
</script>
<style>
.app {
  background-color: #00eeee;
  padding: 20px;
}
</style>
```

------

### setup （组合式）

1. 单文件结构中通常写法：`<script setup> <script>`

2. 这种写法的导入和顶层变量、函数都能在模板中直接使用

3. 通过组合式API，我们可以使用导入的API函数来描述组件的逻辑结构：improt { ref,reactive } from 'vue'

   ​				**一个简单的组合式API实现：响应式数据**

```html
<template>
  <h1>{{ title }}</h1>
  <h1>{{ obj.name }}</h1>
  <h1>{{ obj.hot }}</h1>
  <button @click="changeHot">hot++</button>
</template>

<script setup>
name: 'App';
// 导入组合式API
import { ref, reactive } from 'vue';
// 组合式API的使用
// 响应式变量声明
let title = ref('Hello World!');
const obj = reactive({
  name: 'YZling',
  hot: 0
});
// 普通变量声明
let hot = 412;
//
function changeHot() {
  obj.hot = obj.hot + 1;
}

</script>

<style scoped></style>
```



------

# ref函数

作用：vue3新增。**ref函数定义一个响应式数据。**

ref**处理基本数据类型**

1. 将数据加工为一个RefImpl的实例对象。
2. **RefImpl**（reference implement）： 引用实现的实例对象（简称ref引用对象）。
3. 同样借助Object.defineProerty的getter和setter实现响应式。
4. 使用ref定义的对象数据类型，编写代码需要.value，模板引用不需要。

**ref处理对象类型**

1. ref求助reactive将第一层数据变为Proxy实例对象实现。

```html
<template>
  <div class="app">
    <h3>歌手：{{ singer }}</h3>
    <h3>歌词：{{ lyrics }}</h3>
    <h3>歌曲：{{ song.one }}</h3>
    <h3>歌曲：{{ song.tow }}</h3>
    <h3>歌曲：{{ song.three.four }}</h3>
    <button @click="changeSong">修改歌曲</button>
    <button @click="changeLyrics">修改歌词</button>
  </div>
</template>

<script>
import { reactive, ref } from 'vue'
export default {
  name: 'App',
  setup() {
    // ref处理基本数据类型
    let singer = ref('邓紫棋')
    let lyrics = ref('阳光下的泡沫,是彩色的')
    // ref处理对象类型
    let song = ref({
      one: '泡沫',
      tow: '再见',
      three: {
        four: '倒数'
      }
    })

    function changeLyrics() { lyrics.value = '就像被骗的我,是幸福的' }
    function changeSong() { song.value.three.four = '句号' }

    return {
      singer, lyrics, changeLyrics, changeSong, song
    }
  }
}
</script>

<style></style>

```

## ref（组合式）

```html
<template>
    <!-- 模板引用ref数据：基本数据类型、对象类型 -->
    <h3>歌手：{{ singer }}</h3>
    <h3>歌词：{{ lyrics }}</h3>
    <h3>歌曲1：{{ song.one }}</h3>
    <h3>歌曲2：{{ song.tow }}</h3>
    <h3>歌曲3：{{ song.three.four }}</h3>
    <button @click="changeSong">修改歌曲3</button>
    <button @click="changeLyrics">修改歌词</button>
</template>

<script setup>
// 导入组合式API
import { ref } from 'vue';
name: 'refTest'
// ref处理基本数据类型
let singer = ref('邓紫棋')
let lyrics = ref('阳光下的泡沫,是彩色的')
// ref处理对象类型
let song = ref({
    one: '泡沫',
    tow: '再见',
    three: {
        four: '倒数'
    }
})
// 修改ref定义的响应式需要.value
function changeLyrics() { lyrics.value = '就像被骗的我,是幸福的' }
function changeSong() { song.value.three.four = '句号' }
</script>
<style></style>
```



------

# reactive函数

作用：**reactive函数定义一个对象类型的响应式数据。**借助es6的Proxy实现。

1. ref求助reactive将第一层数据变为Proxy实例对象实现。
2. 语法：const 代理对象 = reacive( 源对象 )，返回一个代理对象（Proxy对象）。
3. reactive定义的响应式是深层次的。
4. 基于es6的Proxy实现，通过代理对象操作源对象。

```html
<template>
  <div class="app">
    <h3>歌手：{{ singerInfo.singer }}</h3>
    <h3>歌词：{{ singerInfo.lyrics }}</h3>
    <h3>歌曲：{{ singerInfo.song.one }}</h3>
    <h3>歌曲：{{ singerInfo.song.tow }}</h3>
    <h3>歌曲：{{ singerInfo.song.three.four }}</h3>
    <h3>{{ singerInfo.other }}</h3>
    <button @click="changeSong">修改歌曲</button>
    <button @click="changeLyrics">修改歌词</button>
  </div>
</template>

<script>
import { reactive } from 'vue'
export default {
  name: 'App',
  setup() {
    let singerInfo = reactive({
      singer: '邓紫棋',
      lyrics: '阳光下的泡沫,是彩色的',
      song: {
        one: '泡沫',
        tow: '再见',
        three: {
          four: '倒数'
        }
      },
      other: ['光年之外', '偶尔', '攀登']
    })

    function changeLyrics() { singerInfo.lyrics = '就像被骗的我,是幸福的' }
    function changeSong() {
      singerInfo.song.three.four = '句号'
      singerInfo.other[0] = '天空没有极限'
    }

    return {
      changeLyrics, changeSong, singerInfo
    }
  }
}
</script>

<style></style>

```

## reactive（组合式）

```html
<template>
    <!-- 模板引用reactive数据：对象类型 -->
    <h3>歌手：{{ singerInfo.singer }}</h3>
    <h3>歌词：{{ singerInfo.lyrics }}</h3>
    <h3>歌曲：{{ singerInfo.song.one }}</h3>
    <h3>歌曲：{{ singerInfo.song.tow }}</h3>
    <h3>歌曲：{{ singerInfo.song.three.four }}</h3>
    <h3>{{ singerInfo.other }}</h3>
    <button @click="changeSong">修改歌曲</button>
    <button @click="changeLyrics">修改歌词</button>
</template>

<script setup>
// 1. 导入组合式API
import { reactive } from 'vue'
name: 'Reactive';
// 2. reactive只能处理对象类型（对象、数组、Map、Set）对String、Number、Boolean无效
let singerInfo = reactive({
    singer: '邓紫棋',
    lyrics: '阳光下的泡沫,是彩色的',
    song: {
        one: '泡沫',
        tow: '再见',
        three: {
            four: '倒数'
        }
    },
    other: ['光年之外', '偶尔', '攀登']
})
// 3. 修改ref定义的响应式不需要.value
function changeLyrics() { singerInfo.lyrics = '就像被骗的我,是幸福的' }
function changeSong() {
    singerInfo.song.three.four = '句号'
    singerInfo.other[0] = '天空没有极限'
}
</script>
<style scope></style>
```



# reactive对比ref

-  从定义数据角度对比：
   -  ref用来定义：<strong style="color:#DD5145">基本类型数据</strong>。
   -  reactive用来定义：<strong style="color:#DD5145">对象（或数组）类型数据</strong>。
   -  备注：ref也可以用来定义<strong style="color:#DD5145">对象（或数组）类型数据</strong>, 它内部会自动通过```reactive```转为<strong style="color:#DD5145">代理对象</strong>。
-  从原理角度对比：
   -  ref通过``Object.defineProperty()``的```get```与```set```来实现响应式（数据劫持）。
   -  reactive通过使用<strong style="color:#DD5145">Proxy</strong>来实现响应式（数据劫持）, 并通过<strong style="color:#DD5145">Reflect</strong>操作<strong style="color:orange">源对象</strong>内部的数据。
-  从使用角度对比：
   -  ref定义的数据：操作数据<strong style="color:#DD5145">需要</strong>```.value```，读取数据时模板中直接读取<strong style="color:#DD5145">不需要</strong>```.value```。
   -  reactive定义的数据：操作数据与读取数据：<strong style="color:#DD5145">均不需要</strong>```.value```。

------

# vue2响应式问题

存在一个问题，新增属性，修改属性，但页面没有响应式。借助set或$set解决。

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

# vue3响应式原理

## 模拟vue2响应式

```javascript
// 源数据
let singer ={
    name:'邓紫棋',
    hot:666
}
// 【=== 模拟vue2响应式原理 ===】
let s = {}
// （1）模拟代理singer的name (2) 模拟代理singer的hot同理
Object.defineProperty(s, 'name', {
    configurable:true,//可配置的
    get(){
        console.log('【模拟读取】读取了')
        return singer.name
    },
    set(value){
        console.log('【模拟更新】vue更新界面了')
        singer.name = value
    }
})
// 【模拟操作】
// p.name //读取已有属性 true
// p.name = 'G.E.M.' //修改已有属性 true
// delete s.name //删除已有属性 false 需要配置configurable:true //可配置的
// singer.sing = '句号' //增加新属性 false
```

------

## 模拟vue3响应式

```javascript
// 【=== 模拟vue3响应式 ===】
//Proxy（代理）用 s 映射数据源 singer
//get和set与vue2的不同
//没有get\set\deleteProperty直接模拟操作，会直接修改数据源没意义。

// 源数据
let singer ={
    name:'邓紫棋',
    hot:666
}

const s = new Proxy(singer,{
    get(target,propName){
        console.log(`【模拟读取】读取了s上的${propName}属性`)
        // return target[propName]
        return Reflect.get(target,propName) // 看window.Reflec来
    },
    // 修改s上属性或追加新属性时 都调用，奠定了增删查改实现的一部分
    set(target,propName,value){
        console.log(`【模拟更新】修改了s上的${propName}属性，【界面更新】`)
        // target[propName]=vlaue
        return Reflect.set(target,propName,value) // 看window.Reflec来
    },
    deleteProperty(target,propName){
        console.log(`【模拟删除】删除了s上的${propName}属性，【界面更新】`)
        // return delete target[propName]
        return Reflect.defleteProperty(target,propName) // 看window.Reflec来
    }
}) 
//【模拟操作】
// singer //输出{......} 
// s //输出Proxy{......}
// s.name // true 读取属性
// s.name = "G.E.M." // true 修改已有属性
// s.singer = '句号' // true 添加新属性
// delete s.hot // true 删除已有属性
```

------

## window.Reflec



```javascript
// 【=== window.Reflec ===】
let obj ={a:1,b:2}
// 无法实现 Object.defineProperty
/*Object.defineProperty(obj, 'a', { get(){ return 3 } })
Object.defineProperty(obj, 'a', { get(){ return 4 } })*/
// 可以实现 Reflec.defineProperty
const x1 = Reflec.defineProperty(obj, 'a', { get(){ return 3 } })
const x2 = Reflec.defineProperty(obj, 'a', { get(){ return 4 } })
if(x1){console.log('代码运行成功：减少trycatch，更友好，代码更健壮')}
// window.Reflect（window上的Reflect（反射）,ECMA规范趋势将Object上的API=>给Reflect）
// Reflect.get(obj,'a') // 读取 true
// Reflect.set(obj,'a',666) // 修改 true
// Reflect.deleteProperty(obj,'a') // 删除 true
```

------

# methods方法







------

# computed计算属性

1. **缓存机制：** 计算属性默认会对它的依赖进行缓存，只有依赖发生变化时才会重新计算。这是优化性能的重要手段，但有时你可能需要关闭这种缓存，例如，当计算属性依赖的数据可能频繁变化，但计算代价较低时。你可以使用`computed`选项的`cache`属性来控制缓存，将其设置为`false`。
2. **不要在计算属性中修改依赖的数据：** 计算属性应该是只读的，不要在计算属性中修改其依赖的数据。这会导致数据的不一致性和难以追踪的bug。如果需要修改数据，应该使用方法（Methods）或其他途径。
3. **计算属性的使用场景：** 计算属性适用于基于现有响应式数据派生出新数据的场景。如果你只需要执行一段逻辑并返回一个结果，但不需要维护派生数据的状态，那么方法（Methods）可能更合适。
4. **计算属性和侦听属性的区别：** 计算属性用于派生数据，而侦听属性（Watchers）用于监听数据的变化并执行相应的操作。如果你需要在数据变化时执行异步操作或者执行一些副作用，侦听属性更合适。
5. **计算属性的响应性：** 计算属性本身是响应式的，它会随着它所依赖的数据的变化而重新计算。这意味着你可以在模板中像访问普通属性一样使用计算属性。
6. **计算属性的getter和setter：** 在Vue 3中，计算属性的定义方式不再需要`get`和`set`，你可以直接定义一个返回值的函数作为计算属性。如果你需要为计算属性提供setter，可以使用`ref`引用并使用`value`属性来实现。
7. **计算属性的命名：** 计算属性应该具有描述性的名字，以便于理解它的用途。避免使用过于复杂的计算逻辑，以免降低代码可读性。

vue3与vue2相似

```html
<template>
  <div class="helloworld">
    SingerName：<input type="text" v-model="data.singerName">
    SongName：<input type="text" v-model="data.songName">
    SongInfo：<input type="text" v-model="data.songInfo">
    <span>SongInfo：{{ data.songInfo }}</span>
  </div>
</template>

<script>
import { reactive, computed } from 'vue'
export default {
  name: 'HelloWorld',
  setup() {
    // 数据源
    let data = reactive({
      singerName: '111',
      songName: '222'
    })
    // 计算属性----简写
    /* data.songInfo = computed(() => {
      return data.singerName + '--' + data.songName
    }) */
    // 计算属性----完整
    data.songInfo = computed({
      get() { return data.singerName + '-' + data.songName },
      set(value) {
        const infoArr = value.split('-')
        data.singerName = infoArr[0]
        data.songName = infoArr[1]
      }
    })
    // 返回一个对象
    return {data}
  }
}
</script>

<style scoped>
.helloworld {
  background-color: #ee8888;
  padding: 20px;
}
</style>
```

## computed（组合式）

```html
<template>
    SingerName：<input type="text" v-model="data.singerName"><br>
    SongName：<input type="text" v-model="data.songName"><br>
    SongInfo：<input type="text" v-model="data.songInfo"><br>
    <span>SongInfo：{{ data.songInfo }}</span>
</template>

<script setup>
// 1. 引入API
import { reactive, computed } from 'vue';
name: 'ComputedTest';
// 数据源
let data = reactive({
    singerName: 'YZling',
    songName: '石墨',
})
// 计算属性----简写
// data.songInfo = computed(() => {
//     return data.singerName + '--' + data.songName
// })
// 计算属性----完整
data.songInfo = computed({
    get() { return data.singerName + '-' + data.songName },
    set(value) {
        const infoArr = value.split('-')
        data.singerName = infoArr[0]
        data.songName = infoArr[1]
    }
})
</script>

<style></style>
```



------

# watch监视属性

`watch`函数的参数如下：

- `source`: 要监视的数据源，可以是一个响应式对象、函数、`ref`等。
- `callback`: 在数据变化时执行的回调函数，函数的参数为新值和旧值。
- options（可选）: 一个选项对象，可以包含以下属性：
  - `immediate`: 是否在开始时立即执行一次回调，默认为`false`。
  - `deep`: 是否深度监视嵌套对象或数组的变化，默认为`false`。
- 监视数据结构越复杂，监视越消耗性能。

## 监视ref和reactive定义的数据

```html
<template>
    <!-- 监视ref定义数据 -->
    <button @click="hot += 1">当前hot：{{ hot }}</button>
    <button @click="say += '!'">当前say：{{ say }}</button>
    <br>
    <!-- 监视reactive定义数据 -->
    <button @click="SongInfo.singer += '!'">SongInfo.singer歌手：{{ SongInfo.singer }}</button>
    <button @click="SongInfo.song += '~'">SongInfo.song歌曲：{{ SongInfo.song }}</button>
    <br>
    <button @click="wtchDeepReactiveData">深层次数据：{{ SongInfo.one.tow.three }}</button>
    <button @click="wtchDeepReactiveData">监视某一个属性：{{ SongInfo.one.tow.three }}</button>
    <br>
    <button @click="unWactch">取消监视ref数据</button>
</template>

<script setup>
// 1. 导入组合式API
import { ref, reactive, watch } from 'vue';
naem: 'WatchTest'
// 数据源
let hot = ref(1)
let say = ref('hello')
let SongInfo = reactive({
    singer: '张杰',
    song: '逆战',
    one: {
        tow: {
            three: 'four'
        }
    }
})

// 【情况一】监视ref定义的一个响应式数据
const watchHot = watch(hot, (newValue, oldValue) => {
    console.log('hot变了', newValue, oldValue)
}, { immediate: true })

// 【情况二】监视ref定义的一个响应式数据
const watchHotAndSay = watch(
    [hot, say],
    (newValue, oldValue) => {
        console.log('hot或say变了', newValue, oldValue) // 包装为一个对象
    },
    { immediate: true }
)
/* 
    【情况三】监视reactive定义的一个响应式数据，注意：
    1. 此时无法正确获得oldValue。
    2. 无法关闭深度监视（即 deep: false）。 
*/
watch(
    SongInfo,
    (newValue, oldValue) => {
        console.log('SongInfo变了', newValue, oldValue)
    },
    { deep: false }
)
//【情况四】监视reactive定义的一个响应式的某一个属性变化
watch(
    () => SongInfo.one.tow.three,
    (newValue, oldValue) => {
        console.log('SongInfo的four变了', newValue, oldValue)
    }
)
//【情况五】监视reactive定义的一个响应式的某些属性变化
watch(
    [() => SongInfo.singer, () => SongInfo.song],
    (newValue, oldValue) => {
        console.log('SongInfo的singer或song变了', newValue, oldValue)
    }
)
//【特殊情况】当监视reactive定义的一个响应式中的某个属性时，深度监视有效，默认未开启
watch(
    () => SongInfo.one,
    (newValue, oldValue) => {
        console.log('SongInfo的one变了', newValue, oldValue)
    },
    { deep: true }
)

// 修改reactive数据
function wtchDeepReactiveData() {
    SongInfo.one.tow.three += '~'
}
// 取消ref数据监视、监视回调函数之外的操作还会执行
function unWactch() {
    watchHot()
    watchHotAndSay()
}
</script>

<style></style>
```

## 监视ref定义数据是否加.value问题

```html
<template>
  <div class="helloworld">
    <button @click="hot += 1">当前hot：{{ hot }}</button>
    <br>
    <hr>
    <button @click="SongInfo.singer += '!'">歌手：{{ SongInfo.singer }}</button>
  </div>
</template>

<script>
import { ref, watch } from 'vue'
export default {
  name: 'HelloWorld',
  setup() {
    // 数据源
    let hot = ref(1)
    let say = ref('hello')
    let SongInfo = ref({
      singer: '张杰',
      song: '逆战',
      one: {
        tow: {
          three: 'four'
        }
      }
    })

    // ref定义的基本数据类型，加上.value监视的是一个值，不是属性，监视无效。
    watch(hot, (newValue, oldValue) => {
      console.log('hot变了', newValue, oldValue)
    })

    /* 
        ref定义的复杂数据类型，不加xxx.value监视的是ref实现的RefImpl的实例对象，里面的value是一个浅层次的地址值,监视的不是value:Proxy的实例对象。
        1. 加上.value。2. 设置深度监视。
    */
    watch(SongInfo.value, (newValue, oldValue) => {
      console.log('SongInfo变了', newValue, oldValue)
    })

    // 返回一个对象
    return {
      hot, SongInfo
    }
  }
}
</script>

<style scoped>
.helloworld {
  background-color: #ee8888;
  padding: 20px;
  cursor: pointer;
}
</style>
```

------

## watchEffect函数

- watch的套路是：既要指明监视的属性，也要指明监视的回调。

- watchEffect的套路是：不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性。

- watchEffect有点像computed：

  - 但computed注重的计算出来的值（回调函数的返回值），所以必须要写返回值。
  - 而watchEffect更注重的是过程（回调函数的函数体），所以不用写返回值。

```html
<template>
  <div class="helloworld">
    <button @click="hot += 1">当前hot：{{ hot }}</button>
    <br>
    <hr>
    <button @click="SongInfo.singer += '!'">歌手：{{ SongInfo.singer }}</button>
  </div>
</template>

<script>
import { reactive, ref, watch, watchEffect } from 'vue'
export default {
  name: 'HelloWorld',
  setup() {
    // 数据源
    let hot = ref(1)
    let say = ref('hello')
    let SongInfo = reactive({
      singer: '张杰',
      song: '逆战',
      one: {
        tow: {
          three: 'four'
        }
      }
    })
    // 【watch】
    /*  watch(hot, (newValue, oldValue) => {
       console.log('hot变了', newValue, oldValue)
     }, { immediate: true }) */
    // 【watchEffect】
    watchEffect(() => {
      const x1 = hot.value
      const x2 = SongInfo.singer
      console.log('watchEffect执行了！')
    })
    // 返回一个对象
    return {
      hot, SongInfo
    }
  }
}
</script>

<style scoped>
.helloworld {
  background-color: #ee8888;
  padding: 20px;
  cursor: pointer;
}
</style>
```

### watchEffect（组合式）

```html
<template>
    <button @click="hot += 1">当前hot：{{ hot }}</button>
    <br>
    <button @click="SongInfo.singer += '!'">歌手：{{ SongInfo.singer }}</button>
</template>

<script setup>
// 1. 导入组合式API
import { ref, reactive, watchEffect, watch } from 'vue';
name: 'WatchEffect'
// 数据源
let hot = ref(1)
let SongInfo = reactive({
    singer: '张杰',
    song: '逆战',
    one: {
        tow: {
            three: 'four'
        }
    }
})
// 【watch】
watch(
    hot,
    (newValue, oldValue) => {
        console.log('hot变了', newValue, oldValue)
    },
    { immediate: true }
)
// 【watchEffect】
watchEffect(() => {
    // 当这里使用的响应式数据变化这里就会执行
    const x1 = hot.value
    const x2 = SongInfo.singer
    console.log('watchEffect执行了！')
})
</script>

<style></style>
```

# 深入组件

## prop（组合式）

**props 可以使用 `defineProps()` 宏来声明**

ps：注意命名风格

1. prop可以传递多种数据类型：Array、Number、Object、Boolean......
2. prop传递一个的对象使用`没有参数的v-bind:object`，相当于单独传递这个对象中的每个属性，子组件只需要接收这个对象的每个key，就可以得到它的value。
3. 所有的props都遵循单向绑定原则，根据数据的更新，自然地将新的状态向下流往子组件，而不会逆向传递。
4. 每个props都是只读的，不能去修改。
5. ***和vue2比？***`const props = defineProps([]) 相当于`props`

```html
<!-- 父组件传值给子组件 -->
<propsTest singer-name="YZling" :hot-number="412"></propsTest>

<!-- 子组件接收 -->
<template>
    <h3>singer:{{ singerName }}</h3>
    <h3>hot:{{ hotNumber }}</h3>
</template>

<script setup lang="ts">
name: "propsTest"
// 方式一、数组形式简单接收(声明)
const props = defineProps(['singerName', 'hotNumber']);

// 方式二、对象形式声明接收类型（类型不同警告）
// const props = defineProps({
//     singerName: String,
//     hotNumber: Number
// });

// 方式三、<script setup lang="ts">
// defineProps<{
//     singerName?: string
//     hotNumber?: number
// }>()

// 在 setup 中通过 props 访问属性
// console.log(typeof props.hot);
</script>
<style></style>

```

### prop校验

要声明对 props 的校验，你可以向 `defineProps()` 宏提供一个带有 props 校验选项的对象

```javascript
defineProps({
  // 基础类型检查
  // （给出 `null` 和 `undefined` 值则会跳过任何类型检查）
  propA: Number,
  // 多种可能的类型
  propB: [String, Number],
  // 必传，且为 String 类型
  propC: {
    type: String,
    required: true
  },
  // Number 类型的默认值
  propD: {
    type: Number,
    default: 100
  },
  // 对象类型的默认值
  propE: {
    type: Object,
    // 对象或数组的默认值
    // 必须从一个工厂函数返回。
    // 该函数接收组件所接收到的原始 prop 作为参数。
    default(rawProps) {
      return { message: 'hello' }
    }
  },
  // 自定义类型校验函数
  propF: {
    validator(value) {
      // 该值必须与这些字符串之一匹配
      return ['success', 'warning', 'danger'].includes(value)
    }
  },
  // 函数类型的默认值
  propG: {
    type: Function,
    // 不像对象或数组的默认，这不是一个
    // 工厂函数。这会是一个用来作为默认值的函数
    default() {
      return 'Default function'
    }
  }
})
```

------

## 自定义事件（组合式）

1. 向子组件传递自定义事件
2. 子组件触发父组件中的自定义事件（可传值）
3. 事件校验失败依然可以进行传值等操作，设置为null表示关闭验证。
4. ***和vue2比？***`const emit = defineEmits([]); 相当于`this.$emit`

```html
<!-- ========================父组件======================== -->
<template>
  <MyEventTest @sing-a-song-event="singeASong"></MyEventTest>
</template>

<script setup>
// 引入子组件
import MyEventTest from './components/MyEventTest.vue';
name: 'App';
// 注册组件
components: { MyEventTest }
// 自定义事件
function singeASong(song) {
  alert(`唱首${song}给你听！`)
}
</script>
<style scoped></style>

<!-- ========================子组件======================== -->
<template>
    <input type="text" v-model="song" placeholder="请输入歌曲名">
    <button @click="sendASong">点歌</button>
</template>

<script setup>
// 引入组合式API
import { ref, defineEmits } from 'vue'
name: 'MyEventTest'
const song = ref('樱花草');
// 1. 声明defineEmits宏要触发的其他组件事件
//(1) 数组写法
// const emit = defineEmits(['singASongEvent']) 
//(2) 对象写法：事件校验
const emit = defineEmits({
    // singASongEvent: null, // 关闭事件校验
    singASongEvent: (songName) => {
        if (songName.length > 2) {
            return true; // 允许触发事件
        } else {
            return false; // 阻止触发事件
        }
    }
})

// 2. 触发父组件的自定义事件
function sendASong() {
    emit('singASongEvent', song.value)
}
</script>
<style></style>v-model
```

## v-model在组件

可以实现父子数据同步

```html
<!-- 情形一 ：复杂同步代码 -->
<LuoTianYi :modelValue="hot" @update:modelValue="handler"></LuoTianYi>
<!-- 情形一 ：简略同步代码 -->
<LuoTianYi v-model="hot"></LuoTianYi>
<!-- 情形二 ：双向绑定多个数据 -->
<YueZhengLing v-model:hot="hot" v-model:hobby="hobby"></YueZhengLing>

// 自定义事件回调（接受子组件传递多来的数据）
const handler = (value) => {
    hot.value = value
}
```

```javascript
=======子组件LuoTianYi=======
// 接收props
let props = defineProps(['modelValue'])
// 接收自定义事件
let $emit = defineEmits(['update:modelValue'])

// 触发父组件的自定义事件
const handler = () => {
    $emit('update:modelValue', props.modelValue + 100)
}

=======子组件YueZhengLing=======
// 接收props
let props = defineProps(['hot', 'hobby'])
// 接收自定义事件
let $emit = defineEmits(['update:hot', 'update:hobby'])
// 触发父组件自定义事件并传值
const hobbyhandler = () => {
    $emit('update:hobby', '弹吉他')
}
const hothandler = () => {
    $emit('update:hot', props.hot + 100)
}
```

### v-model修饰符

1.  `.trim` 修饰符用于自动去除输入内容的首尾空格。

2.  `.number` 修饰符用于将输入内容转换为数字类型。

3.  `.lazy` 修饰符用于将输入内容的同步延迟到 `change` 事件而不是 `input` 事件。

4.  自定义修饰符：

    ```javascript
    /*
      1. 定义 modelValue 和 update:modelValue 属性
    */
    
    
    ```

    

------

## 透传属性

1. ***什么是透传属性？***“透传 attribute”指的是传递给一个组件，却没有被该组件声明为 [props](https://cn.vuejs.org/guide/components/props.html) 或 [emits](https://cn.vuejs.org/guide/components/events.html#defining-custom-events) 的属性或者 `v-on` 事件监听器。最常见的例子就是 `class`、`style` 和 `id`。
2. ***透传影响前提？***当一个组件以单个元素为根作渲染时，透传的 attribute 会自动被添加到根元素上。
3. 多个相同样式属性会合并。
4. 这些透传进来的 attribute 可以在模板的表达式中直接用 `$attrs` 访问到。
5. `foo-bar` 这样的一个 attribute 需要通过 `$attrs['foo-bar']` 来访问。
6. ***和vue2比？***`let attrs = useAttrs();`相当于选项式API中的this.$attrs、模板中也可以直接引用$attrs或attrs

```html
<!-- ====================父组件==================== -->
<template>
  <h3>透传属性和点击事件</h3>
  <TouChuanTest class="textColor" @sing-a-song-event="singeASong" @click="say"></TouChuanTest>
</template>
<script setup>
// 引入组件
import TouChuanTest from './components/TouChuanTest.vue';
name: 'App';
// 注册组件
components: { TouChuanTest }
// 自定义事件
function singeASong(song) { alert(`唱首${song}给你听！`) }
// 点击事件
function say() { alert('hello') }
</script>
<style scoped>
.textColor { background-color: #e88; }
</style>

<!-- ====================子组件==================== -->
<template>
    <!-- 透传的属性[props]和事件[emits]只有没被 [props] 或 [emits] 声明才才存在与$attr/useAttrs() -->
    <div v-bind="$attrs">我是div</div>
    <div>
        <button @click="ttt" v-bind="$attrs">获取透传的属性和事件</button>
    </div>
</template>

<script setup>
// 引入组合式API
import { useAttrs, ref } from 'vue'
name: 'TouChuanTest';
// 数据源
let song = ref('十年');
// 相当于选项式API中的this.$attrs、模板中也可以直接引用$attrs或attrs
let attrs = useAttrs(); // 透传的属性和事件
// 点击事件(会触发透传的点击事件)
function ttt() {
    console.log(attrs.class); // 透传的类名
    console.log(attrs);// 透传的属性和事件
    console.log(attrs.onSingASongEvent); // 透传的自定义事件
    attrs.onClick(); // 触发透传的点击事件
    attrs.onSingASongEvent(song.value); // 触发透传事件
}
</script>
<style></style>

<!-- ====================子组件设置阻止透传属性、事件==================== -->
<script setup>
defineOptions({
  inheritAttrs: false
})
</script>
```

### 透传的其他妙用

透传 attribute 都应用在内部的 `<button>` 上而不是外层的 `<div>` 上，没有参数的 `v-bind`会将一个对象的所有属性都作为 attribute 应用到目标元素上。

多根元素没有设置`v-bind="$attrs"`会出现警告

```html
<div class="btn-wrapper">
  <button class="btn" v-bind="$attrs">click me</button>
</div>
```

## 插槽

类似vue2

父组件（填坑）：可以是任意合法的模板内容，不局限于文本。例如我们可以传入多个元素，甚至是组件。

注意插槽上的 `name` 是一个 Vue 特别保留的 attribute，不会作为 props 传递给插槽。

### 父组件（填坑）

```html
<!-- 默认插槽 -->
<SlotText>
  <button>插槽按钮</button>
</SlotText>

<!-- 具名插槽：必须template组合v-slot: -->
<SlotText>
  <!-- <template #color> -->
  <template v-slot:color>
    <p>我是蓝色</p>
    <p>我是红色</p>
  </template>
</SlotText>

<!-- 作用域插槽 -->
<SlotText v-slot="{ singer, song }">
  <ul>
    <li v-for="(s, index) in singer" :key="index">{{ s }}--{{ song }}</li>
  </ul>
</SlotText>

<!-- 具名作用域插槽：简写#vsinger="{ singer, song }" -->
<SlotText>
  <template v-slot:vsinger="{ singer, song }">
    <ul>
      <li v-for="(s, index) in singer" :key="index">{{ s }}--{{ song }}</li>
    </ul>
  </template>
</SlotText>
```

### 子组件（挖坑）

```html
<!-- 默认插槽：默认名字default -->
<slot>
    <p>我是未传入结构时的默认内容</p>
</slot>

<!-- 具名插槽 -->
<slot name="color">
    <p>我是未传入结构时的默认内容</p>
</slot>

<!-- 作用域插槽 -->
<slot :singer="singers" :song="songs">
    <p>我是未传入结构时的默认内容</p>
</slot>


<!-- 具名作用域插槽 -->
<slot name="vsinger" :singer="singers" :song="songs">
    <p>我是未传入结构时的默认内容</p>
</slot>

```

## 依赖注入

为了解决props在面对多个组件通信时，深层次结构的组件访问外部组件时，props会逐层传递，哪怕过程中某些组件并不需要这些数据，依然要参与通信过程，使得这一过程过于繁琐，使用依赖注入就可以很好的解决。

### 提供Provide

```javascript
// =======================【组件设置】=======================
<script setup>
import { ref, provide } from 'vue'
// 普通数据
provide(/* 注入名 */ 'message', /* 值 */ 'hello!')
// 响应式
const count = ref(0)
provide('key', count)
</script>

// =======================【main.js设置】=======================
import { createApp } from 'vue'
const app = createApp({})
app.provide(/* 注入名 */ 'message', /* 值 */ 'hello!')

```

### 注入Inject

1. 注入的数据没有提供者，设置默认数据可以让警告不在出现

```html
// =======================【组件设置】=======================
<script setup>
import { inject } from 'vue'； 

const message = inject('message'); // 一般注入
const message = inject('message', '这是默认值'); // 默认值设置方法一
const message = inject('key', () => new ExpensiveClass(), true); // 默认值设置方法二
</script>
```

### 注入方修改响应式数据

#### 提供Provide

```html
<template>
  <div style="background-color: #E88; padding: 50px;">
    <h3>vsinger</h3>
    <h3>{{ singerObj.singerName }}</h3>
    <h3>{{ singerObj.hot }}</h3>
    <DependenceTest1></DependenceTest1>
  </div>
</template>

<script setup>
// 引入组件
import DependenceTest2 from './components/DependenceTest2.vue';
import { provide, reactive } from 'vue';
name: 'App';
// 注册组件
components: { DependenceTest1 }
// 数据源
const singerObj = reactive({
  singerName: '乐正绫',
  hot: 412
})

// 修改方法
function updateSingerObj(value) { singerObj.hot = value }
    
// 提供（数据和修改方法）（箭头函数要写在这个前面、建议此代码写在后面）
provide('updateSingerObj', { singerObj, updateSingerObj })
    
</script>
<style></style>

```

#### 注入Inject

1. 选项式：相当于props:[ ]写法

```html
<template>
    <div style="background-color: #88e; padding: 50px;">
        <h3>sing a song</h3>
        <input type="text" @focusout="updateHot" v-model="inputNum">
    </div>
</template>

<script setup>
import { inject, ref } from "vue"
name: 'DependenceTest2';
// 数据源 
const inputNum = ref(1);
// 注入数据
const { updateSingerObj, singerObj } = inject('updateSingerObj');
// 使用数据
function updateHot() {
    updateSingerObj(inputNum.value); // 修改数据
    console.log(singerObj.singerName);
    console.log(singerObj.hot);
}
</script>
<style></style>
```

------

## ref

```html
<template>
    <input type="text" ref="account">
    <button @click="getElement1">得到元素1</button><br>
    <input type="text" :ref="getElement2">
    <button @click="getElement2">得到元素2</button>
</template>

<script setup>
import { ref } from 'vue'
name: 'RefEleTest';
// 数据源属性名必须和ref绑定的一致
let account = ref(null);
let textInput = ref(null);
// 方法
function getElement1() {
    console.log(account.value); // DOm 元素
}
function getElement2(el) {
    textInput.value = el; // el：立马得到DOM元素
}
</script>

<style></style>
```

## 全局事件总线

npm install --save mitt

```javascript
// bus.js 引入mitt插件
import mitt from 'mitt';
const $bus = mitt();
export default $bus

// A组件触发
import bus from './bus'
 bus.emit('foo', '豆豆')
// B组件接收
import bus from './bus'
bus.on('foo', e => {
    console.log('e', e) // '豆豆'
})
```



------

# vue3生命周期

1. 相较于vue2**更新了生命周期钩子名称**   ***beforeDestroy***和**Destroyed**=====》**beforeUnmount****和**unmounted**
2. vue3先创建vm和挂载App然后初始化，vue2是先挂载后判断是否el绑定App容器。
3. vue3生命周期在setup中配置加上onXXXX。
4. beforeCreate和created在vue3中表现为setup。

HelloWorld.vue

setup里面的生命周期钩子先执行。

```html
<template>
    <div class="helloworld">
        <button @click="hot += 1">当前hot：{{ hot }}</button>
    </div>
</template>

<script setup>
import { ref, onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted } from 'vue'
name: 'LifeCycleTest'
// 数据源
let hot = ref(1)
console.log('---setup---')
// 组件视图渲染之前：可以访问数据、函数、不能访问DOM元素
onBeforeMount(() => { console.log('---onBeforeMount---') });
// 组件视图渲染之后：同步子组件都已经被挂载、可以访问数据、函数、DOM元素
onMounted(() => { console.log('---onMounted---') })
// 数据源发生变化、视图重新渲染之前、可以访问修改之前的数据
onBeforeUpdate(() => { console.log('---onBeforeUpdate---') })
// 数据源发生变化、视图重新渲染之后、可以访问修改之后的数据
onUpdated(() => { console.log('---onUpdated---') })
// 组件销毁之前、可以访问视图、数据源、函数等
onBeforeUnmount(() => { console.log('---onBeforeUnmount---') })
// 组件销毁之后、不能访问视图、可以访问数据源、函数
onUnmounted(() => { console.log('---onUnmounted---') })
</script>

<style scoped></style>
```

```html
<template>
  <div class="helloworld">
    <button @click="hot += 1">当前hot：{{ hot }}</button>
  </div>
</template>

<script>
import { ref,onBeforeMount,onMounted,onBeforeUpdate,onUpdated,onBeforeUnmount,onUnmounted } from 'vue'
export default {
  name: 'HelloWorld',
  setup() {
    // 数据源
    let hot = ref(1)

    console.log('---setup---')
    onBeforeMount(()=>{
       console.log('---onBeforeMount---')
    })
    onMounted(()=>{
       console.log('---onMounted---')
    })
    onBeforeUpdate(()=>{
       console.log('---onBeforeUpdate---')
    })
    onUpdated(()=>{
       console.log('---onUpdated---')
    })
    onBeforeUnmount(()=>{
       console.log('---onBeforeUnmount---')
    })
    onUnmounted(()=>{
       console.log('---onUnmounted---')
    })
    
    return {
      hot
    }
  },
  // beforeCreate() {
  //   console.log('---beforeCreate---')
  // },
  // created() {
  //   console.log('---created---')
  // },
  // beforeMount() {
  //   console.log('---beforeMount---')
  // },
  // mounted() {
  //   console.log('---mounted---')
  // },
  // beforeUpdate() {
  //   console.log('---beforeUpdate---')
  // },
  // updated() {
  //   console.log('---updated---')
  // },
  // beforeUnmount() {
  //   console.log('---beforeUnmount---')
  // },
  // unmounted() {
  //   console.log('---unmounted---')
  // }
}
</script>

<style scoped>
.helloworld {
  background-color: #ee8888;
  padding: 20px;
  cursor: pointer;
}
</style>
```

------

# 路由

## 与vue2不同

### 安装

1. 安装路由 npm install vue-router@4

### mode：路径样式

`mode: 'history'` 配置已经被一个更灵活的 `history` 配置所取代。history配置值如下：

- `"history"`: `createWebHistory()`（HTML5模式，url不携带#，可能返回404错误，需要后端支持）
- `"hash"`: `createWebHashHistory()`（Hash模式，url携带#，#前的路径不会传给服务器）
- `"abstract"`: `createMemoryHistory()`

- hash - 使用 url hash 变化记录路由地址
- history - 使用 H5 history API 来改 url 记录路由地址
- abstract - 不修改 url ，路由地址在内存中，**但页面刷新会重新回到首页**。

### 组件的router对象

1. vue2中

```javascript
this.$route.params
this.$route.query

this.$router.push(path)
this.$router.push({
    name:'',
    parmas:{}
})
// 相当于vue3
import { useRoute,useRouter } from 'vue-route'
useRoute().params
useRouter().push
```



## 快速入门

### router/index.js

```javascript
// 1. 引入组合式API
import { createRouter, createWebHistory, createWebHashHistory } from 'vue-router'
// 2. 路由规则
const routes = [
    {
        path: '/reactiveTest',
        name: 'reactive-test',
        component: () => import('@/views/ReactiveTest.vue')
    },
    {
        path: '/refDataTest',
        name: 'ref-data-test',
        component: () => import('@/views/RefDataTest.vue')
    },
    {
        path: '/vsinger',
        name: 'vsinger',
        component: () => import('@/views/vsinger/index.vue'),
        // 嵌套路由path没有 /
        children: [
            {
                path: 'yueZhengLing',
                name: 'yue-Zheng-ling',
                component: () => import('@/views/vsinger/YueZhengLing.vue'),
            },
            {
                path: 'luoTianYi',
                name: 'luo-tian-yi',
                component: () => import('@/views/vsinger/LuoTianYi.vue'),
            }
        ]
    },
    {
        // 重定向
        path: '/',
        redirect: '/refDataTest'
    }
]
// 3. 创建路由、引入路由规则
const router = createRouter({
    // history: createWebHistory(), // 路由模式
    history: createWebHashHistory(), // 路由模式
    routes // 引入路由规则
})
// 3. 暴露路由
export default router
```

### main.js

```javascript
import { createApp } from 'vue'
import App from './App.vue'
// 引入路由模块
import router from './router'

const app = createApp(App)
// 初始化路由(必须在mount前)
app.use(router)
app.mount('#app');
```

------

## 路由传参

### 路由的params参数

#### 传递params参数步骤

1. 路由规则path设置参数占位。
2. 组件跳转路径传递参数（可以使用模板字符串引入响应式数据）
3. 参数的key必须对相应（路由规则中和组件中）

```javascript
{
    path: '/vsinger',
    name: 'vsinger',
    component: () => import('@/views/vsinger/index.vue'),
    children: [
        // 1. path参数占位
        {
            path: 'yueZhengLing/:hot',
            name: 'yue-Zheng-ling',
            component: () => import('@/views/vsinger/YueZhengLing.vue'),
        },
        {
            path: 'luoTianYi/:hot',
            name: 'luo-tian-yi',
            component: () => import('@/views/vsinger/LuoTianYi.vue'),
        }
    ]
}]


```

```html
<div>
    <h3>Vsinger</h3>
    <ul style="list-style: none;">
        <!-- 2. 路径设置传递参数 -->
        <li><router-link to="/vsinger/yueZhengLing/412">YZl</router-link></li>
        <li><router-link to="/vsinger/luoTianYi/712">LTY</router-link></li>
    </ul>
</div>
<router-view></router-view>
```

#### 获取prams参数步骤

1. 选项式API：js代码中：`this.$route.params`、模板中：`$route.params`
2. 组合式API：引入：`import {usrRoute} from 'vue-router’`、模板、js代码中：`uerRoute.params`
3. 值类型：都在一个对象里存储

```html
<template>
    <div>
        <h3>洛天依</h3>
        <!-- 获取参数 -->
        <h4>{{ $route.params }}</h4>
        <h4>{{ params.hot }}</h4>
    </div>
</template>

<script setup>
import { useRoute } from "vue-router";
const params = useRoute().params;
</script>

<style></style>
```

------

### 路由的query参数

1. to的字符串写法。（数据包含特殊字符不能用例如：#）
2. to的对象写法。（可以包含特殊字符）
3. query在$route上，所以接收参数为：$route.query.hot、useRoute.query.hot

```html
<!-- to的字符串写法(没有用：传递的数据不能包含特殊字符：例如 blue: '#00EEEE' ) -->
<router-link to="/vsinger/yueZhengLing?hot=412&song=世末歌者">YZl</router-link>

<!-- to的对象写法(有用：传递的数据可以包含特殊字符)  -->
<router-link :to="{
    path: '/vsinger/yueZhengLing',
    query: {
        hot: 412,
        song: '世末歌者'
    }
}">YZl</router-link>
```



------

### 路由的props参数

1. 路由设置`props: true`会将路径上的动态参数传递给组件上的props
2. 获取参数和prop设置一样：①`const props = defineProps(['hot']); `② `{{props.hot}}`
3. 详见目录【深入组件/props】

```javascript
{
    // 1. path参数占位
    path: 'yueZhengLing/:hot',
    name: 'yue-Zheng-ling',
    component: () => import('@/views/vsinger/YueZhengLing.vue'),
    // 打开props传参
    props：true，
},
```

## 路由的跳转模式

### 声明式导航

1. <router-link>的replace模式：破坏浏览器上一条记录。一直保持当前一条记录在栈顶。（替换记录）
2. <router-link>的push模式（默认）：不破坏上一条记录。一直压入栈中。（保存记录）

```html
<router-link to="/vsinger/yueZhengLing" replace>YZl</router-link>
<router-link :to="/vsinger/yueZhengLing">YZl</router-link>
<router-link to="/vsinger/luoTianYi">LTY</router-link>
```

### 编程式导航

1. 选项式：this.$router.push(path)
2. 组合式：引入：`import {usrRouter} from 'vue-router'`、模板、js代码中：`uerRoute().push(path)`
3. 跳转方法：`uerRoute().push({})`、`uerRoute().replace({})`、`uerRoute().back(number)`、`uerRoute().go(number)`、`uerRoute().forward()`、`uerRoute().back()`
4. 备注：①{}：可以配置name、path、params；②number：可以式正数、负数

```javascript
// 1. 选项式编程式跳转
this.$router.push('/xiangqing')
this.$router.push({name:'xiangqing'})
this.$router.push({ name: 'xiangqing' ,params: {blue: b.blue, msg: b.msg} })
// params不要和 path使用
// this.$router.push({ path: '/xiangqing' ,params: {blue: b.blue, msg: b.msg} })

// 2. 组合式编程式跳转
import {useRouter} from 'vue-router'
const router = useRouter()
router.push({ name: 'xiangqing' ,params: {blue: b.blue, msg: b.msg} })

// 3. 组合式编程式跳转中使用replace
router.push({ replace:ture, name: 'xiangqing' ,params: {blue: b.blue, msg: b.msg}  })
router.replace({ name: 'xiangqing' ,params: {blue: b.blue, msg: b.msg}  })

// 4. 
router.go(1) // 前进一步
router.go(-1) // 回退一步
router.back() // 返回一步
router.forward() // 前进一步

```

------

## 路由守卫

------

### 全局路由守卫

详情参考vue2

#### 全局前置守卫

```javascript
/*
 1. 取消直接暴露router，使用变量承接。
 2. 暴露之前使用API的beforeEach调用一个回调函数。
 3. 路由每一次活动都会调用这个函数（切换之前调用和初始化时也调用）。
 4. 回调函数接收三个参数（to:你去哪里，from:你来自哪里，next:放行）
*/

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
```

#### 全局后置守卫

```

```

------

### 独享路由守卫

```

```

------

### 组件内路由守卫

```

```



------

# Pinia状态管理

## Pinia

有**三个概念**，[state](https://pinia.web3doc.top/core-concepts/state.html)、[getters](https://pinia.web3doc.top/core-concepts/getters.html) 和 [actions](https://pinia.web3doc.top/core-concepts/actions.html) 并且可以安全地假设这些概念等同于组件中的“数据”、“计算”和“方法”。

1. vue3安装 npm install pinia（vue2还需安装组合 API：`@vue/composition-api`）

2. 引入使用：

   ```javascript
   import { createApp } from 'vue'
   import App from './App.vue'
   // 引入pinia
   import { createPinia } from 'pinia'
   const app = createApp(App)
   // 使用pinia
   app.use(createPinia())
   // 初始化app(使用其它插件必须在mount前)
   app.mount('#app');
   ```

3. 创建store

4. ***注意：***defineStore( )，有两个参数，第一个确定唯一标签（字符串），第二个为store配置（对象或者函数返回）

## 选项式

### 选项式创建store

```javascript
import { defineStore } from "pinia";

// 选项式
const storeOptions = defineStore('mian', {
    // state
    state: () => ({
        singer: '乐正绫',
        bth: 412,
        hobby:'read'
    }),
    // getters
    getters: {
        singerInfo(store){
            return store.singer+'--'+this.bth
        },
        hobbyChinses:store => store.hobby == 'read'? '阅读':'其它';
    },
    // action
    actions: {
        setSingerInfo(singer,bth){
            // this指当前的store实例对象
            this.singer = singer;
            this.bth = bth
        },
        setSingerHobby(hobby){
            this.hobby = hobby
		}
    }
})
// 暴露store
export { storeOptions } 
```

### 选项式使用state

```javascript
// 引入piniaAPI操作store
import { mapState，mapWritableState } from 'pinia'
// 引入创建的store
import { storeOptions } from './store/storeOptions.js'
export default{
	computed:{ 
        // mapState( myStore ,array | object ) 映射属性只读
        ...mapState(storeOptions,[ 'singer','bth' ]),
        ...mapState(storeOptions,{
        	singerName:'singer',
        	bthDay:'bth'
    	})，
        // mapWritableState( myStore ,array | object ) 映射属性可读写
         ...mapWritableState(storeOptions,[ 'singer','bth' ]),
    }
}
```

### 选项式使用getters

```javascript
// 引入piniaAPI操作store
import { mapState，mapWritableState } from 'pinia'
// 引入创建的store
import { storeOptions } from './store/storeOptions.js'
export default{
	computed:{ 
        // mapState( myStore ,array | object ) 映射属性只读,
        ...mapState( storeOptions,[ 'singerInfo','hobbyChinses' ] ),
        ...mapState( storeOptions,{
        	singer:'singerInfo',
        	hobby:'hobbyChinses'
    	})，
        // mapWritableState( myStore ,array | object ) 映射属性可读写
         ...mapWritableState(storeOptions,[ 'singerInfo','hobbyChinses' ]),
    }
}
```

### 选项式使用actions

```javascript
// 引入piniaAPI操作store
import { mapAction } from 'pinia'
// 引入创建的store
import { storeOptions } from './store/storeOptions.js'
export default{
    methods:{
        // 映射为自己的方法
        ...mapActions(storeOptions,[ 'setSingerInfo','setSingerHobby' ]),
        ...mapActions(storeOptions,{
            hobby:'setSingerHobby'
        }),
        setSingerInfo('洛天依', 712),
        hobby('阅读'),
    }
}
```



------

## 组合式

### 组合式创建store

```javascript
// 引入piniaAPI操作store
import {mapState} from 'pinia'
// 引入创建的store
const storeComponents = defineStore('mian', () => {
    // 【ref变量 --> 相当于state】
    const singer = ref('乐正绫');
    const bth = ref(412);
    const hobby = ref('read');
    // 【computed() --> 相当于getters】
    const singerInfo = computed(() => {
    	return singer.value + '--' + bth.value;
	})
	const speakChinses = computed(() => hobby.value == 'read' ? '阅读' : '其它')
    // 【function() --> 相当于actions】
    // 形参尽量不要和state冲突
    function setSingerInfo(s_singer,s_bth){
        singer.value = s_singer;
        bth.value = s_bth;
	}
    function setSingerHobby(s_hobby){
        hobby.value = s_hobby
    }
    // 记得返回
    return {
        singer, bth, singerInfo, speakChinses, setSingerHobby, setSingerInfo
    }
})
// 暴露store
export { storeComponents } 
```

### 组合式使用store

```html
<template>
	<h3>乐正绫</h3>
    <!-- state数据 -->
	<h3>{{ options_store.singer }}</h3>
	<h3>{{ options_store.bth }}</h3>
	<h5>{{ singerName }}</h5>
	<h5>{{ bth }}</h5>
    <!-- getters数据 -->
    <h3>{{ singerInfo }}</h3>
    <h3>{{ speakChinses }}</h3>
    <!-- actions数据 -->
	<button @click="options_store.setSingerInfo('洛天依', 712)">修改singerInfo</button>
	<button @click="hobby('game')">修改singerHobby</button>
</template>

<script setup>
// 引入创建的store
import { storeComponents } from '@/store/storeComponents'
// 获取创建的sotre实例
const options_store = storeComponents();// 具有响应式
// 获取--->state   
const singerName = computed(() => options_store.singer); // 只读，具有响应式
const { bth } = storeToRefs(options_store) // 可读写，具有响应式
// 获取--->getters
const singerInfo = computed(() => options_store.singerInfo); // 只读，具有响应式
const { speakChinses } = storeToRefs(options_store); // 可读写，具有响应式
// 获取 actions
options_store.setSingerInfo('洛天依', 712); // 直接stor实例调用
const { setSingerHobby: hobby } = options_store; // 解构
</script>
<style></style>
```

------

# axios请求

跨域

```javascript
server: {
    proxy: {
      '/api': {
        target: "http://gmall-h5-api.atguigu.cn",
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/api/, ''),
      },
      '/imgapi': {
        target: 'https://www.dmoe.cc',
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/imgapi/, ''),
      }
    }
  }
```



------

# hook自定义函数

1. 鼠标点击获取（x，y）实例。
2. 把setup中用到的组合式API（composition API）封装，复用代码，让setup中的代码简洁。

## src/hooks/mousePoint.js

```javascript
import { onBeforeUnmount, onMounted, reactive } from "vue"
export default function () {
    // 数据
    let point = reactive({
        x: 0,
        y: 0
    })
    // 获取鼠标坐标
    function savePoint(event) {
        console.log('---savePoint---')
        point.x = event.pageX
        point.y = event.pageY
    }
    // 生命周期钩子挂载方法实现获取鼠标坐标
    onMounted(() => {
        console.log('---开始监听---')
        window.addEventListener("click", savePoint)
    })
    onBeforeUnmount(() => {
        console.log('---销毁监听---')
        window.removeEventListener("click", savePoint)
    })
    return point
}
```

## HelloWorld.vue

```html
<template>
  <div class="helloworld">
    <button @click="hot += 1">当前hot：{{ hot }}</button>
    <h3>（{{ point.x }}，{{ point.y }}）</h3>
  </div>
</template>

<script>
import { ref } from "vue"
import mouseOoint from '../hooks/mousePoint'
export default {
  name: "HelloWorld",
  setup() {
    // 数据源
    let hot = ref(1)
    let point = mouseOoint()
    return {
      hot,
      point,
    }
  }
}
</script>

<style scoped>
.helloworld {
  background-color: #ee8888;
  padding: 20px;
  cursor: pointer;
}
</style>
```

------

## toRef

- 作用：创建一个 ref 对象，其value值指向另一个对象中的某个属性。
- 语法：```const name = toRef(person,'name')```
- 应用:   要将响应式对象中的某个属性单独提供给外部使用时。


- 扩展：```toRefs``` 与```toRef```功能一致，但可以批量创建多个 ref 对象，语法：```toRefs(person)```

------

```html
<template>
  <div class="app">
    <h3>歌手：{{ singer }}</h3>
    <h3>歌词：{{ lyrics }}</h3>
    <h3>歌曲：{{ song.one }}</h3>
    <h3>歌曲：{{ song.tow }}</h3>
    <h3>歌曲：{{ song.three.four }}</h3>
    <h3>{{ other }}</h3>
  </div>
</template>

<script>
import { reactive, toRef, toRefs } from 'vue'
export default {
  name: 'App',
  setup() {
    let singerInfo = reactive({
      singer: '邓紫棋',
      lyrics: '阳光下的泡沫,是彩色的',
      song: {
        one: '泡沫',
        tow: '再见',
        three: {
          four: '倒数'
        }
      },
      other: ['光年之外', '偶尔', '攀登']
    })

    return {
      // 处理一个数据，
      singer: toRef(singerInfo, 'singer'),
      // 处理多个数据(拆第一层)
      ...toRefs(singerInfo)
    }
  }
}
</script>
```

------

# 其它API（选项式风格）

------

### 1.shallowReactive 与 shallowRef

- shallowReactive：只处理对象最外层属性的响应式（浅响应式）。
- shallowRef：只处理基本数据类型的响应式, 不进行对象的响应式处理。

- 什么时候使用?
  -  如果有一个对象数据，结构比较深, 但变化时只是外层属性变化 ===> shallowReactive。
  -  如果有一个对象数据，后续功能不会修改该对象中的属性，而是生新的对象来替换 ===> shallowRef。

------

### 2.readonly 与 shallowReadonly

- readonly: 让一个响应式数据变为只读的（深只读）。
- shallowReadonly：让一个响应式数据变为只读的（浅只读）。
- 应用场景: 不希望数据被修改时。

------

### 3.toRaw 与 markRaw

- toRaw：
  - 作用：将一个由**```reactive```**生成的<strong style="color:orange">响应式对象</strong>转为<strong style="color:orange">普通对象</strong>。
  - 使用场景：用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新。
- markRaw：
  - 作用：标记一个对象，使其永远不会再成为响应式对象。
  - 应用场景:
    1. 有些值不应被设置为响应式的，例如复杂的第三方类库等。
    2. 当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能。

```html
<template>
    <h3>singer:{{ singer }}</h3>
    <h3>song:{{ song }}</h3>
    <h3 v-show="singerInfo.car">car:{{ singerInfo.car }}</h3>
    <button @click="addCar">addCar</button>
    <button @click="changeCarPrice" v-show="singerInfo.car">changeCarPrice</button>
</template>
<script>
import { reactive, toRefs, markRaw } from 'vue'
export default {
    name: 'TestThree',
    setup() {
        let singerInfo = reactive({
            singer: '邓紫棋',
            song: '句号'
        })
        // 添加一台车的信息（响应式和非响应式数据）
        function addCar() {
            let car = { color: 'red', price: '50' }
            // singerInfo.car = markRaw(car)
            singerInfo.car = car
        }
        // 测试修改响应式数据
        function changeCarPrice() {
            singerInfo.car.price++
        }
        return {
            singerInfo,
            addCar,
            changeCarPrice,
            ...toRefs(singerInfo)
        }
    }
}
</script>
```

------

### 4.customRef

- 作用：创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制。
- 实现防抖效果：<input>输入信息后<h3>展示信息

```html
<template>
	<input type="text" v-model="keyword">
	<h3>{{keyword}}</h3>
</template>

<script>
	import {ref,customRef} from 'vue'
	export default {
		name:'Demo',
		setup(){
             // 1. 使用Vue准备好的内置ref
			// let keyword = ref('hello') 
            
			// 2. 使用自定义的一个myRef
			function myRef(value,delay){
				let timer
				//通过customRef去实现自定义
				return customRef((track,trigger)=>{
					return{
						get(){
							track() //告诉Vue这个value值是需要被“追踪”的
							return value
						},
						set(newValue){
							clearTimeout(timer)
							timer = setTimeout(()=>{
								value = newValue
								trigger() //告诉Vue去更新界面
							},delay)
						}
					}
				})
			}
			let keyword = myRef('hello',500) //使用程序员自定义的ref
			return {
				keyword
			}
		}
	}
</script>
```

------

### 5.provide 与 inject

- 作用：实现<strong style="color:#DD5145">祖(provide )与后代( inject)组件间</strong>通信

- 套路：父组件有一个 `provide` 选项来提供数据，后代组件有一个 `inject` 选项来开始使用这些数据

- 具体写法：

  1. 祖组件中：

  ```javascript
  setup(){    
      ......    
      let car = reactive({name:'奔驰',price:'40万'})    provide('car',car)    
      ......
  }
  ```

  2. 后代组件中：

  ```javascript
  setup(){    
      ......    
      let car = reactive({name:'奔驰',price:'40万'})    
      provide('car',car)    
      ......
  }
  ```

------

### 6.响应式数据的判断

- isRef: 检查一个值是否为一个 ref 对象
- isReactive: 检查一个对象是否是由 `reactive` 创建的响应式代理
- isReadonly: 检查一个对象是否是由 `readonly` 创建的只读代理
- isProxy: 检查一个对象是否是由 `reactive` 或者 `readonly` 方法创建的代理

```javascript
import {ref, reactive,toRefs,readonly,isRef,isReactive,isReadonly,isProxy } from 'vue'
	export default {
		name:'App',
		setup(){
			let car = reactive({name:'奔驰',price:'40W'})
			let sum = ref(0)
			let car2 = readonly(car)

			console.log(isRef(sum))// ture
			console.log(isReactive(car))// ture
			console.log(isReadonly(car2))// ture
			console.log(isProxy(car))// ture
             console.log(isProxy(car2))// ture
			console.log(isProxy(sum))// false

			return {...toRefs(car)}
		}
	}
```

# 组合式API优势

------

### 1.Options API 存在的问题

使用传统OptionsAPI中，新增或者修改一个需求，就需要分别在data，methods，computed里修改 。

<div style="width:600px;height:370px;overflow:hidden;float:left">
    <img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f84e4e2c02424d9a99862ade0a2e4114~tplv-k3u1fbpfcp-watermark.image" style="width:600px;float:left" />
</div>
<div style="width:300px;height:370px;overflow:hidden;float:left">
    <img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e5ac7e20d1784887a826f6360768a368~tplv-k3u1fbpfcp-watermark.image" style="zoom:50%;width:560px;left" /> 
</div>







------























------

### 2.Composition API 的优势

我们可以更加优雅的组织我们的代码，函数。让相关功能的代码更加有序的组织在一起。

<div style="width:500px;height:340px;overflow:hidden;float:left">
    <img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bc0be8211fc54b6c941c036791ba4efe~tplv-k3u1fbpfcp-watermark.image"style="height:360px"/>
</div>
<div style="width:430px;height:340px;overflow:hidden;float:left">
    <img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6cc55165c0e34069a75fe36f8712eb80~tplv-k3u1fbpfcp-watermark.image"style="height:360px"/>
</div>





























------

# 新的组件

### 1.Fragment

- 在Vue2中: 组件必须有一个根标签
- 在Vue3中: 组件可以没有根标签, 内部会将多个标签包含在一个Fragment虚拟元素中
- 好处: 减少标签层级, 减小内存占用
- （Fragment：片段）

------

### 2.Teleport

- 什么是Teleport？—— `Teleport` 是一种能够将我们的<strong style="color:#DD5145">组件html结构</strong>移动到指定位置的技术。
- `<Teleport>` 是一个内置组件，它可以将一个组件内部的一部分模板“传送”到该组件的 DOM 结构外层的位置去。
- to可以是body，选择器，#id  .class等。
- （Teleport：瞬间移动，传送，闪现）

App.vue

```html
<template>
  <div class="app">
    <h3>App</h3>
    <TowApp />
  </div>
</template>

<script>
import OneApp from './components/TowApp'
export default {
  name: 'App',
  components: {
    OneApp
  },
}
</script>

<style>
.app {
  background-color: #00eeee;
  padding: 20px;
}
</style>
```

TowApp.vue

```html
<template>
    <div>
        <button @click="isShow = !isShow">----弹----</button>
        <!-- V-IF -->
        <teleport to="body">
            <div class="mask" v-if="isShow">
                <div class="tow-app">
                    <h3> TowApp</h3>
                    <h4>------1------</h4>
                    <h4>------2------</h4>
                    <h4>------3-----</h4>
                    <h4>------4------</h4>
                    <button @click="isShow = false">----关----</button>
                </div>
            </div>
        </teleport>
    </div>
</template>

<script>
import { ref } from 'vue';
export default {
    name: 'TowApp',
    setup() {
        let isShow = ref(false)
        return {
            isShow
        }
    }
}
</script>

<style scoped>
.tow-app {
    display: block;
    width: 300px;
    margin: 300px auto;
    padding: 10px;
    background-color: #e88;
}

.mask {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.5);
}
</style>
```

------

### 3.Suspense

`<Suspense>` 是一个内置组件，用来在组件树中协调对异步依赖的处理。它让我们可以在组件树上层等待下层的多个嵌套异步依赖项解析完成，并可以在等待时渲染一个加载状态。

`<Suspense>` 组件有两个插槽：`#default` 和 `#fallback`。两个插槽都只允许**一个**直接子节点。在可能的时候都将显示默认槽中的节点。否则将显示后备槽中的节点。

Child.vue

```html
<template>
    <div class="child">
        <h3>Child</h3>
    </div>
</template>
<script>
export default {name: 'Child'}
</script>
<style>
.child {
    padding: 20px;
    background-color: #555;
}
</style>
```

App.vue

```html
<template>
  <div class="app">
    <h3>App</h3>
    <Suspense>
      <!-- 主体内容 -->
      <template v-slot:default>
        <Child />
      </template>
      <!-- 加载中显示内容 -->
      <template v-slot:fallback>
        <p>加载中......</p>
      </template>
    </Suspense>
  </div>
</template>

<script>
// 静态引入组件
// import Child from './components/Child' 

// 异步引入组件
import { defineAsyncComponent } from 'vue';
const Child = defineAsyncComponent(() => import('./components/Child.vue'))

export default {
  name: 'App',
  components: {
    Child
  },
}
</script>

<style>
.app {
  background-color: #00eeee;
  padding: 20px;
}
</style>

```

------

# vue3其它改变

### 1.全局API的转移

- Vue 2.x 有许多全局 API 和配置。

  - 例如：注册全局组件、注册全局指令等。

    ```js
    //注册全局组件
    Vue.component('MyButton', {
      data: () => ({
        count: 0
      }),
      template: '<button @click="count++">Clicked {{ count }} times.</button>'
    })
    
    //注册全局指令
    Vue.directive('focus', {
      inserted: el => el.focus()
    }
    ```

- Vue3.0中对这些API做出了调整：

  - 将全局的API，即：```Vue.xxx```调整到应用实例（```app```）上

    | 2.x 全局 API（```Vue```） | 3.x 实例 API (`app`)                        |
    | ------------------------- | ------------------------------------------- |
    | Vue.config.xxxx           | app.config.xxxx                             |
    | Vue.config.productionTip  | <strong style="color:#DD5145">移除</strong> |
    | Vue.component             | app.component                               |
    | Vue.directive             | app.directive                               |
    | Vue.mixin                 | app.mixin                                   |
    | Vue.use                   | app.use                                     |
    | Vue.prototype             | app.config.globalProperties                 |
    
    ```javascript
    // main.js=============
    app.config.globalProperties.$API = API // 挂载全局属性
    
    // 组件使用
    import { getCurrentInstance } from "vue";
    const app = getCurrentInstance();
    app.appContext.config.globalProperties.$API
    ```
    
    

### 2.其他改变

- data选项应始终被声明为一个函数。

- 过度类名的更改：

  - Vue2.x写法

    ```css
    .v-enter,
    .v-leave-to {
      opacity: 0;
    }
    .v-leave,
    .v-enter-to {
      opacity: 1;
    }
    ```

  - Vue3.x写法

    ```css
    .v-enter-from,
    .v-leave-to {
      opacity: 0;
    }
    
    .v-leave-from,
    .v-enter-to {
      opacity: 1;
    }
    ```

- <strong style="color:#DD5145">移除</strong>keyCode作为 v-on 的修饰符，同时也不再支持```config.keyCodes```

- <strong style="color:#DD5145">移除</strong>```v-on.native```修饰符（vue2中用native标明自定义事件）

  - 父组件中绑定事件

    ```vue
    <my-component
      v-on:close="handleComponentEvent"
      v-on:click="handleNativeClickEvent"
    />
    ```

  - 子组件中声明自定义事件

    ```vue
    <script>
      export default {
        emits: ['close']
      }
    </script>
    ```

- <strong style="color:#DD5145">移除</strong>过滤器（filter）

  > 过滤器虽然这看起来很方便，但它需要一个自定义语法，打破大括号内表达式是 “只是 JavaScript” 的假设，这不仅有学习成本，而且有实现成本！建议用方法调用或计算属性去替换过滤器。

- ......





# 其它

*//* *关闭规范检测*  lintOnSave: false,





























