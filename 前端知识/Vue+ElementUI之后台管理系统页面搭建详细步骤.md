# Vue+ElementUI之后台管理系统页面搭建详细步骤

## 一、项目创建

### 1、必备环境

> 首先需要安装有npm包管理工具。这里先介绍一下npm是什么，npm是世界上最大的软件注册处。开源开发者使用npm来分享和使用开源包，也有很多组织使用npm来管理私有开发包。可以看出npm是来管理各种包的，你写的js包也可以发布到npm来让其他开发者使用。所以我们必须要安装有这个工具，这样我们可以使用很多上面的包。

### 2、创建项目

1. 我们使用vue-cli来通过命令的方式创建项目，可以通过图形界面等方式。前提：安装有vue脚手架，vue-cli

2. 命令
   
   ```bash
   vue create admin-ui
   ```
   
   显示界面
   
   ![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-25-10-55-35-image.png)
   
   选择自定义，第三项Manually
   
   ![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-25-10-57-12-image.png)
   
   这里让我们选择一些组件
   
   我选择了
   
   ![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-25-10-58-29-image.png)
   
   然后回车
   
   ![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-25-10-58-55-image.png)
   
   选择vue版本，这里选择2.x，注意：3.x可能elementUI支持还不是很好，所以只能选2.x。回车
   
   ![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-25-11-00-02-image.png)
   
   这也是关于路由的设置，路由有两种模式：hash和history。hash适合构建单页面（SPA），路径中有#，history规避了这一现象。但是history有一个问题就是访问二级页面的时候刷新会导致404。我选择Y，创建history模式的。
   
   ![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-25-11-17-06-image.png)
   
   选择Less
   
   ![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-25-11-17-40-image.png)
   
   代码检查，选择标准
   
   ![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-25-11-18-15-image.png)
   
   回车
   
   ![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-25-11-18-34-image.png)
   
   分配配置文件，回车
   
   ![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-25-11-19-29-image.png)
   
   这里问是否保存你的设置，这个自己决定，回车开始创建项目
   
   ![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-25-11-22-10-image.png)
   
   创建成功。

### 3、运行

> 项目创建完成了我们来运行一下，这时候vue-cli已经给我们创建了一个最简单的demo，我们可以运行看一下

```bash
cd admin-ui 


npm run serve
```

