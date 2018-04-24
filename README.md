# Vue-basic
### MVC与MVVM设计模式
#### angular使用的MVC的设计模式:MVC指的是 Model View Controller 模型-视图-控制器设计模式.
#### V-View:       视图层，一般是我们的html文件层，用于展示数据内容
#### C-Controller: 控制器，控制器帮助将M层数据给V,或者当View层数据有所改变时，通知M层，M层数据也做相应的改变
#### M-Model:      模型，当我们需要使用到数据的时候，数据暂存在这里

#### Vue使用的是MVVM设计:
#### Model：    就是业务逻辑相关的数据对象，通常从数据库映射而来，我们可以说是与数据库对应的model
#### View：     就是展现出来的用户界面
#### ViewModel：就是View与Model的连接器，

### Vue双大括号模板语法
#### Vue的双大括号模板语法:Vue通过其底层的模板语法声明式的将数据绑定到DOM元素上。双大括号内可以为纯文本，也可以为表达式。
```
<div id="app">
    {{ mun+1 }}

    {{ ok ? 'YES' : 'NO' }}
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    new Vue({
        el:"#app",
        data:{
            mun:23,
            ok:true
        }
    })
</script>
```

### 渲染函数与template
#### template：可以通过选项template来渲染html
```
<div id="app"></div>
<script type="text/x-template" id="tpl">
    <div>
        <ul>
            <li>beijing</li>
            <li>shanghai</li>
            <li>huangzhou</li>       
        </ul>
    </div>
</script>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el:"#app",
        template:"#tpl"
    })
</script>
```
#### render函数
#### render函数渲染模板相当于是js的创建dom节点。还可以在render函数的选项中对dom元素各个属性进行设置。
```
<style>
    .red{
        background-color: antiquewhite;
        width: 100px;
        height: 200px;
    }
</style>
<body>
    <div id="app"></div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
    <script>
        var vm = new Vue({
            el: "#app",
            render: createElement => createElement("div", {
                'class': {
                    red: true
                },
                style: {
                    color: 'blue',
                    fontSize: "20px"
                },
                //属性
                attrs: {
                    id: 'foo'
                },
                // DOM 属性
                domProps: {
                    innerHTML: 'Hello！'
                },
            })
        })
    </script>
```

### 指令
#### 条件渲染 v-if
```
 <div id="app">
    <div v-if="type === 'A'">
            A
    </div>
    <div v-else-if="type === 'B'">
            B
    </div>
    <div v-else-if="type === 'C'">
            C
    </div>
    <div v-else>
            Not A/B/C
    </div>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    new Vue({
        el: "#app",
        data: {
            type: 'g'
        }
    })
</script>
```

#### 列表渲染 v-for
```
<div id="app">
    <ul>
        <!--1.0 当显示的数据为一个对象时,obj代表要循环的对象, () 代表需要显示的对象的每一项-->
        <li v-for="(value, key, index) in obj">
            {{index}} --- {{key}} --- {{value}}
        </li>
    </ul>
    <ul>
        <!--2.0 当显示的数据为一个数组对象时,items代表要循环的对象, (value,索引) 代表需要显示的对象的每一项-->
        <!--3.0 我们可以在行内来取到items的值.-->
        <li v-for="(value,index) in items" @click="getitems(items)">
            {{index}} ===== {{value.first}}
        </li>
    </ul>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
new Vue({
    el: "#app",
    data: {
        obj: {
            message: "mn",
            id: "1",
            age: 11
        },
        items: [{
            id: 1,
            first: "mn",
            age: 18
        }, {
            id: 2,
            first: "llei",
            age: 19
        }, {
            id: 3,
            first: "hanmeimei",
            age: 20
        }]
    },
    methods: {
        getitems(a) {
            console.log(a);
        }
    }
});
```
#### 属性绑定 v-bind
```
//demo-one
<div id="app">
    <input type="text" :value="message">
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el:"#app",
        data:{
            message:"mn"
        }
    })
</script>
```

```
//demo-two
<style>
    .red {
        color: red;
    }
    .blue {
        color: blue;
    }
</style>

<div id="app">
    <p :class="{red:isRed,blue:isBlue}">aaaa</p>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el: "#app",
        data: {
            isRed: true,
            isBlue: false
        }
    })
</script>
```
#### 双向数据绑定 v-model
```
//demo-one
<div id="app">
    <input type="checkbox" id="isOk" v-model="ok">
    <lable for="isOk">{{ok}}</lable>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var app = new Vue({
        el:"#app",
        data:{
            ok:true
        }
    })
</script>
```

