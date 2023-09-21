# HTML基本了解

## HTML介绍

超文本标记语言（HyperText Markup Language）是一种标记语言，用于**<u>描述和定义网页的结构和内容</u>。**

主要包括：

1. 网页结构：HTML 可以定义网页的结构，如标题、段落、列表、表格等，使得网页的结构更加清晰和有序。
2. 文本格式：HTML 可以定义文本的格式，如字体、颜色、大小、加粗、斜体等，使得网页的文本内容更加丰富和有吸引力。
3. 图像和媒体：HTML 可以插入图像、音频、视频等媒体元素，丰富网页内容，提供更好的用户体验。
4. 链接：HTML 可以创建超链接，使得不同网页之间可以相互跳转，方便用户访问相关信息。
5. 表单：HTML 可以创建表单元素，如文本框、下拉框、单选框、复选框等，用于收集用户的输入和提交数据。
6. 其他元素：HTML 还可以定义网页的其他元素，如注释、特殊符号、meta 标签等。

## HTML基本骨架

```html
<!-- 使用HTML5 -->
<!DOCTYPE html>
<!-- 中文-中国  -->
<html lang="zh-CN">
  <head>
      <!-- 字符编码  -->
      <meta charset="UTF-8">
      <!-- 兼容ie -->
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <!-- 兼容移动端 -->
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
     
      <title>我的网页</title>
  </head>
  <body>
    <h1>欢迎访问我的网页</h1>
    <p>这是一个演示网页。</p>
  </body>
</html>
```

- `<!DOCTYPE html>`: ***文档类型声明***，表示这个文档使用 HTML5 规范。

- `<html>`: HTML 根元素，包含了整个 HTML 文档。可***设置语言***。

- `<head>`: HTML 文档头部，用于引入外部样式表、脚本、图标等资源。

- `<meta>`: 用于指定文档的编码方式和视口（viewport）设置。（meta元信息）

    ```html
    <!-- 网页关键字、SEO优化 -->
    <meta name="keywords" content="8-12个字符以英文逗号隔开的单词/词语">
    <!-- 网页描述、SEO优化 -->
    <meta name="description" content="80个字符描述网页">
    <!-- 爬虫:all等价：【index(允许)、follow(跟随网页)】、none等价：【noidex(不允许)、nofollow(不跟随网页)】 、noarchive(不缓存网页内容)-->
    <meta name="robots" content="index">
    ```

- `<title>`: 网页标题，将显示在浏览器标签页上。

- `<body>`: HTML 主体元素，包含了页面的主要内容。

## HTML标签种类

### 排版标签

```html
<!-- 标题标签 -->
<!-- 独占一行 字体加粗 -->
<h1>标题标签</h1>
<h2>标题标签</h2>
<h3>标题标签</h3>
<h4>标题标签</h4>
<h5>标题标签</h5>
<h6>标题标签</h6>
```
```html
<!-- 段落标签 -->
<!-- 独占一行，每个段落存在间隙 -->
<p>段落标签1</p>
<p>段落标签2</p>
```

```html
<!-- 强制换行标签 -->
<br>
<!-- 水平线标签 -->
<hr>
```

```html
<!-- 预格式化文本 -->
<pre>
   /\_/\  
  ( o.o ) 
   > ^ <
</pre>
```



### 文本标签

**修饰标签中的文本、修饰语义、语气的强弱**

```html
<!-- 后者的语气都强于前者。PS：h标签强于加粗效果 -->

<b>加粗</b>
<strong>加粗</strong>

<u>下划线</u>
<ins>下划线</ins>
    
<i>倾斜</i>
<em>倾斜</em>

<s>删除线</s>
<del>删除线</del>


<!-- 不常用的文本标签 -->
<cite>作品标题（书籍、歌曲、电影、绘画、雕塑...）</cite>
<dfn>特殊术语、专属名词</dfn>
<ins>插入文本</ins>
<del>删除文本</del>
<sub>上标</sub>
<sup>下标</sup>
<code>一段代码</code>
<samp></samp>
<kbd>键盘文本，表示文本是键盘输入的、一般用于计算机使用手册</kbd>
<abbr>缩写，配合title属性</abbr>
<bdo>文本方向、配合属性dir，选值：ltr（默认）、rtl，你是年少的欢喜</bdo>
<var>标记变量，配合code使用</var>
<small>附属细则:版权、法律文本</small>
<b>摘要关键字、产品名称</b>
<i>人的思想、话语，今多用于字体图标</i>
<u>下划线</u>
<q>短引用</q>
<!-- 块级标签 -->
<blockquote>长引用，格式：换行、段前缩进</blockquote>
<address>地址信息、联系信息...</address>
```



### 媒体标签

```html
<!-- 图片标签
	1. 从左到右排列多个图像
	2. src：图片路径
	3. alt：图片未加载文本提示、SEO优化
 	4. title：鼠标悬停文本提示
	5. width和height：单位px，单独设置一个则等比例修改图片大小 
	6. 行内标签 -->
<img src="./../img/橘猫.jpeg" alt="图片加载失败！" title="等比例缩小猫" width="50px">

<!-- 音频标签
	src: 音频路径
	controls:显示播放控件 
	autoplay：自动播放控件，大部分不支持（静音播放）
	loop：循环播放
	播放格式支持：MP3、Wav、Ogg -->
<audio src=""></audio>

<hr>
<!-- 视频标签
	src: 视频路径
	controls:显示播放控件
	autoplay：自动播放控件，大部分不支持 谷歌浏览器需要配合muted实现静音播放
	loop：循环播放
 	播放格式支持：MP4 、WebM 、Ogg -->
<video src="" controls autoplay muted></video>
```

图片格式

| 常见格式 | 特点                                                         |
| -------- | ------------------------------------------------------------ |
| jpg      | 有损压缩，支持颜色丰富；不支持透明背景、动态图；占用空间小   |
| png      | 无损压缩，支持颜色丰富、透明背景；不支持动态图；占用空间略大 |
| bmp      | 不进行压缩，支持颜色丰富、保留细节多；不支持动态图、透明背景；占用空间极大 |
| gif      | 支持256种颜色，色彩一般、支持简单透明背景、动态图            |
| webp     | 兼容性不好、谷歌推出专门在网页呈现的图片格式，具备上述格式优点 |
| base64   | 一串特殊文本、将图片进行base64编码，用img的src属性借助浏览器查看 |

### 列表标签

| css样式属性 | 黑心圆 | 空心圆 | 小黑方 |
| :---------- | ------ | ------ | ------ |
| list-style  | disc   | circle | square |

```html

<!-- 有序列表 -->
<h3>有序列表-type="i"</h3>
<!--type取值： 1，a，A，小罗马i，大罗马Ⅰ -->
<ol type="i">
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
    <li>jQuery</li>
    <li>Vue.js</li>
</ol>

<!-- 无序列表 -->
<ul type="circle">
    <li>诡秘之主</li>
    <li>佛本是道</li>
    <li>诛仙</li>
</ul>

 <!-- 自定义列表
	  以 <dl> 标签开始。
      自定义列表项以 <dt> 开始。
	  自定义列表项的定义以 <dd> 开始 -->
<h3>自定义列表</h3>
<dl>  
    <dt>小说</dt>  
    <dd>诡秘之主</dd>
    <dd>诛仙</dd>
    <dd>佛本是道</dd>
    <dt>角色</dt>  
    <dd>克莱恩</dd>
    <dd>碧瑶</dd>
    <dd>周青</dd>
</dl>

```

### 表格标签

#### 基本标签

```html
<!-- 基本标签 
    1. table：表格整体
    2. tr：行
    3. td：每行的一项
    4. caption：表格大标题，默认居中顶部显示
    5. th：表格单元头，通常使用于第一行，默认文字加粗居中 -->
<table border="2" cellspacing="0" >
    <caption><strong>小说</strong></caption>
   <tr>
       <th>书名</th>
       <th>作者</th>
       <th>类型</th>
   <tr>
       <td>诡秘之主</td>
       <td>爱潜水的乌贼</td>
       <td>西方玄幻</td>
   </tr>
   <tr>
       <td>龙族</td>
       <td>江南</td>
       <td>西方魔幻</td>
   </tr>
</table>
```

#### 表格结构

```html
 <!-- 表格结构标签 -->
    <!-- 
        thead：表格头部
        tbody：表格主题
        tfoot：表格底部
     -->
<h3>表格的结构</h3>
<table border="2" cellspacing="0" >
	<caption><strong>小说</strong></caption>
	<thead hright="30" align="center" valign="middle" >
		<tr>
			<th>书名</th>
			<th>作者</th>
			<th>类型</th>
			<th>是否完结</th>
			<th>是否推荐</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>龙族Ⅲ黑月之潮</td>
			<td>江南</td>
			<td>西方魔幻</td>
			<td>否</td>
			<td>否</td>
		</tr>
		<tr>
			<td>诡秘之主</td>
			<td>爱潜水的乌贼</td>
			<td>西方玄幻</td>
			<td>是</td>
			<td>是</td>
		</tr>
		<tr>
			<td>诛仙</td>
			<td>萧鼎</td>
			<td>东方修仙</td>
			<td>是</td>
			<td>是</td>
		</tr>
	</tbody>
	<tfoot>
		<tr>
			<td>评价</td>
			<td>还行</td>
			<td>可以</td>
			<td>不错</td>
		</tr>
  	</tfoot>
</table>
```

#### 表格合并

```html
<table border="2">
    <caption><strong>小说</strong></caption>
    <thead>
        <tr>
            <th>书名</th>
            <th>作者</th>
            <th>类型</th>
            <th>是否完结</th>
            <th>是否推荐</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>龙族Ⅲ黑月之潮</td>
            <td>江南</td>
            <td>西方魔幻</td>
            <!-- 跨列合并，删除合并列 -->
            <td colspan="2">否</td>
            <!-- <td>否</td> -->
        </tr>
        <tr>
            <td>诡秘之主</td>
            <td>爱潜水的乌贼</td>
            <td>西方玄幻</td>
            <!-- 跨行合并，删除合并行 -->
            <td rowspan="2">是</td>
            <td>是</td>
        </tr>
        <tr>
            <td>诛仙</td>
            <td>萧鼎</td>
            <td>东方修仙</td>
            <!-- <td>是</td> -->
            <td>是</td>
        </tr>
    <tfoot>
        <tr>
            <td>评价</td>
            <td>还行</td>
            <td>可以</td>
            <td>不错</td>
            <td>还好</td>
        </tr>
    </tfoot>
    </tbody>
</table>
```

### 表单标签

#### form表单标签综合

```html
<!-- from标签
	1. 可以是所有表单标签表演的舞台。
	2. action：数据提交地址
	3. target：_self,_bank
	4. method：get、post
	PS：https://www.baidu.com/s?wd=代码
-->
<form action="https://www.baidu.com/s" target="_self" >
	<input type="text" name="wd" >
	<button>搜索</button>
</form>


<form action="https://search.jd.com/search">
    <fieldset>
        <legend>登录信息</legend>
        账号：<input type="text" name="account" value="zhangsan" maxlength="10"><br>
        密码：<input type="password" name="account" value="123456" maxlength="6"><br>
    </fieldset>
    <fieldset>
        <legend>附加信息</legend>
        性别：
        <label for="1">
            <input id="1" type="radio" name="gender" value="male" checked>男
        </label>
        <label for="2">
            <input id="2" type="radio" name="gender" value="famale">女
        </label>
        <br>
        爱好：
        <input type="checkbox" name="hobby" value="read">阅读
        <input type="checkbox" name="hobby" value="sing">唱歌
        <input type="checkbox" name="hobby" value="draw">绘画
        <input type="checkbox" name="hobby" value="dance">跳舞
        <!-- 隐藏域 -->
        <input type="hidden" name="vsinger" value="yzl">
        <br>
        其他：
        <textarea cols="29" rows="5" name="info">默认内容</textarea>
        <br>
        小说：
        <select name="book">
            <optgroup label="分组1">
                <option value="1">诡秘之主</option>
                <option value="2">诛仙</option>
                <option value="3" selected>龙族</option>
            </optgroup>
            <optgroup label="分组2">
                <option value="value3">选项3</option>
                <option value="value4">选项4</option>
            </optgroup>
        </select>
    </fieldset>
    <br>
    <input type="submit" value="提交">
    <input type="reset" value="重置">
    <input type="button" value="空按钮">
</form>
```



#### input标签(文本和密码)

```html
 <!-- 文本框 text -->
 <!-- 密码框 password 默认隐藏-->
 <!-- 属性：
        placeholder : 占位符，用户输入框提示文字
        value ： 用户输入内容，传递给后端
        name ：说明用户输入内容代表什么，给后端,允许重复
        id ：唯一标识
	    maxlength ：输入字符长度限制
		disable：禁止用户编辑
 -->
<table border="2">
	<caption><strong>login</strong></caption>
	<tr>
		<td>username</td>
		<td><input type="text" placeholder="输入昵称"></td>
	</tr>
	<tr>
		<td>password</td>
		<td><input type="password" placeholder="输入密码"></td>
	</tr>
</table>
```

#### input标签(单选和多选)