![](file:///Users/jay/Library/Application%20Support/marktext/images/2022-03-25-11-23-26-image.png?msec=1648178606991)

打开浏览器访问一下 http://localhost:8080/

![](file:///Users/jay/Library/Application%20Support/marktext/images/2022-03-25-11-24-05-image.png?msec=1648178645043)

说明我们创建成功了。结束服务，直接control+c

## 二、安装模块

- 安装axios
  
  > 用于发送http请求
  
  ```bash
  npm instal axios
  ```

- 安装qs
  
  > 运行期使用，所以我们需要加 -S
  
  ```bash
  npm install qs -S
  ```

- 安装mockjs
  
  > 模拟后台数据，开发测试期使用
  
  ```bash
  npm install mockjs -D
  ```

- 安装elementUI
  
  > 最主要的前端elementUI
  
  ```bash
  npm install element-ui -S
  ```

我们在项目中的package.json文件中能看到我们安装的组件

<img src="file:///Users/jay/Library/Application%20Support/marktext/images/2022-03-25-11-53-04-image.png" title="" alt="" width="287">

## 三、页面搭建

> 现在万事具备了，只等开发了，我们开始我们页面的创作，用vscode打开我们刚刚创建的项目。

### 1. 登录

#### 1.1登录页面最终样式：

![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-25-13-14-19-image.png)

#### 1.2 页面搭建

- main.js 修改代码如下
  
  ```js
  import Vue from 'vue'
  import App from './App.vue'
  import router from './router'
  import store from './store'
  import axios from 'axios'
  import ElementUI from 'element-ui'
  
  import 'element-ui/lib/theme-chalk/index.css'
  
  Vue.config.productionTip = false
  
  // 定义全局axios
  Vue.prototype.$axios = axios
  
  Vue.use(ElementUI)
  
  new Vue({
    router,
    store,
    render: h => h(App)
  }).$mount('#app')
  ```

- 先将之前构建工具给我们创建的页面删除，将App.vue里面的内容改成如下： 
  
  ```html
  <template>
    <div id="app">
      <router-view></router-view>
    </div>
  </template>
  <script></script>
  
  <style lang="less">
  html,
  body,
  #app {
    height: 100%;
    margin: 0;
    padding: 0;
  }
  </style>
  ```

- 删除views下面的home.vue和about.vue

- 新建login.vue，在views目录下，新建Login.vue。我们去elementUI官网，寻找样式，一个是表单样式，一个是左边的图片样式。最终代码
  
  ```html
  <template>
    <el-row type="flex" class="row-bg" justify="center">
      <el-col :xl="6" :lg="7">
        <h2>欢迎来到xxx管理系统</h2>
        <el-image
          :src="require('@/assets/logo.png')"
          style="height: 180px; width: 180px"
        ></el-image>
        <p>公众号：xxx</p>
        <p>扫描二维码，回复【xxx】获取登录密码</p>
      </el-col>
      <el-col :span="1">
        <el-divider direction="vertical"></el-divider>
      </el-col>
      <el-col :xl="6" :lg="7">
        <el-form
          :model="loginForm"
          :rules="rules"
          ref="loginForm"
          label-width="100px"
          class="demo-loginForm"
        >
          <el-form-item label="用户名" prop="userName" style="width: 380px">
            <el-input
              v-model="loginForm.userName"
              style="width: 100%;"
              autocomplete="off"
            ></el-input>
          </el-form-item>
          <el-form-item label="密码" prop="password" style="width: 380px">
            <el-input
              type="password"
              v-model="loginForm.password"
              style="width: 100%;"
              autocomplete="off"
            ></el-input>
          </el-form-item>
          <el-form-item label="验证码" prop="verifyCode" style="width: 380px">
            <el-input
              v-model="loginForm.verifyCode"
              style="width: 172px; float: left"
            ></el-input>
            <el-image :src="captchaImg" class="captchaImg"></el-image>
          </el-form-item>
          <el-form-item style="widht: 380px">
            <el-button type="primary" @click="submitForm('loginForm')"
              >提交</el-button
            >
            <el-button @click="resetForm('loginForm')">重置</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
  </template>
  
  <script>
  export default {
    name: 'admin-login',
    data () {
      return {
        loginForm: {
          userName: '',
          password: '',
          verifyCode: '',
          token: ''
        },
        captchaImg: null,
        rules: {
          userName: [
            { required: true, message: '请输入用户名', trigger: 'blur' }
          ],
          password: [{ required: true, message: '请输入密码', trigger: 'blur' }],
          verifyCode: [
            { required: true, message: '请输入验证码', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      resetForm (formName) {
        this.$refs[formName].resetFields()
      }
    }
  }
  </script>
  
  <style scoped>
  .el-row {
    background-color: #fafafa;
    height: 100%;
    display: flex;
    align-items: center;
    text-align: center;
  }
  .el-divider {
    height: 200px;
  }
  .captchaImg {
    float: left;
    margin-left: 8px;
    border-radius: 4px;
  }
  </style>
  ```

- 修改路由，router/index.js，页面路由到登录页面
  
  ```js
  import Vue from 'vue'
  import VueRouter from 'vue-router'
  import LoginVue from '../views/Login.vue'
  
  Vue.use(VueRouter)
  
  const routes = [
    {
      path: '/',
      name: 'admin-login',
      component: LoginVue
    }
  ]
  
  const router = new VueRouter({
    mode: 'history',
    base: process.env.BASE_URL,
    routes
  })
  
  export default router
  ```

>  这时候我们启动服务就能看到登录页面了，下面我们完成功能，获取验证码和登录

#### 1.3 功能实现

> 主要实现两个功能，验证码获取和登录。验证码获取是从后端获取一个验证码图片，展示在验证码的地方。登录是拿着用户填写的用户名、密码和验证码去后端校验是否正确。
> 
> 这里面我们使用mockjs来模拟后端返回数据。http请求使用axios来做。

##### 创建mock.js

> 在项目scr目录下创建mock.js的文件，里面有两个方法

```js
const Mock = require('mockjs')

const Random = Mock.Random

const Result = {
  code: 200,
  msg: '操作成功',
  data: null
}

const authority = [
  {
    id: '1',
    name: '首页',
    label: 'home',
    type: 'Menu',
    icon: 'el-icon-s-operation',
    code: 'home',
    url: '',
    parentCode: '00',
    component: '',
    children: [
      {
        id: '2',
        name: '修改密码',
        label: '',
        type: 'Button',
        icon: '',
        code: 'home-updatePassword',
        url: '/home/updatePassword',
        parentCode: 'home',
        component: 'main/users/UpdatePassword',
        children: []
      }
    ]
  }
]

Mock.mock('/captcha', 'get', () => {
  console.log('get verify code')
  Result.data = {
    token: Random.string(32),
    captchaImg: Random.dataImage('100x40', Random.string(6))
  }
  return Result
})

Mock.mock('/login', 'post', () => {
  Result.data = authority
  return Result
})
```

##### 创建全局axios

> 在项目scr目录下创建axios.js的文件

```js
import axios from 'axios'
import router from './router'
import Element from 'element-ui'

// axios.defaults.baseURL = "https://localhost:8081"

const request = axios.create({
  timeout: 5000,
  headers: {
    'Content-Type': 'application/json; charset=utf-8'
  }
})

request.interceptors.request.use(config => {
  config.headers.Authorization = localStorage.getItem('token')
  return config
})

request.interceptors.response.use(
  response => {
    const res = response.data

    if (res.code === 200) {
      return response
    } else {
      Element.Message.error(res.msg ? '系统异常' : res.msg)
    }
  },
  error => {
    if (error.response.data) {
      error.message = error.response.msg
    }
    if (error.response.status === 401) {
      router.push('/login')
    }

    Element.Message.error(error.message, { duration: 3000 })
    return Promise.reject(error)
  }
)

export default request
```

##### main.js中引入

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'
import axios from './axios'
import ElementUI from 'element-ui'

import 'element-ui/lib/theme-chalk/index.css'

Vue.config.productionTip = false

require('./mock.js')

// 定义全局axios
Vue.prototype.$axios = axios

Vue.use(ElementUI)

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')
```

##### 编写获取验证码方法

> 在Login.vue中编写获取验证码的方法，因为验证码是在外面vue渲染的时候就应该有的，所以我们在created方法中增加请求验证码的方法。代码如下

```js
methods: {
    resetForm (formName) {
      this.$refs[formName].resetFields()
    },
    getCaptcha () {
      this.$axios.get('/captcha').then(res => {
        this.loginForm.token = res.data.data.token
        this.captchaImg = res.data.data.captchaImg
      })
    }
  },
  created () {
    this.getCaptcha()
  }
```

##### 编写登录方法

> 登录的方法，请求后台会返回token，我们将token缓存到本地。获取了当前登录用户的权限，将权限也放入到本地缓存。

```js
submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.$axios.post("/login", this.loginForm).then((res) => {
            const jwt = res.headers["authorization"];
            this.$store.commit("SET_TOKEN", jwt);
            this.$router.push("/home");
            const authorities = [];
            const authData = res.data.data;
            const authDataFor = (authData) => {
              authData.forEach((item) => {
                authorities.push(item.code);
                if (item.children.length > 0) {
                  authDataFor(item.children);
                }
              });
            };
            authDataFor(authData);
            this.$store.commit("setAuthority", authorities);
          });
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
```

这样我们的登录就写好了。登录成功以后我们看到直接push到router中一个home的地址，所以我们需要接下来编写路由。

### 2. 路由

在路由router/index.js文件中，添加

```json
{
    path: '/home',
    name: 'admin-login',
    component: () => import('@views/main/Home.vue')
  }
```

路由最终内容

```js
import Vue from 'vue'
import store from '@/store'
import axios from 'axios'
import VueRouter from 'vue-router'
import LoginVue from '../views/Login.vue'

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'admin-login',
    component: LoginVue
  },
  {
    path: '/login',
    name: 'admin-login',
    component: LoginVue
  },
  {
    path: '/home',
    name: 'Admin-Home',
    component: () => import('@/views/main/AdminHome.vue'),
    children: [
      {
        path: '/index',
        name: 'Index',
        meta: { label: 'Index' },
        component: () => import('@/views/main/include/Index.vue')
      }
    ]
  }
]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

router.beforeEach((to, from, next) => {
  if (!store.state.menus.hasRouter) {
    axios
      .post('/system/menu/list', {
        headers: {
          Authorization: localStorage.getItem('token')
        }
      })
      .then(res => {
        const menus = res.data.data
        const newRoutes = router.options.routes
        menus.forEach(menu => {
          if (menu.children) {
            menu.children.forEach(e => {
              const route = menuToRoute(e)
              if (route) {
                newRoutes[2].children.push(route)
              }
            })
          }
        })
        router.addRoutes(newRoutes)
        // 只显示菜单
        const showMenu = menus.filter(item => {
          const childMenu = item.children.filter(c => c.type === 'Menu')
          item.children = childMenu
          return item
        })
        store.commit('setMenuList', showMenu)
        const authorities = []

        const authDataFor = authData => {
          authData.forEach(item => {
            authorities.push(item.code)
            if (item.children.length > 0) {
              authDataFor(item.children)
            }
          })
        }
        authDataFor(menus)
        store.commit('setAuthority', authorities)
        store.commit('changeHasRouterStatus', true)
      })
  }
  next()
})

const menuToRoute = menu => {
  if (!menu.component) {
    return null
  }
  const route = {
    name: menu.name,
    path: menu.url,
    meta: { label: menu.label },
    component: () => import('@/views/' + menu.component + '.vue')
  }
  return route
}