```
//demo-two
<div id="app">
    <label>v-model.lazy: </label><input type="text" v-model.lazy="message"><br>
    <label>v-model.number: </label><input type="text" v-model.number="message"><br>
    <label>v-model.trim: </label><input type="text" v-model.trim="message"><br>
    v-model.lazy: {{message+1}}
    v-model.number: {{message+1}}
    v-model.trim: {{message+1}}
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    //v-model实现双向数据绑定
    new Vue({
        el:"#app",
        data:{
            message:1
        }
    })

</script>
```

#### 事件绑定 v-on
```
<div id="app">
    <button v-on:click=" mun += 1 ">自增</button>
    //<button @click=" mun += 1 ">自增</button>
    <p>{{mun}}</p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    new Vue({
        el:"#app",
        data:{
            mun:0
        }
    })
</script>
```

#### v-html渲染，v-text文本渲染
```
<div id="app">
    <p v-html="message"></p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            message: "<span> mn </span>"
        }
    })
</script>
```

```
<div id="app">
    <p v-text = 'message'></p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    new Vue ({
        el:'#app',
        data:{
            message:'cat catt'
        }
    })
</script>
```
### 全局API
#### Vue.set();设置对象的属性
```
<div id="app">
    <ul>
        <li v-for="val in one">{{val}}</li>
    </ul>
    <p>
        <button @click="change();">gaibian</button>
    </p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var obj = {
        one: ["上海", "北京", "广州"]
    };
    var vm = new Vue({
        el: "#app",
        data: obj,
        methods: {
            change: function () {
                Vue.set(vm.one, 0, "ni");
                Vue.set(vm.one, 1, "wo");
                Vue.set(vm.one, 2, "ta");
            }
        }
    })
</script>
```
#### Vue.delete();删除对象的属性
```
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var obj = {
        age:1,
        name:"mn"
    }
    Vue.delete(obj,'name');
    console.log(obj);
</script>
```
#### Vue.nextTick();由于dom数据改变，vue会有延迟
```
<div id="app">
    <p id="p">{{num}}</p>
    <button @click="add()"> 点击 </button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el:"#app",
        data:function () {
            return{
                num:900
            }
        },
        methods:{
            add:function(){
                this.num=700;
                var pbox = document.getElementById("p");
                console.log(pbox.innerText);   //900,vue所渲染的值不能同步到js所控制的dom元素上，是有延迟的。
            }
        }
    });

    Vue.nextTick(function () {
        console.log(bigbox.innerText);  //700,此时dom元素的值改变了
    })
</script>
```
#### Vue.mixin();允许在method外来定义方法
```
<div id="app-one">
    {{num}}
    <button @click="add()">增加</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    Vue.mixin({
        methods: {
            add: function () {
                vmOne.$data.num += 10;
            }
        }
    });

    let vmOne = new Vue({
        el: "#app-one",
        data: { num: 20 }
    })
</script>
```
#### Vue.extend();可以扩展一个实例
```
<div id="app">{{num}}</div>
<div id="demo"></div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    // 创建了一个vue实例
    var vm = new Vue({
        el:"#app",
        data:{
            num:1
        }
    });
    //使用Vue.extend();方法也是来创建一个实例，类似于vue实例
    //使用基础 Vue 构造器，Vue.extend();方法，创建一个“子类”，类似于vue实例
    var newExtend = Vue.extend({
        template:"<p><a :href='one'>{{two}}</a></p>",
        data:function () {
            return {
                one:"https://www.baidu.com/",
                two:"我是连接"
            }
        }
    });

    new newExtend().$mount('#demo');

</script>
```
#### Vue.filter();过滤器
```
<div id="app">
    <p>{{ num | jspang }}</p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    Vue.filter("jspang",function (value) {
        return value+10;
    })
    var vm = new Vue({
        el: "#app",
        data: {
            num:1,
        }
    })
</script>
```
#### Vue.directive();自定义指令
```
<div id="app">
    <p v-jspang>111</p>
</div>
<p v-jspang>222</p>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    Vue.directive("jspang",function (el,binding) {
        el.style.color="red";
        console.log(el);         //dom节点
        console.log(binding);    //自定义指令本身
    });
    new Vue({
        el:"#app"
    })
</script>
```
##### 自定义指令的钩子函数
```
<div id="app">
    <div v-jspang="color" id="demo">
        {{num}}
    </div>
    <button @click="add" id="btn">Add</button>
    <p><button onclick="unbind();">解绑</button></p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    Vue.directive('jspang', {
        bind: function () {//被绑定
            console.log('1 - bind');
        },
        inserted: function () {//绑定到节点
            console.log('2 - inserted');
        },
        update: function () {//组件更新
            console.log('3 - update');
        },
        componentUpdated: function () {//组件更新完成
            console.log('4 - componentUpdated');
        },
        unbind: function () {//解绑
            console.log('5 - unbind');
        }
    });

    //如何解绑vue
    function unbind() {
        vm.$destroy();
    }
    var vm = new Vue({
        el: '#app',
        data: {
            num: 10,
            color: 'green'
        },
        methods: {
            add: function () {
                this.num++;
            }
        }
    })
</script>
```
#### Vue.component();定义全局组件
```
<div id="app">
    <!--组件-->
    <demo-three></demo-three>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<template id="myComponent">
    <a>{{message}}</a>
</template>
<script>
    /*注意:要求先注册组件再声明vue实例*/
    Vue.component("demo-three", {
        template: "#myComponent",
        data: function () {
            return {message: "This is a component!"}
        }
    })
    new Vue({
        el: "#app"
    })
</script>
```