```html
	<!-- 单选框 radio -->
	<!-- 多选框 checkbox -->
	<!-- 
		name: 联合单选框实现单选
		checked：默认选中 
		value：from收集的数据
		type: radio、checkbox
    -->
    <div>
        男：<input type="radio" name="gender" value="male" checked>&nbsp;
        女：<input type="radio" name="gender" value="female">
    </div>
    <div>
        阅读<input type="checkbox" name="hobby" id="1">
        下棋<input type="checkbox" name="hobby" id="2">
        运动<input type="checkbox" name="hobby" id="3">
    </div>
```

#### input标签(文件选择)

```html
<!-- 文件选择 file，multiple : 多文件选择-->
<input type="file" name="" id="" multiple>
```

#### input标签(表单按钮)

```html
<!-- 
	type="submit"：提交按钮
	type="reset"：重置按钮
	type="button"：普通按钮，button无意义
	value：此时value可以设置按钮名称，默认type的取值
-->
<!-- 
	表单域标签form实现input属性submit、reset域作用范围
	action：传值地址
-->
<form action="">
	<table border="2">
		<caption><strong>login</strong></caption>
		<tr>
			<td>username</td>
			<td><input type="text" placeholder="输入昵称"></td>
		</tr>
		<tr>
			<td>password</td>
			<td><input type="password" placeholder="输入密码"></td>
		</tr>
		<tr>
			<td><input type="submit"></td>
			<td><input type="reset"></td>
		</tr>
	</table>
	<input type="button" value="空按钮">
</form>
```

#### button标签(按钮)

***在form里，button默认为submit，使用普通按钮记得设置type为button***

```html
  <button type="reset">重置</button>
  <button type="submit">提交</button>
  <button type="button">普通按钮</button>
```

#### select标签(下拉菜单)

```html
<!-- 
	select标签 下拉菜单整体 
	option标签 下拉菜单的每一项
	optgroup标签 选项分组 -->
<!--  
    selected: 默认选中
	multiple: 控制select是否可多选
    没有selected属性，默认第一个option标签显示
 -->
<select name="" id="">
    <optgroup label="分组1">
    	<option value="1">诡秘之主</option>
    	<option value="2">诛仙</option>
    	<option value="3" selected>龙族</option>
    </optgroup>
    <optgroup label="分组2">
    	<option value="value3">选项3</option>
    	<option value="value4">选项4</option>
  </optgroup>
</select>
```

#### textare标签(文本域)

```html
<!-- 
	cols:宽
	rows:高
	name:指代文本域的输入，便于传递给后端
	文本域框用户可以拉动，一般用css禁止(resize: none)
	value: 可设置默认输入值
-->
    <textarea name="" id="" cols="30" rows="10"></textarea>
```

#### label标签(绑定表单)

```html
<!-- 用于绑定内容与表单标签的关系 -->

<!-- 实现点击文字也可以实现选中单选多选按钮-->
<!--
	方法一、困难
	1、label包裹文字 
	2、input id属性和label for属性值一样 
-->
  性别：
<input type="radio" name="gender" id="1">
<label for="1">男</label>
<input type="radio" name="gender" id="2">
<label for="2">女</label>

<!--
	方法二、简单
	1、label包裹input标签和文字
	2、label for 和 label id 属性值一样
-->
<label for="3">
    <input type="radio" name="coloer" id="3">yellow
</label>
<label for="4">
    <input type="radio" name="coloer" id="4">skyblue
</label>

```

fieldset与legend标签

```html
<form action="https://search.jd.com/search">
	<fieldset>
		<legend>登录信息</legend>
		账号：<input type="text" name="account" value="zhangsan" maxlength="10"><br>
		密码：<input type="password" name="account" value="123456" maxlength="6"><br>
	</fieldset>
</form>
```



### 超链接

```html
<!-- 当前页面打开
	1. href：跳转位置，可以是外部网站、内部文件、内部段落锚点、唤起应用
	2. name：跳转锚点，锚点值唯一（H5推荐：其它标签没有name属性可用id属性）
	3. target：跳转方式，_bank（默认当前页面打开），_self（新建页面打开）
	4. 具有默认样式：颜色、下划线 -->

<!-- 跳转到：外部链接 -->
<a href="https://www.baidu.com" target="_self">百度</a>
<!-- 跳转到：回到顶部 | 刷新页面 -->
<a href="#" target="_bank">回到顶部</a>
<a href="" target="_bank">刷新页面</a>
<a href="" target="_bank">刷新页面</a>
<a href="javascript:;">阻止跳转</a>
<a href="javascript:alert(1);">执行代码</a>
<!-- 跳转到：锚点 -->
<a href="#comehere">去那里？</a>
<a name="comehere">来这里！</a>
<!-- 唤起应用 -->
<a href="tel:10010">电话</a>
<a href="mailto:1612437886@qq.com">邮箱</a>
<a href="sms:10086">短信</a>
```

### 标签语义

```html
 <!-- html 5 新增-->
 <!-- 有语义标签 -->
<header>网页头部header</header>
<nav>网页导航nav</nav>
<aside>网页侧边栏aside</aside>
<section>网页区块section</section>
<article>网页文章article</article>
<footer>网页底部footer</footer>

 <!-- 无语义标签 -->
<div>div独占一行</div>
<span>span一行</span>
<span>span一行</span>
```

### 框架标签

```html
<!-- 框架标签
    1. width、height：设置宽高
    2. frameborder：设置边框
    3. 应用：嵌入广告、其他内容
-->
<!-- 1. 嵌入一个网页 -->
<iframe src="https://www.taobao.com" width="900" height="300" frameboder="0"></iframe>

<!-- 2. 配合a链接 --><br>
<a href="https://www.taobao.com" target="container1">看淘宝</a>
<a href="https://www.toutiao.com" target="container1">看头条</a>
<iframe name="container1" width="900" height="300" frameboder="0"></iframe>

<!-- 3. 配合表单 -->
<form action="https://so.toutiao.com/search" target="container2">
    <input type="text" name="keyword">
    <input type="submit" value="搜索">
</form>
<iframe name="container2" width="900" height="300" frameboder="0"></iframe>
```

## HTML字符实体

```html
=============基本字符实体：===============
&lt;：小于号 (<)
&gt;：大于号 (>)
&amp;：和号 (&)
&quot;：双引号 (")
&apos;：单引号 (') - 在HTML5中，通常不建议使用，而是使用&rsquo;或&lsquo;来表示左/右单引号。
=============特殊字符实体：================
&nbsp;：非断空格 (常用于在HTML中创建额外的空格)
&copy;：版权符号 ©
&reg;：注册商标 ®
&trade;：商标 ™
&deg;：度符号 °
&micro;：微符号 µ
&euro;：欧元符号 €
&pound;：英镑符号 £
&yen;：日元符号 ¥
&cent;：美分符号 ¢
===========其他常见字符实体：===============
&aacute;：小写字母á
&egrave;：小写字母è
&uuml;：小写字母ü
&Ouml;：大写字母Ö
&frac12;：分数½
&times;：乘号 ×
&divide;：除号 ÷
&sup2;：上标²
&sup3;：上标³
&le;：小于等于 ≤
&ge;：大于等于 ≥
```

## HTML全局属性

------

# HTML5

1.  针对 JavaScript ，新增了很多可操作的接口。 
2.  新增了一些语义化标签、全局属性。 
3.  新增了多媒体标签，可以很好的替代 flash 。
4.  更加侧重语义化，对于 SEO 更友好。 
5.  可移植性好，可以大量应用在移动设备上。

兼容性：IE 浏览器必须是 9 及以上版本才支持 HTML5 ，且 IE9 仅支持部分 HTML5 新特性。

------

## HTML5新语义化标签

### 布局标签

| 标签名    | 语义                                            | 双标签 |
| --------- | ----------------------------------------------- | ------ |
| `header`  | 部分区域的头部                                  | T      |
| `footer`  | 部分区域的尾部                                  | T      |
| `nav`     | 导航                                            | T      |
| `article` | 文章、帖子、杂志、新闻、博客、评论等            | T      |
| `section` | `article`标签中的某段文字，通常该段文字含有标题 | T      |
| `aside`   | 侧边栏                                          | T      |
| `main`    | 文档主要内容                                    | T      |
| `hgroup`  | 包裹连续的标题。如主副标题组合。                | T      |

------

### 状态标签

**1. `mater`：定义已知范围内的标量测量（又称`gauge`尺度）。双标签，用例：电量，磁盘用量等。**

| 属性      | 值   | 描述       |
| --------- | ---- | ---------- |
| `high`    | 数值 | 规定高值   |
| `low`     | 数值 | 规定低值   |
| `max`     | 数值 | 规定最大值 |
| `min`     | 数值 | 规定最小值 |
| `optimum` | 数值 | 规定最优值 |
| `value`   | 数值 | 规定当前值 |

**2.`progress`：表示任务的进度条指示器，双标签。**

| 属性    | 值   | 描述       |
| ------- | ---- | ---------- |
| `max`   | 数值 | 规定目标值 |
| `value` | 数值 | 规定当前值 |

------

### 列表标签

| 标签名     | 语义                                        | 双标签 |
| ---------- | ------------------------------------------- | ------ |
| `datalist` | 搜索框关键词提示                            | T      |
| `details`  | 解释标签、解释名词，展示答案                | T      |
| `summary`  | 写在 details 的里面，用于指定问题或专有名词 | T      |

```html
<input type="text" list="mydata">
<datalist id="mydata">
	<option value="周冬雨">周冬雨</option>
	<option value="周杰伦">周杰伦</option>
	<option value="温兆伦">温兆伦</option>
	<option value="马冬梅">马冬梅</option>
</datalist>
```

```html
<details>
	<summary>想听什么歌？</summary>
	<p>随便</p>
</details>

```

------

### 文本标签

| 标签名 | 语义                                           | 双标签 |
| ------ | ---------------------------------------------- | ------ |
| ruby   | 包裹需要注音文字                               | T      |
| rt     | 包裹在ruby标签里，填写注音                     | T      |
| mark   | 标记，W3C 建议 mark 用于标记搜索结果中的关键字 | T      |

```html
<ruby>
	<span>喂</span>
	<rt>wèi</rt>
</ruby>
```

------

### 表单标签

| 表单新属性     | 功能                                                         |
| -------------- | ------------------------------------------------------------ |
| `placeholder`  | 设置文本输入框提示文字                                       |
| `required`     | 用户输入必填项，适用于按钮外其它控件                         |
| `autofocus`    | 自动获取输入框焦点，按钮也适用但视觉效果不美观               |
| `autocomplete` | 自动完成，类似输入历史记录提示，推荐使用于文字输入（`type="text"`）表单控件 |
| `pattern`      | 设置正则表达式，校验输入内容。通常与`required`属性配合       |

| input标签新属性 | 功能                                                         |
| --------------- | ------------------------------------------------------------ |
| email           | **邮箱**类型输入限制，提交表单空输入不验证，建议配合`required`属性 |
| url             | **url**类型输入限制，提交表单空输入不验证，建议配合`required`属性 |
| number          | **数字**类型输入限制，提交表单空输入不验证，建议配合`required`属性 |
| search          | **数字**类型输入限制，提交表单不验证输入                     |
| tel             | **电话**类型输入限制，提交表单不验证输入，移动端唤起数字键盘 |
| range           | **范围选择**，默认值50，提交表单不验证输入                   |
| color           | **颜色选择**框，默认值黑色，可以选择多种颜色格式，提交表单不验证输入 |
| date            | **日期选择**框，默认值为空，提交表单不验证输入。             |
| mounth          | **月份选择**框，默认值为空，提交表单不验证输入。             |
| week            | **周选择**框，默认值为空，提交表单不验证输入。               |
| time            | **时间选择**框，默认值为空，提交表单不验证输入。             |
| datetime-local  | **日期+时间选择**框，默认值为空，提交表单不验证输入。        |

| form标签新属性 | 功能                                           |
| -------------- | ---------------------------------------------- |
| novalidate     | form标签添加该属性，表单提交的时候不再进行验证 |

------

### 媒体标签

**1.`vedio`：双标签，定义视频。**

| 属性          | 值                     | 描述                                                         |
| ------------- | ---------------------- | ------------------------------------------------------------ |
| src           | URL地址                | 视频地址                                                     |
| width、height | 像素值                 | 设置宽高                                                     |
| controls      |                        | 显示视频控件（播放暂停等）                                   |
| muted         |                        | 静音                                                         |
| autoplay      |                        | 自动播放                                                     |
| loop          |                        | 循环播放                                                     |
| poster        | URL地址                | 视频封面                                                     |
| preload       | auto / metadata / none | none:不加载视频，metadata：仅获取视频元数据(如时长)，auto：浏览器自适应预加载。 |

**2.`audio`:双标签，定义音频。**

| 属性     | 值                     | 描述                                                         |
| -------- | ---------------------- | ------------------------------------------------------------ |
| src      | URL地址                | 音频地址                                                     |
| controls |                        | 显示音频控件（播放暂停等）                                   |
| autoplay |                        | 自动播放                                                     |
| muted    |                        | 静音                                                         |
| loop     |                        | 循环播放                                                     |
| preload  | auto / metadata / none | none:不加载音频，metadata：仅获取元数据(如时长)，auto：浏览器自适应预加载。 |