export default router
```

**注意：** 这里要要注意，所有的权限必须注册到index路由下面，要不然会导致跳不进tab

### 3. 缓存js编写

> 我们需要将登录信息缓存到本地，所以我们创建一个js，在store目录下，修改index.js，每一个页面都需要用到的缓存我们放到这里。新建目录modules，创建菜单的缓存menus.js

- store下的index.js

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    token: ''
  },
  getters: {
  },
  mutations: {
    SET_TOKEN: (state, token) => {
      state.token = token
      localStorage.setItem('token', token)
    }
  },
  actions: {
  },
  modules: {
  }
})
```

- modules下的menus.js
  
  ```js
  import Vue from 'vue'
  import Vuex from 'vuex'
  
  Vue.use(Vuex)
  
  export default {
    state: {
      menuList: [],
      authority: [],
      hasRouter: false,
      editableTabsValue: 'Index',
      editableTabs: [
        {
          name: '首页',
          lable: '首页',
          code: 'Index'
        }
      ]
    },
    getters: {},
    mutations: {
      setMenuList (state, menus) {
        state.menuList = menus
      },
  
      setAuthority (state, authority) {
        state.authority = authority
      },
  
      changeHasRouterStatus (state, hasRouter) {
        state.hasRouter = hasRouter
      },
  
      addTab (state, tab) {
        console.log('code:' + tab.code)
        console.log(state.editableTabs)
        const index = state.editableTabs.findIndex(e => e.code === tab.code)
        if (index === -1) {
          state.editableTabs.push({
            name: tab.name,
            code: tab.code
          })
        }
        state.editableTabsValue = tab.code
      },
      resetState (state) {
        state.hasRouter = false
        state.editableTabsValue = 'Index'
        state.editableTabs = [
          {
            name: '首页',
            lable: '首页',
            code: 'Index'
          }
        ]
      }
    },
    actions: {},
    modules: {}
  }
  ```

### 4. 首页

> 首页实现思路：
> 
> 1、首页包括菜单栏、头部和页面部分，所以我们需要将每一个部分抽象成一个组件，让首页来加载。
> 
> 2、创建menus.vue、header.vue、index.vue
> 
> 3、在首页引入这些组件，由这些组件组成一个首页
> 
> 4、我们还需要一个tab来展示点击的菜单对应的页面
> 
> 在views目录下创建目录main，创建AdminHome.vue

```html
<template>
  <el-container>
    <Menu></Menu>
    <el-container direction="vertical">
      <Header></Header>
      <el-main style="padding: 0;">
        <Tab></Tab>
        <div style="margin: 0 15px;">
          <router-view></router-view>
        </div>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
import Menu from '@/views/main/include/Menu'
import Header from '@/views/main/include/Header'
import Tab from '@/views/main/include/Tab'
export default {
  name: 'AdminHome',
  components: {
    Menu,
    Header,
    Tab
  }
}
</script>

<style scoped>
.el-container {
  height: 100%;
  padding: 0;
  margin: 0;
}
</style>
```

#### 1. 左侧菜单

> 在views/main/include/目录下面创建Menu.vue
> 
> 实现思路：
> 
> 1. 登录成功以后要查询出客户对应的menus
> 
> 2. 在router的创建中查询menus，见 
> 
> 3. 查询出来的menus放到了本地缓存中
> 
> 4. 在页面渲染的时候会从本地缓存中取出来，在页面循环获取
> 
> 5. 一个默认的菜单是首页，这个是写死在页面的
> 
> 6. 点击菜单会打开一个tab来显示页面内容

```html
<template>
  <el-aside width="200px">
    <el-menu
      :unique-opened="true"
      :default-active="this.$store.state.menus.activeTabsValue"
      class="el-menu-vertical-demo"
      background-color="#545c64"
      text-color="#fff"
      active-text-color="#ffd04b"
      router
    >
      <el-menu-item index="Index" route="/index">
        <i class="el-icon-s-home"></i>
        <span slot="title">首页</span>
      </el-menu-item>
      <el-submenu :index="menu.name" v-for="menu in menuList" :key="menu.id">
        <template slot="title">
          <i :class="menu.icon"></i>
          <span>{{ menu.label }}</span>
        </template>
        <el-menu-item
          :index="item.name"
          v-for="item in menu.children"
          :key="item.id"
          :route="item.url"
          @click="addTab(item)"
        >
          <i :class="item.icon"></i>
          <span>{{ item.label }}</span>
        </el-menu-item>
      </el-submenu>
    </el-menu>
  </el-aside>
</template>

<script>
export default {
  name: 'SideMenu',
  data () {
    return {}
  },
  methods: {
    addTab (menu) {
      this.$store.commit('addTab', menu)
    }
  },
  computed: {
    menuList () {
      return this.$store.state.menus.menuList
    }
  }
}
</script>

<style>
.el-menu-vertical-demo {
  height: 100%;
}
</style>
```

#### 2. 头部

> 在views/main/include/目录下面创建Header.vue

```html
<template>
  <el-header>
    <strong>Vue-Admin管理后台</strong>
    <div class="header-avatar">
      <el-avatar> user </el-avatar>
      <el-dropdown @command="handleCommand">
        <span class="el-dropdown-link">
          admin<i class="el-icon-arrow-down el-icon--right"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item command="userInfo">修改密码</el-dropdown-item>
          <el-dropdown-item command="logout">退出</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
  </el-header>
</template>

<script>
export default {
  name: 'MainHeader',
  methods: {
    handleCommand (command) {
      if (command === 'logout') {
        this.$axios.post('/logout').then(res => {
          localStorage.clear()
          sessionStorage.clear()
          this.$store.commit('resetState')
          this.$router.replace('/login')
        })
      } else if (command === 'userInfo') {
        this.$router.push('/system/User/updatePasword')
      }
    }
  }
}
</script>

<style>
.el-dropdown-link {
  cursor: pointer;
  color: #fff;
}
.header-avatar {
  float: right;
  width: 150px;
  display: flex;
  align-items: center;
  justify-content: space-around;
}
.el-header {
  background-color: #3e8bf0;
  color: #fff;
  text-align: center;
  line-height: 60px;
}
</style>
```

#### 3. 默认首页

> 在views/main/include/目录下面创建Index.vue

```html
<template>
  <el-card class="box-card">
    欢迎xxx登录，上次登录时间是xxxx
  </el-card>
</template>

<script>
export default {
  name: 'HomeIndex',
  created () {
    console.log('zxlm')
  }
}
</script>

<style></style>
```

#### 4、tab组件

> 在views/main/include/目录下面创建Tab.vue
> 
> 实现原理
> 
> 1. 默认会打开一个主页的tab，这个tab是不允许关闭的
> 
> 2. 点击菜单的时候会调用addTab方法，这个方法是定义在menu.js中的
> 
> 3. 点击tab的时候页面要切换到对应的路由
> 
> 4. 移除tab，需要根据name进行移除，会看全局一个有几个tab，然后将和要移除的tab一样的name进行移除，下一个tab获得焦点
> 
> 5. 如果已经打开了tab，刷新以后要把之前打开的tab保持打开状态，我们会根据本地缓存中的activeTabs进行循环打开。
> 
> 6. 在App.vue中有一个watch的方法，每次路由都会调用，这里会调用addTab，所以点击菜单会打开一个tab

