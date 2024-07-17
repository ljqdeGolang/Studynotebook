## Vue 项目实践

### 项目前置

1、安装node.js环境

注意 切换国内镜像

```js
npm config get registry
npm config set registry=https://registry.npmmirror.com
```

2、vite创建项目（参考官网版本）

```JS
npm init vite@latest<project-name>   ----template vue
```

3、Vue3 UI框架 Element Plus

查看官网即可

4、 引入Windi CSS工具库

`npm i -D vite-plugin-windicss windicss`

vscode 可安装Windi CSS Intellegence插件

5、Vue router（Vue.js 的官方路由）

在项目目录 src 建立 `router/index.js`导入路由，并建立`pages/xxx.vue`书写各页面

配置文件路径别名，在 vite 官网 resolve.alias 配置项

### 登录页面开发

pages 文件夹下新建一个 login.vue 作为登录页面。

1、利用elementplus和WindiCSS 插件 给出登录页面基础样式和架构

`<el-row></el-row>`与`<el-col></el-col>`

2、页面响应式布局

```
<el-col :lg="18" :md="12">
```

3、图标引入

```js
npm install @element-plus/icons-vue
```

```vue
<el-input v-model="sizeForm.username" placeholder="请输入用户名">
     <template #prefix>
      	<el-icon><User /></el-icon>
     </template>
</el-input>

<script>
import { User} from '@element-plus/icons-vue'
</script>
```

  4、登录表单验证处理（element plus）

`Form` 组件提供了表单验证的功能，只需为 `rules` 属性传入约定的验证规则，并将 `form-Item` 的 `prop` 属性设置为需要验证的特殊键值即可。 

```
const rules = {
    username: [
        {
            required: true,
            message: '用户名不能为空',
            trigger: 'blur'
        },
        {
            min: 2, max: 8,
            message: 'Length should be 2 to 8',
            trigger: 'blur'
        },
    ],
    password: [
        {
            required: true,
            message: '密码不能为空',
            trigger: 'blur'
        },
    ]
}
```

5、axios引入和登录页面交互

`npm install axios`

建立`axios.js`，再建立`api/manage.js`用以处理管理者登录页面接口交互（如何写处理函数与后端提供的api有关）

还需在vite.config.js配置避免跨域失败

```
server: {
    proxy: {
      // 选项写法
      '/api': {
        target: 'http://ceshi13.dishait.cn',
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/api/, '')
      },
    },
  },
```

6、cookie c存储用户token

引入`vueuse`

```
npm i @vueuse/integrations
npm i universal-cookie@^6

import { useCookies } from '@vueuse/integrations/useCookies'
cookies.set('token', xxxx)
console.log(cookies.get('token'))
```

7、请求与响应拦截器

请求拦截：

 1）登录后为请求头自动加上token，减少后续请求添加token代码 

响应拦截：

1） 处理响应头，如本项目为减少res.data.data的撰写代码重复，此逻辑可放在响应拦截器内

2）响应失败错误处理代码可以写在`axios.js`里的响应拦截器，统一处理，避免代码重复

8、常用工具库封装

将cookie存储、消息提示等功能函数封装在工具库进行管理

登录页面封装的工具库 包括cookie存储等认证相关`auth.js`，登录成功消息提示相关`util.js`

9、vuex使用

对共享状态（数据）进行管理，本项目是用户信息等。

创建store/index.js，将user对象存入state，通过mutations实现用户信息记录

10、路由守卫

实现登录页面的未登录前只能访问登录页和勿重复登录。

在src目录下建立permission.js，用于实现路由守卫。

11、页面功能完善

- 登录后保证首页数据一直在，刷新不会影响

首先将获取当前用户信息功能写入 store 中的actions（利用api 文件里的manage.js中的getinfo功能实现用户信息获取），返回一个异步函数。

其次，在pemission.js文件中加入 如果管理员token存在调用vuex里的getinfo功能

- 封装一些功能到vuex里，实现共享共享状态管理

将 设置token功能封装到actions中formalLogin功能中，简化login.vue代码

- 实现回车登录

在login.vue中加入监听回车事件，但要在组件挂载前和卸载后执行。