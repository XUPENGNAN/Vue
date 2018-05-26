### Vue Router 是单页应用的路径管理器
#### Vue Router的基本使用
#### <router-link>组件支持用户在具有路由功能的应用中（点击）导航,通过to属性指定目标地址，默认渲染成带有正确链接的<a> 标签.当目标路由成功激活时链接元素自动设置一个表示激活的 CSS 类名 (router-link-exact-activ router-link-active)
```
// demo-one
html:
   <div id="app">
        <h1>Hello App!</h1>
        <p>
            <!-- 使用 router-link 组件来导航 -->
            <!-- 通过传入 `to` 属性指定链接 -->
            <!-- <router-link> 默认会被渲染成一个 `<a>` 标签  -->
            <router-link to="/one">|| --one-- ||</router-link>
            <router-link to="two">--two-- ||</router-link>
            <router-link :to="{path: '/three'}">--three-- ||</router-link>
            <router-link :to="{name:'ccc'}">--four-- ||</router-link>
        </p>
        <!-- 路由出口 -->
        <!-- 路由匹配到的组件将渲染在这里 -->
        <router-view></router-view>
    </div>

js:
      //由于vue-router是vue一个插件，因此使用时需要引入，并且需要创建一个new VueRouter({})实例，并把路由实例注入到根//实例中使用
        const twoTpl = {template:"<div>twoTpl twoTpl twoTpl</div>"};
        var router = new VueRouter({
            routes: [
                { path: '/one', component: { template: '<div>ONE</div>' } },
                { path: '/two', name:'two', component: { template: '<div>TWO</div>' } },
                { path: '/three', component: { template: '<div>three</div>' } },
                { path: '/four', name: "ccc", component: { template: '<div>four</div>' } },
            ]
        });

       //把路由实例注入到根实例中
        var vm = new Vue({
            el: '#app',
            router: router
        })
```

#### 动态路由匹配:多个路由一个视图。
```
<div id="app">
    <router-link to = "/one/foo">-- foo -- ||</router-link>
    <router-link to = "/one/bar">-- bar -- ||</router-link>
    <router-view> </router-view>
</div>
<script>
    const one = {template:"<div>{{this.$route.params}}</div>"};
    const router = new VueRouter({
        routes:[
            //不同的路由匹配相同的组件
            { path: '/one/:id', component: one }
        ]
    });

    const vm = new Vue({
        el:"#app",
        router:router
    })
</script>
```

#### 命名路由：给路由命名便于操作
```
<div id="app">
    <router-link to="one"> -- foo -- ||</router-link>
    <router-link to="two"> -- bar -- ||</router-link>
    <router-view> </router-view>
</div>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
<script>
    const one = {
        template: "<div>foo</div>",
    };
    const two = {
        template: "<div>bar</div>",
    };
    const router = new VueRouter({
        routes: [{
                    path: '/one',
                    name: 'one',
                    component: one
                },
                {
                    path: '/two',
                    name: 'two',
                    component: two
                }
        ]
    });

    const vm = new Vue({
            el: "#app",
            router: router
    })
</script>
```

#### 命名视图，多视图：一个路由多视图,就需要个视图命名了
```
<div id="app">
    <router-link to="one"> -- foo -- ||</router-link>
    <router-link to="two"> -- bar -- ||</router-link>
    <router-view> </router-view>
    <!-- 给视图命名用来展现多视图 -->
    <router-view name ="b"> </router-view>
    <router-view name ="c"> </router-view>
</div>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
<script>
    const Foo = { template: '<div>foo</div>' }
    const Bar = { template: '<div>bar</div>' }
    const Baz = { template: '<div>baz</div>' }
    
    const router = new VueRouter({
        routes: [{
                path: '/one',
                name: 'one',
                components: {
                    default:Foo,
                    b:Bar,
                    c:Baz
                }
            },
                {
                path: '/two',
                name: 'two',
                components: {
                    default:Baz,
                    b:Bar,
                    c:Foo
                }
            }
        ]
    });

    const vm = new Vue({
        el: "#app",
        router: router
    })
</script>

```
#### 路由重定向,别名
```
//路由重定向
<div id="app">
        <router-link to="/one">||one||</router-link>
        <router-link to="/two">||two||</router-link>
        <router-view></router-view>
</div>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
<script>
    const one = {template:"<div>one</div>"};
    const router = new VueRouter({
        routes:[
            { path: '/one', name:'one',component: one},
            { path: '/two', redirect: 'one'},
        ]
    }) ;
    const vm = new Vue({
        router
    }).$mount("#app");
</script>
```