```html
<template>
  <el-tabs
    v-model="activeTabsValue"
    type="card"
    closable
    @tab-remove="removeTab"
    @tab-click="clickTab"
  >
    <el-tab-pane
      v-for="item in activeTabs"
      :key="item.id"
      :label="item.label"
      :name="item.name"
    >
    </el-tab-pane>
  </el-tabs>
</template>

<script>
export default {
  name: 'AdminTab',
  data () {
    return {}
  },
  computed: {
    activeTabs: {
      get () {
        return this.$store.state.menus.activeTabs
      },
      set (val) {
        this.$store.state.menus.activeTabs = val
      }
    },
    activeTabsValue: {
      get () {
        return this.$store.state.menus.activeTabsValue
      },
      set (val) {
        this.$store.state.menus.activeTabsValue = val
      }
    }
  },
  methods: {
    removeTab (targetName) {
      if (targetName === 'Index') {
        return
      }
      const tabs = this.activeTabs
      let activeName = this.activeTabsValue
      if (activeName === targetName) {
        tabs.forEach((tab, index) => {
          if (tab.name === targetName) {
            const nextTab = tabs[index + 1] || tabs[index - 1]
            if (nextTab) {
              activeName = nextTab.name
            }
          }
        })
      }
      this.activeTabsValue = activeName
      this.activeTabs = tabs.filter(tab => tab.name !== targetName)
      this.$router.push({ name: activeName })
    },
    clickTab (target) {
      this.$router.push({ name: target.name })
    }
  }
}
</script>

<style></style>
```

#### 5. 全局方法

在App.vue中添加js方法

```js
 watch: {
    $route (to, from) {
      if (
        to.path !== '/' &&
        to.path !== '/login' &&
        to.path !== '/home' &&
        to.path !== '/logout'
      ) {
        const obj = { name: to.name, code: to.meta.code }
        this.$store.commit('addTab', obj)
      }
    }
  }
```

  

### 7. 用户管理

#### 创建用户vue

> 实现思路
> 
> 1. 点击用户管理展示用户列表
> 
> 2. 点击新增弹出dialog添加用户信息
> 
> 3. 修改同新增，和新增共用一个dialog
> 
> 4. 删除选中行
> 
> 5. 分配角色，给用户分配角色，弹出分配角色dialog，选择角色。单选
> 
> 6. 选中需要启用的用于批量启用用户，禁用和启用相似
> 
> 7. 也可以选中需要删除的用户进行批量删除

列表vue

```html
<template>
  <div>
    <el-form :inline="true" :model="formInline" class="demo-form-inline">
      <el-form-item label="用户名">
        <el-input v-model="formInline.userName" placeholder="用户名"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button
          type="primary"
          @click="query"
          v-if="hasAuthority('system-user-queryList')"
          >查询</el-button
        >
      </el-form-item>
      <el-button
        type="primary"
        @click="add()"
        v-if="hasAuthority('system-user-add')"
        >新增</el-button
      >
      <el-button
        type="primary"
        :disabled="batchBtnDisable"
        @click="handleStatusBatch('enable')"
        v-if="hasAuthority('system-user-batchEnable')"
        >批量启用</el-button
      >
      <el-button
        type="danger"
        :disabled="batchBtnDisable"
        @click="handleStatusBatch('disable')"
        v-if="hasAuthority('system-user-batchDisable')"
        >批量禁用</el-button
      >
      <el-button
        type="danger"
        :disabled="batchBtnDisable"
        @click="handleBatchDelete"
        v-if="hasAuthority('system-user-batchDelete')"
        >批量删除</el-button
      >
    </el-form>
    <el-table
      ref="userTable"
      :data="tableData"
      border
      @select="handleSelectChange"
      style="width: 100%"
    >
      <el-table-column type="selection" width="55"></el-table-column>
      <el-table-column
        prop="userName"
        label="用户名"
        width="180"
      ></el-table-column>
      <el-table-column
        prop="roleName"
        label="角色名称"
        width="180"
      ></el-table-column>
      <el-table-column prop="status" label="状态" width="100">
        <template slot-scope="scope">
          <el-tag size="mini" v-if="scope.row.status === 'normal'">正常</el-tag>
          <el-tag
            size="mini"
            type="danger"
            v-else-if="scope.row.status === 'disable'"
            >禁用</el-tag
          >
        </template>
      </el-table-column>
      <el-table-column prop="desc" label="描述"></el-table-column>
      <el-table-column label="操作" width="160">
        <template slot-scope="scope">
          <el-button
            type="text"
            @click="handleSetRole(scope.row)"
            v-if="hasAuthority('system-user-setRole')"
            >分配角色</el-button
          >
          <el-button
            type="text"
            @click="handleEdit(scope.$index, scope.row)"
            v-if="hasAuthority('system-user-update')"
            >修改</el-button
          >
          <el-button
            type="text"
            @click="handleDelete(scope.$index, scope.row)"
            v-if="hasAuthority('system-user-delete')"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>

    <Pagination ref="pagination"></Pagination>
    <AddAndUpdateDialog
      ref="addAndUpdateDialog"
      @reloadData="loadTableData"
    ></AddAndUpdateDialog>

    <UserRoleDialog ref="userRoleDialog"></UserRoleDialog>
  </div>
</template>

<script>
import AddAndUpdateDialog from './AddAndUpdateDialog.vue'
import Pagination from '@/views/main/include/Pagination'
import UserRoleDialog from './UserRoleDialog.vue'

export default {
  name: 'UserList',
  data () {
    return {
      batchBtnDisable: true,
      selectIds: [],
      tableData: [],
      formInline: {
        user: '',
        region: ''
      }
    }
  },
  methods: {
    query () {
      console.log('query!')
    },
    loadTableData () {
      this.$axios.post('/user/list').then(response => {
        this.tableData = response.data.data.records
        this.$refs.pagination.total = response.data.data.total
        this.$refs.pagination.pageSize = response.data.data.pageSize
        this.$refs.pagination.currentPage = response.data.data.currentPage
      })
    },
    // 新增
    add () {
      this.showAddAndUpdateDialog()
    },
    showAddAndUpdateDialog () {
      this.$refs.addAndUpdateDialog.addAndUpdateDialogVisible = true
    },
    // 选中、取消checkbox触发事件
    handleSelectChange (selection) {
      this.selectIds = []
      if (selection.length > 0) {
        this.batchBtnDisable = false
        selection.forEach(element => {
          this.selectIds.push(element.id)
        })
      } else {
        this.batchBtnDisable = true
      }
    },
    // 删除的时候触发
    handleDelete () {
      this.common
        .myConfirm('此操作将永久删除角色, 确定要继续吗?')
        .then(() => {
          this.common.myMessageSuccess('删除成功')
          this.loadTableData()
        })
        .catch(() => {})
    },
    handleBatchEnable () {
      this.common
        .myConfirm('确定要启用吗？')
        .then(() => {
          this.common.myMessageSuccess('启用成功')
          this.loadTableData()
        })
        .catch(() => {})
    },
    // 批量禁用或启用
    handleStatusBatch (status) {
      this.common
        .myConfirm('确定要操作吗？')
        .then(() => {
          this.batchOperation(
            '/user/updateStatus',
            {
              status: status,
              ids: this.selectIds
            },
            '批量操作成功'
          )
        })
        .catch(() => {})
    },
    // 批量删除
    handleBatchDelete () {
      console.log(this.selectIds)
      this.common
        .myConfirm('此操作将永久删除角色, 确定要继续吗?')
        .then(() => {
          this.batchOperation(
            '/user/batchDelete',
            { ids: this.selectIds },
            '删除成功'
          )
        })
        .catch(() => {})
    },
    batchOperation (url, option, message) {
      this.$axios
        .post(url, option)
        .then(res => {
          if (res.status === 200) {
            this.common.myMessageSuccess(message)
            this.loadTableData()
          } else {
            this.common.myMessageFaild('操作失败' + res.status)
          }
        })
        .catch(() => {})
    },
    // 编辑
    handleEdit (id) {
      this.$axios.post('/user/getInfo/' + id).then(res => {
        this.$refs.addAndUpdateDialog.userForm = res.data.data
        this.showAddAndUpdateDialog()
      })
    },
    handleSetRole (row) {
      this.$refs.userRoleDialog.userId = row.id
      this.$refs.userRoleDialog.userRoleDialogVisible = true
    }
  },
  created () {
    // 加载数据
    this.loadTableData()
  },
  components: {
    AddAndUpdateDialog,
    Pagination,
    UserRoleDialog
  }
}
</script>

<style scoped></style>
```

