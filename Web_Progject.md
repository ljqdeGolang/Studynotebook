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

在项目目录 src 建立 router 文件夹 导入路由

配置文件路径别名，在 vite 官网 resolve.alias 配置项

### 登录页面开发

