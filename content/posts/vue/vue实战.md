---
title: "Vue实战"
subtitle: "Vue入门实战笔记"
date: 2022-06-05T13:58:22+08:00
lastmod: 2022-06-05T13:58:22+08:00
draft: false
toc:
enable: true
weight: false
categories: ["程序设计"]
tags: ["前端","Vue"]
---
# 一 初识Vue.js

## 1、Vue.js是什么？

​	简单小巧的核心，渐进式技术栈，足以应付任何规模的应用。

​	使用Vue.js可以让web开发变得简单，同时也颠覆了传统前端开发模式。他提供了现代Web开发中常见的高级功能：

- 解耦视图与数据
- 可复用的组件
- 前端路由
- 状态管理
- 虚拟DOM

### 1.1.1 MVVM模式

​	MVVM模式是由经典的软件架构MVC衍生而来的，当View(视图层)变化时，会自动更新到ViewModel(视图模型)，反之亦然，View(视图层)和ViewModel之间通过双向绑定建立联系。

![mvvm](/Users/limingqi/Documents/notes/LearnNotes/vue/img/mvvm.png)

该模式相对于MVC模式的优点：

1.低耦合，可重用性，model以及view实现分离状态

2.双向绑定，在mvc中只能是由原始数据控制产生数据，在mvvm模式中可以实现视图与数据双向的绑定

3.减少dom操作。解决了数据频繁更新的问题。

## 2、如何使用Vue.js

### 1.2.1 通过script加载CDN文件

```html
<!--自动识别最新稳定版本的Vue.js-->
<script src="https://unpkg.com/vue/dist/vue.min.js"></script>
<!--指定某个具体版本的Vue.js-->
<script src="https://unpkg.com/vue@2.1.6/dist/vue.min.js"></script>
```

### 1.2.2  将代码下载下来，通过相对路径来引用vue.js文件

### 1.2.3 Vue但文件的形式配合webpack使用

# 二 数据绑定和第一个Vue应用

## 2.1 Vue实例与数据绑定

### 2.1.1 实例与数据

​	通过构造函数Vue创建一个Vue的根实例，并启动Vue应用：

```html
<html>
    <head>
        <script src='package2.64/dist/vue.js'></script>
    </head>
    <body>
      <div id='app'> 
          {{message}}
      </div>
      <div id='app-2'>
          <span v-bind:title="message">
            鼠标悬停几秒钟查看此处动态绑定的提示信息！ {{message}}
          </span>
      </div>
      <div id='app-3'>
          <p v-if="seen">现在看到我了</p>
      </div>
      <div id="app-4">
          <ol>
              <li v-for="todo in todos">
                {{todo.text}}
              </li>
              
          </ol>
      </div>
      <div id="app-5">
       <p>
           {{message}}
       </p>
       <button v-on:click="reverseMessage">翻转消息</button>
      </div>
      <div id="app-6">
        <p>{{ message }}</p>
        <input v-model="message">
      </div>
      <div id="app-7">
          <ol>
              <todo-item v-for="item in groceryList"
                         v-bind:todo="item"
                         v-bind:key="item.id">
              </todo-item>
              
          </ol>
      </div>
    </body>
</html>
<script>
    var app=new Vue({
        el:'#app',
        data:{
            message:'Hello Vue'
        }
    })
    var app2=new Vue({
        el:'#app-2',
        data:{
            message: '页面加载于 ' + new Date().toLocaleString()
        }
    })
    var app3=new Vue({
        el:'#app-3',
        data:{
            seen:false
        }
    })
    var app4=new Vue({
        el:'#app-4',
        data:{
            todos:[
                {text:'c#'},
                {text:"Java"},
                {text:"go"},
                {text:"mysql"},
                {text:"redis"},
                {text:"docker"}
            ]
        }
    })
    var app5 = new Vue({
        el: '#app-5',
        data: {
            message: 'Hello Vue.js!'
        },
        methods: {
            reverseMessage: function () {
            this.message = this.message.split('').reverse().join('')
            }
        }
    })
    var app6 = new Vue({
        el: '#app-6',
        data: {
            message: 'Hello Vue!'
        }
    })
    //组件化应用构建
    Vue.component('todo-item',{
        //todo-item组件现在接受一个prop，
        props:['todo'],
        template:'<li>{{todo.text}}</li>'
    })
    var app7 = new Vue({
        el: '#app-7',
        data: {
        groceryList: [
            { id: 0, text: '蔬菜' },
            { id: 1, text: '奶酪' },
            { id: 2, text: '随便其它什么人吃的东西' }
        ]
        }
    })
</script>
```