添加、修改dialog

```html
<template>
  <el-dialog
    :visible.sync="addAndUpdateDialogVisible"
    width="30%"
    :before-close="handleClose"
    :destroy-on-close="true"
    :close-on-click-modal="false"
  >
    <el-form
      :model="userForm"
      :rules="userFormRules"
      ref="userForm"
      label-width="100px"
    >
      <el-form-item label="用户名" prop="userName">
        <el-input v-model="userForm.userName"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" v-model="userForm.password"></el-input>
      </el-form-item>
      <el-form-item label="描述" prop="desc">
        <el-input v-model="userForm.desc"></el-input>
      </el-form-item>
      <el-form-item label="状态" prop="status">
        <el-select v-model="userForm.status" placeholder="请选择状态">
          <el-option label="正常" value="normal"></el-option>
          <el-option label="禁用" value="disable"></el-option>
        </el-select>
      </el-form-item>
    </el-form>

    <span slot="footer" class="dialog-footer">
      <el-button type="primary" @click="roleSubmitForm('userForm')"
        >保存</el-button
      >
      <el-button @click="resetForm('userForm')">重置</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  data () {
    return {
      addAndUpdateDialogVisible: false,
      userForm: { id: '', name: '', code: '', desc: '', status: '' },
      userFormRules: {
        userName: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 2, max: 10, message: '长度在2到10个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输密码', trigger: 'blur' },
          { min: 2, max: 10, message: '长度在2到10个字符', trigger: 'blur' }
        ],
        status: [{ required: true, message: '请选择状态', trigger: 'blur' }]
      }
    }
  },
  methods: {
    // 关闭dialog触发
    handleClose (done) {
      this.addAndUpdateDialogVisible = false
      this.resetForm('userForm')
    },
    // 新增的时候触发
    roleSubmitForm (formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          if (this.userForm.id) {
            this.sendRequest('/user/update', '修改')
          } else {
            this.sendRequest('/user/add', '添加')
          }
        } else {
          return false
        }
      })
    },
    // 发送请求
    sendRequest (requestUrl, operation) {
      this.$axios
        .post(requestUrl, this.roleForm)
        .then(res => {
          if (res.data.code === 200) {
            this.common.myMessageSuccess(operation + '成功')
            this.handleClose()
            this.$emit('loadTableData')
          }
        })
        .catch(error => {
          this.common.myMessageFaild(operation + '失败' + error)
        })
    },
    // 重置
    resetForm (formName) {
      this.$refs[formName].resetFields()
      this.roleForm = {}
    }
  }
}
</script>

<style scoped>
.el-input {
  width: 200px;
}
</style>
```

#### 按钮权限控制

定义全局函数

```js
import Vue from 'vue'

Vue.mixin({
  methods: {
    hasAuthority (authorityCode) {
      const authority = this.$store.state.menus.authority

      return authority.indexOf(authorityCode) > -1
    }
  }
})
```

在main.js中引入就可以使用全局函数

```js
import global from "./globalFunction"
```

这样在使用的时候就可以直接在需要加权限的按钮上使用

v-if="hasAuthority('system-user-update')"

<b>实现按钮权限控制</b>

#### 封装自己message

新建common.js

```js
import { MessageBox, Message } from 'element-ui'

export function myConfirm (msg = '操作成功') {
  return MessageBox.confirm(msg, '提示', {
    confirmButtonText: '确定',
    cancelButtonText: '取消',
    type: 'warning',
    showClose: false,
    closeOnClickModal: false,
    closeOnPressEscape: false
  })
}

export function myMessageSuccess (message) {
  return myMessage(message, 'success')
}

export function myMessageFaild (message) {
  return myMessage(message, 'error')
}

function myMessage (message, type) {
  return Message({
    type: type,
    message: message
  })
}

export default {
  myConfirm,
  myMessageSuccess,
  myMessageFaild
}
```

在main.js中引入

```js
const common = require('./common')

Vue.prototype.common = common
```

#### App.vue最终代码

```js
<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>
<script>
export default {
  watch: {
    $route (to, from) {
      if (
        to.path !== '/' &&
        to.path !== '/login' &&
        to.path !== '/home' &&
        to.path !== '/logout'
      ) {
        const obj = { name: to.name, label: to.meta.label }
        this.$store.commit('addTab', obj)
      }
    }
  }
}
</script>

<style lang="less">
html,
body,
#app {
  height: 100%;
  margin: 0;
  padding: 0;
}
</style>
```

#### 封装pagination组件

> 因为我们每一个列表页面几乎都需要用到分页，所以我们需要把它封装成一个组件，需要在需要的地方引入调用就就可以，减少了页面代码。

1. 新建Pagination.vue
   
   ```html
   <template>
     <el-pagination
       background
       @size-change="handleSizeChange"
       @current-change="handleCurrentChange"
       :current-page="currentPage"
       :page-sizes="[10, 20, 50, 100]"
       :page-size="pageSize"
       layout="total, sizes, prev, pager, next, jumper"
       :total="total"
     ></el-pagination>
   </template>
   
   <script>
   export default {
     name: 'System-Pagination',
     data () {
       return {
         currentPage: 1,
         pageSize: 10,
         total: 0
       }
     },
     methods: {
       handleSizeChange (val) {
         console.log(`每页 ${val} 条`)
       },
       handleCurrentChange (val) {
         console.log(`当前页: ${val}`)
       }
     }
   }
   </script>
   
   <style>
   .el-pagination {
     text-align: right;
   }
   </style>
   ```