### options 选项
#### el ,data :
```
 console.log(vm.$data);   //Object
 console.log(vm);         //Vue$3
 console.log(vm.$el);     //<div id="app"></div>

```
#### computed, methods
```
<div id="app">
    <p v-text="firstFun"></p>
    <p v-text="secondFun"></p>
    <p>{{firstFun}}</p>
    <p>{{secondFun}}</p>

</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var  vm = new Vue({
        el:"#app",
        data:{
            a:10
        },
        computed:{
            firstFun:function () {
              return "first";
            },
            secondFun:function () {
                return "second";
            }
        }
    });
</script>
```
#### props 父组件的值传递给子组件
```
<div id="app">                    <!--父组件包含子组件,但是由于父组件包含子组件那么这里面的数据都属于父组件的-->
    <child msg="hello"></child>   <!--子组件算是父子间内的数据,在父组件的作用域中-->
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    // 什么是父子组件:父子组件之间的作用域是相互独立的,需通过props传递,这里面定义的是子组件.
    Vue.component('child',{
        props: ['msg'],                                            //接收父组件传递的数据
        template: '<span> {{msg}}---{{val}} </span>',
        data(){return {val: 2};}                                   //这才是子组件里面的数据
    })

    // 实例化Vue
    new Vue({
        el : '#app',
        data : {val : 1}                                          //这个数据是初始化Vue实例就定义的
    });

</script>
```
#### component 局部组件
```
<div id="app">
    <my-child></my-child>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    /* 1.0 在实例化的方法选项里可以定义局部的组件,*/
    var child = {
        template: '<div>A custom component!</div>'
    }

    new Vue({
        el: "#app",
        /*  components:{'my-child':{
              template: '<div>A custom component!</div>'
          }}*/

        components: {
            'my-child':child
        }
    })
</script>
```
#### extends 扩展实例方法
```
<div id="app">
    <p v-text="num"></p>
    <button @click="add()">点击执行方法</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var extendsObj = {
            updated: function () {  //在钩子函数中,数据更新时执行,同时在构造器内部与扩展中,先执行扩展
                console.log("我是扩展出来的钩子函数updated!!");
            },
        methods:{
            add:function () {      //当扩展的方法与同时在构造器内部的方法同名时,只执行内部方法,不执行扩展方法
                console.log("我是扩展的add方法");
                this.num +=10;
            }
        }
    };

    var vm = new Vue({
        el: "#app",
        data: {
            num:1
        },
        methods:{
          add:function () {
              console.log("我是原生的add方法");
              this.num +=3;
          }
        },
        updated:function () {
            console.log("我是原生的钩子函数updated!!");
        },
        extends: extendsObj,
    })
</script>
```
#### mixins 扩展实例方法
```
<div id="app">
    <p v-text="num"></p>
    <button @click="add">点击</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    //如果混入的方法與選項的方法有同名,優先使用選項內的方法
    var mixObjects = {
        data: function () {
            return { num: 90 }
        },
        updated: function () {
            console.log("我是mixins的updated方法!");
        },
        methods: {
            add: function () {
                console.log("我是mixins的add方法!");
                this.num += 10;
            }
        }
    };

    var vm = new Vue({
        el: "#app",
        data: {
            num: 1
        },
        methods: {
            add: function () {
                console.log("我是原生的add方法!");
                his.num += 3;
            }
        },
        updated: function () {
            console.log("我是原生的updated方法!");
        },
        mixins: [mixObjects],
    })
</script>
```
#### directives 自定义指令
```
<div id="app">
    <input type="text" v-focus>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    new Vue({
        el: "#app",
        data: {
            message: 1
        },
        components: {
            'my-child':{template: '<div>A custom component!</div>'}
        },
        directives: {
            focus: {
                // 指令的定义
                inserted: function (el) {
                    el.focus();
                    el.style.color="red";
                }
            }
        }
    })
</script>
```
#### filter 过滤器
```
<div id="app">
    <p>金额:{{message | filterFunone | filterFuntwo | filterFunthree }} </p>
    <input type="text" v-bind:value="message | filterFunthree">
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el: "#app",
        data: {
            message: 29,
        },
        filters: {
            filterFunone(value) {
                return value;
            },
            filterFuntwo(value) {
                return value + 10;
            },
            filterFunthree(value) {
                return "$" + value.toFixed(2) + "元";
            }
        }
    })
</script>
```
#### 生命周期钩子
```
<div id="app">
    <p>{{ message }}</p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            message : "xuxiao is a boy"
        },
        beforeCreate: function () {
            /*1.0 beforeCreate是实例被创建之前，任何dom元素都还未定义，任何数据都和事件都还没有被执行*/
            console.group('beforeCreate 创建前状态===============》');
            /*控制台输出  添加样式  什么样式     输出内容         */
            console.log("%c%s", "color:red" , "el     : " + this.$el);    //undefined
            console.log("%c%s", "color:red","data   : " + this.$data);    //undefined
            console.log("%c%s", "color:red","message: " + this.message)   //undefined
        },
        /*2.0 实例创建完了，但是dom元素还没有生成，数据和事件已经可见，但是还没有完成挂载。*/
        created: function () {
            console.group('created 创建完毕状态===============》');
            console.log("%c%s", "color:red","el     : " + this.$el);      //undefined
            console.log("%c%s", "color:red","data   : " + this.$data);    //已被初始化
            console.log("%c%s", "color:red","message: " + this.message);  //已被初始化
        },
        /*3.0 在挂载开始之前被调用：相关的 render 函数首次被调用。产生dom元素 */
        beforeMount: function () {
            console.group('beforeMount 挂载前状态===============》');
            console.log("%c%s", "color:red","el     : " + (this.$el));    //已被初始化
            console.log(this.$el);
            console.log("%c%s", "color:red","data   : " + this.$data);    //已被初始化
            console.log("%c%s", "color:red","message: " + this.message);  //已被初始化
        },
        /*4.0 完成了 el 和 data 初始化 。产生dom元素*/
        mounted: function () {
            console.group('mounted 挂载结束状态===============》');
            console.log("%c%s", "color:red","el     : " + this.$el); //已被初始化
            console.log(this.$el);
            console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化
            console.log("%c%s", "color:red","message: " + this.message); //已被初始化
        },

        /*5.0 数据更新操作的，下面就能看到data里的值被修改后，将会触发update的操作。
        * 在console.log()内输入app.message='数据更新状态'*/
        beforeUpdate: function () {
            console.group('beforeUpdate 更新前状态===============》');
            console.log(this.$el);
            console.log("%c%s", "color:red","data   : " + this.$data);
            console.log("%c%s", "color:red","message: " + this.message);
        },
        /*6.0 数据更新操作完成，下面就能看到data里的值被修改后，将会触发update的操作。*/
        updated: function () {
            console.group('updated 更新完成状态===============》');
            console.log("%c%s", "color:red","el     : " + this.$el);
            console.log(this.$el);
            console.log("%c%s", "color:red","data   : " + this.$data);
            console.log("%c%s", "color:red","message: " + this.message);
        },
        /*7.0 vue实例被销毁之前：我们在console里执行下命令对 vue实例进行销毁。
        销毁完成后，我们再重新改变message的值，vue不再对此动作进行响应了。但是原先生成的dom元素还存在，
        可以这么理解，执行了destroy操作，后续就不再受vue控制了。*/
        beforeDestroy: function () {
            console.group('beforeDestroy 销毁前状态===============》');
            console.log("%c%s", "color:red","el     : " + this.$el);
            console.log(this.$el);
            console.log("%c%s", "color:red","data   : " + this.$data);
            console.log("%c%s", "color:red","message: " + this.message);
        },
        /*8.0 vue实例被销毁结束*/
        destroyed: function () {
            console.group('destroyed 销毁完成状态===============》');
            console.log("%c%s", "color:red","el     : " + this.$el);
            console.log(this.$el);
            console.log("%c%s", "color:red","data   : " + this.$data);
            console.log("%c%s", "color:red","message: " + this.message)
        }
    })
</script>
```
### 实例属性方法
#### vm.$el，vm.$data
```
<div id="app"></div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el:"#app",
        data:{
            name:"mn",
            age:"19"
        }
    })
    console.log(vm.$el);     //dom元素
    console.log(vm.$data);   //data对象
</script>
```
#### vm.$children
```
<div id="app">
    <child></child>
    <child></child>
    <child></child>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    Vue.component('child',{
        template:'<span>nihao</span>'
    })
    var vm=new Vue({
        el:'#app'
    })
    console.log(vm.$children);
</script>
```
#### vm.$root
```
<div id="app"></div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el:"#app"
    })
    console.log(vm.$root);   //如果当前vm有父实例指的他的父实例,如果没有父实例,指的是他自己
</script>
```
#### vm.$refs
```
<div id="parent">
  <user-profile ref="profile"></user-profile>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
var parent = new Vue({ el: '#parent' })
// 访问子组件实例
var child = parent.$refs.profile
</script>
```