### 全局属性

配合js才能达到较好的应用。

| 属性名          | 取值       | 功能                                               |
| --------------- | ---------- | -------------------------------------------------- |
| contenteditable | true\false | 元素是否可编辑                                     |
| draggable       | true\false | 元素是否可移动                                     |
| hidden          |            | 隐藏元素                                           |
| spellcheck      | true\false | 元素是否进行拼写和语法检查                         |
| contextmenu     |            | 规定元素的上下文菜单，在用户鼠标右键点击元素时显示 |
| data-*          |            | 用于存储页面的私有定制数据                         |



------

# CSS基本了解

## CSS基本介绍

Cascading Style Sheets，层叠样式表，简称CSS。是一种用于**<u>描述网页样式和布局的标记语言</u>**。与 HTML 相对应，HTML 负责网页的结构和内容，而 CSS 则负责网页的外观和风格。通过 CSS，我们可以指定网页中各种元素（例如文本、链接、图像等）的颜色、字体、大小、边距、背景等样式，从而实现网页的美化和优化。

- 

### **CSS特性**：

**三大特性：**

- **层叠性**：CSS 样式可以按照特定的规则进行叠加和覆盖，使得网页的样式更加灵活和可控。
- **继承性**：CSS 样式可以被子元素继承，使得网页样式的定义更加简洁和高效。
- **优先级**：①：`!important > 内联样式>ID 选择器 > 类选择器、属性选择器、伪类选择器 > 元素选择器、伪元素选择器 > 继承`②：权重计算
- **CSS其他特性**
    - 分离性：CSS 将网页的内容和样式分离，使得网页结构和样式可以独立开发和维护，提高了代码的可读性和可维护性。
    - 适应性：CSS 样式可以根据不同的设备、屏幕大小和浏览器自动适应，使得网页在不同的设备和环境下都可以保持良好的显示效果。

#### 继承性

```css
/* 继承 
 一般文字控制属性可以继承
  1. color
  2. font-style、font-weight、font-size、font-family
  3. text-indent、text-align
  4. line-height */

/* 不能继承
  1. 添加了!important不能继承给后代 
  2. 与盒子属性有关系的、布局属性、背景、边框、外边距、宽高、溢出方式
*/

```

#### 层叠性

```css
/* 同一个标签不同样式 层层叠加 作用标签上 */
/* 选择器优先级相同，才能判断重叠性结果 */
```



### CSS语法

CSS 的语法包括选择器、属性和值三个部分：（选择器、声明块`{属性名：属性值;}`）

选择器：用于选择网页中的元素
属性：用于指定元素的样式
值用：于指定属性的取值

**css代码风格**：展开风格（代码开发）、紧凑风格（项目上线）

### CSS引入

**优先级：**`内联样式 > 内部样式 > 外部样式`

**内联样式**（行内样式）：在 HTML 标签中通过 `style` 属性指定 CSS 样式，例如:

```html
<h1 style="color: red; font-size: 24px;">Hello, World!</h1>
```

**内部样式**：在 HTML 文件头部使用 `<style>` 标签定义 CSS 样式，例如：

```html
<head>
 	<style>
		h1 {
      		color: red;
      		font-size: 24px;
    		}
  	</style>
</head>
<body>
  	<h1>Hello, World!</h1>
</body>
```

**外部样式**：将 CSS 样式定义在一个独立的 CSS 文件中，然后在 HTML 文件中通过 `<link>` 标签引入，例如：

```html
<head>
	<!-- 注意多个css样式引入顺序：覆盖、继承 -->
    <!-- rel 表示“关系 (relationship) ” -->
  	<link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
  	<h1>Hello, World!</h1>
</body>

```



------

## CSS常见选择器

#### 常见选择器

|      选择器      | 例子                     | 选择范围                             |
| :--------------: | ------------------------ | ------------------------------------ |
| 标签(元素)选择器 | `p { }`                  | 标签名相同                           |
|     类选择器     | `.className { }`         | 相同类名                             |
|     ID选择器     | `#id { }`                | 唯一指定的id                         |
|   通配符选择器   | `* { }`                  | 所有标签                             |
|    属性选择器    | `p [ type = "hhh" ] { }` | 相匹配拥有的`选择器`和`属性值`的元素 |
|                  | `p [ type^= "h" ] { }`   | 匹配type属性h开头的p标签             |
|                  | `[title] { }`            | 匹配拥有title属性的标签              |

------

#### 复合选择器

|   选择器   | 例子              | 选择范围                                  |
| :--------: | ----------------- | ----------------------------------------- |
| 后代选择器 | `div .pp { }`     | 浅层次到深层次的所有子级（儿子、孙子...） |
| 子代选择器 | `div > .pp { }`   | 浅层次子级，选择匹配的第一层（只是儿子）  |
| 并集选择器 | `div,p,.test { }` | 匹配 div、`.test `和 p 的元素             |
| 交集选择器 | `div.aa#bb { }`   | 同时满足多个选择器的元素(注意选择器顺序)  |

------

#### 动态伪类选择器

| 选择器         | 例子                         | 选择范围                  |
| -------------- | ---------------------------- | ------------------------- |
| 动态伪类选择器 | `a:hover { }`                | 元素的状态(鼠标悬停)      |
|                | `a:link { }` `a:vsitied { }` | 未访问、访问过的a标签状态 |
|                | `a:active { }` `a:focus { }` | 激活、聚焦的标签状态      |

------

#### 结构伪类选择器

ps：可以是一个`: `或者两个冒号`::`,CSS3之后才规定标准（为了区分伪类用一个、伪元素用两个）

| 结构伪类选择器          | 例子                      | 选择范围            |
| ----------------------- | ------------------------- | ------------------- |
| E:first-child(){ }      | li:first-child { }        | 第一个`li`标签      |
| E:last-child(){ }       | li:last-child { }         | 最后一个`li`标签    |
| E:nth-child(n){ }       | li:nth-child(2) { }       | 正序第2个`li`标签   |
| E:nth-child{-n+m }      | li:nth-child(-n+3) { }    | 正序前3个`li`标签   |
| E:nth-child{n+m }       | li:nth-child(n+3) { }     | 倒序到第3个`li`标签 |
| E:nth-last-child(n){ }  | li:nth-last-child(2) { }  | 倒序第2个`li`标签   |
| E:nth-last-child(2n){ } | li:nth-last-child(2n) { } | 倒序2*n个`li`标签   |

```html
<!--
	E:nth-child(n) 第几个li、括号里面不能为0，但n可以为0
        n         （0，1，2，3，...）
        2n even    (0,2,4,6,...)
        2n+1  odd  (1,3,5,...)
        -n+5       (5,4,3,2,1,0)
-->
```

## CSS特别选择器

### 否定伪类选择器

| 选择器                 | 匹配元素                                 |
| ---------------------- | ---------------------------------------- |
| `div>p:not(.fail) { }` | 匹配子代选择器后，除去拥有类名fail的元素 |

### UI伪类选择器

```css
/* 选中勾选的复选框 */
input:checked{
    width:100px;
    height:100px;
}
/* 选中禁用的input元素 */
input:disabled{
    background-color: #0ee;
}
/* 选中禁可用的input元素 */
input:enabled{
    background-color: #00e;
}
```

### 目标伪类选择器

```css
/* 选中锚点所指向的元素 */
<a href="#one"><a>
<a href="#two"><a>
<a href="#three"><a>
<a href="#four"><a>

<div class="one">第一个</div>
<div class="two">第二个</div>
<div class="three">第三个</div>
<div class="four">第四个</div>

div:target{
    background-color: green;
}
```

### 语言伪类选择器

```css
/* 选中相匹配语言类型的元素 */
div:lang(en){
    color:red;
}
:lang(zh-CN){
    color:green;
}
```

### 伪元素选择器

***什么是伪元素？***像是元素，但不是元素，是元素中的一些特殊位置

```css
/* 选中div中第一个文字 */
div::first-letter{
    color: yellow;
    font-size: 40px;
}
/* 选中div中第一行文字 */
div::first-line{
    background-color: #e88;
}
/* 选中div被鼠标选择的文字 */
div::selection{
    background-color: #e00;
    color: orange;
}
/* 选中input的提示文字 */
input::placeholder{
    color: skyblue;
}
/* p元素之前、之后添加子伪元素：实现鼠标复制金额不包括特殊文字符号 */
p::before{
    content: "￥";
}
p::after{
    content: ".00";
}
/**/
/**/
```



------

## css选择器优先级

1. !important > 内联样式 > ID 选择器 > 类选择器、属性选择器、伪类选择器 > 元素选择器、伪元素选择器 > 继承
2. `#id > .class > type`，权重：例子如下（最后是orange）

```html
<style>
 /* 行内样式 id选择器 类选择 标签 */
 /* (0, 1, 0, 1) */
     div #one {
      color: orange;
    }

    /* (0, 0, 2, 0) */
    .father .son {
      color: skyblue;
    }

    /* 0, 0, 1, 1 */
    .father p {
      color: purple;
    }
    
    /* 0, 0, 0, 2 */
    div p {
      color: pink;
    } 
</style>
```



## CSS属性

------

### 字体和文本

用于控制文本的外观和样式

| 字体样式     | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| font-style   | `字体风格：italic(斜体)，normal，(正常)，oblique(倾斜)`      |
| font-variant | `字体的变体："normal"（默认值）、"small-caps"（小型大写字母）` |
| font-weight  | `字体粗细，值100~1000无单位，具体看字体自身设计多少个等级，`<br />`(lighter)细100~300、(nrmoal)正常400~500、(bold、bolder)加粗600以上，一般700` |
| font-size    | `字体大小，数字+px，chrome浏览器最小12px，默认16px，0px字体消失` |
| line-height  | `行高，(字体高加上下边距)，数字+px / 倍数(当前font-size的倍数)` |
| font-family  | `字体类型（族），可设置多个备选字体，用逗号分隔，字体含有空格必须双引号包裹，`<br />`最后无衬线字体（sans-serif）类型打底（是一些字体类型的集合，可不用双引号包裹以示区分）` |

**font属性连写** `font : style、variant 、weight、size / line-height family`

| 文本样式\修饰   | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| color           | `文本颜色、transparent为透明`                                |
| text-indemt     | `文本缩进，(数字+px / 数字＋em)`                             |
| text-align      | `文本水平对齐方式，(父标签设置)，center left right`          |
| letter-spacing  | `字母间距，每一个单词、字符之间的间距，单位像素px，可以负值` |
| word-spacing    | `单词间距，通过空格区分，单位像素px，可以负值`               |
| text-decoration | `underline(下划线)、none(去除所有修饰线) > line-through(删除线)、overline (上划线),`<br />`可以搭配 dotted(虚线)、wavy(波浪线) 和指定颜色` |
| text-transform  | `capitalize(首大写)、lowercase(小写)、uppercase(大写)、none` |
| user-select     | none：文本不可选中                                           |

**属性连写** `font : style、variant 、weight、size / line-height family`

1.  字体大小、字体族必须写上、其它如果省略了相当于设置了默认值。
2.  不连写的单独写，单独写的不连写。
3.  如果同时设置了行高和font连写，注意覆盖问题。

`font-family`：如果属性值中包含空格、括号、引号等特殊字符，应该将整个属性值用双引号或单引号括起来

1. 衬线字体（serif），文字笔画粗细不均，并且首尾有笔锋装饰，报刊书籍中应用广泛，如 Times New Roman、Georgia 等。
2. 无衬线字体（sans-serif），文字笔画粗细均匀，并且首尾无装饰，网页中大多采用无衬线字体,如 Arial、Helvetica 等。
3. 等宽字体（monospace），每个字符的宽度相同，一般用于程序代码编写，有利于代码的阅读和编写，如 Courier、Consolas 等

------

### 列表属性

```css
ul{
    /* 每行列表li开头符号：none(无)、square(黑方)、decimal(数字) */
    list-style-type: none;
    /* 控制每行列表li开头符号位置：inside(里面) 、outside(外面)*/
    list-style-position: inside;
    /* 自定义列表li开头符号 */
    list-style-image: url();
}
ul{
    /* 复合写法、无序 */
    list-style: decimal inside;
}
```



------

### 背景和边框

#### 背景属性

用于控制 HTML 元素的背景色、图片、边框样式、宽度和颜色等。用于设置比较不重要的图，一般用img标签设置重要图片。

| 背景和边框属性        | 说明与设置方法                                               |
| --------------------- | ------------------------------------------------------------ |
| background-color      | 背景颜色。颜色名称、RGB 值、HEX 值等表示方式                 |
| background-image      | 背景图片。相对或绝对 URL 地址，也可以使用 data URI           |
| background-size       | 背景图片的大小。像素、百分比或关键字（如 "cover"、"contain" 等） |
| background-position   | 背景图片的位置。像素、百分比或关键字（如 "center"、"left" 等） |
| background-repeat     | 背景图片的重复方式。"repeat"（默认值）、"repeat-x"、"repeat-y" 和 "no-repeat" |
| background-attachment | 背景图片是否固定在视口中，可以使用 "scroll" 或 "fixed" 值。  |
| background-clip       | 背景图片的裁剪方式，可以使用 "border-box"、"padding-box" 或 "content-box" 值。 |