2. 分页的地方引入组件Pagination
   
   ```js
   import Pagination from '@/views/main/include/Pagination'
   ```
   
   添加组件
   components: {
   
       Pagination
   
     }
   
   使用的地方
   <Pagination ref="pagination"></Pagination>

```
> 这样大大减少了页面代码

#### 批量启用、禁用

> 选择复选框然后把选中的记录删除。我们用到了复选框的选择事件，然后将选中的行记录的id放入一个数组，点击删除的时候将数组传给后台进行删除
> 
> 方法如下：

```js
handleBatchEnable () {
   this.common
     .myConfirm('确定要启用吗？')
     .then(() => {
       this.common.myMessageSuccess('启用成功')
       this.loadTableData()
     })
     .catch(() => {})
 },
 // 批量禁用或启用
 handleStatusBatch (status) {
   this.common
     .myConfirm('确定要操作吗？')
     .then(() => {
       this.batchOperation(
         '/user/updateStatus',
         {
           status: status,
           ids: this.selectIds
         },
         '批量操作成功'
       )
     })
     .catch(() => {})
 },
 // 批量删除
 handleBatchDelete () {
   console.log(this.selectIds)
   this.common
     .myConfirm('此操作将永久删除角色, 确定要继续吗?')
     .then(() => {
       this.batchOperation(
         '/user/batchDelete',
         { ids: this.selectIds },
         '删除成功'
       )
     })
     .catch(() => {})
 },
 batchOperation (url, option, message) {
   this.$axios
     .post(url, option)
     .then(res => {
       if (res.status === 200) {
         this.common.myMessageSuccess(message)
         this.loadTableData()
       } else {
         this.common.myMessageFaild('操作失败' + res.status)
       }
     })
     .catch(() => {})
 },
```

#### 批量删除

原理同上

### 8. 角色管理

> 实现原理和用户管理是一样的，这里直接上代码

#### 列表页面

```html
<template>
  <div>
    <el-form :inline="true" :model="formInline" class="demo-form-inline">
      <el-form-item label="角色编码">
        <el-input v-model="formInline.code" placeholder="角色编码"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button
          type="primary"
          @click="query"
          v-if="hasAuthority('system-role-list')"
          >查询</el-button
        >
      </el-form-item>
      <el-button
        type="primary"
        @click="add()"
        v-if="hasAuthority('system-role-add')"
        >新增</el-button
      >
      <el-button
        type="primary"
        :disabled="batchBtnDisable"
        @click="handleStatusBatch('enable')"
        v-if="hasAuthority('system-role-batchEnable')"
        >批量启用</el-button
      >
      <el-button
        type="danger"
        :disabled="batchBtnDisable"
        @click="handleStatusBatch('disable')"
        v-if="hasAuthority('system-role-batchDisable')"
        >批量禁用</el-button
      >
      <el-button
        type="danger"
        :disabled="batchBtnDisable"
        @click="handleBatchDelete"
        v-if="hasAuthority('system-role-batchDelete')"
        >批量删除</el-button
      >
    </el-form>
    <el-table
      ref="roleTable"
      :data="tableData"
      border
      @select="handleSelectChange"
      style="width: 100%"
    >
      <el-table-column type="selection" width="55"></el-table-column>
      <el-table-column
        prop="name"
        label="角色名称"
        width="180"
      ></el-table-column>
      <el-table-column prop="code" label="编码" width="180"></el-table-column>
      <el-table-column prop="status" label="状态" width="100">
        <template slot-scope="scope">
          <el-tag size="mini" v-if="scope.row.status === 'normal'">正常</el-tag>
          <el-tag
            size="mini"
            type="danger"
            v-else-if="scope.row.status === 'disable'"
            >禁用</el-tag
          >
        </template>
      </el-table-column>
      <el-table-column prop="desc" label="描述"></el-table-column>
      <el-table-column label="操作" width="160">
        <template slot-scope="scope">
          <el-button
            type="text"
            @click="handleAuthority(scope.row)"
            v-if="hasAuthority('system-role-setAuth')"
            >分配权限</el-button
          >
          <el-button
            type="text"
            @click="handleEdit(scope.$index, scope.row)"
            v-if="hasAuthority('system-role-update')"
            >修改</el-button
          >
          <el-button
            type="text"
            @click="handleDelete(scope.$index, scope.row)"
            v-if="hasAuthority('system-role-delete')"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>

    <Pagination ref="pagination"></Pagination>

    <AddAndUpdateDialog
      ref="addAndUpdateDialog"
      @reloadData="loadTableData"
    ></AddAndUpdateDialog>
    <RoleAuthorityDialog ref="roleAuthorityDialog"></RoleAuthorityDialog>
  </div>
</template>

<script>
import AddAndUpdateDialog from './AddAndUpdateDialog.vue'
import RoleAuthorityDialog from './RoleAuthorityDialog.vue'
import Pagination from '@/views/main/include/Pagination'

export default {
  name: 'Admin-Role',
  data () {
    return {
      batchBtnDisable: true,
      selectIds: [],
      tableData: [],
      formInline: {
        user: '',
        region: ''
      }
    }
  },
  methods: {
    query () {
      console.log('query!')
    },
    loadTableData () {
      this.$axios.post('/role/list').then(response => {
        this.tableData = response.data.data.records
        this.$refs.pagination.total = response.data.data.total
        this.$refs.pagination.pageSize = response.data.data.pageSize
        this.$refs.pagination.currentPage = response.data.data.currentPage
      })
    },
    // 新增
    add () {
      this.showAddAndUpdateDialog()
    },
    showAddAndUpdateDialog () {
      this.$refs.addAndUpdateDialog.addAndUpdateDialogVisible = true
    },
    // 授权
    handleAuthority (row) {
      console.log(row.id)
      this.$refs.roleAuthorityDialog.roleId = row.id
      this.$refs.roleAuthorityDialog.roleAuthorityDialogVisible = true
    },
    // 选中、取消checkbox触发事件
    handleSelectChange (selection) {
      this.selectIds = []
      if (selection.length > 0) {
        this.batchBtnDisable = false
        selection.forEach(element => {
          this.selectIds.push(element.id)
        })
      } else {
        this.batchBtnDisable = true
      }
    },
    // 删除的时候触发
    handleDelete () {
      this.common
        .myConfirm('此操作将永久删除角色, 确定要继续吗?')
        .then(() => {
          this.common.myMessageSuccess('删除成功')
          this.loadTableData()
        })
        .catch(() => {})
    },
    handleBatchEnable () {
      this.common
        .myConfirm('确定要启用吗？')
        .then(() => {
          this.common.myMessageSuccess('启用成功')
          this.loadTableData()
        })
        .catch(() => {})
    },
    // 批量禁用或启用
    handleStatusBatch (status) {
      this.common
        .myConfirm('确定要操作吗？')
        .then(() => {
          this.batchOperation(
            '/role/updateStatus',
            {
              status: status,
              ids: this.selectIds
            },
            '批量操作成功'
          )
        })
        .catch(() => {})
    },
    // 批量删除
    handleBatchDelete () {
      console.log(this.selectIds)
      this.common
        .myConfirm('此操作将永久删除角色, 确定要继续吗?')
        .then(() => {
          this.batchOperation(
            '/role/batchDelete',
            { ids: this.selectIds },
            '删除成功'
          )
        })
        .catch(() => {})
    },
    batchOperation (url, option, message) {
      this.$axios
        .post(url, option)
        .then(res => {
          if (res.status === 200) {
            this.common.myMessageSuccess(message)
            this.loadTableData()
          } else {
            this.common.myMessageFaild('操作失败' + res.status)
          }
        })
        .catch(() => {})
    },
    // 编辑
    handleEdit (id) {
      this.$axios.post('/role/getInfo/' + id).then(res => {
        this.$refs.addAndUpdateDialog.roleForm = res.data.data
        this.showAddAndUpdateDialog()
      })
    }
  },
  created () {
    // 加载数据
    this.loadTableData()
  },
  components: {
    AddAndUpdateDialog,
    RoleAuthorityDialog,
    Pagination
  }
}
</script>