## 2.2 指令与事件

## 2.3 语法糖

# 三 计算属性

# 四 v-bind及class与style绑定

# 五 内置指令

# 六 表单与v-model

# 七 组件详解

​	组件(Component)是Vue.js最核心的功能，也是整个框架设计最精彩的地方

## 7.1组件与复用

### 7.1.1组件用法

​	组件需要注册后才可以使用，注册有全局注册和局部注册两种方式，全局注册后，任何Vue示例都可以使用

```html
 <div id='app'> 
        <my-component></my-component>
 </div>
<script>
      Vue.component('my-component',{
        //选项template的DOM元素必须被一个元素包含
        template:'<div>这里是组件的内容</div>'
    })
      var app=new Vue({
        el:'#app',
    })
</script>
```

components选项可以局部注册组件，注册后的组件只有在该实例作用域下有效。组件中也可以使用components选项来注册组件，使组件可以嵌套

```html
 <div id='app-child'> 
        <child-component></child-component>
 </div>
<script>
    var Child={
        template:'<div>这里是Child组件的内容</div>'
    }
    var appChild=new Vue({
        el:'#app-child',
        components:{
            'child-component':Child
        }
    })
</script>
```

​    Vue组件的模板在某些情况下会受到HTML的限制，比如table内规定只允许是tr、td、th、这些元素，所以在table内直接使用组件是无效的，这种情况下需要使用is属性来挂载组件

```html
      <div id='app-table'> 
        <table>
            <!--tbody在渲染的时候会被替换为组件的内容，常见的限制元素还有ul、ol、select-->
            <tbody is="table-component"></tbody>
        </table>
      </div>
<script>
    var table_component={
        template:'<div>这里是Table组件的内容</div>'
    }
    var appTable=new Vue({
        el:'#app-table',
        components:{
            'table-component':table_component
        }
    })
</script>
```

除了template，组件中还可以像VUE实例那样使用其他的选项，比如data、computed、methods等。但是在使用data时，data必须是函数，然后将数据return

```html
  <div id='app-data'> 
        <data-component></data-component>
      </div>
<script>
    Vue.component('data-component',{
        //选项template的DOM元素必须被一个元素包含
        template:'<div>{{message}}</div>',
        data:function(){
            return{
                message:"data组件内容"
            }
        }
    })
    var appData=new Vue({
        el:'#app-data'
    })
</script>
```

​	JavaScript对象时引用关系，所以如果return出来的对象引用了外部的一个对象，那么这个对象就是共享的，任何一个地方修改都会同步

```html
<div id='app-share'> 
  <share-component></share-component>
  <share-component></share-component>
  <share-component></share-component>
</div>
<script>
    var data={
        counter: 0
    };
    Vue.component('share-component',{
        //选项template的DOM元素必须被一个元素包含
        template:'<button @click="counter++">{{counter}}</div>',
            data:function(){
                return data;
            }
    })
    var appShare=new Vue({
        el:'#app-share'
    })
</script>
```

​	组件使用了三次，但是点击任一个button，三个数字都会增加，那是因为组件data引用的事外部对象。如果想要达到预想的效果需要给组件返回一个新的data对象来独立。

```html
<div id='app-share'> 
  <share-component></share-component>
  <share-component></share-component>
  <share-component></share-component>
</div>
<script>
    var data={
        counter: 0
    };
    Vue.component('share-component',{
        //选项template的DOM元素必须被一个元素包含
        template:'<button @click="counter++">{{counter}}</div>',
            data:function(){
                return {
                   counter: 0
                };
            }
    })
    var appShare=new Vue({
        el:'#app-share'
    })
</script>
```

