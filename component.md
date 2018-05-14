## Component 组件
#### component 组件:组件可以扩展 HTML 元素,封装可重用的代码.组件是自定义元素.

#### 1. 全局组件：Vue.component(tagName, {options})；这个全局 API 来声明一个全局组件.

```
<!-- 此时的组件相当于一个自定义模板,可由js控制渲染html -->
html：
<div id="app">
    <!--组件-->
    <demo-one></demo-one>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>

js:
<script>
    /* 注意:要求先注册组件再声明vue实例 */
    Vue.component("demo-one", {
        template: "<div style='background-color: deeppink'>{{message}}</div>",
        data: function () {
            return {message: "A custom component! "}
        }
    })；

    new Vue({
        el: "#app"
    })
</script>
```

#### 2. 局部组件：在一个确定的实例，只是在这个实例作用域中才可使用的组件.组件与组件之间的值是相互独立的.

```
html:
    <div id="app">
        <demo-one></demo-one>
        <demo-two message = "hello" ></demo-two>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
js:
    //在实例中声明的组件只能在当前实例中使用.并且组件与组件之间是相互独立的,父子组件之间可以使用props传值.
    const vm = new Vue({
        el:"#app",
        data () {
            return {
                hello:"mn"
            }
        },
        components: {
            'demo-one':{
                template:"<p>{{this.msg}}</p>",
                data:function(){
                    return {msg:"aaa"}
                }
            },
            'demo-two':{
                props: ['message'],
                template:"<p>{{message}}</p>",
            }
        }
    })
```

#### 3. notice：

#### 3.1 在使用组件渲染时需注意,在 html 中某些元素中只允许放置特殊的元素 table,ul,ol,select,因此在这些标签放置自定义组件会失败,也会把其放置在外面.

```
  //当我们这放置时,其实渲染时会把 div 放在外面
   <table>
   <div>qqq</div>
   </table>

   //替换为
    <div>qqq</div>
    <table>
    </table>
```

#### 3.2 在组件中 data 必须是函数

```
<div id="app">
    <my-component></my-component>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    Vue.component('my-component', {
        template: '<span>{{ message }}</span>',
        //报错
        /*data: {
            message: 'hello'
        }*/
       //正确
        data:function () {
            return {message: 'hello'};
        }
    })

    new Vue({
        el:"#app",
    })
</script>
```

#### 3.3 data 中返回的是同一个对象，而对象的 js 存储在堆中因此复制的是一个同一个对象地址，一个变化另一个也会有影响但如果返回的是 { counter：0 }；返回的是一个变量，则是不同的

```
  //此时返回的是同一个对象，而对象的js存储在堆中因此复制的是一个同一个对象地址，一个变化另一个也会有影响。
   var data= {counter:0}
    Vue.component('simple',{
        template:'<button @click="counter += 1">{{counter}}</button>',
        data:function () {
            return data;
        }
    })

    //但如果返回的是 { counter：0 }；返回的是一个变量，则是不同的。
    Vue.component('simple',{
        template:'<button @click="counter += 1">{{counter}}</button>',
        data:function () {
            return {counter:0};
        }
    })
    var vm = new Vue({
        el:"#app"
    });
```

#### 4. 父子组件:父组件向子组件传值 props，父子组件的作用域是相互独立的因此需要用 props 来传递值,需要利用 dom 元素做一个桥梁.

```
html:
   <div id="app">
        <demo-one :message = "msg"></demo-one>
    </div>
js:
   const vm = new Vue({
            el:"#app",
            data:function(){
                return {msg:"mn"};
            },
            components: {
                'demo-one':{
                    props: ['message'],
                    template:"<p>{{message}}</p>"
                }
            }
   })
```

#### 5. 子父组件:子组件发射,父组件事件接受.

```
html:
<div id="app">
    <demo-one @click='father'></demo-one>
</div>

js:
const vm = new Vue({
    el: "#app",
    components: {
        'demo-one': {
            data() {
                return {
                        count: 0
                    }
                },
                template: "<p>{{count}}</p>",
                //子组件发射值
                mounted() {
                    this.$emit('click', ["我是子组件传过来的"])
                }
            }
    },
            //下边想要在父组件内操纵这个数据.在父组件中监听子组件事件,接受传来的值.
            //当子组件中的事件被触发时,在父组件中这里就监听到了.
            methods: {
                father: function (m) {
                    console.log(m);  
                }
            }
        })
```

#### 6. 兄弟组件:创建一个空的实例,使用兄弟组件分别访问这个实例,进行发射与接受

```
html:
    <div id="app">
        <dom-a></dom-a>
        <dom-b></dom-b>
    </div>

js:
    //兄弟之间传值需要一个空的实例,使得这两个组件都能访问这个实例.
    //在一个组件中发射
        Vue.component('dom-a', {
            template: "<div @click='brother'>{{count}}</div>",
            data() {
                return {
                    count: 0
                }
            },
            methods: {
                brother: function () {
                    this.count++;
                    vm.$emit('click', 'params'); //触发事件
                }
            }
        });

        //在一个组件中接受
        Vue.component('dom-b', {
            template: "<div></div>",
            created() {
                vm.$on('click', function (e) {
                    console.log(e);
                });
            }
        });

        var vm = new Vue();
        var bus = new Vue({
            el: "#app"
        });
```