<style>
.el-pagination {
  margin-top: 10;
  text-align: right;
}
</style>
```

#### 添加修改页面

```html
<template>
  <el-dialog
    :visible.sync="addAndUpdateDialogVisible"
    width="30%"
    :before-close="handleClose"
    :destroy-on-close="true"
    :close-on-click-modal="false"
  >
    <el-form
      :model="roleForm"
      :rules="roleFormRules"
      ref="roleForm"
      label-width="100px"
    >
      <el-form-item label="角色名称" prop="name">
        <el-input v-model="roleForm.name" style="width:200px;"></el-input>
      </el-form-item>
      <el-form-item label="编码" prop="code">
        <el-input v-model="roleForm.code" style="width:200px;"></el-input>
      </el-form-item>
      <el-form-item label="描述" prop="desc">
        <el-input v-model="roleForm.desc" style="width:200px;"></el-input>
      </el-form-item>
      <el-form-item label="状态" prop="status">
        <el-select
          v-model="roleForm.status"
          placeholder="请选择状态"
          style="width:100"
        >
          <el-option label="正常" value="normal"></el-option>
          <el-option label="禁用" value="disable"></el-option>
        </el-select>
      </el-form-item>
    </el-form>

    <span slot="footer" class="dialog-footer">
      <el-button type="primary" @click="roleSubmitForm('roleForm')"
        >保存</el-button
      >
      <el-button @click="resetForm('roleForm')">重置</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  data () {
    return {
      addAndUpdateDialogVisible: false,
      roleForm: { id: '', name: '', code: '', desc: '', status: '' },
      roleFormRules: {
        name: [
          { required: true, message: '请输入权限名称', trigger: 'blur' },
          { min: 2, max: 10, message: '长度在2到10个字符', trigger: 'blur' }
        ],
        code: [
          { required: true, message: '请输入权限编码', trigger: 'blur' },
          { min: 2, max: 10, message: '长度在2到10个字符', trigger: 'blur' }
        ],
        status: [{ required: true, message: '请选择状态', trigger: 'blur' }]
      }
    }
  },
  methods: {
    // 关闭dialog触发
    handleClose (done) {
      this.addAndUpdateDialogVisible = false
      this.resetForm('roleForm')
    },
    // 新增的时候触发
    roleSubmitForm (formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          if (this.roleForm.id) {
            this.sendRequest('/role/update', '修改')
          } else {
            this.sendRequest('/role/add', '添加')
          }
        } else {
          return false
        }
      })
    },
    // 发送请求
    sendRequest (requestUrl, operation) {
      this.$axios
        .post(requestUrl, this.roleForm)
        .then(res => {
          if (res.data.code === 200) {
            this.common.myMessageSuccess(operation + '成功')
            this.handleClose()
            this.$emit('loadTableData')
          }
        })
        .catch(error => {
          this.common.myMessageFaild(operation + '失败' + error)
        })
    },
    // 重置
    resetForm (formName) {
      this.$refs[formName].resetFields()
      this.roleForm = {}
    }
  }
}
</script>

<style></style>
```

#### 分配权限页面

```js
<template>
  <el-dialog
    :visible.sync="roleAuthorityDialogVisible"
    width="30%"
    destroy-on-close
    :close-on-click-modal="false"
  >
    <el-tree
      :data="data"
      show-checkbox
      node-key="id"
      ref="tree"
      check-strictly
      default-expand-all
      @check="checkeTree"
    ></el-tree>
    <span slot="footer" class="dialog-footer">
      <el-button
        type="primary"
        @click="getCheckedKeys"
        v-if="hasAuthority('system:role-authorization-save')"
        >确定</el-button
      >
      <el-button @click="handlerCancle">取消</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  data () {
    return {
      roleId: null,
      roleAuthorityDialogVisible: false,
      data: [
        {
          id: 1,
          label: '首页',
          children: []
        },
        {
          id: 2,
          label: '系统管理',
          children: [
            {
              id: 5,
              label: '用户管理'
            },
            {
              id: 6,
              label: '角色管理'
            },
            {
              id: 7,
              label: '权限管理'
            }
          ]
        }
      ]
    }
  },
  methods: {
    getCheckedKeys () {},
    checkeTree (data) {
      let thisNode = this.$refs.tree.getNode(data.id) // 获取当前节点
      const keys = this.$refs.tree.getCheckedKeys() // 获取已勾选节点的key值

      if (thisNode.checked) {
        // 当前节点若被选中
        for (let i = thisNode.level; i > 1; i--) {
          // 判断是否有父级节点
          if (!thisNode.parent.checked) {
            // 父级节点未被选中，则将父节点替换成当前节点，往上继续查询，并将此节点key存入keys数组
            thisNode = thisNode.parent
            keys.push(thisNode.data.id)
          }
        }
      }
      this.$refs.tree.setCheckedKeys(keys) // 将所有keys数组的节点全选中
    },
    handlerCancle () {
      this.$refs.tree.setCheckedKeys([])
      this.roleAuthorityDialogVisible = false
    }
  }
}
</script>

