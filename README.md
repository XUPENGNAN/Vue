## Vue-basic

### MVC 与 MVVM 设计模式：
#### angular 使用的 MVC 的设计模式: MVC 指的是 Model View Controller 模型-视图-控制器设计模式
#### V-View: 视图层，一般是我们的 html 文件层，用于展示数据内容
#### C-Controller: 控制器，控制器帮助将 M 层数据给 V,或者当 View 层数据有所改变时，通知 M 层，M 层数据也做相应的改变
#### M-Model: 模型，当我们需要使用到数据的时候，数据暂存在这里
#### Vue 使用的是 MVVM 设计
#### Model： 就是业务逻辑相关的数据对象，通常从数据库映射而来，我们可以说是与数据库对应的 model
#### View： 就是展现出来的用户界面
#### ViewModel：就是 View 与 Model 的连接器

### Vue 双大括号模板语法：
#### Vue 的双大括号模板语法: Vue 通过其底层的模板语法声明式的将数据绑定到 DOM 元素上。双大括号内可以为纯文本，也可以为表达式。
```
<div id="app">                    - View ----------- View -
    {{ mun+1 }}
    {{ ok ? 'YES' : 'NO' }}
</div>                            - View ----------- View -

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    new Vue({
        el:"#app",
        data:{                   - Model ----------- Model -
            mun:23,
            ok:true
        },                       - Model ----------- Model -
        methods: {               - ViewModel ------ ViewModel -
            getitems(a) {
                console.log(a);
        }  
    }                            - ViewModel ------ ViewModel -
})
</script>
```

### 实例对象
#### 创建一个vue实例是将一个根节点的DOM元素实例化

### 渲染 render 函数与 template
#### template：可以通过选项 template 来渲染 html
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
<script src = "https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el:"#app",
        template:"#tpl"
    })
</script>
```

#### render 函数
#### render 函数渲染模板相当于是 js 的创建 dom 节点。还可以在 render 函数的选项中对 dom 元素各个属性进行设置
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
    <div v-if = "type === 'A'" >
            A
    </div>
    <div v-else-if = "type === 'B'">
            B
    </div>
    <div v-else-if ="type === 'C'" >
            C
    </div>
    <div v-else>
            Not A/B/C
    </div>
 </div>
<script src = "https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js" ></script>
<script>
    new Vue({
        el: "#app",
        data: {
            type: 'g'
        }
    })
</script>
```
#### 控制显示 v-show
#### v-show 与 v-if 的区别，v-show的显示与隐藏式设置 css 的dispaly，但是v-if的隐藏是直接删除与添加dom元素。
#### 因此如果频繁的在隐藏于显示之间切换，最好使用 v-show，因为 v-if 会频繁的渲染 dom 树非常消耗性能。
```
<div id="app">
    <button @click="ControlShow_one()"> 点击one </button>
    <button @click="ControlShow_two()"> 点击two </button>
    <p v-show="one" >
        one
    </p>
    <p v-if="two" @click="ControlShow_two()">
        two
    </p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    new Vue({
        el:"#app",
        data:{
            one:true,
            two:true,
        },
        methods: {
            ControlShow_one:function(){
                this.one = this.one ? false : true ;
        },
            ControlShow_two:function(){
                this.two = this.two ? false : true ;
            },

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
        <p :class="classNameone">aaaa</p>
        <p :class="[classNametwo]">bbbbb</p>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
    <script>
        var vm = new Vue({
            el: "#app",
            data: {
                classNameone:{red:true,blue:false},
                classNametwo:"blue",
            }
        })
    </script>
```

#### 双向数据绑定 v-model
```
<div id="app">
    <input type="text" id="isOk" v-model="ok">
    <lable> {{ok}} </lable>
</div>
<script src = "https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js" ></script>
<script>
    var app = new Vue({
        el:"#app",
        data:{
            ok:{}
        }
    })
</script>
```