​	这样，点击三个按钮就互不影响了，达到完全复用的目的

## 7.2 使用props传递数据

### 7.2.1 基本用法

​	组件不仅仅是要把模板的内容进行复用，更重要的事组件间要进行通信，通常父组件的模板中包含子组件，父组件要正向的向子组件传递数据或参数，子组件接收后根据参数的不同来渲染不同的内容或操作，这个正向传递参数的过程就是通过props来实现的。

​	在组件中，使用选项props来声明需要从父级接收的数据。props的值可以是字符串数组，也可以是对象

```html
 <div id='app'> 
        <my-component message="来自父组件的数据"></my-component>
 </div>
<script>
      Vue.component('my-component',{
        //选项template的DOM元素必须被一个元素包含
        template:'<div>这里是组件的内容</div>'
    })
      var app=new Vue({
        el:'#app',
    })
</script>	
```

渲染后的结果为：

```html
 <div id='app'> 
        <div>来自父组件的数据</div>
 </div>
```

​	props中声明的数据与组件data函数return的数据的主要区别是前者来自父级，而后者是组件自己的数据。作用域是组件本身。这两种数据都可以在模板template及计算属性computed和方法methods中使用。

​	由于HTML特性是不区分大小写，当使用DOM模板时，驼峰命名的props名称要转换为短横杆分割命名

​	有时候传递的数据并不是直接写死的，而是来自父级的的动态数据，这时可以使用v-bind来动态绑定props的值，当父组件的数据变化时，也会传递给子组件

```html
  <div id='app-databinding'> 
    <!-- 这里用 v-model绑定了父级的数据parentMessage，当通过输入框输入任意内容时，子组件接收到的props“message”也会实时相应，并更新组件模板-->
    <input type="text" v-model="parentMessage">
    <databinding-component :message="parentMessage"></databinding-component>
</div>
<script>
  Vue.component('databinding-component',{
        //选项template的DOM元素必须被一个元素包含
        template:'<div>{{message}}</div>',
        props: ['message']
    })
      var app=new Vue({
        el:'#app',
    })
  var appDatabinding=new Vue({
        el:'#app-databinding',
        data:{
            parentMessage: ""
        }
    })
</script>	
```

​	注意，如果直接传递数字、布尔值、数组、对象，而且不使用v-bind，传递的仅是字符串。

```html
<div id='app-nobinding'> 
  <nobinding-component message="[1,2,3]"></nobinding-component>
  <nobinding-component :message="[1,2,3]"></nobinding-component>
</div>
<script>
  //同一个组件使用了两次，区别仅仅是第二个使用的是 v-bind ，渲染后的结果，第一个是7，，第二个才是数组的长度 3,
    Vue.component('nobinding-component',{
        //选项template的DOM元素必须被一个元素包含
        template:'<div>{{message.length}}</div>',
        props: ['message']
    })
      var app=new Vue({
        el:'#app',
    })
    var app_nobinding =new Vue({
        el:'#app-nobinding'
    })
</script>	
```

### 7.2.2 单项数据流

​	1）父组件传递初始值进来，子组件将他作为初始值保存起来，在自己的作用域下可以随意使用和修改，这种情况下可以在组件data内再声明一个数据，引用父组件的prop就可以了。

```html
<div id='app-initcount'> 
<initcount-component :initCount="1"></initcount-component>
</div>
<script>
     Vue.component('initcount-component',{
        //选项template的DOM元素必须被一个元素包含
        template:'<div>{{count}}</div>',
        props: ['initCount'],
        data: function(){
            return{
                count:this.initCount
            }
        }
    })
    var app_initcount =new Vue({
        el:'#app-initcount'
    })
</script>	
```

​	2）prop作为需要被转变的原始值传入，这种情况用计算属性就可以了。

```vue
<div id='app'> 
<initcount-component :width="1"></initcount-component>
</div>
<script>
     Vue.component('initcount-component',{
        template:'<div :style="style">{{组件内容}}</div>',
        props: ['width'],
        computed: {
          style:function(){
            return{
              //因为CSS传递宽度要带单位px，但是每次都写都太麻烦，而且数值一般都是不带单位的，所以同一在组件内使用计算属性就可以了。
                width:this.width+'px'
            }
          }
        }
    })
    var app_initcount =new Vue({
        el:'#app'
    })
</script>	
```