#### 7. 组件嵌套

```
html:
    <div id="app">
        <demo-one></demo-one>
    </div>

js:
    let vm = new Vue({
        el:"#app",
        components:{
            'demo-one':{
                template:"<div> <demo-two></demo-two> </div>",
                components:{
                    'demo-two':{template: "<a>qqq</a>"}
                }
            }
        }
    })
```

#### 8. component 标签:

#### <component>标签是 vue 内部自定义的，它的用途就是可以动态绑定我们的组件，根据数据的不同更换不同的组件。依据 is 的值，来决定哪个组件被渲染

```
html:
    <h1>component</h1>
    <hr>
    <div id="app">
        <component v-bind:is="who"></component>
        <button @click="changeComponent">changeComponent</button>
    </div>

js:
    var componentA = {
        template: '<div>I am componentA</div>'
    }
    var componentB = {
        template: `<div>I'm componentB</div>`
    }
    var componentC = {
        template: `<div>I'm componentC</div>`
    };

    let vm = new Vue({
        el: "#app",
        data:{
            who:'componentA'
        },
        components: {
            "componentA": componentA,
            "componentB": componentB,
            "componentC": componentC,
        },
        methods: {
            changeComponent: function () {
                if (this.who == 'componentA') {
                    this.who = 'componentB';
                } else if (this.who == 'componentB') {
                    this.who = 'componentC';
                } else {
                    this.who = 'componentA';
                }
            }
        }
    })
```

#### 9. ref 与 $refs 来访问子组件

```
html:
    <div id="parent">
        <demo-one ref="one"></demo-one>
        <demo-two ref="two"></demo-two>
    </div>

js:
     Vue.component( 'demo-one',{
            template:"<div>{{msg}}</div>",
            data:function(){
                return {msg:222}
            }
        });

        Vue.component( 'demo-two',{
            template:"<button>{{num}}</button>",
            data:function(){
                return {num:1111}
            }
        });

        let vm = new Vue({
            el:"#parent",
            mounted:function(){
                //上边的两个子组件如何在这里进行操作，这就需要ref与$refs
                console.log(this.$refs.one);      //这里是vue的component
                console.log(this.$refs.one.msg);  //这里是vue的component的内部属性

                console.log(this.$refs.two);      //这里是vue的component
                console.log(this.$refs.two.num);  //这里是vue的component的内部属性

            }
        })
```

#### 10. solt 插槽为了保留父组件的有些内容而是用的 solt 标签插槽，在子组建的 solt 标签内会保存父组件的内容。

```
html:
    <div id="app">
        <h1>我是父组件的标题</h1>
        <my-component>
            <p>这是一些初始内容</p>
            <p>这是更多的初始内容</p>
        </my-component>
    </div>

js:
     //为了保留父组件的有些内容而是用的solt标签插槽。
    Vue.component('my-component', {
        //在子组建的solt标签内会保存父组件的内容。
        template: "<div>" +
            "<h4>这是子组件</h4>" +
            "<slot>只有在没有要分发的内容时才会显示</slot>" +
            "</div>"
    })

    let vm = new Vue({
         el: "#app"
    })
```
#### 10.1 <transition></transition>
#### vue 允许我们利用其为元素添加动画过度效果,利用<transition></transition>可以告诉 vue 在这个标签包裹的部分要使用效果了.
#### 当添加了<transition>自动嗅探目标元素是否应用了 CSS 过渡或动画，如果是，在恰当的时机添加/删除 CSS 类名。

#### 10.2 过渡类名:
#### v-enter：定义进入过渡的开始状态。在元素被插入时生效，在下一个帧移除。

#### v-enter-active：定义过渡的状态。在元素整个过渡过程中作用，在元素被插入时生效，在 transition/animation 完成之后移除。#### 这个类可以被用来定义过渡的过程时间，延迟和曲线函数。

#### v-enter-to: 2.1.8 版及以上 定义进入过渡的结束状态。在元素被插入一帧后生效 (与此同时 v-enter 被删除)，在transition/animation 完成之后移除。

#### v-leave: 定义离开过渡的开始状态。在离开过渡被触发时生效，在下一个帧移除。

#### v-leave-active：定义过渡的状态。在元素整个过渡过程中作用，在离开过渡被触发后立即生效，在 transition/animation 完成之后移除。这个类可以被用来定义过渡的过程时间，延迟和曲线函数。

#### v-leave-to: 2.1.8 版及以上 定义离开过渡的结束状态。在离开过渡被触发一帧后生效 (与此同时 v-leave 被删除)，在 transition/animation 完成之后移除。