##### 双向数据绑定的表单处理
```
// demo-one
<div id="app">
    <input type="checkbox" v-model="chengshi" value = "上海"> 
    <input type="checkbox" v-model="chengshi" value = "北京"> 
    <input type="checkbox" v-model="chengshi" value = "广州"> 

    <p> {{chengshi | Symbol}} </p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var app = new Vue({
        el:"#app",
        data:{
            chengshi:[]
        },
        filters:{
            Symbol(value){
                var one = value.toString();
                return one.replace(/[\[]\]/,"");
            }
        }
    })
</script>
```

```
// demo-two
<div id="app">
    <select v-model="chengshi">
        <option :value="chushi.one"> {{chushi.one}} </option>
        <option :value="chushi.two"> {{chushi.two}} </option>
        <option :value="chushi.three"> {{chushi.three}} </option>
    </select>
    <div> {{chengshi}} </div>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var app = new Vue({
        el: "#app",
        data: {
            chengshi:null,
            chushi: {
                one: "上海",
                two:"北京",
                three:"广州"
            }
        }
    })
</script>
```

```
//demo-three
<div id="app">
    <select v-model="chengshi">
        <option value="" selected="selected">请选择</option>
        <option :value="item" v-for="item in chushi">{{item}}</option>
    </select>
    <div>{{chengshi}}</div>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var app = new Vue({
        el: "#app",
        data: {
            chengshi:null,
            chushi: ["上海","北京","广州"],
        }
    })
</script>

```

```
//demo-four
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
##### 事件绑定的事件修改器
```
//demo-one
<div id="app">
    <input @keydown.enter="keydown" type="text">
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el: "#app",
        methods:{
            keydown(){
                console.log("enter");
            }
        }

    })
</script>


//demo-two
<div id="app">
    <input @keydown.13="keydown" type="text">
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el: "#app",
        methods:{
            keydown(){
                console.log("enter");
            }
        }

    })
</script>
```

#### v-html 渲染，v-text 文本渲染
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

### 全局 API

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
<script src = "https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js" ></script>
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

#### Vue.nextTick();由于 vue 不希望我们直接来操作 dom 节点，因此使用 vue 来控制的 dom 数据改变，vue 会有延迟

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

#### Vue.mixin();允许在 method 以外来定义方法

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
    // 使用Vue.extend();方法也是来创建一个实例，类似于vue实例
    // 使用基础 Vue 构造器，Vue.extend();方法，创建一个“子类”，类似于vue实例
    var newExtend = Vue.extend({
        template:"<p><a :href='one'>{{two}}</a></p>",
        data:function () {
            return {
                one:"https://www.baidu.com/",
                two:"我是连接"
            }
        }
    });
    // 挂载实例
    new newExtend().$mount('#demo');

</script>
```
#### Vue.filter();全局的过滤器
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

##### 自定义指令的五个钩子函数（五个生命周期）

```
<div id="app">
    <div v-jspang="color" id="demo">
        {{num}}
    </div>
    <button @click="add" id="btn">Add</button>
    <p>
        <button onclick="unbind();">解绑</button>
        <!-- 解绑就不能使用vue的方法了，也就不能执行其他vue的方法了 -->
    </p>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
Vue.directive('jspang', {
    bind: function (el,bind) {                //只执行一次，指令第一次被绑定到元素时调用，用这个钩子函数可义一个绑定时执行一次的初始化动作。
        console.log('1 - bind');
        console.log(el);
        console.log(el.style.color = "red");
        console.log(bind);
    },
    inserted: function (el,bind) {           //被绑定元素插入父节点时调用
        console.log('2 - inserted');
        console.log(el);
        console.log(bind);
    },
    update: function (el,bind) {             //被绑定于元素所在的模板更新时调用
        console.log('3 - update');
        vm.$data.num = 22;
        console.log(el);
        console.log(bind);
    },
    componentUpdated: function (el,bind) {   //组件更新完成
        console.log('4 - componentUpdated');
        vm.$data.num = 33;
        console.log(el);
        console.log(bind);
    },
    unbind: function (el,bind) {             //只调用一次，指令与元素解绑时调用
        console.log('5 - unbind');
        vm.$data.num = 44;
        console.log(el);
        console.log(bind);
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
                his.num++;
        }
    }
})
</script>
```