备注：

1. 默认透明，background-color: transparent; 
2. background-position：x  y;  水平 垂直
3. 连写背景属性需要注意顺序background：color image repeat position/size attachment
4. 要设置宽高 背景图片不会撑开容器

#### 背景图片缩放

```html
 <style>
        .box{
                width: 500px;
                height: 400px;
                background-color: #ee9999;
                /* 图 100 × 100 */
                background-image: url(./../img/cat.png);
                background-repeat: no-repeat;

                /* 1. 宽高等比缩放 */
                /* background-size: 300px; */
                background-size: 100%;

                /* 2. 包含 任一边缩放到盒子尺寸相同就停止 ，另一边停止缩放 可能留白 */
                /* background-size: contain; */

                /* 3. 覆盖 图片缩放到覆盖盒子 可能图片显示不全 */
                /* background-size: cover; */
        }
</style>
```

#### 背景颜色过度

```css
圆心扩散
/* 红椭圆填充黄色  */
background: radial-gradient(red, yellow);
/* 红正圆填充黄色 */
background: radial-gradient(circle, red, yellow);
/* 红60%，之后黄色 */
background: radial-gradient( red 60%, yellow 60%);
/* 圆的垂直半径，水平半径设置  */
background: radial-gradient(50px 50px, red 50px, black 100px);
/* at设置圆心位置，来达到控制方向 */
background: radial-gradient(at left top, red 50%, black 50%);
/* 圈圈套圈圈，循环 */
background: repeating-radial-gradient(red 20px, green 50px);

上到下
/* 颜色由红变黄  */
background: linear-gradient(red, yellow, #e88, #0ee);
/* 红色占60%，然后开始渐变红到黄。 然后黄色从60%开始，就会形成分割线 */
background: linear-gradient(red 60%, yellow 60%);
/* 到右，从左到右 */
background: linear-gradient(to right, red, yellow);
/* 到左，从右到左 */
background: linear-gradient(to left, red, yellow);
/* 到右下 */
background: linear-gradient(to right bottom, red, yellow); 
/* 旋转60度 */
background: linear-gradient(60deg, red, yellow); 
/* repeating-linear-gradient规定颜色范围，方便循环 */
background: repeating-linear-gradient(rgb(2, 2, 2) 5px, rgba(219, 18, 35, 0.144) 20px, transparent 20px, transparent 20px); 
```

------

#### 边框属性

| 边框属性          | 说明（其它元素也可以设置border属性）                         |
| ----------------- | ------------------------------------------------------------ |
| border-width      | 边框粗细，单位像素，没有默认值，可设置表格                   |
| border-left-width |                                                              |
| border-color      | 边框颜色，没有默认值，可设置表格                             |
| border-left-color |                                                              |
| border-style      | 边框风格，`solid（实线）dashed（虚）dotted（点）double（双实）`没有默认值，可设置表格 |
| border-left-style |                                                              |
| border            | 复合属性，不设顺序,可设置表格                                |
| border-left       |                                                              |
| table-layout      | 表格独有，控制列宽:auto（默认、自动）、fixed（固定）         |
| boder-spacing     | 表格独有，控制单元格间距，单位像素px                         |
| border-collappse  | 表格独有，合并相邻单元格边框: separate(默认)、collapse(合并)此时单元格间距无效 |
| empty-cells       | 表格独有，隐藏没有内容的单元格，show，hide，若合并单元格边框，视觉效果无效 |
| caption-side      | 表格独有，设置表格标题位置，button，top                      |



PS：表格独有，只能设置在table标签

```css
table{
	border: 2px soled green;
	width:500px;
    /* 表格独有控制列宽:auto（默认、自动）、fixed（固定） */
    table-layout: fixed;
    /* 表格独有控制单元格间距 */
    border-spacing:2px;
    /* 表格独有合并相邻单元格边框: separate(默认)、collapse(合并)*/
    border-collappse: collapse;
}
td,th{
    border: 2px soled orange;
}

```

------

### 动画和过渡

用于控制 HTML 元素的动画效果和过渡效果，实现简单的状态变化，比如颜色、大小、位置等的变化。

#### **缓动函数（easing functions）**

1. **linear**：线性缓动函数，动画速度保持恒定，没有加速或减速效果。元素以恒定的速度移动。

2. **ease**：默认的缓动函数，速度在动画开始和结束时较慢，中间时较快。使动画效果平滑。

3. **ease-in**：速度在动画开始时较慢，然后逐渐加速。动画开始时显得更平缓。

4. **ease-out**：速度在动画结束时逐渐减慢，使动画结束时较平缓。

5. **ease-in-out**：速度在动画开始和结束时都较慢，在中间时较快。使动画开始和结束时都较平缓，中间时变化较快。

6. **cubic-bezier(t1,t2,t3,t4)**：自定义贝塞尔缓动函数，通过控制四个点的位置来定义速度曲线。你可以自定义参数来实现更精细的控制。

    

------

#### **transition（过渡）**

**基本用法:** ` .element { transition: <属性> <持续时间> <缓动函数> <延迟时间> ; }`

- `<属性>`：表示要过渡的CSS属性。可以使用通配符 `all` 表示所有属性。
- `<持续时间>`：表示过渡的持续时间，以秒（s）或毫秒（ms）为单位。
- `<缓动函数>`：表示过渡的缓动函数，控制过渡速度曲线。
- `<延迟时间>`：表示过渡开始前的延迟时间，以秒（s）或毫秒（ms）为单位。

| 属性                       | 描述                                                         |
| -------------------------- | ------------------------------------------------------------ |
| transition-property        | 设置需要**过渡的属性**，多个属性逗号分隔，不是所有属性都可过渡（取值不为数值） |
| transition-duration        | 设置过渡的**持续时间**，单位：s（秒）、ms毫秒，多个属性可以设置多个时间逗号分隔 |
| transition-delay           | 设置过渡的**延迟时间**，单位：s（秒）、ms毫秒，**必须设置在持续时间后面** |
| transition-timing-function | 设置过渡的**缓动函数**                                       |



```html
<style>
	/* 使用常用的缓动函数 */
    /* 1. 线性缓动函数，速度恒定，适用于简单的移动、渐变等效果 */
	.element { transition: all 1s linear; }
	/* 2. 默认的缓动函数，速度在开始和结束时较慢，适用于平滑的过渡效果 */
	.element { transition: all 1s ease; }
	/* 3. 速度在开始时较慢，适用于某些需要元素一开始就平缓出现的情况 */
	.element { transition: all 1s ease-in; }
	/* 4. 速度在结束时较慢，适用于某些需要元素平缓离开的情况 */
	.element { transition: all 1s ease-out; }
	/* 5. 速度在开始和结束时都较慢，适用于需要平缓的进入和退出效果 */
	.element { transition: all 1s ease-in-out; }
	/* 6. 使用自定义贝塞尔缓动函数，控制四个点的位置来定义速度曲线 */
	.element { transition: all 1s cubic-bezier(0.25, 0.1, 0.25, 1); }
    /* 7. 多个属性过度 */
    .element { 
        transition: 
        	background-color 1s cubic-bezier(0.25, 0.1, 0.25, 1),
        	width 0.5s linear; 
    }
	/* 初始阶段 */
    .element{   
        width: 30px;
        height: 400px;
        background-color: #0ee;
    }
    /* 结束阶段 */
    .element:hover { background-color: green; width:300px; }
</style>
<body>
    <!-- 实现悬停鼠标背景颜色、元素宽的过渡 -->
    <div class="element"></div>
</body>
```

`PS：` 通过opacity: 0;， overflow: hidden;，隐藏溢出的内容，实现li 标签中的文字随着高度的增加逐渐显示出来，而不是立即显示出来*

------

#### **animation（动画）**

**基本用法:**` .element {  animation: <动画名称> <持续时间> <缓动函数> <延迟时间> <重复次数> <方向> <填充模式>; }`

- `<动画名称>`：表示要应用的动画名称，是通过 `@keyframes` 定义的关键帧名称。
- `<持续时间>`：表示动画的持续时间，以秒（s）或毫秒（ms）为单位。
- `<缓动函数>`：表示动画的缓动函数，控制动画速度曲线。
- `<延迟时间>`：表示动画开始前的延迟时间，以秒（s）或毫秒（ms）为单位。
- `<重复次数>`：表示动画的重复次数，可以使用 `infinite` 表示无限次重复。
- `<方向>`：表示动画的播放方向，可以是 `normal`（默认）、`reverse`、`alternate` 或 `alternate-reverse`。
- `<填充模式>`：表示动画结束后的元素样式，可以是 `none`（默认）、`forwards`、`backwards` 或 `both`。

| 属性                      | 描述                                                         |
| ------------------------- | ------------------------------------------------------------ |
| animation-name            | 应用的动画名称                                               |
| animation-duration        | 动画持续时间，**必须写在延迟动画时间之前**                   |
| animation-timing-function | 动画的函数,越阶函数steps(步数)                               |
| animation-delay           | 动画延迟时间，避免刷新影响，得到完整的动画视觉效果           |
| animation-iteration-count | 动画重复次数，默认一直重复（infinite），取值为数值           |
| animation-direction       | 动画方向，默认normal，reverse(反转)，alternate(往复)，alternate-reverse(反转往复) |
| animation-fill-mode       | 动画停止位置、forwards(最后)、backwards(初始)                |
| animation-play-state      | 动画播放状态，paused（暂停）                                 |
| @keyframes                | 设置的动画名称                                               |
| from、to                  | 可以写为 0%、10%、50、%100%等                                |

------

`transform` 是CSS属性，用于对元素进行变换，包括平移、旋转、缩放和倾斜等操作。
`transform` 属性，可以在不改变元素文档流的前提下，改变元素的外观和位置。

1. **平移（translate）**：移动元素在水平和垂直方向上的位置。
2. **旋转（rotate）**：按照给定的角度旋转元素。
3. **缩放（scale）**：改变元素的尺寸大小。
4. **倾斜（skew）**：按照给定的角度在水平和垂直方向上倾斜元素。
5. **组合变换**：你可以将多个变换组合在一起。
6. **变换原点（transform-origin）**：设置变换的中心点，默认是元素的中心。

```html
<style>
    /* X轴平移（translateX）：移动元素在水平方向上的位置。 */
	@keyframes slide1 {
        	/* from、to 可以写为 0%、10%、50、%100%等 */
        	/*第一帧：可以不设置*/
		    from { transform: translateX(0); }
        	/*最后一帧*/
		    to { transform: translateX(100px); }
	}
	/* 平移（translate）：移动元素在水平和垂直方向上的位置。 */
	@keyframes slide2 {
		    from { transform: translate(50px, 20px); }
		    to { transform: translate(150px, 60px); }
	}
	/* 旋转（rotate）：按照给定的角度旋转元素。（360°） */
	@keyframes slide3 {
		    from {}
		    to { transform: rotate(360deg); }
	}
	/* 缩放（scale）：改变元素的尺寸大小。（放大1.5倍） */
	@keyframes slide4 {
		    from { transform: scale(0.5); }
		    to { transform: scale(1.5); }
	}
	/* 倾斜（skew）：按照给定的角度在水平和垂直方向上倾斜元素。 */
	@keyframes slide5 {
		from {}
         /* 水平方向倾斜20度，垂直方向倾斜-10度 */
		to { transform: skew(20deg, -10deg);}
	}
	/* 组合变换：你可以将多个变换组合在一起。 */
	@keyframes slide6 {
	    from {}
	    to { transform: translate(50px, 50px) rotate(45deg) scale(1.2); }
	}
    .element-base {
		display: inline-block;
		width: 50px;
		height: 50px;
		background-color: blue;
        /* 设置变换中心点在右下角 */
		transform-origin: bottom right;
	}
    /* 2秒的-slide7-动画，无限循环 */
    .element { animation: slide7 2s ease infinite; }
</style>
<body>
    <div class="element .element-base"></div>
</body>
```



------

### 布局

用于控制元素的位置、大小和布局

| 属性            | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| position        | 元素的定位方式，常用值包括static（默认值）、relative、absolute和fixed等 |
| display         | 元素的显示方式，常用值包括block、inline、inline-block、flex等。 |
| float           | 元素的浮动方式，常用值包括left、right等。                    |
| clear           | 清除浮动元素对布局的影响，常用值包括none、left、right和both等。 |
| box-sizing      | 控制元素的盒子模型，常用值包括content-box和border-box等。    |
| margin和padding | 元素的外边距和内边距，以控制元素之间的间距和布局效果。       |
| width和height   | 元素的宽度和高度，以控制元素的大小和布局效果。               |

------

#### 浮动float特点

1. 浮动元素会脱离标准流，覆盖标准流
2. 比标准流高半个级别
3. 下一个浮动元素会在上一个浮动元素左右浮动
4. 子元素浮动不能撑开父级，影响布局（父元素高度为0，子元素浮动）
5. 浮动比标准流高半个级别
6. 浮动元素变成类似行内块

#### 清除浮动

1. 单伪元素清除浮动影响
2. 双伪元素清除浮动影响
3. overflow清除浮动影响