<style></style>
```

### 9. 权限管理

> 实现原理同上，不同的是这里的列表页面没有分页，是树状展示的

#### 列表页面

```html
<template>
  <div>
    <el-row>
      <el-button
        type="primary"
        size="small"
        @click="add()"
        v-if="hasAuthority('system-authorization-add')"
        >新增</el-button
      >
    </el-row>
    <p></p>
    <el-table
      :data="tableData"
      row-key="id"
      border
      stripe
      default-expand-all
      :tree-props="{ children: 'children', hasChildren: 'hasChildren' }"
    >
      <el-table-column prop="name" label="名称" width="180" />
      <el-table-column prop="code" label="编码" width="180" />
      <el-table-column prop="icon" label="图标" />
      <el-table-column prop="type" label="类型">
        <template slot-scope="scope">
          <el-tag size="mini" v-if="scope.row.type === 'Menu'">菜单</el-tag>
          <el-tag
            size="mini"
            type="success"
            v-else-if="scope.row.type === 'Button'"
            >按钮</el-tag
          >
        </template>
      </el-table-column>
      <el-table-column prop="url" label="URL" />
      <el-table-column prop="component" label="组件" />
      <el-table-column prop="order" label="序号" />
      <el-table-column prop="status" label="状态">
        <template slot-scope="scope">
          <el-tag size="mini" v-if="scope.row.status === 'normal'">正常</el-tag>
          <el-tag
            size="mini"
            type="danger"
            v-else-if="scope.row.status === 'disable'"
            >禁用</el-tag
          >
        </template>
      </el-table-column>
      <el-table-column label="操作" width="160">
        <template slot-scope="scope">
          <el-button
            size="mini"
            @click="handleEdit(scope.$index, scope.row)"
            v-if="hasAuthority('system-authorization-update')"
            >修改</el-button
          >
          <el-button
            size="mini"
            type="danger"
            @click="handleDelete(scope.$index, scope.row)"
            v-if="hasAuthority('system-authorization-delete')"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>
    <AuthorizationAddAndUpdateDialog
      ref="AuthorizationAddAndUpdateDialog"
      @reloadData="reloadData"
    ></AuthorizationAddAndUpdateDialog>
  </div>
</template>

<script>
import AuthorizationAddAndUpdateDialog from './AddAndUpdateDialog.vue'
export default {
  name: 'System-Authorization',
  components: {
    AuthorizationAddAndUpdateDialog
  },
  data () {
    return {
      tableData: []
    }
  },
  created () {
    this.loadTableData()
  },
  methods: {
    // dialog调用重新加载数据
    reloadData (reload) {
      this.loadTableData()
    },
    // 加载表格数据
    loadTableData () {
      this.$axios.post('/authorization/list').then(response => {
        this.tableData = response.data.data
      })
    },
    add () {
      this.showDialog()
    },
    showDialog () {
      this.$refs.AuthorizationAddAndUpdateDialog.dialogVisible = true
      this.$refs.AuthorizationAddAndUpdateDialog.authorizations = this.tableData
    },
    handleDelete () {
      this.common
        .myConfirm('此操作将永久删除权限, 确定要继续吗?')
        .then(() => {
          this.common.myMessageSuccess('删除成功')
          this.loadTableData()
        })
        .catch(() => {})
    },
    handleEdit (id) {
      this.$axios.get('/authorization/getInfo/' + id).then(res => {
        this.$refs.AuthorizationAddAndUpdateDialog.editForm = res.data.data
        this.showDialog()
      })
    }
  }
}
</script>

<style lang="less"></style>
```

#### 添加修改

```html
<template>
  <div>
    <el-dialog
      :visible.sync="dialogVisible"
      width="40%"
      :before-close="handleClose"
      :destroy-on-close="true"
      :close-on-click-modal="false"
    >
      <el-form
        :model="editForm"
        :rules="editFormRules"
        ref="editForm"
        label-width="100px"
        class="demo-editForm"
      >
        <el-form-item label="上级权限" prop="parentCode">
          <el-select
            v-model="editForm.parentCode"
            placeholder="请选择类型"
            class="authorizationInput"
          >
            <template v-for="authorization in authorizations">
              <el-option
                :label="authorization.name"
                :value="authorization.code"
                :key="authorization.id"
              ></el-option>
              <template :v-if="authorization.children">
                <el-option
                  :label="item.name"
                  :value="item.code"
                  v-for="item in authorization.children"
                  :key="item.id"
                  >{{ '--' + item.name }}</el-option
                >
              </template>
            </template>
          </el-select>
        </el-form-item>
        <el-form-item label="权限名称" prop="name">
          <el-input
            v-model="editForm.name"
            class="authorizationInput"
          ></el-input>
        </el-form-item>
        <el-form-item label="编码" prop="code">
          <el-input
            v-model="editForm.code"
            class="authorizationInput"
          ></el-input>
        </el-form-item>
        <el-form-item label="图标" prop="icon">
          <el-input
            v-model="editForm.icon"
            class="authorizationInput"
          ></el-input>
        </el-form-item>
        <el-form-item label="类型" prop="type">
          <el-select
            v-model="editForm.type"
            placeholder="请选择类型"
            class="authorizationInput"
          >
            <el-option label="菜单" value="menu"></el-option>
            <el-option label="按钮" value="button"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="URL" prop="url">
          <el-input
            v-model="editForm.url"
            class="authorizationInput"
          ></el-input>
        </el-form-item>

        <el-form-item label="组件" prop="component">
          <el-input
            v-model="editForm.component"
            class="authorizationInput"
          ></el-input>
        </el-form-item>
        <el-form-item label="状态" prop="status">
          <el-select
            v-model="editForm.status"
            placeholder="请选择状态"
            class="authorizationInput"
          >
            <el-option label="正常" value="normal"></el-option>
            <el-option label="禁用" value="disable"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="submitForm('editForm')"
          >保存</el-button
        >
        <el-button @click="resetForm('editForm')">重置</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  methods: {
    submitForm (formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          if (this.editForm.id !== '') {
            this.sendRequest('/authorization/update', '修改')
          } else {
            this.sendRequest('/authorization/add', '添加')
          }
        } else {
          return false
        }
      })
    },
    sendRequest (requestUrl, operation) {
      this.$axios
        .post(requestUrl, this.editForm)
        .then(res => {
          if (res.data.code === 200) {
            this.common.myMessageSuccess(operation + '成功')
            this.handleClose()
            this.$emit('reloadData', 'reload')
          }
        })
        .catch(error => {
          this.common.myMessageFaild(operation + '失败' + error)
        })
    },
    handleClose (done) {
      this.dialogVisible = false
      this.resetForm('editForm')
    },
    resetForm (formName) {
      this.$refs[formName].resetFields()
      this.editForm = {}
    }
  },

  data () {
    return {
      authorizations: [],
      dialogVisible: false,
      editForm: {
        id: '',
        name: '',
        parentId: '',
        code: '',
        type: '',
        status: ''
      },
      editFormRules: {
        parentId: [
          { required: true, message: '全选择上级权限', trigger: 'blur' }
        ],
        name: [
          { required: true, message: '请输入权限名称', trigger: 'blur' },
          { min: 2, max: 10, message: '长度在2到10 个字符', trigger: 'blur' }
        ],
        code: [
          { required: true, message: '请输入权限编码', trigger: 'blur' },
          { min: 2, max: 10, message: '长度在2到10个字符', trigger: 'blur' }
        ],
        type: [{ required: true, message: '请选择类型', trigger: 'blur' }],
        status: [{ required: true, message: '请选择状态' }]
      }
    }
  }
}
</script>

<style scoped>
.authorizationInput {
  width: 300px;
}
</style>
```

## 四、结束

> 以上功能基本将后台管理常用页面都已经包含，所以其他功能模块都是相似操作。

项目地址： https://github.com/jianliangWang/admin-ui

#### 项目截图

![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-28-15-59-41-image.png)

![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-28-16-00-13-image.png)

![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-28-16-00-34-image.png)

![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-28-16-01-16-image.png)

![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-28-16-01-56-image.png)