#### Vue.component();定义全局组件，全局组件可以用在任何地方,组件其实也是实例化了一个对象，组件内也有生命周期钩子函数。
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
        },
        /* 1.0 beforeCreate是实例被创建之前，任何dom元素都还未定义，任何数据都和事件都还没有被执行 */
        beforeCreate: function () {
            console.group('beforeCreate 创建前状态===============》');
            /*控制台输出  添加样式  什么样式     输出内容         */
            console.log("%c%s", "color:red", "el     : " + this.$el); //undefined
            console.log("%c%s", "color:red", "data   : " + this.$data); //undefined
            console.log("%c%s", "color:red", "message: " + this.message) //undefined
        },
        /* 2.0 实例创建完了，但是dom元素还没有生成，数据和事件已经可见，但是还没有完成挂载 */
        created: function () {
            console.group('created 创建完毕状态===============》');
            console.log("%c%s", "color:red", "el     : " + this.$el); //undefined
            console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化
            console.log("%c%s", "color:red", "message: " + this.message); //已被初始化
        },
        /*3.0 在挂载开始之前被调用：相关的 render 函数首次被调用。产生dom元素 */
        beforeMount: function () {
            console.group('beforeMount 挂载前状态===============》');
            console.log("%c%s", "color:red", "el     : " + (this.$el)); //已被初始化
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化
            console.log("%c%s", "color:red", "message: " + this.message); //已被初始化
        },
        /*4.0 完成了 el 和 data 初始化 产生dom元素*/
        mounted: function () {
            console.group('mounted 挂载结束状态===============》');
            console.log("%c%s", "color:red", "el     : " + this.$el); //已被初始化
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化
            console.log("%c%s", "color:red", "message: " + this.message); //已被初始化
        },

        /*5.0 数据更新操作的，下面就能看到data里的值被修改后，将会触发update的操作。
        /* 在console.log()内输入app.message='数据更新状态'*/
        beforeUpdate: function () {
            console.group('beforeUpdate 更新前状态===============》');
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data);
                onsole.log("%c%s", "color:red", "message: " + this.message);
        },
        /*6.0 数据更新操作完成，下面就能看到data里的值被修改后，将会触发update的操作。*/
        updated: function () {
            console.group('updated 更新完成状态===============》');
            console.log("%c%s", "color:red", "el     : " + this.$el);
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data);
            console.log("%c%s", "color:red", "message: " + this.message);
        },
        /*7.0 vue实例被销毁之前：我们在console里执行下命令对 vue实例进行销毁。
        销毁完成后，我们再重新改变message的值，vue不再对此动作进行响应了。但是原先生成的dom元素还存在，
        可以这么理解，执行了destroy操作，后续就不再受vue控制了。*/
        beforeDestroy: function () {
            console.group('beforeDestroy 销毁前状态===============》');
            console.log("%c%s", "color:red", "el     : " + this.$el);
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data);
            console.log("%c%s", "color:red", "message: " + this.message);
        }, 
        /*8.0 vue实例被销毁结束*/
        destroyed: function () {
            console.group('destroyed 销毁完成状态===============》');
            console.log("%c%s", "color:red", "el     : " + this.$el);
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data);
            console.log("%c%s", "color:red", "message: " + this.message)
        }
    })
    new Vue({
        el: "#app"
    })