```html
<style>
/* 父级标签加类清楚浮动影响 */
.clearfix::after{
    content: '';
    display: block;
    /* 清除左右浮动（块元素单独设置就可以解决浮动影响） */
    clear: both;
    /* 兼容性 网页隐藏伪元素 */
    height: 0;
    visibility: hidden;
}
    
/* 父级标签加类清楚浮动影响 */
.clearfix::before,
.clearfix::after{
    content: '';
    /*只有块级才生效*/
    display: block;
    /* 清除左右浮动 */
    /* clear: both; */
    /* 兼容性 网页隐藏伪元素 */
    /* height: 0; */
    /* visibility: hidden; */
}
.clearfix::after{
 	clear: both;
}
    
.father{
    /*元素溢出隐藏效果*/
   	overflow: hidden;
    /*元素超出父元素时，父元素会出现滚动条*/
    overflow: auto;
}
</style>
```

------

#### **Flexbox (弹性布局模型)**

`Flexbox` 是一种一维布局模型，用于在一个方向上（水平或垂直）布局元素。它非常适用于处理一维排列，比如在一行或一列中的元素布局。`Flexbox` 提供了一系列属性，可以更精确地控制元素的尺寸、顺序、对齐方式等。（详见css3布局）

一些常用的 `Flexbox` 属性包括：

- `display: flex;`：将容器元素转换为弹性容器。
- `flex-direction`：设置主轴的方向（`row`、`column`等）。
- `justify-content`：定义主轴上元素的对齐方式。
- `align-items`：定义侧轴上元素的对齐方式。
- `flex`：设置元素的弹性比例等。

------

#### **Grid (网格布局)**

`Grid` 是一种二维布局模型，用于在两个方向上（水平和垂直）布局元素。它适用于复杂的网格布局需求，可以创建灵活的多行多列布局，非常适用于构建网格化的页面结构。

一些常用的 `Grid` 属性包括：

- `display: grid;`：将容器元素转换为网格容器。
- `grid-template-columns`：定义列的大小和数量。
- `grid-template-rows`：定义行的大小和数量。
- `grid-gap`：设置网格项之间的间距。
- `grid-column` 和 `grid-row`：指定网格项的跨越。

------

#### 定位

| 定位                  | 说明                                                         |
| --------------------- | ------------------------------------------------------------ |
| `position: static;`   | **静态定位**（默认）不受 top、bottom、left、right 的影响。   |
| `position: relative;` | **相对定位**：照正常的文档流排列，但是可以通过 top、bottom、left、right 属性来调整元素的位置。 |
| `position: absolute;` | **绝对定位**：不在文档流中，它的位置相对于离它最近的已定位的父元素（如果有），如果没有已定位的父元素，则相对于 body 元素。 |
| `position: fixed;`    | **固定定位**：不在文档流中，它的位置相对于浏览器窗口，即使滚动也不会改变位置。 |
| `position: sticky;`   | **粘性定位**：照正常的文档流排列，参照最近的一个具有“滚动”行为的父元素，专门用于窗口滚动时的定位方式。粘定位元素在达到某一个值时可以固定在某一个位置。 |

`PS`：	①：Z-index 仅能在定位元素上奏效（例如 position:absolute;）！
			 ②：z-index 属性设置元素的堆叠顺序。拥有更高堆叠顺序的元素总是会处于堆叠顺序较低的元素的前面。
			 ③：bottom、right 优先级小于 top、left，推荐两两组合分开写
			 ④：**定位的层级高于普通的标准流**。定位元素会显示在其上面，后开启定位元素会显示在先开启的上面

```html
<style>
/*     《相对定位》
    1. 改变位置参照原来在文档流的位置
    2. 占有原来的位置 即 没有脱离原来的标准流（脱标）
    3. 不会改变原标签的显示模式
*/
.element1 {
  position: relative; /* 开启相对定位 */
  top: 10px; /* 相对于正常位置向下移动 10 像素 */
  left: 20px; /* 相对于正常位置向右移动 20 像素 */
}
</style>
```

```html
<style>
/*     《绝对定位》
    1. 参照最近的开启定位（相对定位）的包含块\祖先元素（没有就参照html）
    2. 不占有原来的位置 即 脱离原来的标准流（脱标）
    3. 改变原标签的显示模式 ——> 行内块
    4. 多个绝对定位的元素重叠在一起，则后面的元素会覆盖前面的元素
*/
.element2 {
  position: absolute; /* 开启绝对定位 */
  top: 10px; /* 相对于其定位祖先元素向下移动 10 像素 */
  left: 20px; /* 相对于其定位祖先元素向右移动 20 像素 */
}
</style>
```

```html
<style>
/*     《固定定位》
    1. 改变位置参照浏览器视口的位置
    2. 不占有原来的位置 即 脱离原来的标准流（脱标）
    3. 改变原标签的显示模式 ——> 行内块
*/    
.element3 {
  position: fixed; /* 开启固定定位 */
  top: 10px; /* 相对于其定位祖先元素向下移动 10 像素 */
  left: 20px; /* 相对于其定位祖先元素向右移动 20 像素 */
}
</style>
```

```html
<style>
/*     《粘性定位》
    1. 改变位置参照最近的具有“滚动”行为的祖先元素（一般是body）的位置
    2. 占有原来的位置 即 没有脱离原来的标准流（脱标）
    3. 不改变原标签的显示模式
*/    
.element3 {
  position: fixed; /* 开启粘性定位 */
  top: 0; /*  0 像素 */
}
</style>
```

注意：转为行内块要注意设置宽高撑大盒子

#### 定位层级

元素的定位属性为 relative、absolute 或 fixed 时，可以使用 z-index 属性来定义它的层级关系。

1. z-index 值越高的元素将显示在 z-index 值较低的元素之上。
2. z-index 值可以是任意整数或 auto（自动）。
3. 多个元素的 z-index 值相同，则后面出现的元素会显示在前面出现的元素之上。

布局方式显示层级的优先级：标准流 < 浮动 < 定位

#### 定位特殊运用

1.  父元素只有高度，子元素不设置宽高，让top、botom、left、right 都为 0 子元素会充满父元素。

#### 绝对定位居中

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>绝对定位-居中</title>
    <style>
        /* 1. 手动计算 */
        .box{
            position: absolute;
            /* 盒子左边对齐中线 */
            left: 50%;
            /* 盒子右移 */
            margin-left: -150px;

            top: 50%;
            margin-top: -100px;

            /* 加了绝对定位失效 */
            /* margin: 0 auto; */
            width: 300px;
            height: 200px;
            background-color: #00eeee;
        }
        
        /* 2. transform自动计算 */
        .box2{
            position: absolute;
            left: 50%;
            top: 50%;
            /* CSS3 位移 */
            transform: translate(-50%,-50%);

            /* 加了绝对定位失效 */
            /* margin: 0 auto; */
            width: 333px;
            height: 99px;
            background-color: #00eeee;
        }
        
        /* 3.  */
         .box3{
            position: absolute;
            left: 0;
            top: 0;
            right: 0;
            bottom: 0;
            margin: auto;
   
            width: 333px;
            height: 99px;
            background-color: #00eeee;
        }
    </style>
</head>
<body>
    <div class="box2"></div>
</body>
</html>
```

#### 布局小技巧

1.  行内元素、行内块元素，可以被父元素当做文本处理。 即：可以像处理文本对齐一样，去处理：行内、行内块在父元素中的对齐。 例如： text-align 、 line-height 、 text-indent 等。 

2.  如何让子元素，在父亲中 水平居中： 
    1.  若子元素为块元素，给父元素加上： margin:0 auto; 。 
    2.  若子元素为行内元素、行内块元素，给父元素加上： text-align:center 。
3.  如何让子元素，在父亲中 垂直居中： 
    1.  若子元素为块元素，给子元素加上： margin-top ，值为：(父元素 content －子元素盒子 总高) / 2。
    2.   若子元素为行内元素、行内块元素： 让父元素的 height = line-height ，每个子元素都加上： vertical-align:middle; 。
    3.   补充：若想绝对垂直居中，父元素 font-size 设置为 0 。

------

### 盒模型

#### 基本属性

用于控制 HTML 元素的宽度、高度、内边距和外边距等。控制元素的布局和空间分配。

| 组成（内到外）      | 说明                                                         |
| :------------------ | ------------------------------------------------------------ |
| 内容区域（content） | 元素实际内容                                                 |
| 内边距（padding）   | 内容区域和边框之间的空间，可以用于设置元素内部的间距。会撑大盒子 |
| 边框（border）      | 围绕内容区域和内边距的线条或区域，用于定义元素的边界。会撑大盒子 |
| 外边距（margin）    | 元素周围的空白区域，用于控制元素与其他元素之间的距离。       |

border: 5px solid red;  //  dotted（点线）  dashed（虚线）

box-sizing: border-box;    
css3盒子模型，**内减模式**，自动减除撑大盒子的padding border 保持宽高不变

**清除默认边距？**： margin: 0;       padding: 0;

***外边距合并问题* ？**：各个元素之间的外边距取最大的生效，只存在上下元素。尽量避免，而且简单，用其它方式复杂化，

***外边距塌陷问题* ？**：互相嵌套的块级元素，子元素的margin-top会作用到父元素上（同理margin-bottom） 。解决方法：

1. 单独父级设置padding。取值不能为0。
2. 父元素设置border 。取值不能为0。
3. 父元素设置 overflow:hidden  （优先，隐藏益处元素）
4. 子元素块元素转为其它元素 

***行内标签的内外边距不生效问题*？**(top bottom 不生效)

***盒子margin外边距注意事项 ?***

1.  子元素的margin参照父元素的content。
2.  上、左margin影响自身位置，下右影响兄弟元素位置。
3.  行内元素上下margin不生效，左右margin有效。
4.  块级元素上下`margin：auto 0;`不生效，左右`margin:0 auto`有效。



------

##### 盒子content内容

| css属性名  | 功能                                         |
| ---------- | -------------------------------------------- |
| width      | 设置盒子内容宽度                             |
| max-width  | 设置盒子内容最大宽度，一般不和width一起使用  |
| min-width  | 设置盒子内容最小宽度，一般不和width一起使用  |
| height     | 设置盒子内容高度                             |
| max-height | 设置盒子内容最大高度，一般不和height一起使用 |
| min-height | 设置盒子内容最小高度，一般不和height一起使用 |

1.  

------

#### 盒子阴影

```html
   <style>
        /* box-shadow 属性顺序连写：
          1. h-shadow: 必要 水平偏移量 允许负值
          2. v-shadow: 必要 垂直偏移量 允许负值
          3. blur: 可选 模糊程度
          4. spread: 可选 阴影扩大 添加虚影
          5. ccolor: 可选 阴影颜色
          6. inset: 可选 改为内部阴影
        */
        /* 阴影 不占标准流位置 */
        .box{
            display: inline-block;
            width: 200px;
            height: 200px;
            background-color: #ee8888;

            /* 装饰性属性写最后面 */
            box-shadow: 10px 30px 10px 10px;
        }
</style>
```

------

#### 盒子圆角

```html
<title>常用圆角</title>
    <style>
        /* 正圆 */
        .box1{
            width: 200px;
            height: 200px;
            background-color: #00eeee;

            /* border-radius: 100px; */
            /* 1. 最大就 50% 不可能比 正圆还圆 */
            /* 2. 长方形变不了正圆 */
            border-radius: 50%;
        }

        /* 胶囊 */
        .box2{
            width: 400px;
            height: 200px;
            background-color: #00eeee;

            /* 取高度一般 */
            border-radius: 100px;
        }
</style>
```



### 伪类和伪元素

| 伪类        | 触发条件               | 说明                                       |
| ----------- | ---------------------- | ------------------------------------------ |
| ele:hover   | 鼠标指针悬停在元素上   |                                            |
| ele:active  | 按下鼠标按钮激活元素时 | 提供短暂的视觉反馈，不是永久的样式更改     |
| ele:visited | 已访问链接样式修改     | js访问伪类样式得到空值（浏览历史隐私安全） |
|             |                        |                                            |
| 伪元素      |                        |                                            |
| ele::before | 在元素之前插入         | 必须设置content属性，默认内联元素          |
| ele::after  | 在元素之后插入         | content可以使用转义字符，继承父元素样式    |
|             |                        |                                            |

### 属性过渡

用于定义选择器的特定状态或位置

```html
<style>
        /* 谁变化谁添加过渡属性 transition */
        .box{
            width: 200px;
            height: 200px;
            background-color: #e66;
            
            /* 宽度 过渡 600px 1秒 */
            /* transition: width 1s , background-color 2s; */
			
            /* 自动匹配所有过渡属性 */
            transition: all 1s;
        }
        .box:hover{
            width: 600px;
            background-color: #0ee;
        }
</style>
```



### 其他属性

other......

## 元素显示模式

在HTML中，每个元素所占据的空间和呈现方式。

### 块级元素（block）

```html
<!--
	块级元素 
	独占一行，
	可以设置宽高，内外边距， 
	宽高默认占满父容器，
	可以放行内\块级元素，
	特殊文本标签不能放块级元素
-->
<!-- div、p、h系列、hr、ul、li、dl、dt、dd、form、header、nav、footer… -->
```

### 内联元素（inline）

```html
<!--
	行内元素（内联元素）
    一行显示多个
    宽高由内容撑开，不能设置 
    只能容纳文本或者其他行内标签
