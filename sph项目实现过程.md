1. 项目环境准备

   1）vue2  

   2) npm i vue-touter@3

   3)  "less": "^4.1.3",

     "less-loader": "^7.3.0",

     "vue": "^2.6.14",

     "vue-router": "^3.6.5"

   4) 关闭语法检测：vue.config.js =>

   ```javascript
   module.exports = defineConfig({
     transpileDependencies: true,
     lintOnSave: false,
   })
   ```

2. 拆分组件

   1）components组件、路由组件

3. 配置路由规则，和各种跳转（编程式导航push||replace，声明式导航（router-link：必要有to属性））

   1） header的登录注册按钮

4. 运用路由元信息控制组件显示（登录和注册没有fooler组件显示 ：meta: {isShow: false}）

5. 路由传参

   1) params

   2) query
   
6. vuex获取数据和三级导航

7. 鼠标悬停样式（css 或 js 实现）

   一开始隐藏，使用hover实现display：block显示

   改为用：:style="{ display: cOneIndex == categoryIndex ? 'block' : 'none' }"

8. lodash函数的防抖和节流，按需引入lodash

9. 全部商品（在home和serach）显示与隐藏

10. 点击三级导航或搜索跳转到serach并传参（导航参数和用户搜索参数）。（怎么确定是a标签，怎么确定是几级a标签）

11. 全部商品鼠标离开和进入的过度动画

12. 优化请求，三级导航数据请求放在App.vue的mounted

13. 合并参数（导航参数和用户搜索参数）

14. mock数据（ npm install mockjs）(mock/banner.json等)（项目打包资源会放到dist文件下，所以图片放到public下）（mock/mockServer.js模拟配置请求1.引入mockjs，2.引入json文件数据）（main.js引入server）

15. mock请求和vuex

16. swiper基本使用（npm install --save swiper@5）：①引包（JS/CSS）②html结构 ③new swiper实例 ④配置swiper项

17. new swiper 实例，结构（已有）和数据（还没返回）异步问题：①定时器（有较大的交互空白时间） ②watch监听bannerList(不能保证数据有了变化，结构已经好了) ③watch+Vue.nextTict(在下次DOM更新循环结束之后延迟回调，数据更新后调用这个方法，立即更新DOM)

18. floor轮播图数据获取与DOM展示

19. 复用轮播图组件

20. search模块的·axios请求、vuex获取数据、简化数据

21. search组件的动态展示

22. $route的query参数、params参数合并返回服务器发送请求，实现动态展示

23. 整合参数

24. 面包屑处理（）

25. 排序（1. 类名）

26. 分页器(1. 起始页，2. 类名)

27. 详情页面（1. 静态与路由 2. 请求 3. vuex 4. 动态展示）

28. 详情页面添加购物车，根据商品id、商品数量、发送请求，判断

29. 按钮实现：跳转购物车页面，跳转详情页面

30. 获取购物车数据，会话存储

31. uid验证接口，加入购物车临时身份，

32. 修改产品数量、状态、删除商品(vuex接口、vuex、dispatch)

33. 删除选中的全部商品

34. 点击全选按钮全选



------

1. 注册【assets】存放组件公共资源，【public】原封不动打包
2. 登录 【获取token】跳转、用户名显示
3. 本地存储token
4. 登录后不能进入登录页，刷新后登录信息失效，需要在每一个组件中发送请求。
5. 退出登录：请求、清除本地、userInfo
6. 路由跳转设置，登录不能进入登录页面，跳转路由发送请求获取用户信息
7. 订单详情
8. 提交订单成功支付二维码
9. 支付成功界面
10. 二级路由我的订单、团购订单
11. 路由守卫 、购物车、订单支付、交易