```
//别名
<div id="app">
    <router-link to="/one">||one||</router-link>
    <router-link to="/three">||two||</router-link>
    <router-view></router-view>
</div>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
<script>
    const one = {template:"<div>one</div>"};
    const two = {template:"<div>two</div>"};
    const router = new VueRouter({
        routes:[
            { path: '/one',name:one,component:one},
            { path: '/two',component:two, alias:'/three'},
        ]
    }) ;
    const vm = new Vue({
        router
    }).$mount("#app");
</script> 

```
#### 嵌套路由:路由组件中嵌套路由
```
<div id="app">
    <p>
        <router-link to="/user/home">/user/foo</router-link>
        <router-link to="/user/profile">/user/foo/profile</router-link>
        <router-link to="/user/posts">/user/foo/posts</router-link>
    </p>
    <router-view></router-view>
</div>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
<script>
    const User = {
        template: `
                    <div class="user">
                        <p>home</p>
                        <router-view></router-view>
                    </div>
                    `
    }

    const UserHome = { template: '<div>Home</div>' }
    const UserProfile = { template: '<div>Profile</div>' }
    const UserPosts = { template: '<div>Posts</div>' }

    const router = new VueRouter({
            routes: [
                {
                    path: '/user', component: User,
                    children: [
                        { path: 'home', component: UserHome },

                        { path: 'profile', component: UserProfile },

                        { path: 'posts', component: UserPosts }
                    ]
                }
            ]
        })

    const app = new Vue({ router }).$mount('#app')
</script>
```

#### 路由传参params与query
```
//demo-one
<div id="app">
    <router-link :to="{name:'a',params:{user:'mn'}}">||one||</router-link>
    <router-view></router-view>
</div>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
<script>
        const one = {template:"<div>{{$router.params}}</div>"};
        const router = new VueRouter({
            routes:[
                { path: '/one',name:'a',component:one}
               
            ]
        }) ;
        const vm = new Vue({
            router,
            watch: {  //监听路由变化
                '$route'(to, from) {
                    console.log(this.$route.params);
                    console.log(this.$route.params.username);
                }
            }
        }).$mount("#app");
</script> 
```
```
//demo-two
    <div id="app">
        <router-link :to="{name:'a',query:{user:'mn'}}">||one||</router-link>
        <router-view></router-view>
    </div>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
    <script>
        const one = {template:"<div>{{$route.query.user}}</div>"};
        const router = new VueRouter({
            routes:[
                { path: '/one',name:'a',component:one}
               
            ]
        }) ;
        const vm = new Vue({
            router
        }).$mount("#app");
    </script> 
```

#### 编程式导航：$router实例为编程式导航提供了API，他是使用js来控制前端路由
#### $router 实例为我们提供了几个方法：
#### back() 后退一步
#### forward() 前进一步
#### go() 后退（参数为负数）和前进（参数为传正数）指定步数
#### push()  跳到指定的 url ，向 history 栈添加一个记录
#### replace()  跳到指定的 url，替换 history 栈中当前记录

```
<div id="app">
        <button @click="one()">go-one</button>
        <button @click="two()">go-two</button>
        <button @click="three()">back</button>
        <button @click="four()">forward</button>
        <button @click="five()">go(-1)</button>
        <button @click="six()">replace()</button>
        <router-view></router-view>
</div>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

    <script>
        const oneTpl = { template: "<div>oneTpl oneTpl oneTpl</div>" };
        const twoTpl = { template: "<div>twoTpl twoTpl twoTpl</div>" };
        const router = new VueRouter({ 
            routes: [
                { path: '/homeone', component: oneTpl },
                { path: '/hometwo', component: twoTpl },
            ]
        });

        const vm = new Vue({
            el: "#app",
            router: router,

            methods: {
                one: function () {
                    this.$router.push('/homeone')
                },
                two: function () {
                    this.$router.push('/hometwo')
                },
                three:function(){
                    this.$router.back()
                },
                four:function(){
                    this.$router.forward()
                },
                five:function(){
                    this.$router.go(-1)
                },
                six:function(){
                    this.$router.replace('/hometwo')
                }
            },
            
        })

    </script>
```