-->
<!-- <br>、<a>、<strong>、<b>、<em>、<i>、<del>、<s>、<ins>、<u>、<span> -->

<!-- a标签不能嵌套a标签浏览器会改为两个不嵌套的a标签，可以放其它块级元素 -->
```

### 内联块元素（inline-block）

```html
 <!--
	行内块元素 
	一行显示多个，会有空白缝隙
	可以设置宽高,内外边距（块级元素特点）
	默认本身内容宽高（行内元素特点）
-->
<!-- <img/>、<input>、<td>、<th>，他们同时具有块元素和行内元素的特点 -->
<!-- input、textarea、button、select…… -->
```

### 模式转换

```css
display: block; 转换为块级元素 较多

display: inline-block; 转化为行内块元素 较多

display: inline; 转换为行内元素 较少

display: none; 隐藏
```



------

## 其它

### 常用类名

```css
顶部导航条 topbar
页头 header 、 page-header
导航 nav 、 navigator 、 navbar
搜索框 search 、 search-box
横幅、广告、宣传图 banner
主要内容 content 、 main
侧边栏 aside 、 sidebar
页脚 footer 、 page-footer
```

### 长度单位

| 单位 |                                                        |
| ---- | ------------------------------------------------------ |
| 1px  | 一个像素点                                             |
| 1em  | 当前元素的font-size的1倍，本身没有就找父级元素         |
| 1rem | 根元素的font-size的1倍，即html元素，默认谷歌浏览器16px |
| 50%  | 相对其父元素的长度，即父元素长度的一半                 |



------

### 颜色取值

| 取值     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| 颜色名称 | 预定义的颜色名称，如 "red"、"green"、"blue" 等               |
| HEX 值   | 十六进制表示法表示颜色，例如 "#ff000088" 表示半透明红色      |
| RGB 值   | RGB 表示法表示颜色，如 "rgb(255, 0%, 0)" 表示红色            |
| RGBA 值  | RGBA 表示法表示带透明度的颜色，例如 "rgba(255, 0, 0, 0.5)" 表示半透明的红色 |
| HSL 值   | HSL 表示法表示颜色，例如 "hsl(0deg, 100%, 50%)" 表示红色，hsl(色相，饱和度，亮度) |
| HSLA 值  | HSLA 表示法表示带透明度的颜色，例如 "hsla(0, 100%, 50%, 0.5)" 表示半透明的红色 |

备注：transparent关键字可以单独设置透明度

------

### 装饰行内块

| 元素对齐          | 文档流中排列方式                     | 缺陷说明                                         | 解决方法                                             |
| ----------------- | ------------------------------------ | ------------------------------------------------ | ---------------------------------------------------- |
| 行内块&行内块对齐 | 两个输入框、输入框和文本默认水平排列 | 因为按文字基线对齐，但文字大小不一样、只一边有字 | 谁设置对齐谁vertical-align: middle;                  |
| 盒子&输入框对齐   | 盒子放入输入框                       | 顶部有缝隙                                       | 输入框设置vertical-align: top;                       |
| 盒子&图片对齐     | 图片标签自动等比例填充父盒子         | 底部对不齐                                       | 图片设置vertical-align: middle;、display: block;     |
| 盒子&图片居中     | 盒子放入图片                         | 实现居中元素                                     | 盒子设置line-height，图片设置vertical-align: middle; |
| 图片&文字对齐     | 盒子里面图片和文字                   | 让文字对齐图片                                   | 图片vertical-align: bottom;                          |

 **vertical-align     // 默认baseline按文字基线对齐**，用于控制同一行元素之间、或单元表格内文字的垂直对齐方式。

1.  不能直接操作文字，但可以控制单元格内文字。
2.  不能操作块元素。

`vertical-align: middle;`：***使元素的中部与父元素的基线加上父元素*x-height（x高度）的一半对齐。**



------

### 光标类型

```html
<style>
        div{
            width: 200px;
            height: 200px;
            background-color: #00eeee;

            /* 默认 */
            /* cursor: default; */

            /* 手型 */
            /* cursor: pointer; */

            /* 工字型 文本可复制*/
            /* cursor: text; */

            /* 十字箭头型 可以移动 */
            cursor: move;
            
            /* 等待 */
            cursor: wait;
            
            /* 帮助 */
            currsor: help;
            
            /* 自定义 */
            currsor: url(),pointer;
        }
    </style>
```

### 圆角

```html
 <style>
    .box{
        margin: 30px auto;

        width: 200px;
        height: 200px;
        background-color: #00eeee;

        /* 圆角半径 border-radius */

        /*  左上开始 顺时针 */
        /* border-radius: 100px 10px 10px 10px; */

        /* 左上开始 对角相等 */
        border-radius: 50px 20px;

        /* 左上开始  两边对角  中间一样*/
        border-radius: 90px 10px 50px;
    }
</style>
```

### 元素溢出

```html
<style>
        .box{
            width: 100px;
            height: 300px;
            background-color: #00eeee;

            /* 默认 益处可见 */
            /* overflow: visible; */

            /* 隐藏 */
            /* overflow: hidden; */

            /* 不多用 */
            /* 滚动隐藏  2. 左边、下面有滚动条*/
            overflow: scroll;

            /* 自适应 1.不溢出没有滚动条  */
            /* overflow: auto; */

        }
</style>
```

### 元素隐藏

```html
<style>
        .box1 {
            /* 占 标准流 隐藏元素：show\hidden */
            /* visibility: hidden; */

            /* 不占 标准流 隐藏元素 */
            display: none;

            width: 200px;
            height: 200px;
            background-color: #00eeee;
        }
</style>
```

### 元素整体透明度

```html
<style>
        div{
            width: 300px;
            height: 300px;
            background-color: #ee0000;
            /* 让元素整体 包括字 图 子元素等等 */
            /* 取值 0 ~ 1 */
            opacity: 0.6;
        }
    </style>
```

### 元素居中

```html
<style>
/* 定位水平垂直居中 */
.parent {
    position: relative;
    height: 100px;
    background-color: #e88;
    border: 2px solid #fff;
}
.child1 {
    position: absolute;
    left: 50%;
    top: 50%;
    margin-top: -20px;
    margin-left: -50px;
    width: 100px;
    height: 40px;
    background-color: #77a;
}
.child2 {
    position: absolute;
    /* 注意减号空格 */
    left: calc(50% - 50px);
    top: calc(50% - 20px);
    width: 100px;
    height: 40px;
    background-color: #77a;
}
.child3 {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 100px;
    height: 40px;
    background-color: #77a;
}

/* 单行元素水平垂直居中 */
.box3 {
    height: 40px;
    line-height: 40px;
    text-align: center;
    background-color: rgb(176, 212, 117);
}

</style>

<!-- 1. 定位水平垂直居中-margin-top-left -->
<div class="parent">
    <div class="child1"></div>
</div>
<!-- 2. 定位水平垂直居中-left:calc(50% - 50px) -->
<div class="parent">
    <div class="child2"></div>
</div>
<!-- 3. 定位水平垂直居中-transform -->
<div class="parent">
    <div class="child3"></div>
</div>
<!-- 单行元素水平垂直居中 -->
<div class="box3">
    <span>行内元素3</span>
</div>
```

```html
<style>
/* 定位+margin水平垂直居中 */
.parent1 {
    position: relative;
    height: 100px;
    background-color: #e88;
    border: 2px solid #fff;
}
.child4 {
    position: absolute;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
    margin: auto;
    width: 100px;
    height: 40px;
    background-color: #77a;
}
/* padding水平垂直居中 */
.parent2 {
    padding: 30px;
    background-color: #e88;
    border: 2px solid #fff;
}
.child5 {
    height: 40px;
    background-color: #77a;
}
/* flex水平垂直居中 */
.parent3 {
    display: flex;
    /* 垂直 */
    align-items: center;
    /* 水平 */
    justify-content: center;
    height: 100px;
    background-color: #e88;
    border: 2px solid #fff;
}
.child6 {
    width: 100px;
    height: 40px;
    background-color: #77a;
}
/* 伪元素水平垂直居中 */
.parent4 {
    height: 100px;
    text-align: center;
    background-color: #e88;
    border: 2px solid #fff;
}
.child7 {
    display: inline-block;
    width: 100px;
    height: 40px;
    background-color: #77a;
    vertical-align: middle;
}
.parent4::before {
    display: inline-block;
    content: "";
    /* width: 20px; */
    height: 100px;
    background-color: rgb(224, 224, 250);
    vertical-align: middle;
}
</style>

<!--  -->
<div class="parent1">
    <div class="child4"></div>
</div>
<!--  -->
<div class="parent2">
    <div class="child5"></div>
</div>
<!--  -->
<div class="parent3">
    <div class="child6"></div>
</div>
<!--  -->
<div class="parent4">
    <div class="child7">7</div>
</div>
```



```html
<style>
/* 行内元素居中 */
.box1 {
    text-align: center;
    background-color: #0ee;
}
</style>
<!--  -->
<div class="box1">
    <span>行内元素1</span>
</div>

<style>
/* 父元素宽度适应子元素宽度居中 */
.box2 {
    margin: auto;
    width: fit-content;
    background-color: rgb(153, 233, 166);
}
</style>
<!--  -->
<div class="box2">
    <span>行内元素3</span>
</div>


<style>
/* 子元素左右自适应水平居中 */
.box4 {
    height: 40px;
    background-color: #99e;
}
.item4 {
    display: block;
    margin: 0 auto;
    width: 300px;
    background-color: #e55;
}
</style>
<!--  -->
<div class="box4">
    <span class="item4">行内元素4</span>
</div>
```

### 元素之间的空白

PS：存在于行内块、行内元素之间。解决方式：

1.  代码不换行。
2.  父元素`font-size:0px;`，其它元素自己设置字体大小。

### 元素幽灵空白

PS：例如一个div内一个图片，图片底部有空白。解决方法：

1.  图片增加vertical-align。
2.  图片display:block;，图片后面不能有元素文字。
3.  父元素font-size: 0px;，后续文字需单独设置。

```html
div{
	width: 300px;
	background-color: red;
}
img{
	height: 200px;
}
<div>
    <img src="">xg
