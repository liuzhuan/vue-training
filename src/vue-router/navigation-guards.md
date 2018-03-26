# 导航守卫

`vue-router` 提供的导航守卫主要用来守卫路由，要么重定向导航，要么取消导航。在路由导航过程中有一些钩子：全局的、单个路由的或者组件内部的。

要记住，**params 或者 query 参数的改变不会处罚导航守卫的进入和离开**。你可以监听 `$route` 对象来应对这些变化，或者使用 `beforeRouteUpdate` 组件內的守卫。

## 全局守卫

你可以注册全局守卫，使用 `router.beforeEach`：

```js
const router = new VueRouter({ ... })
router.beforeEach((to, from, next) => {
  // ...
})
```

## REF

- [Navigation Guards][nav-guard]

[nav-guard]: https://router.vuejs.org/en/advanced/navigation-guards.html