</script>
```

### 实例选项（options） 

#### el ,data

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

#### props，接收数组，父组件的值传递给子组件

```
<div id="app">                    <!--父组件包含子组件,但是由于父组件包含子组件那么这里面的数据都属于父组件的-->
    <child msg="hello"></child>   <!--子组件算是父子间内的数据,在父组件的作用域中-->
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    // 什么是父子组件:父子组件之间的作用域是相互独立的,需通过props传递,这里面定义的是子组件.
    Vue.component('child',{
        props: ['msg'],                                            //接收父组件传递的数据
        template: '<span> {{msg}} --- {{val}} </span>',
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
##### 声明一个局部组件
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
##### 内置组件标签<component>
```
<div id="app">
    <component v-bind:is="who"></component>
    <button @click="changCompnent()">变化组件</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
        let vm = new Vue({
            el:"#app",
            data:{who:"comA"},
            components: {
                "comA":{template:"<div>AAAAA</div>"},
                "comB":{template:"<div>BBBBB</div>"},
                "comC":{template:"<div>CCCCC</div>"},
                 
            },
            methods: {
            changCompnent:function(){
                if (this.who == 'comA') {
                    this.who = 'comB';
                } else if (this.who == 'comB') {
                    this.who = 'comC';
                } else {
                    this.who = 'comA';
                }
            } 
        }
    })
</script>
```

##### 组件嵌套
```
<div id="app">
    <child-one></child-one>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el: "#app",
        data: {

        },
        components: {
            "childOne": {
                template: "<div> {{this.one}} <br> <young-boy> </young-boy> </div>",
                data: () =>{
                    return {one: "my name miaonan!!!"}
                },
                components: {
                    "youngBoy": {
                        template: "<p>youngBoy</p>"
                    }
                }
            }
        }

    })
</script>
```
##### solt 父组件的内容会在子组建的插槽标签中<slot></slot>中保留 
```
<div id="app">
    <h1>我是父组件的标题</h1>
    <my-component>   
        <!-- 父组件的内容会在子组建的插槽标签中<slot></slot>中保留 -->
        <p>这是一些初始内容</p>  
        <p>这是更多的初始内容</p>
        <!-- 父组件的内容会在子组建的插槽标签中<slot></slot>中保留 -->
    </my-component>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
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

</script>
```

#### extends，接收对象，扩展实例方法
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

#### mixins，接收数组，扩展实例方法
```
<div id="app">
    <p v-text="num"></p>
    <button @click="add">点击</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    //如果混入的方法与选项的方法有同名,优先使用选项内的方法
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

#### directives 自定义指令，他其中有五个声明周期及五个钩子函数

```
<div id="app">
    <input type="text" v-focus :value="message">
    <button @click="add">Add</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            message: 1
        },
        methods: {
            add: function () {
                this.message++;
            }
        },
        directives: {
            focus: {
                bind: function (el, bind) {             //只执行一次，指令第一次被绑定到元素时调用
                                                        //用这个钩子函数可以定义一绑定时执行一次的初始化动作。
                    console.log('1 - bind');
                    console.log(el);
                    console.log(el.style.color = "red");
                    console.log(bind);
                },
                inserted: function (el, bind) {          //被绑定元素插入父节点时调用
                    console.log('2 - inserted');
                    console.log(el);
                    console.log(bind);
                },
                update: function (el, bind) {            //被绑定于元素所在的模板更新时调用
                    console.log('3 - update');
                    vm.$data.message = 22;
                    console.log(el);
                    console.log(bind);
                },
                componentUpdated: function (el, bind) {  //组件更新完成
                    console.log('4 - componentUpdated');
                    vm.$data.num = 33;
                    console.log(el);
                    console.log(bind);
                },
                unbind: function (el, bind) {           //只调用一次，指令与元素解绑时调用
                    console.log('5 - unbind');
                    vm.$data.num = 44;
                    console.log(el);
                    console.log(bind);
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
    <button @click="one">数据更新</button>
    <p>{{ message }}</p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
         var app = new Vue({
            el: '#app',
            data: {
                message: 1
            },
            methods:{
                one:function(){
                    this.message ++;
                },
            },
            beforeCreate: function () {
                /* 1.0 beforeCreate是实例被创建之前，任何dom元素都还未定义，任何数据都和事件都还没有被执行 */
                console.group('beforeCreate 创建前状态===============》');
                /*控制台输出  添加样式  什么样式     输出内容         */
                console.log("%c%s", "color:red", "el     : " + this.$el); //undefined
                console.log("%c%s", "color:red", "data   : " + this.$data); //undefined
                console.log("%c%s", "color:red", "message: " + this.message) //undefined
            },
            /* 2.0 实例创建完了，但是dom元素还没有生成，数据和事件已经可见，但是还没有完成挂载 */
            created: function () {
                console.group('created 创建完毕状态===============》');
                console.log("%c%s", "color:red", "el     : " + this.$el); //undefined
                console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化
                console.log("%c%s", "color:red", "message: " + this.message); //已被初始化
            },
            /*3.0 在挂载开始之前被调用：相关的 render 函数首次被调用。产生dom元素 */
            beforeMount: function () {
                console.group('beforeMount 挂载前状态===============》');
                console.log("%c%s", "color:red", "el     : " + (this.$el)); //已被初始化
                console.log(this.$el);
                console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化
                console.log("%c%s", "color:red", "message: " + this.message); //已被初始化
            },
            /*4.0 完成了 el 和 data 初始化 产生dom元素*/
            mounted: function () {
                console.group('mounted 挂载结束状态===============》');
                console.log("%c%s", "color:red", "el     : " + this.$el); //已被初始化
                console.log(this.$el);
                console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化
                console.log("%c%s", "color:red", "message: " + this.message); //已被初始化
            },

            /*5.0 数据更新操作的，下面就能看到data里的值被修改后，将会触发update的操作。
            /* 在console.log()内输入app.message='数据更新状态'*/
            beforeUpdate: function () {
                console.group('beforeUpdate 更新前状态===============》');
                console.log(this.$el);
                console.log("%c%s", "color:red", "data   : " + this.$data);
                console.log("%c%s", "color:red", "message: " + this.message);
            },
            /*6.0 数据更新操作完成，下面就能看到data里的值被修改后，将会触发update的操作。*/
            updated: function () {
                console.group('updated 更新完成状态===============》');
                console.log("%c%s", "color:red", "el     : " + this.$el);
                console.log(this.$el);
                console.log("%c%s", "color:red", "data   : " + this.$data);
                console.log("%c%s", "color:red", "message: " + this.message);
            },
            /*7.0 vue实例被销毁之前：我们在console里执行下命令对 vue实例进行销毁。
            销毁完成后，我们再重新改变message的值，vue不再对此动作进行响应了。但是原先生成的dom元素还存在，
            可以这么理解，执行了destroy操作，后续就不再受vue控制了。*/
            beforeDestroy: function () {
                console.group('beforeDestroy 销毁前状态===============》');
                console.log("%c%s", "color:red", "el     : " + this.$el);
                console.log(this.$el);
                console.log("%c%s", "color:red", "data   : " + this.$data);
                console.log("%c%s", "color:red", "message: " + this.message);
            },
            /*8.0 vue实例被销毁结束*/
            destroyed: function () {
                console.group('destroyed 销毁完成状态===============》');
                console.log("%c%s", "color:red", "el     : " + this.$el);
                console.log(this.$el);
                console.log("%c%s", "color:red", "data   : " + this.$data);
                console.log("%c%s", "color:red", "message: " + this.message)
            }
        })
</script>
```

### 实例属性与方法，事件
#### vm.$el，vm.$data

```
<div id="app">
</div>
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

#### vm.$children，当前实例的子组件
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

#### vm.$refs,获取实例的指定的子组件或 dom 元素
```
<style>
    .red{
        color: blueviolet;
    }
</style>

<div id="parent">
    <div ref="profile">qqq</div>
    <child ref="pro"></child>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var parent = new Vue({
        el: '#parent',
        components: {
            'child': {
                template: "<div :class={red:red}>{{two}}</div>",
                data() {
                    return {
                        two: "twooooo",
                        red:true,
                    }
                }
            }
        }
    })
    // 访问子组件实例
    var childone = parent.$refs.profile;
    var childtwo = parent.$refs.pro;
    console.log(childone);
    console.log(childtwo);
    childone.style.color = "red";
    childtwo.$data.two = "oooooooo";
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

#### vm.$on( event, callback );定义在实例上的事件，

#### vm.$emit( event, […args] );用来触发定义在实例上的事件

```
    <div id="parent">
        <button @click="add">dianji</button>
        <div >qqq</div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>

    <script>
        let vm = new Vue({
            el: '#parent',
            methods: {
                add:function(){
                    vm.$emit('click')
                }
            }

        })

        vm.$on('click',function(){
            console.log("触发了");
        })
    </script>
```
#### vm.$off();解绑实例事件,无法解绑v-on绑定在dom元素上的事件。dom元素上的事件可以使用销毁实例来解绑。
```
<style>
.red{
    color: red;
}
</style>

<div id="app">
    <span :class="{red:isred}">{{message}}</span>
    <button @click="add()">增加</button>
    <button onclick="reduce()">减少</button>
    <button onclick="off()">关闭</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el: "#app",
        data: {
            message: 1,
            isred:false,
        },
        methods:{
            add:function(){
                this.message++;
                this.isred = this.isred ? false:true;
            }
        }
        
    })
        // 实例事件
        vm.$on('reduce', function () {
            this.message --
        })

        // 关闭事件
        function off(){
            vm.$off(vm.add);
            //vm.$off('reduce');
        }
    </script>

```
#### vm.$destroy();销毁 vue 实例
```
<div id="app">
    <p v-text="num"></p>
    <button @click="add()">add</button>
    <button onclick="des()">销毁</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    //完全销毁一个实例。清理它与其它实例的连接，解绑它的全部指令及事件监听器
    let vm = new Vue({
        el:"#app",
        data:function(){
            return {num:1}
        },
        methods: {
            add:function(){
                this.num += 2;
            }
        }
    });

    function des(){
        vm.$destroy()
    }
        
</script>
```
#### vm.$forceUpdate();
```
<div id="app">
    <p @click="add">{{num}}</p>
    <button onclick="up()">重新渲染</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el:"#app",
        data:function(){
            return {num:1}
        },
        methods: {
            add:function(){
                this.num +=2;
            }
        }
    });

    function up(){
        vm.$forceUpdate();
    }