​	在JavaScript中对象和数组是引用类型，指向同一个内存空间，所以props是对象和数组时，在子组件内改变是会影响父组件的。	

### 7.2.3 数据验证

​	一般当组件需要提供给别人使用时，推荐都进行数据验证，比如某个数据必须是数字类型，如果传入的是字符串，就会在控制台给出警告，以下是几个prop的示例：

```vue
<script> 
Vue.component('checked-component',{
        props:{
            //必须是数字类型
            propA: Number,
            //必须是字符串或数字类型
            propB: [String,Number],
            //布尔值，如果未定义，默认为true
            PropC:{
                type:Boolean,
                default:true
            },
            //数字，必传
            PropD:{
                type:Number,
                required:true
            },
            //如果是数组或对象，默认值必须是一个函数来返回
            PropE:{
                type:Array,
                default:function(){
                return [];
                }
            },
            //自定义一个验证函数。
            PropF:{
                validator:function(value){
                    return value>10;
                }
            }
        }
    })
  <script>
```

​	验证的type类型可以是：

- String
- Number
- Boolean
- Object
- Array
- Function

​	type也可以是一个自定义构造器，使用instanceof检测。

​	当prop验证失败时，在开发版本下会在控制台抛出一条警告。	

## 7.3组件通信

​	![component](/Users/limingqi/Documents/notes/LearnNotes/vue/img/component.png)

组件通信可分为父子组件通信，兄弟组件通信，跨级组件通信

### 7.3.1 自定义事件

​	当子组件需要向父组件传递数据时，就要用到自定义事件，v-on除了监听DOM事件外，还可以用于子组件之间的自定义事件，

​	如果你了解过JavaScript的设计模式-观察者模式，一定知道dispatchEvent和addEventListener这两个方法，Vue也有与之类似的一套模式，子组件用$emit()来触发事件，父组件用$on()来监听子组件的事件

​	父组件也可以直接在子组件的自定义标签上使用v-on来监听子组件触发的自定义事件，示例代码如下：

```html
<html>
<head>
    <script src='package2.64/dist/vue.js'></script>
</head>
<body>
    <div id='app'>
        <p>总数：{{total}}</p>
        <customevent-component @increase="handleGetTotal" @reduce="handleGetTotal"></customevent-component>
    </div>
</body>

</html>
<script>
    Vue.component('customevent-component', {
        //选项template的DOM元素必须被一个元素包含
        template: '<div><button @click="handleIncrease">+1</button><button @click="handleReduce">-1</button>sss</div>',
        data: function () {
            return {
                counter: 0
            }
        },
        methods: {
            handleIncrease: function () {
                this.counter++
                this.$emit('increase', this.counter)
            },
            handleReduce: function () {
                this.counter--
                this.$emit('reduce', this.counter)
            }
        }
    })
    var app = new Vue({
        el: "#app",
        data: {
            total: 0
        },
        methods: {
            handleGetTotal: function (total) {
                this.total = total;
            }
        }
    })
</script>
```

​	上面示例中，子组件有两个按钮，分别实现加1和减1的效果改变组件的data "counter"后通过$emit()再把它传递给父组件，父组件用v-on：increase和v-on:reduce(示例使用的事语法糖)。$emit()方法的第一个参数时自定义事件的名称，后面的参数时要传递的数据，可以不填或者填多个。

​	除了用v-on在组件上监听自定义事件外，也可以监听DOM事件，这时，可以使用.native修饰符表示监听的是一个原生事件，监听的是该组件的根元素：

```html
<my-component v-on:click.native=”handleClick” ></my-component>
```

### 7.3.2 使用v-model

​	Vue2.x可以在自定义组件上使用v-model指令