</div>
```

# CSS3

## CSS3基本了解

1.  CSS3 是 CSS2 的升级版本，它在 CSS2 的基础上，新增了很多强大的新功能，从而解决一些实际 面临的问题。
2.  CSS3 在未来会按照模块化的方式去发展： https://www.w3.org/Style/CSS/current-work.html 
3.  CSS3 的 ***新特性***  如下：新增了更加实用的选择器，例如：
    1.  动态伪类选择器、目标伪类选择器、伪元素选择器等等。
    2.  新增了更好的视觉效果，例如：圆角、阴影、渐变等。 
    3.  新增了丰富的背景效果，例如：支持多个背景图片，同时新增了若干个背景相关的属性。
    4.  新增了全新的布局方案 —— 弹性盒子。 
    5.  新增了 Web 字体，可以显示用户电脑上没有安装的字体。 
    6.  增强了颜色，例如： HSL 、 HSLA 、 RGBA 几种新的颜色模式，新增 opacity 属性来控制 透明度。
    7.  增加了 2D 和 3D 变换，例如：旋转、扭曲、缩放、位移等。 
    8.  增加动画与过渡效果，让效果的变换更具流线性、平滑性。
    9.  ……
4.  **私有前缀**：`-webkit-`
    1.  ***为什么需要私有前缀？***：W3C 标准所提出的某个 CSS 特性，在被浏览器正式支持之前，浏览器厂商会根据浏览器的内核， 使用私有前缀来测试该 CSS 特性，在浏览器正式支持该 CSS 特性后，就不需要私有前缀了。
    2.  ***不同浏览器常见的私有前缀？***：hrome浏览器： -webkit-、Safari浏览器： -webkit-、Firefox浏览器： -moz-、Edge浏览器： -webkit-

```css
div {
	width:400px;
	height:400px;
	-webkit-border-radius: 20px;
}
```

------

### 长度单位

| 单位           | 说明                                                        |
| -------------- | ----------------------------------------------------------- |
| 50px           | 五十个像素点                                                |
| 20rem          | 根据元素字体的大小，放大20倍                                |
| 50vw、50vh     | 视口宽度度的50%、视口高度的50%                              |
| 20vamx、20vmin | 比较视口宽高的长度大小，取最大的值决定长度为20%（同理vmin） |

### 颜色设置

CSS3新增三种：rgba、hsl、hsla

### 选择器

CSS3新增选择器：动态伪类、目标伪类、语言伪类、结构伪类、否定伪类、伪元素

------

## 盒子模型相关属性

### 杂项

------

#### **box-sizing**

取值 ① content-box（默认模式）、② border-box（边框内减模式、压缩content）；

------

#### **resize**

取值 ① horizontal（水平宽度可调）、② vertical（垂直高度可调）、③ both (水平垂直可调)。④ none（默认不可操作）。PS：如果盒子有内容需要配合设置overflow：hidden，通常用于设置textarea标签默认行为。

------

#### 盒子阴影

box-shadow

```css
/* 
  box-shadow 复合属性必须顺序连写：
  1. h-shadow: 必要 水平偏移量 允许负值
  2. v-shadow: 必要 垂直偏移量 允许负值
  3. blur: 可选 模糊程度
  4. spread: 可选 阴影外延程度 
  5. ccolor: 可选 阴影颜色
  6. inset: 可选 改为内部阴影
*/
/* 阴影 不占标准流位置 */
.box{
    display: inline-block;
    width: 200px;
    height: 200px;
    background-color: #ee8888;
    /* 装饰性属性写最后面 */
    box-shadow: 10px 30px 10px 10px;
}
```

------

#### 元素整体透明度

opacity

```css
div{
    width: 300px;
    height: 300px;
    background-color: #ee0000;
    /* 影响元素整体 包括字 图 子元素等等 */
    /* 取值 0 ~ 1 */
    opacity: 0.6;
}
```

### 边框圆角

border-radius

```css
/* 正圆 */
.box1{
    width: 200px;
    height: 200px;
    background-color: #00eeee;
    /* border-radius: 100px; */
    /* 1. 最大就 50% 不可能比 正圆还圆 */
    /* 2. 长方形变不了正圆 */
    border-radius: 50%;
}
/* 胶囊 */
.box2{
    width: 400px;
    height: 200px;
    background-color: #00eeee;
    /* 取高度一半 */
    border-radius: 100px;
}
/* 其它写法 */
borfer-top-right-radius: 100px 50px;
border-raidus: 左上角x 右上角x 右下角x 左下角x / 左上y 右上y 右下y 左下y
```

PS：边框外轮廓：outline-width:20px \ outline-color: red; \ outline-style: solid;  outline-offset: 0px; (偏移量默认0px)不占标准流，前三个属性可复合写法

------

## 背景相关属性

**`background-origin`**：**背景原点**。默认为padding左顶点、取值padding-box、boder-box、content-box

**`background-clip`**：**设置背景不可见区域**。默认border以外背景不可见，取值padding-box、boder-box、content-box、text（让背景只呈现在文字的区域，需配合文字透明color：transparent，并且该属性需要CSS3前缀-webkit-）

**`background-size`**：**设置背景宽高**。取值：宽高像素px、contain（让背景等比例完整显示在容器，会出现重复）、cover（让图片铺满容器，直到覆盖容器，不会重复）

复合：`background: color url repeat position / size origin clip`

多背景图片

```css
/* 添加多个背景图 */
background: url(../images/bg-lt.png) no-repeat,
url(../images/bg-rt.png) no-repeat right top,
url(../images/bg-lb.png) no-repeat left bottom,
url(../images/bg-rb.png) no-repeat right bottom;
```

## 文本属性

| 属性            | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| text-shadow     | **文本阴影**，类似盒子阴影，可用来设置文字发光效果。不设置偏移，设置模糊 |
| white-space     | **文本换行**，<br />`normal`（默认值，文本超出边界自动换行，代码中文本的换行识别为一个空格）<br />`pre`（代码文本原文显示，与pre标签相同）、<br />`pre-warp`（在pre基础上，文本超出边界自动换行），<br />`pre-line`（在pre基础上，文本超出边界自动换行，且只识别文本中的换行，空格忽略，文本之间空格不会忽略）<br />`nowrap`（强制不换行，无论代码文字怎么写都变为一行） |
| text-overflow   | **文本溢出**，需要设置`overflow: 非visible取值;`并建议配合设置强制不换行`white-space: nowrap`<br />clicp（超出文本截断）<br />ellipsis（超出文本省略号代替） |
| text-decoration | **文本修饰**，变化为复合属性<br />text-decoation-liner<br />text-decoraion-style<br />text-decoration-color<br />复合：text-decoration: overline solid blue; |
| text-stroke     | **文本描边**，可能需要前缀-webkit-<br />text-stroke-color: red;<br />text-srtoke-width: 3px;<br />复合：text-srtoke: red 3px; 配合文字透明color：transparent，实现空心字 |

------

## 布局

### 多列布局

| 属性              | 描述                                                     |
| ----------------- | -------------------------------------------------------- |
| column-count      | 指定列数                                                 |
| column-width      | 指定每一列宽度，自动计算列数（若和设置列数冲突，取小的值 |
| columns           | 复合属性，因为可能列数冲突，所以复合指定的列数可能无效   |
| column-gap        | 每一列间距                                               |
| column-rule-width | 每一列间边框宽度                                         |
| column-rule-color | 每一列间边框颜色                                         |
| column-rule-style | 每一列间边框样式：solid、dashed                          |
| column-rule       | 每一列间边框复合属性                                     |
| column-span       | 跨列展示:默认none、可选all；可用来标题跨列等             |

```css
.box {
    width: 1000px;
    margin: 0 auto;
    background-color: #0ee;
    /* 指定列数 */
    /* column-count: 4; */
    
    /* 指定每一列宽度，自动计算列数（若和设置列数冲小的值） */
    column-width: 200px;
    
    /* 因为可能列数冲突，复合指定的列数可能无效 */
    /* columns: 3 200px; */
    
    /* 每一列间距 */
    column-gap: 20px;
    
    /* 每一列间边框 */
    /* column-rule-width: 2px;
    column-rule-color: #e55;
    column-rule-style: dashed; */
    column-rule: 2px #e55 dashed;
}

h1 {
    /* 跨列展示:默认none */
    column-span: all;
    text-align: center;
}

<div class="box">
	<h1>...</h1>
	<p>.....</p>
	<p>.....</p>
	<p>.....</p>
	<p>.....</p>
</div>
```

------

### 伸缩盒模型

| 属性             | 取值                                                         | 描述             |
| ---------------- | ------------------------------------------------------------ | ---------------- |
| display          | flex、line-flex                                              | 设置弹性盒子     |
| *flex-direction* | row、row-reverse、column、column-reverse                     | 设置主轴方向     |
| flex-wrap        | nowrap、wrap、wrap-reverse                                   | 主轴方向换行     |
| flex-flow        | 主轴方向、主轴换行                                           | 复合属性         |
| justify-content  | flex-start、flex-end、center、space-between、space-around、space-evenly | 主轴的对齐方式   |
| align-items      | flex-start、flex-end、center、baseline                       | 侧轴项的对齐方式 |
| align-content    | flex-start、flex-end、center、space-between、space-around、space-evenly | 侧轴多行对齐方式 |

| 伸缩的属性  | 描述                                                         | 取值 |
| ----------- | ------------------------------------------------------------ | ---- |
| flex-basis  | 在主轴上，设置伸缩项的基准长度，默认auto，<br />主轴水平排列原宽无效，主轴垂直排列原高无效 | px   |
| flex-grow   | 拉伸：在主轴上，伸缩项等分剩余空间（如果有剩余），取值为数值，默认0<br />压缩：取消父元素换行，配合flex-grow，加入父元素宽不够默认会压缩、拉升。 | 数值 |
| flex-shrink | 压缩：默认值1                                                |      |
| flex        | flex复合属性顺序：grow、shrink、basis，都必填，默认取值简写auto |      |

| 项目排序与单独对齐属性 | 描述                                                         |
| ---------------------- | ------------------------------------------------------------ |
| order                  | 伸缩项目单独排序：取值默认0，取值越小的排在前，例如：`order:-1;` |
| align-self             | 伸缩项目单独对齐（主轴方向）：取值默认auto，例如：`align-self:center` |

**缩放举例计算**

```css
例如：
三个收缩项目，宽度分别为： 200px 、 300px 、 200px ，它们的 flex-shrink 值分别为： 1 、 2 、 3
若想刚好容纳下三个项目，需要总宽度为 700px ，但目前容器只有 400px ，还差 300px所以每个人都要收缩一下才可以放下，具体收缩的值，这样计算：
1. 计算分母： (200×1) + (300×2) + (200×3) = 1400
2. 计算比例：
	项目一： (200×1) / 1400 = 比例值1
	项目二： (300×2) / 1400 = 比例值2
	项目三： (200×3) / 1400 = 比例值3
3. 计算最终收缩大小：
	项目一需要收缩： 比例值1 × 300
	项目二需要收缩： 比例值2 × 300
	项目三需要收缩： 比例值3 × 300

如果写 flex:1 1 auto ，则可简写为： flex:auto == 可以拉升压缩，不设置基准长度
如果写 flex:1 1 0 ，则可简写为： flex:1 == 可以拉升压缩，设置基准长度为0
如果写 flex:0 0 auto ，则可简写为： flex:none == 不可以拉升压缩，不设置基准长度
如果写 flex:0 1 auto ，则可简写为： flex:0 auto == 即 flex 初始值。
```

#### 1.伸缩容器和项

1.  ，伸缩项默认从左到右一字排开

    ```css
    display: flex; /* 开启伸缩容器：只是子元素变为伸缩项 */
    ```

2.  两个伸缩容器之间默认上下排列。可以设置`inline-flex`一字排开，但不多用，一般给他们父容器开启`flex`最好

    ```css
    display: inline-flex; /* 设置多个伸缩容器一字排开，不多用 */
    ```

3.  伸缩项目最后都会**块状化**，即`display: block;`

------

#### 2.主轴方向

1.  **不关注原点，只注重方向**。

2.  **主轴方向**默认水平从左到右。（可修改反向）

3.  **侧轴方向默**认垂直从上到下。（根据主轴方向自动变化）

    ```css
    /* 主轴方向: 水平左到右 默认*/
    flex-direction: row;
    /* 主轴方向: 水平右到左*/
    flex-direction: row-reverse;
    /* 主轴方向: 垂直上到下*/
    flex-direction: column;
    /* 主轴方向: 垂直下到上*/
    flex-direction: column-reverse;
    ```

4.  主轴方向所有元素自适应宽度一字排开（无论伸缩项之前宽度多少，开启换行可以改变）

5.  侧轴方向可能会有空白（高度足够）

    ```css
    /* 主轴方向换行：默认不换行、换行、反向换行（从下到上一字排开）*/
    flex-wrap: nowrap;
    flex-wrap: wrap;
    flex-wrap: wrap-reverse;
    
    /* 复合属性：主轴方向、主轴换行（顺序自定义） */
    flex-flow: row wrap;
    ```

------

#### 3.轴对齐方式

1.  主轴的对齐方式（影响每一行的  `伸缩项元素之间` 的对齐方式）

    ```css
    /* 类似word文档：左对齐、右对齐、居中、两端对齐 */
    justify-content: flex-start;
    justify-content: flex-end;
    justify-content: center;
    justify-content: space-between;
    /* 分散对齐，项与项之间距离 是 项与边缘距离 的二倍 */
    justify-content: space-around;
    /* 分散对齐，项与项之间的距离，项与边缘距离,都相等 */
    justify-content: space-evenly;
    ```

2.  侧轴的对齐方式（① 影响每一行所有的 `伸缩项元素` 的对齐方式）（② 每一行对齐方式）

    ```css
    ① 影响项  `所有的伸缩项元素` 的对齐方式
    /* 侧轴的起始方向对齐 默认 */
    align-items: flex-start;
    /* 侧轴的结束方向对齐 */
    align-items: flex-end;
    /* 侧轴的中间对齐 */
    align-items: center;
    /* 侧轴的基线对齐 */
    align-items: baseline;
    /* 默认，项元素高度拉伸至容器高度，项设置高度就失效 */
    align-items: stretch;
    
    
     多行  每行对齐方式 
    /* 类似word文档：左对齐、右对齐、居中、两端对齐，只是变为侧轴方向 */
    align-content: flex-start;
    align-content: flex-end;
    align-content: center;
    align-content: space-between;
    /* 分散对齐，行与行之间距离 是 行与边缘距离 的两倍 */
    align-content: space-around;
    /* 分散对齐，行与行之间距离，行与边缘距离，都相等 */
    align-content: space-evenly;
    /* 默认，每一行所有项元素平分容器宽度，某一项元素设置宽度它就失效 */
    align-content: stretch;
    ```

#### 4.水平垂直居中

```css
.father {
    width: 300px;
    height: 300px;
    background-color: #0ee;
    display: flex;
    /* justify-content: center; */
    /* align-items: center; */
}

.son {
    width: 100px;
    height: 100px;
    background-color: #e88;
    margin: auto;
}
```

#### 5.项目排序与单独对齐

```css
/* 排序：默认0，取值小的排在前 */
order: -1;
```



------

## 其它

### 渐变

可用来实现信纸效果、立体效果

```css
/* 初始化盒子 
*/
div{
    width: 200px;
    height: 200px;
    border: 1px solid black;
    float: left;
    margin-left: 20px;
}
/*
	线性渐变：linear-gradient()，默认上-->下
	径向渐变：radial-gradient()，默认中心-->四周，圆心可变类似线性渐变
	重复渐变：repeating-linear-gradient()\repeating-radial-gradient()：在没有发生渐变的区域重复
*/
/*========================================线性渐变===============================================*/
/* 线性渐变：默认上-->下 red blue yellow */
.box1{
    background-image: linear-gradient( red, blue, yellow )
}
/* 线性渐变：左-->右 red blue yellow */
.box2{
    background-image: linear-gradient( to right, red, blue, yellow )
}
/* 线性渐变：右上-->左下  */
.box3{
    background-image: linear-gradient( to right top,red, blue, yellow )
}
/* 线性渐变：左下到右上20度角渐变 */
.box4{
    background-image: linear-gradient( 20deg, red, blue, yellow )
}
/* 线性渐变：默认上-->下 red blue yellow 波纹效果 */
.box5{
    background-image: linear-gradient( red 50px, blue 100px, yellow 150px )
}
/*========================================径向渐变===============================================*/
/* 径向渐变：圆心位置 */
.box6{
    background-image: radial-gradient( at 50px 50px, red , blue , yellow )
}
/* 径向渐变：正圆、cricle */
.box7{
    background-image: radial-gradient( 50px 50px, red , blue , yellow )
}
/* 径向渐变：椭圆波纹效果 */
.box8{
    background-image: radial-gradient( 50px 150px, red 50px, blue 100px, yellow 150px )
}