#### replace:设置 replace 属性的话，当点击时，会调用 router.replace() 而不是 router.push()，于是导航后不会留下 history 记录。
这就是利用H5的history的API实现的：
window.history.replaceState();用来替换当前url地址，没有历史记录。
window.history.pushState();   用来向浏览器添加一条历史纪录。

```
html:
    <div id="app">
        <!-- 在标签内添加replace当前路径将没有历史记录 -->
        <router-link to="one" replace>--one--</router-link>
        <router-link to="two">--two--</router-link>
        <!--如果给router-view加了name那么在指定模板式必须加上名字,否则渲染不上 -->
        <router-view name="a"></router-view>
    </div>
js:
     const one = { template: "<div>one one one</div>" };
        const two = { template: "<div>two two two</div>" };
        const router = new VueRouter({
            routes: [
            // {
            //         path: '/one', component:one 
            //     }, 不执行
                {
                    path: '/one', components: {
                        a: one
                    }
                },

                { path: '/two', components: {
                        a: two
                    }}
            ]
        });

        const vm = new Vue({
            el:"#app",
            router
        })
```

#### append:
```
html：
    <div id="app">
        <h1>Hello App!</h1>
        <p>
            <router-link :to="{path: 'a/foo'}" append>qqqq</router-link>
            <router-link :to="{path: '/bar'}">Go to twoChild</router-link>
        </p>
        <!-- 路由出口 -->
        <!-- 路由匹配到的组件将渲染在这里 -->
        <router-view></router-view>
    </div>

js:
     var router = new VueRouter({
            routes: [
                { path: 'a/foo', component: { template: '<div>FOO</div>' } },
                { path: '/bar', component: { template: '<div>BAR</div>' } }
            ]
        })

        var vm = new Vue({
            el: '#app',
            router: router
        })
```

#### tag:可以使你的标签不渲染为a 标签，而是渲染为tag制定的标签,也可以使用router-link，包裹一个a标签。
```
html:
    <div id="app">
        <h1>Hello App!</h1>
        <p>
            <!-- 使用 router-link 组件来导航. -->
            <!-- 通过传入 `to` 属性指定链接. -->
            <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
            <router-link tag="li" to="/foo">
                <a>/foo</a>
            </router-link>
            
            <router-link :to="{path: '/bar'}">Go to twoChild</router-link>
        </p>
        <!-- 路由出口 -->
        <!-- 路由匹配到的组件将渲染在这里 -->
        <router-view></router-view>
    </div>

js:
        var router = new VueRouter({
            routes: [
                { path: '/foo', component: { template: '<div>FOO</div>' } },
                { path: '/bar', component: { template: '<div>BAR</div>' } }
            ]
        })

        var vm = new Vue({
            el: '#app',
            router: router
        })
```

#### 路由钩子:路由文件中我们只能写一个beforeEnter,就是在进入此路由配置时.
#### 三个参数：
to:路由将要跳转的路径信息，信息是包含在对像里边的
from:路径跳转前的路径信息，也是一个对象的形式
next:路由的控制参数，常用的有next(true)和next(false)

```
html:
    <div id="app">
        <router-link to="/one">--one--</router-link>
        <router-link to="/two">--two--</router-link>
        <router-view></router-view>
    </div>
js:
    const router = new VueRouter({
            routes: [
                {
                    path: '/one',
                    component: { template: "<div>one one one</div>" },
                    beforeEnter: (to, from, next) => {
                        console.log('我进入了模板');
                        console.log(to);
                        console.log(from);
                        next();
                    }
                },
                { path: '/two', component:{ template: "<div>two two</div>" } }
            ]
        });

        const vm = new Vue({
            router
        }).$mount("#app");
```

#### 写在模板中的钩子函数
在配置文件中的钩子函数，只有一个钩子-beforeEnter，如果我们写在模板中就可以有两个钩子函数可以使用：
beforeRouteEnter：在路由进入前的钩子函数。
beforeRouteLeave：在路由离开前的钩子函数。

```
export default {
  name: 'params',
  data () {
    return {
      msg: 'params page'
    }
  },
  beforeRouteEnter:(to,from,next)=>{
    console.log("准备进入路由模板");
    next();
  },
  beforeRouteLeave: (to, from, next) => {
    console.log("准备离开路由模板");
    next();
  }
}
```

#### html5 histroy模式
#### vue-router 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。
#### 如果不想要很丑的 hash，我们可以用路由的 history 模式，这种模式充分利用 history.pushState API 来完成 URL 跳转而无须重新加载页面