```html
<html>
<head>
    <script src='package2.64/dist/vue.js'></script>
</head>
<body>
      <div id='app-custom-event-model'> 
            <p>总数：{{total}}</p>
           <customeventmodel-component v-model="total"></customeventmodel-component>
        </div>
</body>

</html>
<script>
        Vue.component('customeventmodel-component',{
        //选项template的DOM元素必须被一个元素包含
        template:'<div>\
            <button @click="handleClick">+1</button><button\
            </div>',
        data: function(){
            return{
                counter:0
            }
        },
        methods:{
            handleClick:function(){
                this.counter++
                this.$emit('input',this.counter)
            }
        }
    })
    var appcustomevent=new Vue({
        el:"#app-custom-event-model",
        data:{
            total:0
        }
    })
</script>
```

​	仍然是按钮加1的效果，不过这次组件$emit()的事件名是特殊的input，在使用组件的父级，并没有在组件上使用@input="handler",而是直接使用了v-model绑定的一个数据total。这也算是一个语法糖，因为上面的示例可以间接地使用自定义事件来实现。

​	v-model还可以用来创建自定义的表单输入组件，进行数据双向绑定：

```html
     <div id='app-two-way-model'> 
            <p>总数：{{total}}</p>
           <customtwoway-component v-model="total"></customtwoway-component>
           <button @click="handleReduce">-1</button>
        </div>
<script>
  Vue.component('customtwoway-component',{
        template:'<input :value=”value” @input=”updateValue”>',
        props: ["value"],
        methods: function(){
            return{
                counter:0
            }
        },
        methods:{
            updateValue:function(event){
                this.$emit('input',event.target.value)
            }
        }
    });
    var app_two_way=new Vue({
        el:"#app-two-way-model",
        data: {
            total:0
        },
        methods:{
            handleReduce:function(){
                this.total--;
            }
        }
    })
</script>
```

​	实现这样一个具有双向绑定的v-model组件要满足下面两个要求：

- 接收一个value属性。
- 在有新的value时触发input事件。

### 7.3.3 非父子组件通信

#### 1）中央事件总线bus

​		在Vue.js 2.X中，推荐使用一个空的 Vue 实例作为中央事件总线（ bus），也就是一个中介	

```html
<html>
<head>
    <script src='package2.64/dist/vue.js'></script>
</head>

<body>
    <div id='app'>
        <p>{{message}}</p>
        <bus-component></bus-component>
    </div>
</body>

</html>
<script>
  // 
    var bus=new Vue();
    Vue.component('bus-component', {
        //选项template的DOM元素必须被一个元素包含
        template: '<button @click="handleEvent">传递事件</button>',
        methods: {
            handleEvent: function () {
              //点击按钮就会通过bus把事件on-message发出去
                bus.$emit('on-message', '来自组件bus-component的内容')
            }
        }
    })
    var app = new Vue({
        el: "#app",
        data: {
            message: ""
        },
      //在app初始化时，也就是在声明周期mounted钩子函数里面监听了来自bus的事件on-message，
        mounted: function(){
            var _this=this;
            bus.$on('on-message',function(msg){
                _this.message=msg;
            })
        }
    })
</script>
```

​	首先创建了一个名为bus的空Vue 实例，里面没有任何内容；然后全局定义了组件bus-component；最后创建 Vue 实例 app ，在 app 初始化时，也就是在生命周期 

mounted 钩子函数里监听了来自 bus 的事件 on-message ，而在组件 component-a 中，点击按钮会通过 bus 把事件 on-message发出去，此时 app 就会接收到来自 bus 的事件，进而在回调里完成自己的业务逻辑。

​	这种方法巧妙而轻巧地市县了任何组件间的通信，包括父子、兄弟、跨级。如果深入使用，可以拓展bus实例，为其添加Vue的各种选项，这些都可以是公用的。在业务中，尤其是协同开发使其非常有用，因为经常需要共享一些通用的信息，比如用户登录的昵称、性别、邮箱等，还有用的授权token等，只需要在初始化的时候让bus获取一次，任何时间，任何组件就可以从中直接使用了。

#### 2）父链

​	在子组件中使用this.$parent可以直接访问该组件的父示例或者组件，父组件也可以通过this.$children访问它的所有子组件，而且可以递归向上或向下无限访问，直到根示例或者最内层的组件