```
#### vm.$mount( [elementOrSelector] ) ,作用手动挂载DOM
```
<div id="app">
    {{num}}
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    // 当我们定义一个vue实例但是没有进行给el赋予一个dom元素，DOM元素并没有挂载在vue上。可以用vm.$mount("dom选项")；
    var vm = new Vue({
        data: {
            num: 0
        }
    });

    vm.$mount("#app");
</script>

```
#### vm.$delecte();与Vue.delecte();用来在实例之外删除对象属性，是对数据的一种删除操作
```
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    //vm.$delecte();与Vue.delecte();用来在实例之外删除对象属性，是对数据的一种删除操作。
    let a = {
        age:19,
        name:"mn"
    }

    Vue.delete(a,"name")
    vm.$delete(a,"age");
    console.log(a);

    </script>
```
#### vm.$set();Vue.set 的作用就是在构造器外部操作构造器内部的数据、属性或者方法
```
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
     // Vue.set 的作用就是在构造器外部操作构造器内部的数据、属性或者方法。
    var vm =new Vue({
        el:"#app",
        data:function(){
            return{msg:[1,2,3],
            num:["o","p","e"]
            }
        }
    });

    Vue.set(vm.num,1,"1");
    vm.$set(vm.msg,1,"p");
        
    console.log(vm.$data.msg);
    console.log(vm.$data.num);
```
#### ref与$refs:vue为我们封装了dom的渲染但是有些时候我们也需要来操作dom元素
```
<div id="parent">
    <demo-one ref="one"></demo-one>
    <div ref="three">111</div>
    <demo-two ref="two"></demo-two>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>
    Vue.component( 'demo-one',{
        template:"<div>{{msg}}</div>",
        data:function(){
            return {msg:222}
        }
    });

    Vue.component( 'demo-two',{
        template:"<button>{{num}}</button>",
        data:function(){
            return {num:点击}
        }
    });

    let vm = new Vue({
        el:"#parent",
        mounted:function(){
            //上边的两个子组件如何在这里进行操作，这就需要ref与$refs
            console.log(this.$refs.one);     
            console.log(this.$refs.one.msg);  
            console.log(this.$refs.three);  
                
            console.log(this.$refs.two);      
            console.log(this.$refs.two.num);  
                
        }
    })
    
</script>
```