```

------

### web字体&字体图标

#### 定制字体

```css
/* 网络字体引入占用空间极其大 */
@font-face{
	font-famaily: "字体名称";
	src: url('字体地址');
}
/* 定制字体 https://www.iconfont.cn/webfont#!/webfont/index 引入并使用 */
@font-face {
	font-family: "哈哈哈哈";
	font-display: swap;
	src: url('webfont.eot'); /* IE9 */
	src: url('webfont.eot?#iefix') format('embedded-opentype'), /* IE6-IE8 */
	url('webfont.woff2') format('woff2'),
	url('webfont.woff') format('woff'), /* chrome、firefox */
	url('webfont.ttf') format('truetype'), /* chrome、firefox、opera、Safari, Android*/
	url('webfont.svg#webfont') format('svg'); /* iOS 4.1- */
}

```

------

#### 字体图标

可以在阿里图标里修改名字，`span`标签可用`i`标签

##### unicode引入

```css
/* 1. 把字体图标woff2、woff、ttf文件存到指代文件夹 */
/* 2. 查看index.html，拷贝第一部引入代码，注意修改引入路径，自定义图标名 */
@font-face {
  font-family: 'iconfont';
  src: url('iconfont.woff2?t=1693227553033') format('woff2'),
       url('iconfont.woff?t=1693227553033') format('woff'),
       url('iconfont.ttf?t=1693227553033') format('truetype');
}
/* 3. 设置字体图标样式 */
.iconfont{
    font-family: "iconfont" !important;
	font-size: 16px;
}
/* 4. 根据index.html文件的unicode码应用 */
<span class="iconfont">&#x33;</span>


==============在线==============
@font-face {
  font-family: 'iconfont';  /* Project id 4228369 */
  src: url('//at.alicdn.com/t/c/font_4228369_r5tfrtcyo6a.woff2?t=1693228585115') format('woff2'),
       url('//at.alicdn.com/t/c/font_4228369_r5tfrtcyo6a.woff?t=1693228585115') format('woff'),
       url('//at.alicdn.com/t/c/font_4228369_r5tfrtcyo6a.ttf?t=1693228585115') format('truetype');
}
.iconfont{
    font-family: "iconfont" !important;
	font-size: 16px;
}
<span class="iconfont">&#xe601</span>
```

##### font class引入

大多数使用

```css
/* 1. 把字体图标CSS存到指代文件夹 */
/* 2. 引入项目下面生成的 fontclass 代码 */
<link rel="stylesheet" href="./iconfont.css">
/* 3. 根据CSS文件挑选相应图标并获取类名，应用于页面 */
<span class="iconfont icon-xxx"></span>

==============在线==============
<link rel="stylesheet" href="//at.alicdn.com/t/c/font_4228369_r5tfrtcyo6a.css">

.iconfont{
	font-size: 16px;
}
<span class="iconfont icon-food-doughnut">&#xe601</span>


```

##### symbol引入

浏览器吃性能，甚至不如png，ie9以上

```css
/* 1. 把字体图标js文件存到指代文件夹 */
/* 2. 引入项目下面生成的 fontclass 代码 */
<script src="./iconfont.js"></script>
/* 3. 加入通用 CSS 代码 */
.icon {
  width: 1em;
  height: 1em;
  vertical-align: -0.15em;
  fill: currentColor;
  overflow: hidden;
}
/* 4. 挑选相应图标并获取类名，应用于页面 */
<svg class="icon" aria-hidden="true">
  <use xlink:href="#icon-xxx"></use>
</svg>

==============在线==============
<script src="//at.alicdn.com/t/c/font_4228369_r5tfrtcyo6a.js"></script>

<svg class="icon" aria-hidden="true">
  <use xlink:href="#icon-xxx"></use>
</svg>

```

------

### 元素变换

#### 位移

| 属性                                            | 描述                                           |
| ----------------------------------------------- | ---------------------------------------------- |
| `transform: translateX(50%);`                   | X轴移动（自身宽度的一半）                      |
| `transform: translateY(50px) translateX(50px);` | XY轴移动（50px）（链式编写）                   |
| `transform: translate(50px,50%)`                | XY轴移动（50px，自身宽度的一半）（必填两个值） |

PS：配合定位可以水平垂直居中：`transform: translate(-50%,-50%)`，不会脱离文档流，行内元素无效。

```css
.box {
    width: 200px;
    height: 200px;
    background-color: #0ee;
    border: 2px solid #e88;
    margin: 0 auto;
    margin-top: 100px;
}
.content {
    width: 200px;
    height: 200px;
    background-color: #e55;
}
/* 变换-位移 */
.content:hover {
    transform: translateX(50px);
}

<div class="box">
    <div class="content">vsinger</div>
</div>
```

#### 缩放

| 属性                                | 描述                                                        |
| ----------------------------------- | ----------------------------------------------------------- |
| transform: scaleX(1.5);             | 宽放大1.5倍                                                 |
| transform: scaleX(1.5) scaleY(1.5); | 宽高放大1.5倍（链式编写）                                   |
| transform: scale( x,y ) ;           | 宽高缩放（值相同可填一个值）                                |
| transform: scaleZ(1) ;              | 默认htm元素没有厚度，此厚度指景深（取值变为原景深除去倍数） |
| transform: scale3d( x, y, z ) ;     | 所有参数不可省略                                            |

PS：缩放元素从中心向四周放大，并不只是一侧缩放。元素内元素（包括文字）都会缩放。行内元素无效。

```css
.content {
    width: 200px;
    height: 200px;
    background-color: #e55;
}
/* 变换-缩放 */
.content:hover {
    transform: scaleX(1.5);
}
<div class="content">vsinger</div>
```

#### 旋转

| 属性                              | 描述                                            |
| --------------------------------- | ----------------------------------------------- |
| transform: rotateZ(30deg);        | 绕Z轴旋转30度                                   |
| transform: rotateX(30deg);        | 绕X轴旋转30度                                   |
| transform: rotateY(30deg);        | 绕X轴旋转30度                                   |
| transform: rotate(30deg， 30deg); | 绕X，Y轴旋转30度（有且只有一个值时才绕Z轴旋转） |

PS：可以负值，变为逆时针旋转，正数顺时针旋转。

#### 扭曲

| 属性                         | 描述                                              |
| ---------------------------- | ------------------------------------------------- |
| transform: skewX(30deg)      | X轴方向上扭曲                                     |
| transform: skewY(30deg)      | Y轴方向上扭曲                                     |
| transform: skew(30deg,30deg) | XY轴方向上扭曲（有且只有一个值时才X轴方向上扭曲） |

PS：可以负值，扭曲方向和正数（左下，右上拉扯）相反。

#### 组合

PS：链式编写`transform: translateX(50px) rotateZ(30deg);`**实现多个变换**。旋转一般最后，视觉效果好看。

```css
.content {
    width: 200px;
    height: 200px;
    background-color: #e55;
}
/* 变换-组合 */
.content:hover {
    transform: translateX(50px) rotateZ(30deg);
}
<div class="content">vsinger</div>
```

#### 原点

PS：默认**`变换原点`**在元素中心，可以设置变换原点`transform-origin: bottom 50px;`可以取负值。
	    只设置一个值，若是像素值表示（?px，50%），若是关键词，另一个轴取值50%

```css
.content {
    width: 200px;
    height: 200px;
    background-color: #e55;
    /* 变换-原点 */
    transform-origin: bottom right;
}
.content:hover {
    transform: rotateZ(30deg);
}
<div class="content">vsinger</div>
```

### 3D变换

| 属性                               | 描述                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| transform: translate3d(x, y, z)    | 就是在2d位移时，提供一个z轴，控制元素的远近。值都必填，不能为%数值。 |
| transform: rotate(x, y, z, 30deg); | xyz默认0，不旋转，如果设置就是在各个方向上扭转30度           |

实现一个拉开窗户效果，视角与窗台平行。

PS:`transform: translateZ(50px);`在开启的景深时，元素往z轴位移，效果类似放大，在配合透视点改变效果类似位移。
	   也可以组合链式编写，rotate建议最后，视觉效果好。

```css
.box {
    width: 200px;
    height: 200px;
    /* background-color: #0ee; */
    border: 2px solid #e88;
    margin: 0 auto;
    margin-top: 100px;
    /* 开启3D空间：默认2D(flat) */
    transform-style: preserve-3d;
    /* 设置景深 */
    perspective: 500px;
    /* 透视点位置：默认元素中心 */
    perspective-origin: bottom;
}
.content {
    width: 200px;
    height: 200px;
    background-color: #e55;
    /* 线性过渡 */
    transition: all 1s linear;
    /* 设置变换原点 */
    transform-origin: left;
    /* 背部可见性:默认可见visible */
	backface-visibility: hidden;
}
.content:hover {
    /* 2. 3D变换-旋转50度 */
    transform: rotateY(-120deg);
}

<div class="box">
    <div class="content">vsinger</div>
</div>
```

### 背部可见性

| 属性                         | 描述                                     |
| ---------------------------- | ---------------------------------------- |
| backface-visibility: hidden; | 背部可见性:默认可见visible，90度后看不见 |

### calc

calc(100hv - 某元素)（100%视口减去目标值）**注意”减号“两端空格**

### 媒体查询和响应式

1.  媒体查询没有优先级，样式遵循普通css优先级，先写就覆盖等
2.  screen（只有屏幕使用的样式）、all（一直运用的样式）
3.  可以使用运算符：and、not、or、only（可能用来兼容ie）

```css
/* =============查询媒体类型============= */
/* 打印模式，去除背景 */
@media print {
    h1 {
        background: transparent;
    }
}

/* =============查询媒体宽度============= */
/* 检测视口宽度为800px，h1颜色改变 */
@media (width:800px) {
    h1 {
        background: #0ee;
    }
}
/* 检测最大视口宽度，小于就触发（同理min-width、高度等） */
@media (max-width:800px) {
    h1 {
        background: #0ee;
    }
}
/* 检测设备宽度：满足就触发 */
@media (device-width:1920px) {
    h1 {
        background: green;
    }
}
/* =============查询媒体设备方向============= */
/* 检测设备方向: 
portrait(纵向：高度大于等于宽度)、landscape(横向：宽度大于高度)*/
@media (orientation:landscape) {
    h1 {
        background: yellow;
    }
}
```

### 屏幕阈值

`超小屏幕 < 768px < 中等屏幕 < 992px < 大屏幕 < 1200px < 超大屏幕`

配合媒体查询书写多个样式。

```css
1. 引入css时判断
<link rel="stylesheet" media="screen and (max-width:768px)" href="./xxx1.css">
<link rel="stylesheet" media="screen and (min-width:768px) and (max-width:992px)" href="./xxx2.css">
<link rel="stylesheet" media="screen and (min-width:992px) and (max-width:1200px)" href="./xxx3.css">

2. css代码里面判断
@media screen and (max-width:768px) {
/*CSS-Code;*/
}
@media screen and (min-width:768px) and (max-width:992px) {
/*CSS-Code;*/
}
@media screen and (min-width:992px) and (max-width:1200px) {
/*CSS-Code;*/
}

```

### BFC

#### ***什么是BFC?***

块格式化上下文（Block Formatting Context，BFC） 是 Web 页面的可视 CSS 渲染的一部分， 是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域。

即

1.  BFC 是 Block Formatting Context （块级格式上下文），可以理解成元素的一个 “特异功能”。
2.   该 “特异功能”，在默认的情况下处于关闭状态；当元素满足了某些条件后，该“特异功 能”被激活。
3.  所谓激活“特异功能”，专业点说就是：该元素创建了 BFC （又称：开启了 BFC ）。

####  ***开启了BFC能解决什么问题*** ？

1.  元素开启 BFC 后，其子元素不会再产生 margin 塌陷问题。 
2.  元素开启 BFC 后，自己不会被其他浮动元素所覆盖。 
3.  元素开启 BFC 后，就算其子元素浮动，元素自身高度也不会塌陷。

#### ***如何开启BFC？*** 

根元素
浮动元素
绝对定位、固定定位的元素
行内块元素
表格单元格： table 、 thead 、 tbody 、 tfoot 、 th 、 td 、 tr 、 caption
overflow 的值不为 visible 的块元素
伸缩项目
多列容器 column-span 为 all 的元素（即使该元素没有包裹在多列容器中)
display 的值，设置为 flow-root（开启一个块格式化上下文）

​                           

​                

# ........