```html
 <html>
<head>
    <script src='package2.64/dist/vue.js'></script>
</head>

<body>
    <div id='app_parent'>
        <p>{{message}}</p>
        <parent-component></parent-component>
    </div>
</body>

</html>
<script>
   Vue.component('parent-component', {
        template: '<button @click="handleEvent">通过父链直接修改数据</button>',
        methods: {
            handleEvent: function () {
               this.$parent.message="来自子组件bus-component修改的内容"
            }
        }
    })
    var app = new Vue({
        el: "#app_parent",
        data: {
            message: ""
        }
    })
</script>
```

​	尽管Vue允许这样操作，但是在业务中，子组件应该尽可能地避免依赖父组件的数据，更不应该主动修改它的数据，因为这样使得父子组件紧耦合，只看父组件，很难理解父组件的状态，因为它可能被任意组件修改，理想情况下，只有组件自己能修改他的状态，父子组件最好还是通过props和$emit来通信。

#### 3）子组件索引

​	当子组件较多时，通过this.$children来一一变量出我们需要的一个组件实例是比较困难的，尤其是组件动态渲染时，他的序列是不固定的，Vue提供了子组件索引的方法，用特殊的属性ref来为子组件指定一个索引名称。

```html
 <html>
<head>
    <script src='package2.64/dist/vue.js'></script>
</head>

<body>
    <div id='app_ref'>
        <button @click="handleRef">通过ref获取子组件实例</button>
        <ref-component ref="comA"></ref-component>
    </div>
</body>

</html>
<script>
      Vue.component('ref-component', {
        template: '<div>子组件</div>',
        data:function(){
            return {
                message:"子组件内容"
            }
        }
    })
    var app = new Vue({
        el: "#app_ref",
        methods: {
            handleRef: function(){
                var msg=this.$refs.comA.message;
                console.log(msg)
            }
        }
    })
</script>
```

​	在父组件模板中，子组件标签上使用ref指定一个名称，并在父组件内通过this.$refs来访问指定名称的子组件。$refs只有在组件渲染完成后才填充，并且它是非响应式的，它仅仅作为一个直接访问子组件的应急方案，应当避免在模板或计算属性中使用$refs。

## 7.4 使用slot分发内容

### 7.4.1 什么是slot

​	当需要让组件组合使用，混合父组件的内容与子组件的模板时，就会用到slot，这个过程叫做内容分发（transclusion）。以app为例子，他有两个特点:

- 组件不知道他的挂载点会有什么内容，挂载点的内容是由app的父组件决定的
- 组件很可能有他自己的模板。

​	props传递数据、events触发事件和solt内容分发就构成了Vue组件的三个API来源，再复杂的组件也是有这三个部分构成的。

## 7.4.2 作用域

​	父组件模板的内容时在父组件作用域内编译，子组件模板的内容是在子组件作用域内编译，例如下面的代码示例：

```html
 <html>
<head>
    <script src='package2.64/dist/vue.js'></script>
</head>

<body>
   <div id='app_area' v-show="showchild">
        <child-component></child-component>
    </div>
</body>

</html>
<script>
    Vue.component('child-component', {
        template: '<div>子组件</div>',
    })
  //这里的showchild绑定的事父组件的数据
    var app = new Vue({
        el: "#app_area",
        data: {
            showchild: true
        }
    })
    //如果想在子组件是哪个绑定 那应该是
    Vue.component('child-component', {
        template: '<div v-show="showchild">子组件</div>',
      	data: {
            showchild: true
        }
    })
  //slot分发的内容，作用域是在父组件上的
</script>
```

### 7.4.3 slot用法

#### 	单个slot

​	在子组件内使用特殊的slot元素就可以为这个子组件开启一个slot(插槽)，在父组件模板里，插入在子组件标签内的所有内容将替代子组件的slot标签及它的内容：

```html
<div id='app_slot'>
  <slot-component> 
    <p>分发的内容</p>
    <p>更多的分发内容</p>
  </slot-component>
</div>
<script>
      Vue.component('slot-component', {
        template: '\
        <div>\
            <slot>\
                <p>如果父组件没有插入内容，我将作为默认出现</p>\
            </slot>\
        </div>',
    });
    var appslot = new Vue({
        el: "#app_slot"
    })
</script>
<--!子组件slot-component的模板内定义了一个solt元素，并且用一个<p>作为默认内容，在父组件没有使用slot时，会渲染这段默认的文本；如果写入了slot，就会替换整个slot，所以以上渲染后的结果为：-->
  <div id='app_slot'>
  <div> 
    <p>分发的内容</p>
    <p>更多的分发内容</p>
  </div>
</div>
```

​	注意，子组件slot内的备用内容，他的作用域是子组件本身。

#### 具名Slot

​	给slot元素指定一个name后可以分发多个内容，具名Slot可以与单个Slot共存：

```vue
<div id='app_name_slot'>
  <nameslot-component> 
    <h2 slot="header">标题</h2>
    <p>分发的内容</p>
    <p>更多的分发内容</p>
    <div slot="footer">底部信息</div>
  </nameslot-component>
</div>
<scrip>
   Vue.component('nameslot-component', {
        template: '\
        <div class="container">\
            <div class="header">\
              <slot name="header"> </slot>\
            </div>\
            <div class="main">\
              <slot> </slot>\
            </div>\
            <div class="footer">\
              <slot name="footer"> </slot>\
            </div>\
        </div>',
    });
    var appNameSlot = new Vue({
        el: "#app_name_slot"
    })
</scrip>
```

​	子组件内声明了三个slot元素，其中在div class="main"内的slot没有name特性的，他将作为默认slot出现，父组件没有使用slot特性的元素与内容都将出现在这里，如果没有指定默认的匿名slot，父组件内多余的内容片段都将被抛弃。上例子最终渲染后的结果为：

```html
<div>
  <div class="container">
    <div class="header">
      <h2> 标题</h2>
    </div>
    <div class="main">
      <p>正文内容<p>
      <p>更多的正文内容<p>
    </div>
    <div class="footer">
      <div>
        底部信息
      </div>
    </div>
  </div>
</div>
```

​	在组合使用组件时，内容分发API至关重要。

### 7.4.4 作用域插槽

​	作用域插槽是一种特殊的slot，使用一个可以复用的模板替代已渲染元素。

```html
<div id='app_scope_slot'>
  <scopeslot-component> 
    <template scope="props">
      <p>来自父组件的内容</p>
      <p>{{props.msg}}</p>
     </template>
  </scopeslot-component>
</div>
<scrip>
      Vue.component('scopeslot-component',{
        template: '\
        <div class="container">\
            <slot msg="来自子组件的内容"></slot>\
        </div>'
    });
    var appScpeSlot=new Vue({
        el:"#app_scope_slot"
    })
</scrip>
```

​	观察子组件的模板，在slot元素上有一个类似props传递给组件的写法msg="xxx",将数据传到了插槽，父组件中使用了template元素，而且拥有一个scope="props"的特性，这里的props只是一个临时变量。template内可以通过它访问来自子组件插槽的数据msg。上面的示例最终渲染的结果为：

```html
<div class="app_scope_slot">
  <div class="container">
    <p>来自父组件的内容</p>
    <p>来自子组件的内容</p>
  </div>
</div>
```

​	作用域插槽更具代表性的用例是列表组件，允许组件自定义应该如何渲染列表的每一项。

```html
<div id='app_scope_list_slot'>
  <mylist-component :books="books"> 
    <!--作用域插槽也可以是具名的Slot-->
    <template scope="props" slot="book">
      <li>{{props.bookName}}</li>
    </template>
  </mylist-component>
</div>
<scrip>
   Vue.component('mylist-component',{
        props:{
            books:{
                type:Array,
                default:function(){
                    return [];
                }
            }
        },
        template:'\
        <ul>\
            <slot name="book" v-for="book in books"\
            :book-name="book.name">\
            </slot>\
            <!--这里也可以写默认slot内容-->\
        </ul>'
    })
    var mylist=new Vue({
        el:"#app_scope_list_slot",
        data:{
            books:[
                {name:"Vue实战"},
                {name:"Kubernetes入门到精通"},
                {name:"Go底层原理分析"},
                {name:"Docker源码解析"},
            ]
        }
    });
</scrip>
```

​	子组件mylist-component接收一个来自父级的props数组books，并且将他在name为book的slot上适应v-for指令循环，同时暴露一个变量bookName。

​	如果你仔细揣摩上面的用法，你可能会产生这样的疑问：我直接在父组件上使用v-for不就好了吗，为什么还要绕一步，在子组件里面循环呢？的确，如果知识针对上面的示例，这样写是多此一举的，此例子的主要目的是介绍作用域的用法，并没有加入使用场景，而作用域插槽的使用场景就是既可以复用子组件的slot，又可以使slot内容不一致，如果上例还在其他组件内使用，li的内容渲染权是由使用者掌握的，而数据却可以通过临时变量（比如props）从子组件内获取。

### 7.4.5 访问slot

​	$slots方法提供了访问被slot分发的内容的方法。

```html
<div id='app_visit_slot'>
  <visitslot-component> 
    <h2 slot="header">标题</h2>
    <p>正文内容</p>
    <p>正文其他内容</p>
    <div slot="footer">底部信息</div>
  </visitslot-component>
</div>  
<scrip>
     Vue.component('visitslot-component',{
        template:'\
        <div class="container">\
            <div class="header">\
                <slot name="header"></slot>\
            </div>\
            <div class="main">\
                <slot></slot>\
            </div>\
            <div class="footer">\
                <slot name="footer"></slot>\
            </div>\
        </div>',
        mounted:function(){
            var header=this.$slots.header;
            var main=this.$slots.default;
            var footer=this.$slots.footer;
            console.log(footer);
            console.log(footer[0].elm.innerHTML);
        }
    });
    var visitslot=new Vue({
        el:"#app_visit_slot"
    })
</scrip>
```

​	$slots在业务中几乎用不到，在用render函数创建组件时会比较有用，主要还是应用独立组件开发中。

## 7.5 组件的高级用法

## 7.5.1 递归组件

​	组件在他的模板内可以递归地调用自己，只要个组件设置name选项就可以了：

```html
<div id="recursion">
  <recursion-component :count="1"></recursion-component>
</div>
<scrip>
  Vue.component('recursion-component',{
          name:'recursion-component',
          props:{
              count:{
                  type:Number,
                  default:1
              }
          },
          template:'\
          <div class="recursion">\
              <recursion-component :count="count+1"\
                                  v-if="count<3"></recursion-component>\
          </div>',

      })
      var recursion=new Vue({
          el:"#recursion"
      })
</scrip>
```

​	设置name后，在组件模板内就可以递归调用了，不过需要注意的事，必须给一个条件来限制递归数量，否则会抛出错误：max stack size exceeded。

​	组件递归使用可以用来开发一些具有未知层级关系的独立组件，比如级联选择器和树形控件等。

## 7.5.2 内联模板

​	组件的模板一般都是在template选项内定义的，Vue提供了一个内联模板的功能，在使用组件时，给组件标签使用inline-template特性，组件就会把它的内容当做模板，而不是把它当做内容分发，这让模板更灵活

```Vue
<div id="recursion">
  <child-component inline-template>
  	<div>
      <h2>在父组件中定义子组件的模板</h2>
      <p>{{message}}</p>
      <p>{{msg}}</p>
    </div>
  </child-component>
</div>
<scrip>
  Vue.component('child-component',{
          data:function(){
              return:{
                  msg:’在子组件中声明的数据‘,
                  default:1
              }
          }

      })
      var recursion=new Vue({
          el:"#recursion"，
  				data:{
  				message:'在父组件中声明的数据'
  }
      })
</scrip>
```

​	在父组件中声明的数据message和子组件声明的数据msg，两个都可以渲染(如果同名。有限使用子组件的数据)，这反而是内联模板的缺点，就是作用域比较难理解，如果不是非常特殊的场景建议不要使用内联模板。

## 7.5.3 动态组件

## 7.5.4 异步组件

# 八 自定义指令

# Render函数

# 使用webpack

# 插件

# iView经典组件剖析
