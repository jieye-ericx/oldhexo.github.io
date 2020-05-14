---
toc: true
title: Vue 笔记
categories: 
 - 笔记
tags:
 - 大前端
 - Vue
---

之前学习很有问题，只看视频不记笔记，直接把他人的笔记复制来也不整理，痛定思痛，特整理如下。
<!-- more -->
## Vue 是什么？

- **Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的渐进式框架**

+ Vue.js 是目前最火的一个前端框架，React是最流行的一个前端框架（React除了开发网站，还可以开发手机App， Vue语法也是可以用于进行手机App开发的，需要借助于Weex）

+ Vue.js 是前端的**主流框架之一**，和Angular.js、React.js 一起，并成为前端三大主流框架！

+ Vue.js 是一套构建用户界面的框架，**只关注视图层**，它不仅易于上手，还便于与第三方库或既有项目整合。（Vue有配套的第三方类库，可以整合起来做大型项目的开发）

+ 前端的主要工作？主要负责MVC中的V这一层；主要工作就是和界面打交道，来制作前端页面效果；

## 为什么要学习流行框架

 + 企业为了提高开发效率：在企业中，时间就是效率，效率就是金钱；

  - 企业中，使用框架，能够提高开发的效率；

 + 提高开发效率的发展历程：原生JS -> Jquery之类的类库 -> 前端模板引擎 -> Angular.js / Vue.js（能够帮助我们减少不必要的DOM操作；提高渲染效率；双向数据绑定的概念【通过框架提供的指令，我们前端程序员只需要关心数据的业务逻辑，不再关心DOM是如何渲染的了】）
 + 在Vue中，一个核心的概念，就是让用户不再操作DOM元素，解放了用户的双手，让程序员可以更多的时间去关注业务逻辑；

 + 增强自己就业时候的竞争力

  - 人无我有，人有我优
  - 你平时不忙的时候，都在干嘛？

## 框架和库的区别

 + 框架：是一套完整的解决方案；对项目的侵入性较大，项目如果需要更换框架，则需要重新架构整个项目。

  - node 中的 express；

 + 库（插件）：提供某一个小功能，对项目的侵入性较小，如果某个库无法完成某些需求，可以很容易切换到其它库实现需求。

  - 1. 从Jquery 切换到 Zepto
  - 2. 从 EJS 切换到 art-template

## Node（后端）中的 MVC 与 前端中的 MVVM 之间的区别

 + MVC 是后端的分层开发概念；
 + MVVM是前端视图层的概念，主要关注于 视图层分离，也就是说：MVVM把前端的视图层，分为了 三部分 Model, View , VM ViewModel

 + 为什么有了MVC还要有MVVM

## Vue.js 基本代码 和 MVVM 之间的对应关系

![01.MVC和MVVM的关系图解](/images/vue.assets/01.MVC和MVVM的关系图解-3997941.png)

## 使用Vue将helloworld  渲染到页面上 

<img  src="17-21 Vue.js项目实战开发/17-19vue基础/day01/4-笔记/images/day01-1.png"  >

##  指令

- 本质就是自定义属性
- Vue中指定都是以 v- 开头 

###  v-cloak

防止页面加载时出现闪烁问题

v-cloak指令的用法

1. 提供样式

*[v-cloak]{*

display: none;

}

2. 在插值表达式所在的标签中添加v-cloak指令

 *背后的原理：先通过样式隐藏内容，然后在内存中进行值的替换，替换好之后再显示最终的结果*

```html
 <style type="text/css">
  /* 
    1、通过属性选择器 选择到 带有属性 v-cloak的标签  让他隐藏
 */
  [v-cloak]{
    /* 元素隐藏    */
    display: none;
  }
  </style>
<body>
  <div id="app">
    <!-- 2、 让带有插值语法的添加 v-cloak 属性 
         在 数据渲染完场之后，v-cloak 属性会被自动去除，
         v-cloak一旦移除也就是没有这个属性了  属性选择器就选择不到该标签
				也就是对应的标签会变为可见
    -->
    <div v-cloak>{{msg}}</div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    var vm = new Vue({
      //  el指定元素 id 是 app 的元素  
      el: '#app',
      //  data  里面存储的是数据
      data: {
        msg: 'Hello Vue'
      }
    });
</script>
</body>
</html>
```

###  v-text

- v-text指令用于将数据填充到标签中，作用于插值表达式类似，但是没有闪动问题
- 如果数据中有HTML标签会将html标签一并输出
- 注意：此处为单向绑定，数据对象上的值改变，插值会发生变化；但是当插值发生变化并不会影响数据对象的值。

```html
<div id="app">
    <!--  
		注意:在指令中不要写插值语法  直接写对应的变量名称 
        在 v-text 中 赋值的时候不要在写 插值语法
		一般属性中不加 {{}}  直接写 对应 的数据名 
	-->
    <p v-text="msg"></p>
    <p>
        <!-- Vue  中只有在标签的 内容中 才用插值语法 -->
        {{msg}}
    </p>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            msg: 'Hello Vue.js'
        }
    });

</script>
```

###  v-html

- 用法和v-text 相似  但是他可以将HTML片段填充到标签中

- 可能有安全问题, 一般只在可信任内容上使用 `v-html`，**永不**用在用户提交的内容上

- 它与v-text区别在于v-text输出的是纯文本，浏览器不会对其再进行html解析，但v-html会将其当html标签解析后输出。

  ```html
  <div id="app">
  　　<p v-html="html"></p> <!-- 输出：html标签在渲染的时候被解析 -->
      
      <p>{{message}}</p> <!-- 输出：<span>通过双括号绑定</span> -->
      
  　　<p v-text="text"></p> <!-- 输出：<span>html标签在渲染的时候被源码输出</span> -->
  </div>
  <script>
  　　let app = new Vue({
  　　el: "#app",
  　　data: {
  　　　　message: "<span>通过双括号绑定</span>",
  　　　　html: "<span>html标签在渲染的时候被解析</span>",
  　　　　text: "<span>html标签在渲染的时候被源码输出</span>",
  　　}
   });
  </script>
  ```

###  v-pre

- 显示原始信息跳过编译过程
- 跳过这个元素和它的子元素的编译过程。
- **一些静态的内容不需要编译加这个指令可以加快渲染**

```html
    <span v-pre>{{ this will not be compiled }}</span>    
	<!--  显示的是{{ this will not be compiled }}  -->
	<span v-pre>{{msg}}</span>  
     <!--   即使data里面定义了msg这里仍然是显示的{{msg}}  -->
<script>
    new Vue({
        el: '#app',
        data: {
            msg: 'Hello Vue.js'
        }
    });
</script>
```

### text html pre比较

![image-20200312153450168](/images/vue.assets/image-20200312153450168.png)

v-text指令用于将数据填充到标签中，作用于插值表达式类似，但是没有闪动问题
v-html指令用于将HTML片段填充到标签中，但是可能有安全问题
v-pre用于显示原始信息

### **v-once**

- 执行一次性的插值【当数据改变时，插值处的内容不会继续更新】

```html
  <!-- 即使data里面定义了msg 后期我们修改了 仍然显示的是第一次data里面存储的数据即 Hello Vue.js  -->
     <span v-once>{{ msg}}</span>    
<script>
    new Vue({
        el: '#app',
        data: {
            msg: 'Hello Vue.js'
        }
    });
</script>
```

### 双向数据绑定

- 当数据发生变化的时候，视图也就发生变化
- 当视图发生变化的时候，数据也会跟着同步变化

#### v-model

- **v-model**是一个指令，限制在 `<input>、<select>、<textarea>、components`中使用

```html
 <div id="app">
      <div>{{msg}}</div>
      <div>
          当输入框中内容改变的时候，  页面上的msg  会自动更新
        <input type="text" v-model='msg'>
      </div>
  </div>
```

###   v-on

- 用来绑定事件的
- 形式如：v-on:click  缩写为 @click;

<img src="17-21 Vue.js项目实战开发/17-19vue基础/day01/4-笔记/images/@click.png"  width="90%">

###  v-on事件函数中传入参数

```html
<body>
<div id="app">
    <div>{{num}}</div>
    <div>
        <!-- 如果事件直接绑定函数名称，那么默认会传递事件对象作为事件函数的第一个参数 -->
        <button v-on:click='handle1'>点击1</button>
        <!-- 2、如果事件绑定函数调用，那么事件对象必须作为最后一个参数显示传递，
             并且事件对象的名称必须是$event 
        -->
        <button v-on:click='handle2(123, 456, $event)'>点击2</button>
    </div>
</div>
<script type="text/javascript" src="js/vue.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el: '#app',
        data: {
            num: 0
        },
        methods: {
            handle1: function(event) {
                console.log(event.target.innerHTML)
            },
            handle2: function(p, p1, event) {
                console.log(p, p1)
                console.log(event.target.innerHTML)
                this.num++;
            }
        }
    });
</script>
```

### v-bind

- v-bind 指令被用来响应地更新HTML属性
- v-bind:href 可以缩写为 :href;

```html
<!-- 绑定一个属性 -->
<img v-bind:src="imageSrc">

<!-- 缩写 -->
<img :src="imageSrc">
```

#### 绑定对象

- 我们可以给`v-bind:class` 一个对象，以动态地切换class。
- 注意：`v-bind:class`指令可以与普通的class特性共存

```html
1、 v-bind 中支持绑定一个对象 
	如果绑定的是一个对象 则 键为 对应的类名  值 为对应data中的数据 
<!-- 
	HTML最终渲染为 <ul class="box textColor textSize"></ul>
	注意：
		textColor，textSize  对应的渲染到页面上的CSS类名	
		isColor，isSize  对应vue data中的数据  如果为true 则对应的类名 渲染到页面上 
		当 isColor 和 isSize 变化时，class列表将相应的更新，
		例如，将isSize改成false，
		class列表将变为 <ul class="box textColor"></ul>
-->

<ul class="box" v-bind:class="{textColor:isColor, textSize:isSize}">
    <li>学习Vue</li>
    <li>学习Node</li>
    <li>学习React</li>
</ul>
  <div v-bind:style="{color:activeColor,fontSize:activeSize}">对象语法</div>

<sript>
var vm= new Vue({
    el:'.box',
    data:{
        isColor:true,
        isSize:true，
    		activeColor:"red",
        activeSize:"25px",
    }
})
</sript>
<style>

    .box{
        border:1px dashed #f0f;
    }
    .textColor{
        color:#f00;
        background-color:#eef;
    }
    .textSize{
        font-size:30px;
        font-weight:bold;
    }
</style>
```

####  绑定class

```html
2、  v-bind 中支持绑定一个数组    数组中classA和 classB 对应为data中的数据

这里的classA  对用data 中的  classA
这里的classB  对用data 中的  classB
<ul class="box" :class="[classA, classB]">
    <li>学习Vue</li>
    <li>学习Node</li>
    <li>学习React</li>
</ul>
<script>
var vm= new Vue({
    el:'.box',
    data:{
        classA:‘textColor‘,
        classB:‘textSize‘
    }
})
</script>
<style>
    .box{
        border:1px dashed #f0f;
    }
    .textColor{
        color:#f00;
        background-color:#eef;
    }
    .textSize{
        font-size:30px;
        font-weight:bold;
    }
</style>
```

#### 绑定对象和绑定数组 的区别

- 绑定对象的时候 对象的属性 即要渲染的类名 对象的属性值对应的是 data 中的数据 
- 绑定数组的时候数组里面存的是data 中的数据 

#### 绑定style

```html
 <div v-bind:style="styleObject">绑定样式对象</div>'
 
<!-- CSS 属性名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用单引号括起来)    -->
 <div v-bind:style="{ color: activeColor, fontSize: fontSize,background:'red' }">内联样式</div>

<!--组语法可以将多个样式对象应用到同一个元素 -->
<div v-bind:style="[styleObj1, styleObj2]"></div>

<script>
	new Vue({
      el: '#app',
      data: {
        styleObject: {
          color: 'green',
          fontSize: '30px',
          background:'red'
        }，
        activeColor: 'green',
   		fontSize: "30px"
      },
      styleObj1: {
             color: 'red'
       },
       styleObj2: {
            fontSize: '30px'
       }

</script>
```

### v-if判断

#### v-if 使用场景

- 1- 多个元素 通过条件判断展示或者隐藏某个元素。或者多个元素
- 2- 进行两个视图之间的切换

```html
<div id="app">
        <!--  判断是否加载，如果为真，就加载，否则不加载-->
        <span v-if="flag">
           如果flag为true则显示,false不显示!
        </span>
</div>

<script>
    var vm = new Vue({
        el:"#app",
        data:{
            flag:true
        }
    })
</script>

----------------------------------------------------------

    <div v-if="type === 'A'">
       A
    </div>
  <!-- v-else-if紧跟在v-if或v-else-if之后   表示v-if条件不成立时执行-->
    <div v-else-if="type === 'B'">
       B
    </div>
    <div v-else-if="type === 'C'">
       C
    </div>
  <!-- v-else紧跟在v-if或v-else-if之后-->
    <div v-else>
       Not A/B/C
    </div>

<script>
    new Vue({
      el: '#app',
      data: {
        type: 'C'
      }
    })
</script>
```

#### v-show 和 v-if的区别

- v-show本质就是标签display设置为none，控制隐藏
  - v-show只编译一次，后面其实就是控制css，而v-if不停的销毁和创建，故v-show性能更好一点。
- v-if是动态的向DOM树内添加或者删除DOM元素
  - v-if切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件

### v-for循环结构

用于循环的数组里面的值可以是对象，也可以是普通元素  

```html
<ul id="example-1">
   <!-- 循环结构-遍历数组  
	item 是我们自己定义的一个名字  代表数组里面的每一项  
	items对应的是 data中的数组-->
  <li v-for="item in items">
    {{ item.message }}
  </li> 

</ul>
<script>
 new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]，
   
  }
})
</script>
```

- **不推荐**同时使用 `v-if` 和 `v-for`
- 当 `v-if` 与 `v-for` 一起使用时，`v-for` 具有比 `v-if` 更高的优先级。

```html
   <!--  循环结构-遍历对象
		v 代表   对象的value
		k  代表对象的 键 
		i  代表索引	
	---> 
     <div v-if='v==13' v-for='(v,k,i) in obj'>{{v + '---' + k + '---' + i}}</div>

<script>
 new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]，
    obj: {
        uname: 'zhangsan',
        age: 13,
        gender: 'female'
    }
  }
})
</script>
```

- key 的作用
  - **key来给每个节点做一个唯一标识**
  - **key的作用主要是为了高效的更新虚拟DOM**

```html
<ul>
  <li v-for="item in items" :key="item.id">...</li>
</ul>

```



### 案例选项卡

#### 1、 HTML 结构

```html
`
    <div id="app">
        <div class="tab">
            <!--  tab栏  -->
            <ul>
                <li class="active">apple</li>
                <li class="">orange</li>
                <li class="">lemon</li>
            </ul>
              <!--  对应显示的图片 -->
            <div class="current"><img src="img/apple.png"></div>
            <div class=""><img src="img/orange.png"></div>
            <div class=""><img src="img/lemon.png"></div>
        </div>
    </div>


`
```

#### 2、 提供的数据

```js
         list: [{
                    id: 1,
                    title: 'apple',
                    path: 'img/apple.png'
                }, {
                    id: 2,
                    title: 'orange',
                    path: 'img/orange.png'
                }, {
                    id: 3,
                    title: 'lemon',
                    path: 'img/lemon.png'
                }]
```



#### 3、 把数据渲染到页面

- 把tab栏 中的数替换到页面上

  - 把 data 中 title  利用 v-for 循环渲染到页面上 
  - 把 data 中 path利用 v-for 循环渲染到页面上 

  ```html
      <div id="app">
          <div class="tab">  
              <ul>
                    <!--  
                      1、绑定key的作用 提高Vue的性能 
                      2、 key 需要是唯一的标识 所以需要使用id， 也可以使用index ，
  						index 也是唯一的 
                      3、 item 是 数组中对应的每一项  
                      4、 index 是 每一项的 索引
                  -->
                     <li :key='item.id' v-for='(item,index) in list'>{{item.title}}</li>
                </ul>
                <div  :key='item.id' v-for='(item, index) in list'>
                      <!-- :  是 v-bind 的简写   绑定属性使用 v-bind -->
                      <img :src="item.path">
                </div>
          </div>
      </div>
  <script>
      new  Vue({
          //  指定 操作元素 是 id 为app 的 
          el: '#app',
              data: {
                  list: [{
                      id: 1,
                      title: 'apple',
                      path: 'img/apple.png'
                  }, {
                      id: 2,
                      title: 'orange',
                      path: 'img/orange.png'
                  }, {
                      id: 3,
                      title: 'lemon',
                      path: 'img/lemon.png'
                  }]
              }
      })
  
  </script>
  ```

  

#### 4、 给每一个tab栏添加事件,并让选中的高亮

- 4.1 、让默认的第一项tab栏高亮

  - tab栏高亮 通过添加类名active 来实现   （CSS  active 的样式已经提前写好）
    - 在data 中定义一个 默认的  索引 currentIndex  为  0 
    - 给第一个li 添加 active 的类名  
      - 通过动态绑定class 来实现   第一个li 的索引为 0     和 currentIndex   的值刚好相等
      - currentIndex     ===  index  如果相等  则添加类名 active  否则 添加 空类名

- 4.2 、让默认的第一项tab栏对应的div 显示 

  - 实现思路 和 第一个 tab 实现思路一样  只不过 这里控制第一个div 显示的类名是 current

  ```html
    <ul>
  	   <!-- 动态绑定class   有 active   类名高亮  无 active   不高亮-->
         <li  :class='currentIndex==index?"active":""'
             :key='item.id' v-for='(item,index) in list'
             >{{item.title}}</li>
    </ul>
  	<!-- 动态绑定class   有 current  类名显示  无 current  隐藏-->
    <div :class='currentIndex==index?"current":""' 
         
         :key='item.id' v-for='(item, index) in list'>
          <!-- :  是 v-bind 的简写   绑定属性使用 v-bind -->
          <img :src="item.path">
    </div>
  
  <script>
      new  Vue({
          el: '#app',
              data: {
                  currentIndex: 0, // 选项卡当前的索引  默认为 0  
                  list: [{
                      id: 1,
                      title: 'apple',
                      path: 'img/apple.png'
                  }, {
                      id: 2,
                      title: 'orange',
                      path: 'img/orange.png'
                  }, {
                      id: 3,
                      title: 'lemon',
                      path: 'img/lemon.png'
                  }]
              }
      })
  
  </script>
  ```

- 4.3 、点击每一个tab栏 当前的高亮 其他的取消高亮 

  - 给每一个li添加点击事件    

  - 让当前的索引 index  和  当前 currentIndex 的  值 进项比较 

  - 如果相等 则当前li  添加active 类名 当前的 li 高亮  当前对应索引的 div 添加 current 当前div 显示 其他隐藏

    ```html
        <div id="app">
            <div class="tab">
                <ul>
                    <!--  通过v-on 添加点击事件   需要把当前li 的索引传过去 
    				-->
                    <li v-on:click='change(index)'		           			
                        :class='currentIndex==index?"active":""'                   
                        :key='item.id' 
                        v-for='(item,index) in list'>{{item.title}}</li>
                </ul>
                <div :class='currentIndex==index?"current":""' 
                     :key='item.id' v-for='(item, index) in list'>
                    <img :src="item.path">
                </div>
            </div>
        </div>
    
    <script>
        new  Vue({
            el: '#app',
                data: {
                    currentIndex: 0, // 选项卡当前的索引  默认为 0  
                    list: [{
                        id: 1,
                        title: 'apple',
                        path: 'img/apple.png'
                    }, {
                        id: 2,
                        title: 'orange',
                        path: 'img/orange.png'
                    }, {
                        id: 3,
                        title: 'lemon',
                        path: 'img/lemon.png'
                    }]
                },
                methods: {
                    change: function(index) {
                        // 通过传入过来的索引来让当前的  currentIndex  和点击的index 值 相等 
                        //  从而实现 控制类名    
                        this.currentIndex = index;
                    }
                }
        
        })
    
    </script>
    ```


## Vue选项

### 表单基本操作

- 获取单选框中的值

  - 通过v-model

  ```html
   	<!-- 
  		1、 两个单选框需要同时通过v-model 双向绑定 一个值 
          2、 每一个单选框必须要有value属性  且value 值不能一样 
  		3、 当某一个单选框选中的时候 v-model  会将当前的 value值 改变 data 中的 数据
  		gender 的值就是选中的值，我们只需要实时监控他的值就可以了
  	-->
     <input type="radio" id="male" value="1" v-model='gender'>
     <label for="male">男</label>
  
     <input type="radio" id="female" value="2" v-model='gender'>
     <label for="female">女</label>
  
  <script>
      new Vue({
           data: {
               // 默认会让当前的 value 值为 2 的单选框选中
                  gender: 2,  
              },
      })
  
  </script>
  ```

- 获取复选框中的值

  - 通过v-model
  - 和获取单选框中的值一样 
  - 复选框 `checkbox` 这种的组合时   data 中的 hobby 我们要定义成数组 否则无法实现多选

  ```html
  	<!-- 
  		1、 复选框需要同时通过v-model 双向绑定 一个值 
          2、 每一个复选框必须要有value属性  且value 值不能一样 
  		3、 当某一个单选框选中的时候 v-model  会将当前的 value值 改变 data 中的 数据
  
  		hobby 的值就是选中的值，我们只需要实时监控他的值就可以了
  	-->
  
  <div>
     <span>爱好：</span>
     <input type="checkbox" id="ball" value="1" v-model='hobby'>
     <label for="ball">篮球</label>
     <input type="checkbox" id="sing" value="2" v-model='hobby'>
     <label for="sing">唱歌</label>
     <input type="checkbox" id="code" value="3" v-model='hobby'>
     <label for="code">写代码</label>
   </div>
  <script>
      new Vue({
           data: {
                  // 默认会让当前的 value 值为 2 和 3 的复选框选中
                  hobby: ['2', '3'],
              },
      })
  </script>
  ```

- #### 获取下拉框和文本框中的值

  - 通过v-model

  ```html
     <div>
        <span>职业：</span>
         <!--
  			1、 需要给select  通过v-model 双向绑定 一个值 
              2、 每一个option  必须要有value属性  且value 值不能一样 
  		    3、 当某一个option选中的时候 v-model  会将当前的 value值 改变 data 中的 数据
  		     occupation 的值就是选中的值，我们只需要实时监控他的值就可以了
  		-->
         <!-- multiple  多选 -->
        <select v-model='occupation' multiple>
            <option value="0">请选择职业...</option>
            <option value="1">教师</option>
            <option value="2">软件工程师</option>
            <option value="3">律师</option>
        </select>
           <!-- textarea 是 一个双标签   不需要绑定value 属性的  -->
          <textarea v-model='desc'></textarea>
    </div>
  <script>
      new Vue({
           data: {
                  // 默认会让当前的 value 值为 2 和 3 的下拉框选中
                   occupation: ['2', '3'],
               	 desc: 'nihao'
              },
      })
  </script>
  ```

### 表单修饰符

- .number 转换为数值

  ![image-20200329173658594](/images/vue.assets/image-20200329173658594.png)

  + 如果你先输入数字，那它就会限制你输入的只能是数字。

  + 如果你先输入字符串，那它就相当于没有加.number

- .trim  自动过滤用户输入的首尾空白字符

  - 只能去掉首尾的 不能去除中间的空格

- .lazy   将input事件切换成change事件

  - .lazy 修饰符延迟了同步更新属性值的时机。即将原本绑定在 input 事件的同步逻辑转变为绑定在 change 事件上,在失去焦点 或者 按下回车键时才更新。

  ```html
  <!-- 自动将用户的输入值转为数值类型 -->
  <input v-model.number="age" type="number">
  
  <!--自动过滤用户输入的首尾空白字符   -->
  <input v-model.trim="msg">
  
  <!-- 在“change”时而非“input”时更新 -->
  <input v-model.lazy="msg" >
  ```

### v-bind修饰符

- **.sync**(2.3.0+ 新增)

在有些情况下，我们可能需要对一个 prop 进行“双向绑定”。不幸的是，真正的双向绑定会带来维护上的问题，因为子组件可以修改父组件，且在父组件和子组件都没有明显的改动来源。我们通常的做法是

```html
//父亲组件
<comp :myMessage="bar" @update:myMessage="func"></comp>
//js
func(e){
 this.bar = e;
}
//子组件js
func2(){
  this.$emit('update:myMessage',params);
}
```

现在这个.sync修饰符就是简化了上面的步骤

```html
//父组件
<comp :myMessage.sync="bar"></comp> 
//子组件
this.$emit('update:myMessage',params);
```

这样确实会方便很多，但是也有很多需要**注意**的点

1. 使用sync的时候，子组件传递的事件名必须为update:value，其中value必须与子组件中props中声明的名称完全一致(如上例中的myMessage，不能使用my-message)
2. 注意带有 .sync 修饰符的 v-bind 不能和表达式一起使用 (例如 v-bind:title.sync=”doc.title + ‘!’” 是无效的)。取而代之的是，你只能提供你想要绑定的属性名，类似 v-model。
3. 将 v-bind.sync 用在一个字面量的对象上，例如 v-bind.sync=”{ title: doc.title }”，是无法正常工作的，因为在解析一个像这样的复杂表达式的时候，有很多边缘情况需要考虑。

- **.prop**

要学习这个修饰符，我们首先要搞懂两个东西的区别。

```
Property：节点对象在内存中存储的属性，可以访问和设置。
Attribute：节点对象的其中一个属性( property )，值是一个对象。
可以通过点访问法 document.getElementById('xx').attributes 或者 document.getElementById('xx').getAttributes('xx') 读取，通过 document.getElementById('xx').setAttribute('xx',value) 新增和修改。
在标签里定义的所有属性包括 HTML 属性和自定义属性都会在 attributes 对象里以键值对的方式存在。
```

其实attribute和property两个单词，翻译出来都是属性，但是《javascript高级程序设计》将它们翻译为特性和属性，以示区分

```
//这里的id,value,style都属于property
//index属于attribute
//id、title等既是属性，也是特性。修改属性，其对应的特性会发生改变；修改特性，属性也会改变
<input id="uid" title="title1" value="1" :index="index">
//input.index === undefined
//input.attributes.index === this.index
```

从上面我们可以看到如果直接使用v-bind绑定，则默认会绑定到dom节点的attribute。
为了

- 通过自定义属性存储变量，避免暴露数据
- 防止污染 HTML 结构

我们可以使用这个修饰符，如下

```
<input id="uid" title="title1" value="1" :index.prop="index">
//input.index === this.index
//input.attributes.index === undefined
```

- **.camel**

由于HTML 特性是不区分大小写的。

```
<svg :viewBox="viewBox"></svg>
```

实际上会渲染为

```
<svg viewbox="viewBox"></svg>
```

这将导致渲染失败，因为 SVG 标签只认 viewBox，却不知道 viewbox 是什么。
如果我们使用.camel修饰符，那它就会被渲染为驼峰名。
另，如果你使用字符串模版，则没有这些限制。

```javascript
new Vue({
  template: '<svg :viewBox="viewBox"></svg>'
})
```

###  事件修饰符/v-on

在事件处理程序中调用 `event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的需求。

Vue 不推荐我们操作DOM，为了解决这个问题，Vue.js 为 `v-on` 提供了**事件修饰符**

- 修饰符是由点开头的指令后缀来表示的

- .stop       阻止冒泡

  一键阻止事件冒泡，简直方便得不行。相当于调用了event.stopPropagation()方法。

- .prevent    用于阻止事件的默认行为，例如，当点击提交按钮时阻止对表单的提交。相当于调用了event.preventDefault()方法。

- .capture    从上面我们知道了事件的冒泡，其实完整的事件机制是：捕获阶段--目标阶段--冒泡阶段。
  默认的呢，是事件触发是从目标开始往上冒泡。
  当我们加了这个.capture以后呢，我们就反过来了，事件触发从包含这个元素的顶层开始往下触发。

  ```html
     <div @click.capture="shout(1)">
        obj1
        <div @click.capture="shout(2)">
          obj2
          <div @click="shout(3)">
            obj3
            <div @click="shout(4)">
              obj4
            </div>
          </div>
        </div>
      </div>
      // 1 2 4 3 
  ```

  从上面这个例子我们点击obj4的时候，就可以清楚地看出区别，obj1，obj2在捕获阶段就触发了事件，因此是先1后2，后面的obj3，obj4是默认的冒泡阶段触发，因此是先4然后冒泡到3~

- .self       只当事件是从事件绑定的元素本身触发时才触发回调。像下面所示，刚刚我们从.stop时候知道子元素会冒泡到父元素导致触发父元素的点击事件，当我们加了这个.self以后，我们点击button不会触发父元素的点击事件shout，只有当点击到父元素的时候（蓝色背景）才会shout~从这个self的英文翻译过来就是‘自己，本身’可以看出这个修饰符的用法

- .once       事件只触发一次

- **.passive**  当我们在监听元素滚动事件的时候，会一直触发onscroll事件，在pc端是没啥问题的，但是在移动端，会让我们的网页变卡，因此我们使用这个修饰符的时候，相当于给onscroll事件整了一个.lazy修饰符

```html
<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```

- **.native** 我们经常会写很多的小组件，有些小组件可能会绑定一些事件，但是，像下面这样绑定事件是不会触发的

```html
<My-component @click="shout(3)"></My-component>
```

必须使用.native来修饰这个click事件（即<My-component @click.native="shout(3)">\</My-component>），可以理解为该修饰符的作用就是把一个vue组件转化为一个普通的HTML标签，
注意：**使用.native修饰符来操作普通HTML标签是会令事件失效的**

**注意：**修饰符可以同时使用多个,但是可能会因为顺序而有所不同。
用 v-on:click.prevent.self 会阻止所有的点击，而 v-on:click.self.prevent 只会阻止对元素自身的点击。
也就是**从左往右判断~**

```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联，即阻止冒泡也阻止默认事件 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
```

使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 v-on:click.prevent.self 会阻止所有的点击，而 v-on:click.self.prevent 只会阻止对元素自身的点击。

### 鼠标按钮修饰符

刚刚我们讲到这个click事件，我们一般是会用左键触发，有时候我们需要更改右键菜单啥的，就需要用到右键点击或者中间键点击，这个时候就要用到鼠标按钮修饰符

- .left 左键点击
- .right 右键点击
- .middle 中键点击

```html
<button @click.right="shout(1)">ok</button>
```

### 按键修饰符

在做项目中有时会用到键盘事件，在监听键盘事件时，我们经常需要检查详细的按键。Vue 允许为 `v-on` 在监听键盘事件时添加按键修饰符。

```html
<!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
<input v-on:keyup.13="submit">

<!-- -当点击enter 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit">

<!--当点击enter或者space时  时调用 `vm.alertMe()`   -->
<input type="text" v-on:keyup.enter.space="alertMe" >

<!--
常用的按键修饰符
.enter =>    enter键
.tab => tab键
.delete (捕获“删除”和“退格”按键) =>  删除键
.esc => 取消键
.space =>  空格键
.up =>  上
.down =>  下
.left =>  左
.right =>  右

//系统修饰键
.ctrl
.alt
.meta
.shift
-->
<script>
	var vm = new Vue({
        el:"#app",
        methods: {
              submit:function(){},
              alertMe:function(){},
        }
    })

</script>
// 可以通过全局 config.keyCodes 对象自定义按键修饰符别名：
// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```

我们从上面看到，键分成了普通常用的键和系统修饰键，区别是什么呢？
当我们写如下代码的时候,我们会发现如果**仅仅**使用系统修饰键是无法触发keyup事件的。

```html
<input type="text" @keyup.ctrl="shout(4)">
```

那该如何呢？我们需要将系统修饰键和其他键码链接起来使用，比如

```html
<input type="text" @keyup.ctrl.67="shout(4)">
```

这样当我们同时按下ctrl+c时，就会触发keyup事件。
另，如果是鼠标事件，那就可以单独使用系统修饰符。

```html
      <button @mouseover.ctrl="shout(1)">ok</button>
      <button @mousedown.ctrl="shout(1)">ok</button>
      <button @click.ctrl.67="shout(1)">ok</button>
```

大概是什么意思呢，就是你不能**单手指使用系统修饰键的修饰符**（最少两个手指，可以多个）。你可以一个手指按住系统修饰键一个手指按住另外一个键来实现键盘事件。也可以用一个手指按住系统修饰键，另一只手按住鼠标来实现鼠标事件。

- **.exact** (2.5新增)

我们上面说了这个系统修饰键，当我们像这样<button type="text" @click.ctrl="shout(4)"></button>绑定了click键按下的事件，惊奇的是，我们同时按下几个系统修饰键，比如ctrl shift点击，也能触发，可能有些场景我们**只需要或者只能**按一个系统修饰键来触发（像制作一些快捷键的时候），而当我们按下ctrl和其他键的时候则无法触发。那就这样写。
注意：这个**只是限制系统修饰键**的，像下面这样书写以后你还是可以按下ctrl + c，ctrl+v或者ctrl+普通键 来触发，但是不能按下ctrl + shift +普通键来触发。

```html
<button type="text" @click.ctrl.exact="shout(4)">ok</button>
```

然后下面这个你可以同时按下enter+普通键来触发，但是不能按下系统修饰键+enter来触发。相信你已经能听懂了8~

```html
<input type="text" @keydown.enter.exact="shout('我被触发了')">
```

### 自定义按键修饰符别名

在Vue中可以通过`config.keyCodes`自定义按键修饰符别名

```html
<div id="app">
    <!-- 预先定义了keycode 116（即F5）的别名为f5，因此在文字输入框中按下F5，会触发prompt方法 -->
    <input type="text" v-on:keydown.f5="prompt()">
</div>
<script>
    Vue.config.keyCodes.f5 = 116;
    let app = new Vue({
        el: '#app',
        methods: {
            prompt: function() {
                alert('我是 F5！');
            }
        }
    });
</script>
```

###  自定义指令

- 内置指令不能满足我们特殊的需求
- Vue允许我们自定义指令

#### Vue.directive  注册全局指令

使用自定义的指令，只需在对用的元素中，加上'v-'的前缀形成类似于内部指令'v-if'，'v-text'的形式。

```html
<input type="text" v-focus>
<script>
// 注意点： 
//   1、 在自定义指令中  如果以驼峰命名的方式定义 如  Vue.directive('focusA',function(){}) 
//   2、 在HTML中使用的时候 只能通过 v-focus-a 来使用 
    
// 注册一个全局自定义指令 v-focus
Vue.directive('focus', {
  	// 当绑定元素插入到 DOM 中。 其中 el为dom元素
  	inserted: function (el) {
    		// 聚焦元素
    		el.focus();
 	}
});
new Vue({
　　el:'#app'
});
</script>
```

#### Vue.directive  注册全局指令 带参数

```html
  <input type="text" v-color='msg'>
 <script type="text/javascript">
    /*
      自定义指令-带参数
      bind - 只调用一次，在指令第一次绑定到元素上时候调用
    */
    Vue.directive('color', {
      // bind声明周期, 只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置
      // el 为当前自定义指令的DOM元素  
      // binding 为自定义的函数形参   通过自定义属性传递过来的值 存在 binding.value 里面
      bind: function(el, binding){
        // 根据指令的参数设置背景色
        // console.log(binding.value.color)
        el.style.backgroundColor = binding.value.color;
      }
    });
    var vm = new Vue({
      el: '#app',
      data: {
        msg: {
          color: 'blue'
        }
      }
    });
  </script>
```

#### 自定义指令局部指令

- 局部指令，需要定义在`directives` 的选项 用法和全局用法一样 
- 局部指令只能在当前组件里面使用
- 当全局指令和局部指令同名时以局部指令为准

```html
<input type="text" v-color='msg'>
 <input type="text" v-focus>
 <script type="text/javascript">
    /*
      自定义指令-局部指令
    */
    var vm = new Vue({
      el: '#app',
      data: {
        msg: {
          color: 'red'
        }
      },
   	  //局部指令，需要定义在  directives 的选项
      directives: {
        color: {
          bind: function(el, binding){
            el.style.backgroundColor = binding.value.color;
          }
        },
        focus: {
          inserted: function(el) {
            el.focus();
          }
        }
      }
    });
  </script>
```

###  计算属性 computed

- 模板中放入太多的逻辑会让模板过重且难以维护  使用计算属性可以让模板更加的简洁
- **计算属性是基于它们的响应式依赖进行缓存的**
- computed比较适合对多个变量或者对象进行处理后返回一个结果值，也就是数多个变量中的某一个值发生了变化则我们监控的这个值也就会发生变化

```html
 <div id="app">
     <!--  
        当多次调用 reverseString  的时候 
        只要里面的 num 值不改变 他会把第一次计算的结果直接返回
		直到data 中的num值改变 计算属性才会重新发生计算
     -->
    <div>{{reverseString}}</div>
    <div>{{reverseString}}</div>
     <!-- 调用methods中的方法的时候  他每次会重新调用 -->
    <div>{{reverseMessage()}}</div>
    <div>{{reverseMessage()}}</div>
  </div>
  <script type="text/javascript">
    /*
      计算属性与方法的区别:计算属性是基于依赖进行缓存的，而方法不缓存
    */
    var vm = new Vue({
      el: '#app',
      data: {
        msg: 'Nihao',
        num: 100
      },
      methods: {
        reverseMessage: function(){
          console.log('methods')
          return this.msg.split('').reverse().join('');
        }
      },
      //computed  属性 定义 和 data 已经 methods 平级 
      computed: {
        //  reverseString   这个是我们自己定义的名字 
        reverseString: function(){
          console.log('computed')
          var total = 0;
          //  当data 中的 num 的值改变的时候  reverseString  会自动发生计算  
          for(var i=0;i<=this.num;i++){
            total += i;
          }
          // 这里一定要有return 否则 调用 reverseString 的 时候无法拿到结果    
          return total;
        }
      }
    });
  </script>
```

你可能已经注意到我们可以通过在表达式中调用方法来达到同样的效果：

```javascript
<p>Reversed message: "{{ reversedMessage() }}"</p>
// 在组件中
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```

我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是**计算属性是基于它们的响应式依赖进行缓存的**。只在相关响应式依赖发生改变时它们才会重新求值。这就意味着只要 `message` 还没有发生改变，多次访问 `reversedMessage` 计算属性会立即返回之前的计算结果，而不必再次执行函数。

###  侦听器 watch

- 使用watch来响应数据的变化
- 一般用于异步或者开销较大的操作
- watch 中的属性 一定是data 中 已经存在的数据 
- **当需要监听一个对象的改变时，普通的watch方法无法监听到对象内部属性的改变，只有data中的数据才能够监听到变化，此时就需要deep属性对对象进行深度监听**

```html
 <div id="app">
        <div>
            <span>名：</span>
            <span>
        <input type="text" v-model='firstName'>
      </span>
        </div>
        <div>
            <span>姓：</span>
            <span>
        <input type="text" v-model='lastName'>
      </span>
        </div>
        <div>{{fullName}}</div>
    </div>

  <script type="text/javascript">
        /*
              侦听器
            */
        var vm = new Vue({
            el: '#app',
            data: {
                firstName: 'Jim',
                lastName: 'Green',
                // fullName: 'Jim Green'
            },
             //watch  属性 定义 和 data 已经 methods 平级 
            watch: {
                //   注意：  这里firstName  对应着data 中的 firstName 
                //   当 firstName 值 改变的时候  会自动触发 watch
                firstName: function(val) {
                    this.fullName = val + ' ' + this.lastName;
                },
                //   注意：  这里 lastName 对应着data 中的 lastName 
                lastName: function(val) {
                    this.fullName = this.firstName + ' ' + val;
                }
            }
        });
    </script>
```

虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。这就是为什么 Vue 通过 `watch` 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。

例如：

```html
<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
<!-- 因为 AJAX 库和通用工具的生态已经相当丰富，Vue 核心代码没有重复 -->
<!-- 提供这些功能以保持精简。这也可以让你自由选择自己更熟悉的工具。 -->
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
  created: function () {
    // `_.debounce` 是一个通过 Lodash 限制操作频率的函数。
    // 在这个例子中，我们希望限制访问 yesno.wtf/api 的频率
    // AJAX 请求直到用户输入完毕才会发出。想要了解更多关于
    // `_.debounce` 函数 (及其近亲 `_.throttle`) 的知识，
    // 请参考：https://lodash.com/docs#debounce
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  },
  methods: {
    getAnswer: function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Questions usually contain a question mark. ;-)'
        return
      }
      this.answer = 'Thinking...'
      var vm = this
      axios.get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = 'Error! Could not reach the API. ' + error
        })
    }
  }
})
</script>
```

结果：

Ask a yes/no question: 

I cannot give you an answer until you ask a question!

在这个示例中，使用 `watch` 选项允许我们执行异步操作 (访问一个 API)，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。这些都是计算属性无法做到的。

除了 `watch` 选项之外，您还可以使用命令式的 [vm.$watch API](https://cn.vuejs.org/v2/api/#vm-watch)。

### 计算属性 vs 侦听属性

Vue 提供了一种更通用的方式来观察和响应 Vue 实例上的数据变动：**侦听属性**。当你有一些数据需要随着其它数据变动而变动时，你很容易滥用 `watch`——特别是如果你之前使用过 AngularJS。然而，通常更好的做法是使用计算属性而不是命令式的 `watch` 回调。细想一下这个例子：

```javascript
<div id="demo">{{ fullName }}</div>
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```

上面代码是命令式且重复的。将它与计算属性的版本进行比较：

```javascript
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

好得多了，不是吗？

###  过滤器

- Vue.js允许自定义过滤器，可被用于一些常见的文本格式化。
- 过滤器可以用在两个地方：**双花括号插值和v-bind表达式。**
- 过滤器应该被添加在JavaScript表达式的尾部，由“管道”符号指示
- 支持级联操作
- 过滤器不改变真正的`data`，而只是改变渲染的结果，并返回过滤后的版本
- 全局注册时是filter，没有s的。而局部过滤器是filters，是有s的

```html
  <div id="app">
    <input type="text" v-model='msg'>
      <!-- upper 被定义为接收单个参数的过滤器函数，表达式  msg  的值将作为参数传入到函数中 -->
    <div>{{msg | upper}}</div>
    <!--  
      支持级联操作
      upper  被定义为接收单个参数的过滤器函数，表达式msg 的值将作为参数传入到函数中。
	  然后继续调用同样被定义为接收单个参数的过滤器 lower ，将upper 的结果传递到lower中
 	-->
    <div>{{msg | upper | lower}}</div>
    <div :abc='msg | upper'>测试数据</div>
  </div>

<script type="text/javascript">
   //  lower  为全局过滤器     
   Vue.filter('lower', function(val) {
      return val.charAt(0).toLowerCase() + val.slice(1);
    });
    var vm = new Vue({
      el: '#app',
      data: {
        msg: ''
      },
       //filters  属性 定义 和 data 已经 methods 平级 
       //  定义filters 中的过滤器为局部过滤器 
      filters: {
        //   upper  自定义的过滤器名字 
        //    upper 被定义为接收单个参数的过滤器函数，表达式  msg  的值将作为参数传入到函数中
        upper: function(val) {
         //  过滤器中一定要有返回值 这样外界使用过滤器的时候才能拿到结果
          return val.charAt(0).toUpperCase() + val.slice(1);
        }
      }
    });
  </script>
```

####  过滤器中传递参数

```html
    <div id="box">
        <!--
			filterA 被定义为接收三个参数的过滤器函数。
  			其中 message 的值作为第一个参数，
			普通字符串 'arg1' 作为第二个参数，表达式 arg2 的值作为第三个参数。
		-->
        {{ message | filterA('arg1', 'arg2') }}
    </div>
    <script>
        // 在过滤器中 第一个参数 对应的是  管道符前面的数据   n  此时对应 message
        // 第2个参数  a 对应 实参  arg1 字符串
        // 第3个参数  b 对应 实参  arg2 字符串
        Vue.filter('filterA',function(n,a,b){
            if(n<10){
                return n+a;
            }else{
                return n+b;
            }
        });
        
        new Vue({
            el:"#box",
            data:{
                message: "哈哈哈"
            }
        })
    </script>
```

### 生命周期

- 事物从出生到死亡的过程
- Vue实例从创建 到销毁的过程 ，这些过程中会伴随着一些函数的自调用。我们称这些函数为钩子函数

不要在选项属性或回调上使用[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，比如 `created: () => console.log(this.a)` 或 `vm.$watch('a', newValue => this.myMethod())`。因为箭头函数并没有 `this`，`this` 会作为变量一直向上级词法作用域查找，直至找到为止，经常导致 `Uncaught TypeError: Cannot read property of undefined` 或 `Uncaught TypeError: this.myMethod is not a function` 之类的错误。

####常用的 钩子函数

| beforeCreate  | 在实例初始化之后，数据观测和事件配置之前被调用 此时data 和 methods 以及页面的DOM结构都没有初始化   什么都做不了 |
| ------------- | ------------------------------------------------------------ |
| created       | 在实例创建完成后被立即调用此时data 和 methods已经可以使用  但是页面还没有渲染出来 |
| beforeMount   | 在挂载开始之前被调用   此时页面上还看不到真实数据 只是一个模板页面而已 |
| mounted       | el被新创建的vm.$el替换，并挂载到实例上去之后调用该钩子。  数据已经真实渲染到页面上  在这个钩子函数里面我们可以使用一些第三方的插件 |
| beforeUpdate  | 数据更新时调用，发生在虚拟DOM打补丁之前。   页面上数据还是旧的 |
| updated       | 由于数据更改导致的虚拟DOM重新渲染和打补丁，在这之后会调用该钩子。 页面上数据已经替换成最新的 |
| beforeDestroy | 实例销毁之前调用                                             |
| destroyed     | 实例销毁后调用                                               |

<img src="/images/vue.assets/lifecycle-20200312161816738.png" alt="Vue 实例生命周期" style="zoom:50%;" />

### 数组变异方法

- 在 Vue 中，直接修改对象属性的值无法触发响应式。当你直接修改了对象属性的值，你会发现，只有数据改了，但是页面内容并没有改变
- 变异数组方法即保持数组方法原有功能不变的前提下对其进行功能拓展

| `push()`    | 往数组最后面添加一个元素，成功返回当前数组的长度             |
| ----------- | ------------------------------------------------------------ |
| `pop()`     | 删除数组的最后一个元素，成功返回删除元素的值                 |
| `shift()`   | 删除数组的第一个元素，成功返回删除元素的值                   |
| `unshift()` | 往数组最前面添加一个元素，成功返回当前数组的长度             |
| `splice()`  | 有三个参数，第一个是想要删除的元素的下标（必选），第二个是想要删除的个数（必选），第三个是删除 后想要在原位置替换的值 |
| `sort()`    | sort()  使数组按照字符编码默认从小到大排序,成功返回排序后的数组 |
| `reverse()` | reverse()  将数组倒序，成功返回倒序后的数组                  |

### 替换数组

不会改变原始数组，但总是返回一个新数组

| filter | filter() 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。 |
| ------ | ------------------------------------------------------------ |
| concat | concat() 方法用于连接两个或多个数组。该方法不会改变现有的数组 |
| slice  | slice() 方法可从已有的数组中返回选定的元素。该方法并不会修改数组，而是返回一个子数组 |

### 动态数组响应式数据

- `Vue.set(a,b,c)`    让 触发视图重新更新一遍，数据动态起来
- a是要更改的数据 、   b是数据的第几项、   c是更改后的数据

###  图书列表案例

- 静态列表效果
- 基于数据实现模板效果
- 处理每行的操作按钮

#### 1、 提供的静态数据

- 数据存放在vue 中 data 属性中

```js
 var vm = new Vue({
      el: '#app',
      data: {
        books: [{
          id: 1,
          name: '三国演义',
          date: ''
        },{
          id: 2,
          name: '水浒传',
          date: ''
        },{
          id: 3,
          name: '红楼梦',
          date: ''
        },{
          id: 4,
          name: '西游记',
          date: ''
        }]
      }
    }); var vm = new Vue({
      el: '#app',
      data: {
        books: [{
          id: 1,
          name: '三国演义',
          date: ''
        },{
          id: 2,
          name: '水浒传',
          date: ''
        },{
          id: 3,
          name: '红楼梦',
          date: ''
        },{
          id: 4,
          name: '西游记',
          date: ''
        }]
      }
    });
```

#### 2、 把提供好的数据渲染到页面上

- 利用 v-for循环 遍历 books 将每一项数据渲染到对应的数据中

```html
 <tbody>
    <tr :key='item.id' v-for='item in books'>
       <!-- 对应的id 渲染到页面上 -->
       <td>{{item.id}}</td>
        <!-- 对应的name 渲染到页面上 -->
       <td>{{item.name}}</td>
       <td>{{item.date}}</td>
       <td>
         <!-- 阻止 a 标签的默认跳转 -->
         <a href="" @click.prevent>修改</a>
         <span>|</span>
       	  <a href="" @click.prevent>删除</a>
       </td>
     </tr>
</tbody>
```

#### 3、 添加图书

- 通过双向绑定获取到输入框中的输入内容 
- 给按钮添加点击事件 
- 把输入框中的数据存储到 data 中的 books  里面

```html
<div>
  <h1>图书管理</h1>
  <div class="book">
       <div>
         <label for="id">
           编号：
         </label>
          <!-- 3.1、通过双向绑定获取到输入框中的输入的 id  -->
         <input type="text" id="id" v-model='id'>
         <label for="name">
           名称：
         </label>
           <!-- 3.2、通过双向绑定获取到输入框中的输入的 name  -->
         <input type="text" id="name" v-model='name'>
            <!-- 3.3、给按钮添加点击事件  -->
         <button @click='handle'>提交</button>
       </div>
  </div>
</div>
  <script type="text/javascript">
    /*
      图书管理-添加图书
    */
    var vm = new Vue({
      el: '#app',
      data: {
        id: '',
        name: '',
        books: [{
          id: 1,
          name: '三国演义',
          date: ''
        },{
          id: 2,
          name: '水浒传',
          date: ''
        },{
          id: 3,
          name: '红楼梦',
          date: ''
        },{
          id: 4,
          name: '西游记',
          date: ''
        }]
      },
      methods: {
        handle: function(){
          // 3.4 定义一个新的对象book 存储 获取到输入框中 书 的id和名字 
          var book = {};
          book.id = this.id;
          book.name = this.name;
          book.date = '';
         // 3.5 把book  通过数组的变异方法 push 放到    books 里面
          this.books.push(book);
          //3.6 清空输入框
          this.id = '';
          this.name = '';
        }
      }
    });
  </script>
```

#### 4 修改图书-上 

- 点击修改按钮的时候 获取到要修改的书籍名单
  - 4.1  给修改按钮添加点击事件，  需要把当前的图书的id 传递过去 这样才知道需要修改的是哪一本书籍
- 把需要修改的书籍名单填充到表单里面
  - 4.2  根据传递过来的id 查出books 中 对应书籍的详细信息
  - 4.3 把获取到的信息填充到表单

```html
 <div id="app">
    <div class="grid">
      <div>
        <h1>图书管理</h1>
        <div class="book">
          <div>
            <label for="id">
              编号：
            </label>
            <input type="text" id="id" v-model='id' :disabled="flag">
            <label for="name">
              名称：
            </label>
            <input type="text" id="name" v-model='name'>
            <button @click='handle'>提交</button>
          </div>
        </div>
      </div>
      <table>
        <thead>
          <tr>
            <th>编号</th>
            <th>名称</th>
            <th>时间</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <tr :key='item.id' v-for='item in books'>
            <td>{{item.id}}</td>
            <td>{{item.name}}</td>
            <td>{{item.date}}</td>
            <td>
              <!--- 
				4.1  给修改按钮添加点击事件，  需要把当前的图书的id 传递过去 
				这样才知道需要修改的是哪一本书籍
  				--->  
              <a href="" @click.prevent='toEdit(item.id)'>修改</a>
              <span>|</span>
              <a href="" @click.prevent>删除</a>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
 <script type="text/javascript">
    /*
      图书管理-添加图书
    */
    var vm = new Vue({
      el: '#app',
      data: {
        flag: false,
        id: '',
        name: '',
        books: [{
          id: 1,
          name: '三国演义',
          date: ''
        },{
          id: 2,
          name: '水浒传',
          date: ''
        },{
          id: 3,
          name: '红楼梦',
          date: ''
        },{
          id: 4,
          name: '西游记',
          date: ''
        }]
      },
      methods: {
        handle: function(){
          // 3.4 定义一个新的对象book 存储 获取到输入框中 书 的id和名字 
          var book = {};
          book.id = this.id;
          book.name = this.name;
          book.date = '';
         // 3.5 把book  通过数组的变异方法 push 放到    books 里面
          this.books.push(book);
          //3.6 清空输入框
          this.id = '';
          this.name = '';
        },
        toEdit: function(id){
          console.log(id)
          //4.2  根据传递过来的id 查出books 中 对应书籍的详细信息
          var book = this.books.filter(function(item){
            return item.id == id;
          });
          console.log(book)
          //4.3 把获取到的信息填充到表单
          // this.id   和  this.name 通过双向绑定 绑定到了表单中  一旦数据改变视图自动更新
          this.id = book[0].id;
          this.name = book[0].name;
        }
      }
    });
  </script>
```

#### 5  修改图书-下

- 5.1  定义一个标识符， 主要是控制 编辑状态下当前编辑书籍的id 不能被修改 即 处于编辑状态下 当前控制书籍编号的输入框禁用  
- 5.2  通过属性绑定给书籍编号的 绑定 disabled 的属性  flag 为 true 即为禁用
- 5.3  flag 默认值为false   处于编辑状态 要把 flag 改为true 即当前表单为禁用 
- 5.4  复用添加方法   用户点击提交的时候依然执行 handle 中的逻辑如果 flag为true 即 表单处于不可输入状态 此时执行的用户编辑数据数据

```html
<div id="app">
    <div class="grid">
      <div>
        <h1>图书管理</h1>
        <div class="book">
          <div>
            <label for="id">
              编号：
            </label>
              <!-- 5.2 通过属性绑定 绑定 disabled 的属性  flag 为 true 即为禁用      -->
            <input type="text" id="id" v-model='id' :disabled="flag">
            <label for="name">
              名称：
            </label>
            <input type="text" id="name" v-model='name'>
            <button @click='handle'>提交</button>
          </div>
        </div>
      </div>
      <table>
        <thead>
          <tr>
            <th>编号</th>
            <th>名称</th>
            <th>时间</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <tr :key='item.id' v-for='item in books'>
            <td>{{item.id}}</td>
            <td>{{item.name}}</td>
            <td>{{item.date}}</td>
            <td>
              <a href="" @click.prevent='toEdit(item.id)'>修改</a>
              <span>|</span>
              <a href="" @click.prevent>删除</a>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>   
<script type="text/javascript">
        /*图书管理-添加图书*/
        var vm = new Vue({
            el: '#app',
            data: {
                // 5.1  定义一个标识符， 主要是控制 编辑状态下当前编辑书籍的id 不能被修改 
                // 即 处于编辑状态下 当前控制书籍编号的输入框禁用 
                flag: false,
                id: '',
                name: '',
              
            },
            methods: {
                handle: function() {
                   /*
                     5.4  复用添加方法   用户点击提交的时候依然执行 handle 中的逻辑
                 		 如果 flag为true 即 表单处于不可输入状态 此时执行的用户编辑数据数据	
                   */ 
                    if (this.flag) {
                        // 编辑图书
                        // 5.5  根据当前的ID去更新数组中对应的数据  
                        this.books.some((item) => {
                            if (item.id == this.id) {
                                // 箭头函数中 this 指向父级作用域的this 
                                item.name = this.name;
                                // 完成更新操作之后，需要终止循环
                                return true;
                            }
                        });
                        // 5.6 编辑完数据后表单要处以可以输入的状态
                        this.flag = false;
                    //  5.7  如果 flag为false  表单处于输入状态 此时执行的用户添加数据    
                    } else { 
                        var book = {};
                        book.id = this.id;
                        book.name = this.name;
                        book.date = '';
                        this.books.push(book);
                        // 清空表单
                        this.id = '';
                        this.name = '';
                    }
                    // 清空表单
                    this.id = '';
                    this.name = '';
                },
                toEdit: function(id) {
                     /*
                     5.3  flag 默认值为false   处于编辑状态 要把 flag 改为true 即当前表单为禁					  用 
                     */ 
                    this.flag = true;
                    console.log(id)
                    var book = this.books.filter(function(item) {
                        return item.id == id;
                    });
                    console.log(book)
                    this.id = book[0].id;
                    this.name = book[0].name;
                }
            }
        });
    </script>
```

#### 6 删除图书

- 6.1 给删除按钮添加事件 把当前需要删除的书籍id 传递过来
- 6.2 根据id从数组中查找元素的索引
- 6.3 根据索引删除数组元素

```html
  <tbody>
          <tr :key='item.id' v-for='item in books'>
            <td>{{item.id}}</td>
            <td>{{item.name}}</td>
            <td>{{item.date}}</td>
            <td>
              <a href="" @click.prevent='toEdit(item.id)'>修改</a>
              <span>|</span>
               <!--  6.1 给删除按钮添加事件 把当前需要删除的书籍id 传递过来  --> 
              <a href="" @click.prevent='deleteBook(item.id)'>删除</a>
            </td>
          </tr>
</tbody>
  <script type="text/javascript">
    /*
      图书管理-添加图书
    */
    var vm = new Vue({
      methods: {
        deleteBook: function(id){
          // 删除图书
          #// 6.2 根据id从数组中查找元素的索引
          // var index = this.books.findIndex(function(item){
          //   return item.id == id;
          // });
          #// 6.3 根据索引删除数组元素
          // this.books.splice(index, 1);
          // -------------------------
         #// 方法二：通过filter方法进行删除
		
          # 6.4  根据filter 方法 过滤出来id 不是要删除书籍的id 
          # 因为 filter 是替换数组不会修改原始数据 所以需要 把 不是要删除书籍的id  赋值给 books 
          this.books = this.books.filter(function(item){
            return item.id != id;
          });
        }
      }
    });
  </script>
```



### 常用特性应用场景

#### 1 过滤器

- Vue.filter  定义一个全局过滤器

```html
 <tr :key='item.id' v-for='item in books'>
            <td>{{item.id}}</td>
            <td>{{item.name}}</td>
     		<!-- 1.3  调用过滤器 -->
            <td>{{item.date | format('yyyy-MM-dd hh:mm:ss')}}</td>
            <td>
              <a href="" @click.prevent='toEdit(item.id)'>修改</a>
              <span>|</span>
              <a href="" @click.prevent='deleteBook(item.id)'>删除</a>
            </td>
</tr>

<script>
    	#1.1  Vue.filter  定义一个全局过滤器
	    Vue.filter('format', function(value, arg) {
              function dateFormat(date, format) {
                if (typeof date === "string") {
                  var mts = date.match(/(\/Date\((\d+)\)\/)/);
                  if (mts && mts.length >= 3) {
                    date = parseInt(mts[2]);
                  }
                }
                date = new Date(date);
                if (!date || date.toUTCString() == "Invalid Date") {
                  return "";
                }
                var map = {
                  "M": date.getMonth() + 1, //月份 
                  "d": date.getDate(), //日 
                  "h": date.getHours(), //小时 
                  "m": date.getMinutes(), //分 
                  "s": date.getSeconds(), //秒 
                  "q": Math.floor((date.getMonth() + 3) / 3), //季度 
                  "S": date.getMilliseconds() //毫秒 
                };
                format = format.replace(/([yMdhmsqS])+/g, function(all, t) {
                  var v = map[t];
                  if (v !== undefined) {
                    if (all.length > 1) {
                      v = '0' + v;
                      v = v.substr(v.length - 2);
                    }
                    return v;
                  } else if (t === 'y') {
                    return (date.getFullYear() + '').substr(4 - all.length);
                  }
                  return all;
                });
                return format;
              }
              return dateFormat(value, arg);
            })
#1.2  提供的数据 包含一个时间戳   为毫秒数
   [{
          id: 1,
          name: '三国演义',
          date: 2525609975000
        },{
          id: 2,
          name: '水浒传',
          date: 2525609975000
        },{
          id: 3,
          name: '红楼梦',
          date: 2525609975000
        },{
          id: 4,
          name: '西游记',
          date: 2525609975000
        }];
</script>
```

#### 2 自定义指令

- 让表单自动获取焦点
- 通过Vue.directive 自定义指定

```html
<!-- 2.2  通过v-自定义属性名 调用自定义指令 -->
<input type="text" id="id" v-model='id' :disabled="flag" v-focus>

<script>
    # 2.1   通过Vue.directive 自定义指定
	Vue.directive('focus', {
      inserted: function (el) {
        el.focus();
      }
    });

</script>
```

#### 3 计算属性

- 通过计算属性计算图书的总数
  - 图书的总数就是计算数组的长度 

```html
 <div class="total">
        <span>图书总数：</span>
     	<!-- 3.2 在页面上 展示出来 -->
        <span>{{total}}</span>
</div>

  <script type="text/javascript">
    /*
      计算属性与方法的区别:计算属性是基于依赖进行缓存的，而方法不缓存
    */
    var vm = new Vue({
      data: {
        flag: false,
        submitFlag: false,
        id: '',
        name: '',
        books: []
      },
      computed: {
        total: function(){
          // 3.1  计算图书的总数
          return this.books.length;
        }
      },
    });
  </script>

```

## 组件

- 组件 (Component) 是 Vue.js 最强大的功能之一
- 组件可以扩展 HTML 元素，封装可重用的代

### 组件注册

#### 全局注册

- `Vue.component('组件名称', { })`     第1个参数是标签名称，第2个参数是一个选项对象
- **全局组件**注册后，任何**vue实例**都可以用

##### 组件基础用

```html
<div id="example">
  <!-- 2、 组件使用 组件名称 是以HTML标签的形式使用  -->  
  <my-component></my-component>
</div>
<script>
    //   注册组件 
    // 1、 my-component 就是组件中自定义的标签名
	Vue.component('my-component', {
      template: '<div>A custom component!</div>'
    })

    // 创建根实例
    new Vue({
      el: '#example'
    })
</script>
```

##### 组件注意事项

- 组件参数的data值必须是函数同时这个函数要求返回一个对象 
- 组件模板必须是单个根元素
- 组件模板的内容可以是模板字符串

```html
 <div id="app">
     <!-- 
		4、  组件可以重复使用多次 
	      因为data中返回的是一个对象所以每个组件中的数据是私有的
		  即每个实例可以维护一份被返回对象的独立的拷贝   
	--> 
    <button-counter></button-counter>
    <button-counter></button-counter>
    <button-counter></button-counter>
      <!-- 8、必须使用短横线的方式使用组件 -->
     <hello-world></hello-world>
</div>

<script type="text/javascript">
	//5  如果使用驼峰式命名组件，那么在使用组件的时候，只能在字符串模板中用驼峰的方式使用组件，
    // 7、但是在普通的标签模板中，必须使用短横线的方式使用组件
     Vue.component('HelloWorld', {
      data: function(){
        return {
          msg: 'HelloWorld'
        }
      },
      template: '<div>{{msg}}</div>'
    });
     
    Vue.component('button-counter', {
      // 1、组件参数的data值必须是函数 
      // 同时这个函数要求返回一个对象  
      data: function(){
        return {
          count: 0
        }
      },
      //  2、组件模板必须是单个根元素
      //  3、组件模板的内容可以是模板字符串  
      template: `
        <div>
          <button @click="handle">点击了{{count}}次</button>
          <button>测试123</button>
			#  6 在字符串模板中可以使用驼峰的方式使用组件	
		   <HelloWorld></HelloWorld>
        </div>
      `,
      methods: {
        handle: function(){
          this.count += 2;
        }
      }
    })
    var vm = new Vue({
      el: '#app',
      data: {
        
      }
    });
  </script>
```

#### 局部注册

- 只能在当前注册它的vue实例中使用

```html
<div id="app">
      <hello-jerry></hello-jerry>
</div>

<script>
    // 定义组件的模板
    var HelloJerry = {
      data: function(){
        return {
          msg: 'HelloJerry'
        }
      },
      template: '<div>{{msg}}</div>'
    };
    var vm = new Vue({
      el: '#app',
      data: {
      },
      components: {
        'hello-jerry': HelloJerry
      }
    });
 </script>
```

### Vue组件之间传值

#### 父组件向子组件传值

- 父组件发送的形式是以属性的形式绑定值到子组件身上。
- 然后子组件用属性props接收
- 在props中使用驼峰形式，模板中需要使用短横线的形式字符串形式的模板中没有这个限制

```html
  <div id="app">
    <div>{{pmsg}}</div>
     <!--1、menu-item  在 APP中嵌套着 故 menu-item   为  子组件      -->
     <!-- 给子组件传入一个静态的值 -->
    <menu-item title='来自父组件的值'></menu-item>
    <!-- 2、 需要动态的数据的时候 需要属性绑定的形式设置 此时 ptitle  来自父组件data 中的数据 . 
		  传的值可以是数字、对象、数组等等
	-->
    <menu-item :title='ptitle' content='hello'></menu-item>
  </div>

  <script type="text/javascript">
    Vue.component('menu-item', {
      // 3、 子组件用属性props接收父组件传递过来的数据  
      props: ['title', 'content'],
      data: function() {
        return {
          msg: '子组件本身的数据'
        }
      },
      template: '<div>{{msg + "----" + title + "-----" + content}}</div>'
    });
    var vm = new Vue({
      el: '#app',
      data: {
        pmsg: '父组件中内容',
        ptitle: '动态绑定属性'
      }
    });
  </script>
```

#### 子组件向父组件传值

- 子组件用`$emit()`触发事件
- `$emit()`  第一个参数为 自定义的事件,第二个参数为需要传递的数据
- 父组件用v-on 监听子组件的事件

```html
 <div id="app">
    <div :style='{fontSize: fontSize + "px"}'>{{pmsg}}</div>
     <!-- 2 父组件用v-on 监听子组件的事件
		这里 enlarge-text  是从 $emit 中的第一个参数对应   handle 为对应的事件处理函数	
	-->	
    <menu-item :parr='parr' @enlarge-text='handle($event)'></menu-item>
   <!-- 注意handle($event)显式指定event，要么是handle不加括号 -->
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      子组件向父组件传值-携带参数
    */
    
    Vue.component('menu-item', {
      props: ['parr'],
      template: `
        <div>
          <ul>
            <li :key='index' v-for='(item,index) in parr'>{{item}}</li>
          </ul>
			###  1、子组件用$emit()触发事件
			### 第一个参数为 自定义的事件名称   第二个参数为需要传递的数据  
          <button @click='$emit("enlarge-text", 5)'>扩大父组件中字体大小</button>
          <button @click='$emit("enlarge-text", 10)'>扩大父组件中字体大小</button>
        </div>
      `
    });
    var vm = new Vue({
      el: '#app',
      data: {
        pmsg: '父组件中内容',
        parr: ['apple','orange','banana'],
        fontSize: 10
      },
      methods: {
        handle: function(val){
          // 扩大字体大小
          this.fontSize += val;
        }
      }
    });
  </script>

```

#### 兄弟之间的传递

- 兄弟之间传递数据需要借助于事件中心，通过事件中心传递数据   
  - 提供事件中心    var hub = new Vue()
- 传递数据方，通过一个事件触发hub.$emit(方法名，传递的数据)
- 接收数据方，通过mounted(){} 钩子中  触发hub.$on()方法名
- 销毁事件 通过hub.$off()方法名销毁之后无法进行传递数据

```html
 <div id="app">
    <div>父组件</div>
    <div>
      <button @click='handle'>销毁事件</button>
    </div>
    <test-tom></test-tom>
    <test-jerry></test-jerry>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      兄弟组件之间数据传递
    */
    //1、 提供事件中心
    var hub = new Vue();

    Vue.component('test-tom', {
      data: function(){
        return {
          num: 0
        }
      },
      template: `
        <div>
          <div>TOM:{{num}}</div>
          <div>
            <button @click='handle'>点击</button>
          </div>
        </div>
      `,
      methods: {
        handle: function(){
          //2、传递数据方，通过一个事件触发hub.$emit(方法名，传递的数据)   触发兄弟组件的事件
          hub.$emit('jerry-event', 2);
        }
      },
      mounted: function() {
       // 3、接收数据方，通过mounted(){} 钩子中  触发hub.$on(方法名
        hub.$on('tom-event', (val) => {
          this.num += val;
        });
      }
    });
    Vue.component('test-jerry', {
      data: function(){
        return {
          num: 0
        }
      },
      template: `
        <div>
          <div>JERRY:{{num}}</div>
          <div>
            <button @click='handle'>点击</button>
          </div>
        </div>
      `,
      methods: {
        handle: function(){
          //2、传递数据方，通过一个事件触发hub.$emit(方法名，传递的数据)   触发兄弟组件的事件
          hub.$emit('tom-event', 1);
        }
      },
      mounted: function() {
        // 3、接收数据方，通过mounted(){} 钩子中  触发hub.$on()方法名
        hub.$on('jerry-event', (val) => {
          this.num += val;
        });
      }
    });
    var vm = new Vue({
      el: '#app',
      data: {
        
      },
      methods: {
        handle: function(){
          //4、销毁事件 通过hub.$off()方法名销毁之后无法进行传递数据  
          hub.$off('tom-event');
          hub.$off('jerry-event');
        }
      }
    });
  </script>

```

### 组件插槽

组件的最大特性就是复用性，而用好插槽能大大提高组件的可复用能力。

#### 匿名插槽

```html
  <div id="app">
    <!-- 这里的所有组件标签中嵌套的内容会替换掉slot  如果不传值则使用 slot 中的默认值  -->  
    <alert-box>有bug发生</alert-box>
    <alert-box>有一个警告</alert-box>
    <alert-box></alert-box>
  </div>

  <script type="text/javascript">
    /*
      组件插槽：父组件向子组件传递内容
    */
    Vue.component('alert-box', {
      template: `
        <div>
          <strong>ERROR:</strong>
		# 当组件渲染的时候，这个 <slot> 元素将会被替换为“组件标签中嵌套的内容”。
		# 插槽内可以包含任何模板代码，包括 HTML
          <slot>默认内容</slot>
        </div>
      `
    });
    var vm = new Vue({
      el: '#app',
      data: {
        
      }
    });
  </script>
</body>
</html>

```

![image-20200312164746313](/images/vue.assets/image-20200312164746313.png)

#### 具名插槽

- 具有名字的插槽 
- 使用 <slot> 中的 "name" 属性绑定元素

```HTML
  <div id="app">
    <base-layout>
       <!-- 2、 通过slot属性来指定, 这个slot的值必须和下面slot组件得name值对应上
				如果没有匹配到 则放到匿名的插槽中   --> 
      <p slot='header'>标题信息</p>
      <p>主要内容1</p>
      <p>主要内容2</p>
      <p slot='footer'>底部信息信息</p>
    </base-layout>

    <base-layout>
      <!-- 注意点：template临时的包裹标签最终不会渲染到页面上     -->  
      <template slot='header'>
        <p>标题信息1</p>
        <p>标题信息2</p>
      </template>
      <p>主要内容1</p>
      <p>主要内容2</p>
      <template slot='footer'>
        <p>底部信息信息1</p>
        <p>底部信息信息2</p>
      </template>
    </base-layout>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      具名插槽
    */
    Vue.component('base-layout', {
      template: `
        <div>
          <header>
			###	1、 使用 <slot> 中的 "name" 属性绑定元素 指定当前插槽的名字
            <slot name='header'></slot>
          </header>
          <main>
            <slot></slot>
          </main>
          <footer>
			###  注意点： 
			###  具名插槽的渲染顺序，完全取决于模板，而不是取决于父组件中元素的顺序
            <slot name='footer'></slot>
          </footer>
        </div>
      `
    });
    var vm = new Vue({
      el: '#app',
      data: {
        
      }
    });
  </script>
</body>
</html>
```

<img src="/images/vue.assets/image-20200312165238018.png" alt="image-20200312165238018" style="zoom:50%;" />

关于上面的template，最终渲染少一个根标签。

####  作用域插槽

- 父组件对子组件加工处理
- 既可以复用子组件的slot，又可以使slot内容不一致

```html
  <div id="app">
    <!-- 
		1、当我们希望li 的样式由外部使用组件的地方定义，因为可能有多种地方要使用该组件，
		但样式希望不一样 这个时候我们需要使用作用域插槽 
	-->  
    <fruit-list :list='list'>
       <!-- 2、 父组件中使用了<template>元素,而且包含scope="slotProps",
			slotProps在这里只是临时变量   
		---> 	
      <template slot-scope='slotProps'>
        <strong v-if='slotProps.info.id==3' class="current">
            {{slotProps.info.name}}		         
         </strong>
        <span v-else>{{slotProps.info.name}}</span>
      </template>
    </fruit-list>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      作用域插槽
    */
    Vue.component('fruit-list', {
      props: ['list'],
      template: `
        <div>
          <li :key='item.id' v-for='item in list'>
			###  3、 在子组件模板中,<slot>元素上有一个类似props传递数据给组件的写法msg="xxx",
			###   插槽可以提供一个默认内容，如果如果父组件没有为这个插槽提供了内容，会显示默认的内容。
					如果父组件为这个插槽提供了内容，则默认的内容会被替换掉
            <slot :info='item'>{{item.name}}</slot>
          </li>
        </div>
      `
    });
    var vm = new Vue({
      el: '#app',
      data: {
        list: [{
          id: 1,
          name: 'apple'
        },{
          id: 2,
          name: 'orange'
        },{
          id: 3,
          name: 'banana'
        }]
      }
    });
  </script>
</body>
</html>

```

###  购物车案例

#### 1.实现组件化布局

- 把静态页面转换成组件化模式
- 把组件渲染到页面上

````html
 <div id="app">
    <div class="container">
      <!-- 2、把组件渲染到页面上 --> 
      <my-cart></my-cart>
    </div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    # 1、 把静态页面转换成组件化模式
    # 1.1  标题组件 
    var CartTitle = {
      template: `
        <div class="title">我的商品</div>
      `
    }
    # 1.2  商品列表组件 
    var CartList = {
      #  注意点 ：  组件模板必须是单个根元素  
      template: `
        <div>
          <div class="item">
            <img src="img/a.jpg"/>
            <div class="name"></div>
            <div class="change">
              <a href="">－</a>
              <input type="text" class="num" />
              <a href="">＋</a>
            </div>
            <div class="del">×</div>
          </div>
          <div class="item">
            <img src="img/b.jpg"/>
            <div class="name"></div>
            <div class="change">
              <a href="">－</a>
              <input type="text" class="num" />
              <a href="">＋</a>
            </div>
            <div class="del">×</div>
          </div>
          <div class="item">
            <img src="img/c.jpg"/>
            <div class="name"></div>
            <div class="change">
              <a href="">－</a>
              <input type="text" class="num" />
              <a href="">＋</a>
            </div>
            <div class="del">×</div>
          </div>
          <div class="item">
            <img src="img/d.jpg"/>
            <div class="name"></div>
            <div class="change">
              <a href="">－</a>
              <input type="text" class="num" />
              <a href="">＋</a>
            </div>
            <div class="del">×</div>
          </div>
          <div class="item">
            <img src="img/e.jpg"/>
            <div class="name"></div>
            <div class="change">
              <a href="">－</a>
              <input type="text" class="num" />
              <a href="">＋</a>
            </div>
            <div class="del">×</div>
          </div>
        </div>
      `
    }
    # 1.3  商品结算组件 
    var CartTotal = {
      template: `
        <div class="total">
          <span>总价：123</span>
          <button>结算</button>
        </div>
      `
    }
    ## 1.4  定义一个全局组件 my-cart
    Vue.component('my-cart',{
      ##  1.6 引入子组件  
      template: `
        <div class='cart'>
          <cart-title></cart-title>
          <cart-list></cart-list>
          <cart-total></cart-total>
        </div>
      `,
      # 1.5  注册子组件   
      components: {
        'cart-title': CartTitle,
        'cart-list': CartList,
        'cart-total': CartTotal
      }
    });
    var vm = new Vue({
      el: '#app',
      data: {

      }
    });
</script>
````

#### 2、实现 标题和结算功能组件

- 标题组件实现动态渲染
  - 从父组件把标题数据传递过来 即 父向子组件传值
  - 把传递过来的数据渲染到页面上  
- 结算功能组件
  - 从父组件把商品列表list 数据传递过来 即 父向子组件传值
  - 把传递过来的数据计算最终价格渲染到页面上  

```html
 <div id="app">
    <div class="container">
      <my-cart></my-cart>
    </div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
     # 2.2  标题组件     子组件通过props形式接收父组件传递过来的uname数据
    var CartTitle = {
      props: ['uname'],
      template: `
        <div class="title">{{uname}}的商品</div>
      `
    }
	# 2.3  商品结算组件  子组件通过props形式接收父组件传递过来的list数据   
    var CartTotal = {
      props: ['list'],
      template: `
        <div class="total">
          <span>总价：{{total}}</span>
          <button>结算</button>
        </div>
      `,
      computed: {
        # 2.4    计算商品的总价  并渲染到页面上 
        total: function() {
          var t = 0;
          this.list.forEach(item => {
            t += item.price * item.num;
          });
          return t;
        }
      }
    }
    Vue.component('my-cart',{
      data: function() {
        return {
          uname: '张三',
          list: [{
            id: 1,
            name: 'TCL彩电',
            price: 1000,
            num: 1,
            img: 'img/a.jpg'
          },{
            id: 2,
            name: '机顶盒',
            price: 1000,
            num: 1,
            img: 'img/b.jpg'
          },{
            id: 3,
            name: '海尔冰箱',
            price: 1000,
            num: 1,
            img: 'img/c.jpg'
          },{
            id: 4,
            name: '小米手机',
            price: 1000,
            num: 1,
            img: 'img/d.jpg'
          },{
            id: 5,
            name: 'PPTV电视',
            price: 1000,
            num: 2,
            img: 'img/e.jpg'
          }]
        }
      },
      #  2.1  父组件向子组件以属性传递的形式 传递数据
      #   向 标题组件传递 uname 属性   向 商品结算组件传递 list  属性  
      template: `
        <div class='cart'>
          <cart-title :uname='uname'></cart-title>
          <cart-list></cart-list>
          <cart-total :list='list'></cart-total>
        </div>
      `,
      components: {
        'cart-title': CartTitle,
        'cart-list': CartList,
        'cart-total': CartTotal
      }
    });
    var vm = new Vue({
      el: '#app',
      data: {

      }
    });
</script>
```

#### 3.实现列表组件删除功能

- 从父组件把商品列表list 数据传递过来 即 父向子组件传值
- 把传递过来的数据渲染到页面上    
- 点击删除按钮的时候删除对应的数据 
  - 给按钮添加点击事件把需要删除的id传递过来  
    - 子组件中不推荐操作父组件的数据有可能多个子组件使用父组件的数据  我们需要把数据传递给父组件让父组件操作数据 
    - 父组件删除对应的数据

```html
 <div id="app">
    <div class="container">
      <my-cart></my-cart>
    </div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    
    var CartTitle = {
      props: ['uname'],
      template: `
        <div class="title">{{uname}}的商品</div>
      `
    }
    #  3.2 把列表数据动态渲染到页面上  
    var CartList = {
      props: ['list'],
      template: `
        <div>
          <div :key='item.id' v-for='item in list' class="item">
            <img :src="item.img"/>
            <div class="name">{{item.name}}</div>
            <div class="change">
              <a href="">－</a>
              <input type="text" class="num" />
              <a href="">＋</a>
            </div>
			# 3.3  给按钮添加点击事件把需要删除的id传递过来
            <div class="del" @click='del(item.id)'>×</div>
          </div>
        </div>
      `,
      methods: {
        del: function(id){
           # 3.4 子组件中不推荐操作父组件的数据有可能多个子组件使用父组件的数据 
          # 	  我们需要把数据传递给父组件 让父组件操作数据 
          this.$emit('cart-del', id);
        }
      }
    }
    var CartTotal = {
      props: ['list'],
      template: `
        <div class="total">
          <span>总价：{{total}}</span>
          <button>结算</button>
        </div>
      `,
      computed: {
        total: function() {
          // 计算商品的总价
          var t = 0;
          this.list.forEach(item => {
            t += item.price * item.num;
          });
          return t;
        }
      }
    }
    Vue.component('my-cart',{
      data: function() {
        return {
          uname: '张三',
          list: [{
            id: 1,
            name: 'TCL彩电',
            price: 1000,
            num: 1,
            img: 'img/a.jpg'
          },{
            id: 2,
            name: '机顶盒',
            price: 1000,
            num: 1,
            img: 'img/b.jpg'
          },{
            id: 3,
            name: '海尔冰箱',
            price: 1000,
            num: 1,
            img: 'img/c.jpg'
          },{
            id: 4,
            name: '小米手机',
            price: 1000,
            num: 1,
            img: 'img/d.jpg'
          },{
            id: 5,
            name: 'PPTV电视',
            price: 1000,
            num: 2,
            img: 'img/e.jpg'
          }]
        }
      },
      # 3.1 从父组件把商品列表list 数据传递过来 即 父向子组件传值  
      template: `
        <div class='cart'>
          <cart-title :uname='uname'></cart-title>
		  #  3.5  父组件通过事件绑定 接收子组件传递过来的数据 
          <cart-list :list='list' @cart-del='delCart($event)'></cart-list>
          <cart-total :list='list'></cart-total>
        </div>
      `,
      components: {
        'cart-title': CartTitle,
        'cart-list': CartList,
        'cart-total': CartTotal
      },
      methods: {
        # 3.6    根据id删除list中对应的数据        
        delCart: function(id) {
          // 1、找到id所对应数据的索引
          var index = this.list.findIndex(item=>{
            return item.id == id;
          });
          // 2、根据索引删除对应数据
          this.list.splice(index, 1);
        }
      }
    });
    var vm = new Vue({
      el: '#app',
      data: {

      }
    });

  </script>
</body>
</html>
```

####  4.实现组件更新数据功能  上

- 将输入框中的默认数据动态渲染出来
- 输入框失去焦点的时候 更改商品的数量 
- 子组件中不推荐操作数据 把这些数据传递给父组件 让父组件处理这些数据
- 父组件中接收子组件传递过来的数据并处理 

```html
 <div id="app">
    <div class="container">
      <my-cart></my-cart>
    </div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    
    var CartTitle = {
      props: ['uname'],
      template: `
        <div class="title">{{uname}}的商品</div>
      `
    }
    var CartList = {
      props: ['list'],
      template: `
        <div>
          <div :key='item.id' v-for='item in list' class="item">
            <img :src="item.img"/>
            <div class="name">{{item.name}}</div>
            <div class="change">
              <a href="">－</a>
				# 1. 将输入框中的默认数据动态渲染出来
				# 2. 输入框失去焦点的时候 更改商品的数量  需要将当前商品的id 传递过来
              <input type="text" class="num" :value='item.num' @blur='changeNum(item.id, $event)'/>
              <a href="">＋</a>
            </div>
            <div class="del" @click='del(item.id)'>×</div>
          </div>
        </div>
      `,
      methods: {
        changeNum: function(id, event){
          # 3 子组件中不推荐操作数据  因为别的组件可能也引用了这些数据
          #  把这些数据传递给父组件 让父组件处理这些数据
          this.$emit('change-num', {
            id: id,
            num: event.target.value
          });
        },
        del: function(id){
          // 把id传递给父组件
          this.$emit('cart-del', id);
        }
      }
    }
    var CartTotal = {
      props: ['list'],
      template: `
        <div class="total">
          <span>总价：{{total}}</span>
          <button>结算</button>
        </div>
      `,
      computed: {
        total: function() {
          // 计算商品的总价
          var t = 0;
          this.list.forEach(item => {
            t += item.price * item.num;
          });
          return t;
        }
      }
    }
    Vue.component('my-cart',{
      data: function() {
        return {
          uname: '张三',
          list: [{
            id: 1,
            name: 'TCL彩电',
            price: 1000,
            num: 1,
            img: 'img/a.jpg'
          }]
      },
      template: `
        <div class='cart'>
          <cart-title :uname='uname'></cart-title>
			# 4  父组件中接收子组件传递过来的数据 
          <cart-list :list='list' @change-num='changeNum($event)' @cart-del='delCart($event)'></cart-list>
          <cart-total :list='list'></cart-total>
        </div>
      `,
      components: {
        'cart-title': CartTitle,
        'cart-list': CartList,
        'cart-total': CartTotal
      },
      methods: {
        changeNum: function(val) {
          //4.1 根据子组件传递过来的数据，跟新list中对应的数据
          this.list.some(item=>{
            if(item.id == val.id) {
              item.num = val.num;
              // 终止遍历
              return true;
            }
          });
        },
        delCart: function(id) {
          // 根据id删除list中对应的数据
          // 1、找到id所对应数据的索引
          var index = this.list.findIndex(item=>{
            return item.id == id;
          });
          // 2、根据索引删除对应数据
          this.list.splice(index, 1);
        }
      }
    });
    var vm = new Vue({
      el: '#app',
      data: {

      }
    });
</script>
```

#### 5.实现组件更新数据功能  下

- 子组件通过一个标识符来标记对用的用户点击  + - 或者输入框输入的内容
- 父组件拿到标识符更新对应的组件

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style type="text/css">
    .container {
    }
    .container .cart {
      width: 300px;
      margin: auto;
    }
    .container .title {
      background-color: lightblue;
      height: 40px;
      line-height: 40px;
      text-align: center;
      /*color: #fff;*/  
    }
    .container .total {
      background-color: #FFCE46;
      height: 50px;
      line-height: 50px;
      text-align: right;
    }
    .container .total button {
      margin: 0 10px;
      background-color: #DC4C40;
      height: 35px;
      width: 80px;
      border: 0;
    }
    .container .total span {
      color: red;
      font-weight: bold;
    }
    .container .item {
      height: 55px;
      line-height: 55px;
      position: relative;
      border-top: 1px solid #ADD8E6;
    }
    .container .item img {
      width: 45px;
      height: 45px;
      margin: 5px;
    }
    .container .item .name {
      position: absolute;
      width: 90px;
      top: 0;left: 55px;
      font-size: 16px;
    }

    .container .item .change {
      width: 100px;
      position: absolute;
      top: 0;
      right: 50px;
    }
    .container .item .change a {
      font-size: 20px;
      width: 30px;
      text-decoration:none;
      background-color: lightgray;
      vertical-align: middle;
    }
    .container .item .change .num {
      width: 40px;
      height: 25px;
    }
    .container .item .del {
      position: absolute;
      top: 0;
      right: 0px;
      width: 40px;
      text-align: center;
      font-size: 40px;
      cursor: pointer;
      color: red;
    }
    .container .item .del:hover {
      background-color: orange;
    }
  </style>
</head>
<body>
  <div id="app">
    <div class="container">
      <my-cart></my-cart>
    </div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    
    var CartTitle = {
      props: ['uname'],
      template: `
        <div class="title">{{uname}}的商品</div>
      `
    }
    var CartList = {
      props: ['list'],
      template: `
        <div>
          <div :key='item.id' v-for='item in list' class="item">
            <img :src="item.img"/>
            <div class="name">{{item.name}}</div>
            <div class="change">
			  # 1.  + - 按钮绑定事件 
              <a href="" @click.prevent='sub(item.id)'>－</a>
              <input type="text" class="num" :value='item.num' @blur='changeNum(item.id, $event)'/>
              <a href="" @click.prevent='add(item.id)'>＋</a>
            </div>
            <div class="del" @click='del(item.id)'>×</div>
          </div>
        </div>
      `,
      methods: {
        changeNum: function(id, event){
          this.$emit('change-num', {
            id: id,
            type: 'change',
            num: event.target.value
          });
        },
        sub: function(id){
          # 2 数量的增加和减少通过父组件来计算   每次都是加1 和 减1 不需要传递数量   父组件需要一个类型来判断 是 加一 还是减1  以及是输入框输入的数据  我们通过type 标识符来标记 不同的操作   
          this.$emit('change-num', {
            id: id,
            type: 'sub'
          });
        },
        add: function(id){
         # 2 数量的增加和减少通过父组件来计算   每次都是加1 和 减1 不需要传递数量   父组件需要一个类型来判断 是 加一 还是减1  以及是输入框输入的数据  我们通过type 标识符来标记 不同的操作
          this.$emit('change-num', {
            id: id,
            type: 'add'
          });
        },
        del: function(id){
          // 把id传递给父组件
          this.$emit('cart-del', id);
        }
      }
    }
    var CartTotal = {
      props: ['list'],
      template: `
        <div class="total">
          <span>总价：{{total}}</span>
          <button>结算</button>
        </div>
      `,
      computed: {
        total: function() {
          // 计算商品的总价
          var t = 0;
          this.list.forEach(item => {
            t += item.price * item.num;
          });
          return t;
        }
      }
    }
    Vue.component('my-cart',{
      data: function() {
        return {
          uname: '张三',
          list: [{
            id: 1,
            name: 'TCL彩电',
            price: 1000,
            num: 1,
            img: 'img/a.jpg'
          },{
            id: 2,
            name: '机顶盒',
            price: 1000,
            num: 1,
            img: 'img/b.jpg'
          },{
            id: 3,
            name: '海尔冰箱',
            price: 1000,
            num: 1,
            img: 'img/c.jpg'
          },{
            id: 4,
            name: '小米手机',
            price: 1000,
            num: 1,
            img: 'img/d.jpg'
          },{
            id: 5,
            name: 'PPTV电视',
            price: 1000,
            num: 2,
            img: 'img/e.jpg'
          }]
        }
      },
      template: `
        <div class='cart'>
          <cart-title :uname='uname'></cart-title>	
		# 3 父组件通过事件监听   接收子组件的数据  
          <cart-list :list='list' @change-num='changeNum($event)' @cart-del='delCart($event)'></cart-list>
          <cart-total :list='list'></cart-total>
        </div>
      `,
      components: {
        'cart-title': CartTitle,
        'cart-list': CartList,
        'cart-total': CartTotal
      },
      methods: {
        changeNum: function(val) {
          #4 分为三种情况：输入框变更、加号变更、减号变更
          if(val.type=='change') {
            // 根据子组件传递过来的数据，跟新list中对应的数据
            this.list.some(item=>{
              if(item.id == val.id) {
                item.num = val.num;
                // 终止遍历
                return true;
              }
            });
          }else if(val.type=='sub'){
            // 减一操作
            this.list.some(item=>{
              if(item.id == val.id) {
                item.num -= 1;
                // 终止遍历
                return true;
              }
            });
          }else if(val.type=='add'){
            // 加一操作
            this.list.some(item=>{
              if(item.id == val.id) {
                item.num += 1;
                // 终止遍历
                return true;
              }
            });
          }
        }
      }
    });
    var vm = new Vue({
      el: '#app',
      data: {

      }
    });

  </script>
</body>
</html>
```

## 接口调用方式

- 原生ajax
- 基于jQuery的ajax
- fetch
- axios

###  异步

- JavaScript的执行环境是「单线程」
- 所谓单线程，是指JS引擎中负责解释和执行JavaScript代码的线程只有一个，也就是一次只能完成一项任务，这个任务执行完后才能执行下一个，它会「阻塞」其他任务。这个任务可称为主线程
- 异步模式可以一起执行**多个任务**
- JS中常见的异步调用
  - 定时任何
  - ajax
  - 事件函数

### promise

- 主要解决异步深层嵌套的问题
- promise 提供了简洁的API  使得异步操作更加容易

```html
<script type="text/javascript">
    /*
     1. Promise基本使用
           我们使用new来构建一个Promise  Promise的构造函数接收一个参数，是函数，并且传入两个参数：		   resolve，reject， 分别表示异步操作执行成功后的回调函数和异步操作执行失败后的回调函数
    */
    var p = new Promise(function(resolve, reject){
      //2. 这里用于实现异步任务  setTimeout
      setTimeout(function(){
        var flag = false;
        if(flag) {
          //3. 正常情况
          resolve('hello');
        }else{
          //4. 异常情况
          reject('出错了');
        }
      }, 100);
    });
    //  5 Promise实例生成以后，可以用then方法指定resolved状态和reject状态的回调函数 
    //  在then方法中，你也可以直接return数据而不是Promise对象，在后面的then中就可以接收到数据了  
    p.then(function(data){
      console.log(data)
    },function(info){
      console.log(info)
    });
  </script>
```

###  基于Promise发送Ajax请求

```html
<script type="text/javascript">
    /*
      基于Promise发送Ajax请求
    */
    function queryData(url) {
     #   1.1 创建一个Promise实例
      var p = new Promise(function(resolve, reject){
        var xhr = new XMLHttpRequest();
        xhr.onreadystatechange = function(){
          if(xhr.readyState != 4) return;
          if(xhr.readyState == 4 && xhr.status == 200) {
            # 1.2 处理正常的情况
            resolve(xhr.responseText);
          }else{
            # 1.3 处理异常情况
            reject('服务器错误');
          }
        };
        xhr.open('get', url);
        xhr.send(null);
      });
      return p;
    }
	  # 注意：  这里需要开启一个服务 
    # 在then方法中，你也可以直接return数据而不是Promise对象，在后面的then中就可以接收到数据了
    queryData('http://localhost:3000/data')
      .then(function(data){
        console.log(data)
        #  1.4 想要继续链式编程下去 需要 return  
        return queryData('http://localhost:3000/data1');
      })
      .then(function(data){
        console.log(data);
        return queryData('http://localhost:3000/data2');
      })
      .then(function(data){
        console.log(data)
      });
</script>
```

### Promise  基本API

####  实例方法

##### `.then()`

- 得到异步任务正确的结果

##### `.catch()`

- 获取异常信息

##### `.finally()`

- 成功与否都会执行（不是正式标准） 

```html
<script type="text/javascript">
    /*
      Promise常用API-实例方法
    */
    // console.dir(Promise);
    function foo() {
      return new Promise(function(resolve, reject){
        setTimeout(function(){
          // resolve(123);
          reject('error');
        }, 100);
      })
    }
    // foo()
    //   .then(function(data){
    //     console.log(data)
    //   })
    //   .catch(function(data){
    //     console.log(data)
    //   })
    //   .finally(function(){
    //     console.log('finished')
    //   });

    // --------------------------
    // 两种写法是等效的
    foo()
      .then(function(data){
        # 得到异步任务正确的结果
        console.log(data)
      },function(data){
        # 获取异常信息
        console.log(data)
      })
      # 成功与否都会执行（不是正式标准） 
      .finally(function(){
        console.log('finished')
      });
  </script>
```

#### 静态方法

#####  `.all()`

- `Promise.all`方法接受一个数组作参数，数组中的对象（p1、p2、p3）均为promise实例（如果不是一个promise，该项会被用`Promise.resolve`转换为一个promise)。它的状态由这三个promise实例决定

#####  `.race()`

- `Promise.race`方法同样接受一个数组作参数。当p1, p2, p3中有一个实例的状态发生改变（变为`fulfilled`或`rejected`），p的状态就跟着改变。并把第一个改变状态的promise的返回值，传给p的回调函数

```html
<script type="text/javascript">
    /*
      Promise常用API-对象方法
    */
    // console.dir(Promise)
    function queryData(url) {
      return new Promise(function(resolve, reject){
        var xhr = new XMLHttpRequest();
        xhr.onreadystatechange = function(){
          if(xhr.readyState != 4) return;
          if(xhr.readyState == 4 && xhr.status == 200) {
            // 处理正常的情况
            resolve(xhr.responseText);
          }else{
            // 处理异常情况
            reject('服务器错误');
          }
        };
        xhr.open('get', url);
        xhr.send(null);
      });
    }

    var p1 = queryData('http://localhost:3000/a1');
    var p2 = queryData('http://localhost:3000/a2');
    var p3 = queryData('http://localhost:3000/a3');
     Promise.all([p1,p2,p3]).then(function(result){
       //   all 中的参数  [p1,p2,p3]   和 返回的结果一 一对应["HELLO TOM", "HELLO JERRY", "HELLO SPIKE"]
       console.log(result) //["HELLO TOM", "HELLO JERRY", "HELLO SPIKE"]
     })
    Promise.race([p1,p2,p3]).then(function(result){
      // 由于p1执行较快，Promise的then()将获得结果'P1'。p2,p3仍在继续执行，但执行结果将被丢弃。
      console.log(result) // "HELLO TOM"
    })
  </script>
```

###  fetch

- Fetch API是新的ajax解决方案 Fetch会返回Promise
- **fetch不是ajax的进一步封装，而是原生js，没有使用XMLHttpRequest对象**。
- fetch(url, options).then(）

```html
  <script type="text/javascript">
    /*
      Fetch API 基本用法
      	fetch(url).then()
     	第一个参数请求的路径   Fetch会返回Promise   所以我们可以使用then 拿到请求成功的结果 
    */
    fetch('http://localhost:3000/fdata').then(function(data){
      // text()方法属于fetchAPI的一部分，它返回一个Promise实例对象，用于获取后台返回的数据
      return data.text();
    }).then(function(data){
      //   在这个then里面我们能拿到最终的数据  
      console.log(data);
    })
  </script>
```

####  fetch API  中的 HTTP  请求

- fetch(url, options).then()
- HTTP协议，它给我们提供了很多的方法，如POST，GET，DELETE，UPDATE，PATCH和PUT
  - 默认的是 GET 请求
  - 需要在 options 对象中 指定对应的 method       method:请求使用的方法 
  - post 和 普通 请求的时候 需要在options 中 设置  请求头 headers   和  body

```html
   <script type="text/javascript">
        /*
              Fetch API 调用接口传递参数
        */
       #1.1 GET参数传递 - 传统URL  通过url  ？ 的形式传参 
        fetch('http://localhost:3000/books?id=123', {
            	# get 请求可以省略不写 默认的是GET 
                method: 'get'
            })
            .then(function(data) {
            	# 它返回一个Promise实例对象，用于获取后台返回的数据
                return data.text();
            }).then(function(data) {
            	# 在这个then里面我们能拿到最终的数据  
                console.log(data)
            });

      #1.2  GET参数传递  restful形式的URL  通过/ 的形式传递参数  即  id = 456 和id后台的配置有关 
        fetch('http://localhost:3000/books/456', {
            	# get 请求可以省略不写 默认的是GET 
                method: 'get'
            })
            .then(function(data) {
                return data.text();
            }).then(function(data) {
                console.log(data)
            });

       #2.1  DELETE请求方式参数传递      删除id  是  id=789
        fetch('http://localhost:3000/books/789', {
                method: 'delete'
            })
            .then(function(data) {
                return data.text();
            }).then(function(data) {
                console.log(data)
            });

       #3 POST请求传参
        fetch('http://localhost:3000/books', {
                method: 'post',
            	# 3.1  传递数据 
                body: 'uname=lisi&pwd=123',
            	#  3.2  设置请求头 
                headers: {
                    'Content-Type': 'application/x-www-form-urlencoded'
                }
            })
            .then(function(data) {
                return data.text();
            }).then(function(data) {
                console.log(data)
            });

       # POST请求传参
        fetch('http://localhost:3000/books', {
                method: 'post',
                body: JSON.stringify({
                    uname: '张三',
                    pwd: '456'
                }),
                headers: {
                    'Content-Type': 'application/json'
                }
            })
            .then(function(data) {
                return data.text();
            }).then(function(data) {
                console.log(data)
            });

        # PUT请求传参     修改id 是 123 的 
        fetch('http://localhost:3000/books/123', {
                method: 'put',
                body: JSON.stringify({
                    uname: '张三',
                    pwd: '789'
                }),
                headers: {
                    'Content-Type': 'application/json'
                }
            })
            .then(function(data) {
                return data.text();
            }).then(function(data) {
                console.log(data)
            });
    </script>
```

####  fetchAPI 中 响应格式

- 用fetch来获取数据，如果响应正常返回，我们首先看到的是一个response对象，其中包括返回的一堆原始字节，这些字节需要在收到后，需要我们通过调用方法将其转换为相应格式的数据，比如`JSON`，`BLOB`或者`TEXT`等等

```js
    /*
      Fetch响应结果的数据格式
    */
    fetch('http://localhost:3000/json').then(function(data){
      // return data.json();   //  将获取到的数据使用 json 转换对象
      return data.text(); //  //  将获取到的数据 转换成字符串 
    }).then(function(data){
      // console.log(data.uname)
      // console.log(typeof data)
      var obj = JSON.parse(data);
      console.log(obj.uname,obj.age,obj.gender)
    })

```

###  axios

- 基于promise用于浏览器和node.js的http客户端
- 支持浏览器和node.js
- 支持promise
- 能拦截请求和响应
- 自动转换JSON数据
- 能转换请求和响应数据

#### axios基础用法

- get和 delete请求传递参数
  - 通过传统的url  以 ? 的形式传递参数
  - restful 形式传递参数 
  - 通过params形式传递参数 
- post  和 put  请求传递参数
  - 通过选项传递参数
  - 通过 URLSearchParams  传递参数 

```js
# 1. 发送get 请求 
	axios.get('http://localhost:3000/adata').then(function(ret){ 
      #  拿到 ret 是一个对象      所有的对象都存在 ret 的data 属性里面
      // 注意data属性是固定的用法，用于获取后台的实际数据
      // console.log(ret.data)
      console.log(ret)
    })
# 2.  get 请求传递参数
    # 2.1  通过传统的url  以 ? 的形式传递参数
	axios.get('http://localhost:3000/axios?id=123').then(function(ret){
      console.log(ret.data)
    })
    # 2.2  restful 形式传递参数 
    axios.get('http://localhost:3000/axios/123').then(function(ret){
      console.log(ret.data)
    })
	  # 2.3  通过params  形式传递参数 
    axios.get('http://localhost:3000/axios', {
      params: {
        id: 789
      }
    }).then(function(ret){
      console.log(ret.data)
    })
#3 axios delete 请求传参     传参的形式和 get 请求一样
    axios.delete('http://localhost:3000/axios', {
      params: {
        id: 111
      }
    }).then(function(ret){
      console.log(ret.data)
    })

# 4  axios 的 post 请求
    # 4.1  通过选项传递参数
    axios.post('http://localhost:3000/axios', {
      uname: 'lisi',
      pwd: 123
    }).then(function(ret){
      console.log(ret.data)
    })
# 4.2  通过 URLSearchParams  传递参数 
    var params = new URLSearchParams();
    params.append('uname', 'zhangsan');
    params.append('pwd', '111');
    axios.post('http://localhost:3000/axios', params).then(function(ret){
      console.log(ret.data)
    })

#5  axios put 请求传参   和 post 请求一样 
    axios.put('http://localhost:3000/axios/123', {
      uname: 'lisi',
      pwd: 123
    }).then(function(ret){
      console.log(ret.data)
    })
```

#### axios 全局配置

```js
#  配置公共的请求头 
axios.defaults.baseURL = 'https://api.example.com';
#  配置 超时时间
axios.defaults.timeout = 2500;
#  配置公共的请求头
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
# 配置公共的 post 的 Content-Type
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
```

####  axios 拦截器

- 请求拦截器
  - 请求拦截器的作用是在请求发送前进行一些操作
    - 例如在每个请求体里加上token，统一做了处理如果以后要改也非常容易
- 响应拦截器
  - 响应拦截器的作用是在接收到响应后进行一些操作
    - 例如在服务器返回登录状态失效，需要重新登录的时候，跳转到登录页

```js
# 1. 请求拦截器 
axios.interceptors.request.use(function(config) {
     console.log(config.url)
     # 1.1  任何请求都会经过这一步   在发送请求之前做些什么   
     config.headers.mytoken = 'nihao';
     # 1.2  这里一定要return   否则配置不成功  
     return config;
   }, function(err){
      #1.3 对请求错误做点什么    
     console.log(err)
   })
#2. 响应拦截器 
   axios.interceptors.response.use(function(res) {
     #2.1  在接收响应做些什么  
     var data = res.data;
     return data;
   }, function(err){
     #2.2 对响应错误做点什么  
     console.log(err)
   })
```

###  async 和 await

- `async`作为一个关键字放到函数前面
  - 任何一个`async`函数都会隐式返回一个`promise`
- `await`关键字只能在使用`async`定义的函数中使用
  - await后面可以直接跟一个 Promise实例对象
  - await函数不能单独使用
- **async/await 让异步代码看起来、表现起来更像同步代码**

```js
# 1.  async 基础用法
    # 1.1 async作为一个关键字放到函数前面
	async function queryData() {
      # 1.2 await关键字只能在使用async定义的函数中使用 await后面可以直接跟一个 Promise实例对象
      var ret = await new Promise(function(resolve, reject){
        setTimeout(function(){
          resolve('nihao')
        },1000);
      })
      // console.log(ret.data)
      return ret;
    }
	# 1.3 任何一个async函数都会隐式返回一个promise   我们可以使用then 进行链式编程
    queryData().then(function(data){
      console.log(data)
    })

	#2.  async函数处理多个异步函数
    axios.defaults.baseURL = 'http://localhost:3000';
    async function queryData() {
      # 2.1  添加await之后 当前的await 返回结果之后才会执行后面的代码   
      var info = await axios.get('async1');
      #2.2  让异步代码看起来、表现起来更像同步代码
      var ret = await axios.get('async2?info=' + info.data);
      return ret.data;
    }
    queryData().then(function(data){
      console.log(data)
    })
```

### 图书列表案例

#### 1. 基于接口案例-获取图书列表

- 导入axios 用来发送ajax 
- 把获取到的数据渲染到页面上 

```html
  <div id="app">
        <div class="grid">
            <table>
                <thead>
                    <tr>
                        <th>编号</th>
                        <th>名称</th>
                        <th>时间</th>
                        <th>操作</th>
                    </tr>
                </thead>
                <tbody>
                    <!-- 5.  把books  中的数据渲染到页面上   -->
                    <tr :key='item.id' v-for='item in books'>
                        <td>{{item.id}}</td>
                        <td>{{item.name}}</td>
                        <td>{{item.date }}</td>
                        <td>
                            <a href="">修改</a>
                            <span>|</span>
                            <a href="">删除</a>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
    <script type="text/javascript" src="js/vue.js"></script>
	1.  导入axios   
    <script type="text/javascript" src="js/axios.js"></script>
    <script type="text/javascript">
        /*
             图书管理-添加图书
         */
        # 2   配置公共的url地址  简化后面的调用方式
        axios.defaults.baseURL = 'http://localhost:3000/';
        axios.interceptors.response.use(function(res) {
            return res.data;
        }, function(error) {
            console.log(error)
        });

        var vm = new Vue({
            el: '#app',
            data: {
                flag: false,
                submitFlag: false,
                id: '',
                name: '',
                books: []
            },
            methods: {
                # 3 定义一个方法 用来发送 ajax 
                # 3.1  使用 async  来 让异步的代码  以同步的形式书写 
                queryData: async function() {
                    // 调用后台接口获取图书列表数据
                    // var ret = await axios.get('books');
                    // this.books = ret.data;
					# 3.2  发送ajax请求  把拿到的数据放在books 里面   
                    this.books = await axios.get('books');
                }
            },

            mounted: function() {
				#  4 mounted  里面 DOM已经加载完毕  在这里调用函数  
                this.queryData();
            }
        });
    </script>
```

#### 2   添加图书

- 获取用户输入的数据   发送到后台
- 渲染最新的数据到页面上

```js
 methods: {
    handle: async function(){
          if(this.flag) {
            // 编辑图书
            // 就是根据当前的ID去更新数组中对应的数据
            this.books.some((item) => {
              if(item.id == this.id) {
                item.name = this.name;
                // 完成更新操作之后，需要终止循环
                return true;
              }
            });
            this.flag = false;
          }else{
            # 1.1  在前面封装好的 handle 方法中  发送ajax请求  
            # 1.2  使用async  和 await 简化操作 需要在 function 前面添加 async   
            var ret = await axios.post('books', {
              name: this.name
            })
            # 1.3  根据后台返回的状态码判断是否加载数据 
            if(ret.status == 200) {
             # 1.4  调用 queryData 这个方法  渲染最新的数据 
              this.queryData();
            }
          }
          // 清空表单
          this.id = '';
          this.name = '';
        },        
 }         
```

#### 3  验证图书名称是否存在

- 添加图书之前发送请求验证图示是否已经存在
- 如果不存在 往后台里面添加图书名称
  - 图书存在与否只需要修改submitFlag的值即可

```js
 watch: {
        name: async function(val) {
          // 验证图书名称是否已经存在
          // var flag = this.books.some(function(item){
          //   return item.name == val;
          // });
          var ret = await axios.get('/books/book/' + this.name);
          if(ret.status == 1) {
            // 图书名称存在
            this.submitFlag = true;
          }else{
            // 图书名称不存在
            this.submitFlag = false;
          }
        }
},
```

#### 4.  编辑图书

- 根据当前书的id 查询需要编辑的书籍
- 需要根据状态位判断是添加还是编辑 

```js
 methods: {
        handle: async function(){
          if(this.flag) {
            #4.3 编辑图书   把用户输入的信息提交到后台
            var ret = await axios.put('books/' + this.id, {
              name: this.name
            });
            if(ret.status == 200){
              #4.4  完成添加后 重新加载列表数据
              this.queryData();
            }
            this.flag = false;
          }else{
            // 添加图书
            var ret = await axios.post('books', {
              name: this.name
            })
            if(ret.status == 200) {
              // 重新加载列表数据
              this.queryData();
            }
          }
          // 清空表单
          this.id = '';
          this.name = '';
        },
        toEdit: async function(id){
          #4.1  flag状态位用于区分编辑和添加操作
          this.flag = true;
          #4.2  根据id查询出对应的图书信息  页面中可以加载出来最新的信息
          # 调用接口发送ajax 请求  
          var ret = await axios.get('books/' + id);
          this.id = ret.id;
          this.name = ret.name;
        },
```

####   5 删除图书

- 把需要删除的id书籍 通过参数的形式传递到后台

```js
   deleteBook: async function(id){
          // 删除图书
          var ret = await axios.delete('books/' + id);
          if(ret.status == 200) {
            // 重新加载列表数据
            this.queryData();
          }
   }
```

## 路由

###1.路由的概念

路由的本质就是一种对应关系，比如说我们在url地址中输入我们要访问的url地址之后，浏览器要去请求这个url地址对应的资源。
那么url地址和真实的资源之间就有一种对应的关系，就是路由。

路由分为前端路由和后端路由
1).后端路由是由服务器端进行实现，并完成资源的分发
2).前端路由是依靠hash值(锚链接)的变化进行实现 

后端路由性能相对前端路由来说较低，所以，我们接下来主要学习的是前端路由
前端路由的基本概念：根据不同的事件来显示不同的页面内容，即事件与事件处理函数之间的对应关系
前端路由主要做的事情就是监听事件并分发执行事件处理函数

###2.前端路由的初体验

前端路由是基于hash值的变化进行实现的（比如点击页面中的菜单或者按钮改变URL的hash值，根据hash值的变化来控制组件的切换）
核心实现依靠一个事件，即监听hash值变化的事件。

```javascript
window.onhashchange = function(){
    //location.hash可以获取到最新的hash值
    location.hash
}
```

前端路由实现tab栏切换：

```html
<!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <meta http-equiv="X-UA-Compatible" content="ie=edge" />
        <title>Document</title>
        <!-- 导入 vue 文件 -->
        <script src="./lib/vue_2.5.22.js"></script>
    </head>
    <body>
        <!-- 被 vue 实例控制的 div 区域 -->
        <div id="app">
        <!-- 切换组件的超链接 -->
        <a href="#/zhuye">主页</a> 
        <a href="#/keji">科技</a> 
        <a href="#/caijing">财经</a>
        <a href="#/yule">娱乐</a>

        <!-- 根据 :is 属性指定的组件名称，把对应的组件渲染到 component 标签所在的位置 -->
        <!-- 可以把 component 标签当做是【组件的占位符】 -->
        <component :is="comName"></component>
        </div>

        <script>
        // #region 定义需要被切换的 4 个组件
        // 主页组件
        const zhuye = {
            template: '<h1>主页信息</h1>'
        }

        // 科技组件
        const keji = {
            template: '<h1>科技信息</h1>'
        }

        // 财经组件
        const caijing = {
            template: '<h1>财经信息</h1>'
        }

        // 娱乐组件
        const yule = {
            template: '<h1>娱乐信息</h1>'
        }
        // #endregion

        // #region vue 实例对象
        const vm = new Vue({
            el: '#app',
            data: {
            comName: 'zhuye'
            },
            // 注册私有组件
            components: {
            zhuye,
            keji,
            caijing,
            yule
            }
        })
        // #endregion

        // 监听 window 的 onhashchange 事件，根据获取到的最新的 hash 值，切换要显示的组件的名称
        window.onhashchange = function() {
            // 通过 location.hash 获取到最新的 hash 值
            console.log(location.hash);
            switch(location.hash.slice(1)){
            case '/zhuye':
                vm.comName = 'zhuye'
            break
            case '/keji':
                vm.comName = 'keji'
            break
            case '/caijing':
                vm.comName = 'caijing'
            break
            case '/yule':
                vm.comName = 'yule'
            break
            }
        }
        </script>
    </body>
    </html>
```

案例效果图：

![01前端路由(/images/vue.assets/01前端路由(1).png)](17-21 Vue.js项目实战开发/20-21vue电商/1.vue-路由/笔记/images/01前端路由(1).png)

点击每个超链接之后，会进行相应的内容切换，如下：

![](/images/vue.assets/01前端路由效果图.png)

核心思路：
在页面中有一个vue实例对象，vue实例对象中有四个组件，分别是tab栏切换需要显示的组件内容
在页面中有四个超链接，如下：

```
<a href="#/zhuye">主页</a> 
<a href="#/keji">科技</a> 
<a href="#/caijing">财经</a>
<a href="#/yule">娱乐</a>
```

当我们点击这些超链接的时候，就会改变url地址中的hash值，当hash值被改变时，就会触发onhashchange事件
在触发onhashchange事件的时候，我们根据hash值来让不同的组件进行显示：

```javascript
window.onhashchange = function() {
    // 通过 location.hash 获取到最新的 hash 值
    console.log(location.hash);
    switch(location.hash.slice(1)){
        case '/zhuye':
        //通过更改数据comName来指定显示的组件
        //因为 <component :is="comName"></component> ，组件已经绑定了comName
        vm.comName = 'zhuye'
        break
        case '/keji':
        vm.comName = 'keji'
        break
        case '/caijing':
        vm.comName = 'caijing'
        break
        case '/yule':
        vm.comName = 'yule'
        break
    }
}
```

###3.Vue Router简介

它是一个Vue.js官方提供的路由管理器。是一个功能更加强大的前端路由器，推荐使用。
Vue Router和Vue.js非常契合，可以一起方便的实现SPA(single page web application,单页应用程序)应用程序的开发。
Vue Router依赖于Vue，所以需要先引入Vue，再引入Vue Router.

Vue Router的特性：
支持H5历史模式或者hash模式
支持嵌套路由
支持路由参数
支持编程式路由
支持命名路由
支持路由导航守卫
支持路由过渡动画特效
支持路由懒加载
支持路由滚动行为

###4.Vue Router的使用步骤(★★★)

A.导入js文件

`\<script src="lib/vue_2.5.22.js”>\</script>`
\`<script src="lib/vue-router_3.0.2.js”>\</script>`

B.添加路由链接:\<router-link>是路由中提供的标签，默认会被渲染为a标签，to属性默认被渲染为href属性，to属性的值会被渲染为#开头的hash地址
\<router-link to="/user”>User\</router-link>
\<router-link to="/login”>Login\</router-link>
C.添加路由填充位（路由占位符）
\<router-view>\</router-view>
D.定义路由组件
var User = { template:”\<div>This is User\</div>" }
var Login = { template:”\<div>This is Login\</div>" }
E.配置路由规则并创建路由实例
var myRouter = new VueRouter({
    //routes是路由规则数组
    routes:[
        //每一个路由规则都是一个对象，对象中至少包含path和component两个属性
        //path表示  路由匹配的hash地址，component表示路由规则对应要展示的组件对象
        {path:"/user",component:User},
        {path:"/login",component:Login}
    ]
})
F.将路由挂载到Vue实例中
new Vue({
    el:"#app",
    //通过router属性挂载路由对象
    router:myRouter
})

小结：
Vue Router的使用步骤还是比较清晰的，按照步骤一步一步就能完成路由操作
A.导入js文件
B.添加路由链接
C.添加路由占位符(最后路由展示的组件就会在占位符的位置显示)
D.定义路由组件
E.配置路由规则并创建路由实例
F.将路由挂载到Vue实例中

补充：
路由重定向：可以通过路由重定向为页面设置默认展示的组件
在路由规则中添加一条路由规则即可，如下：
var myRouter = new VueRouter({
    //routes是路由规则数组
    routes: [
    //path设置为/表示页面最初始的地址 / ,redirect表示要被重定向的新地址，设置为一个路由即可
        { path:"/",redirect:"/user"},
        { path: "/user", component: User },
        { path: "/login", component: Login }
    ]
})

###5.嵌套路由，动态路由的实现方式

####A.嵌套路由的概念(★★★)

当我们进行路由的时候显示的组件中还有新的子级路由链接以及内容。

嵌套路由最关键的代码在于理解子级路由的概念：
比如我们有一个/login的路由
那么/login下面还可以添加子级路由，如:
/login/account
/login/phone

参考代码如下：

```javascript
var User = { template: "<div>This is User</div>" }
//Login组件中的模板代码里面包含了子级路由链接以及子级路由的占位符
var Login = { template: `<div>
    <h1>This is Login</h1>
    <hr>
    <router-link to="/login/account">账号密码登录</router-link>
    <router-link to="/login/phone">扫码登录</router-link>
    <!-- 子路由组件将会在router-view中显示 -->
    <router-view></router-view>
    </div>` }
//定义两个子级路由组件
var account = { template:"<div>账号：<input><br>密码：<input></div>"};
var phone = { template:"<h1>扫我二维码</h1>"};
var myRouter = new VueRouter({
    //routes是路由规则数组
    routes: [
        { path:"/",redirect:"/user"},
        { path: "/user", component: User },
        { 
            path: "/login", 
            component: Login,
            //通过children属性为/login添加子路由规则
            children:[
                { path: "/login/account", component: account },
                { path: "/login/phone", component: phone },
            ]
        }
    ]
})
var vm = new Vue({
    el: '#app',
    data: {},
    methods: {},
    router:myRouter
});
```

页面效果大致如下：

![](/images/vue.assets/03嵌套路由.png)

####B.动态路由匹配(★★★)

`var User = { template:"<div>用户：{{$route.params.id}}</div>"}`

```javascript
var myRouter = new VueRouter({
    //routes是路由规则数组
    routes: [
        //通过/:参数名  的形式传递参数 
        { path: "/user/:id", component: User },
    ]

})
```

补充：
如果使用$route.params.id来获取路径传参的数据不够灵活。
1.我们可以通过props来接收参数

```javascript
var User = { 
    props:["id"],
    template:”\div>用户：{{id}}\</div>"
}

var myRouter = new VueRouter({
    //routes是路由规则数组
    routes: [
        //通过/:参数名  的形式传递参数 
        //如果props设置为true，route.params将会被设置为组件属性
        { path: "/user/:id", component: User,props:true },
    ]

})
```

2.还有一种情况，我们可以将props设置为对象，那么就直接将对象的数据传递给
组件进行使用

```javascript
var User = { 
    props:["username","pwd"],
    template:"<div>用户：{{username}}---{{pwd}}</div>"
    }

var myRouter = new VueRouter({
    //routes是路由规则数组
    routes: [
        //通过/:参数名  的形式传递参数 
        //如果props设置为对象，则传递的是对象中的数据给组件
        { path: "/user/:id", component: User,props:{username:"jack",pwd:123} },
    ]

})
```

3.如果想要获取传递的参数值还想要获取传递的对象数据，那么props应该设置为
函数形式。

```javascript
var User = { 
    props:["username","pwd","id"],
    template:"<div>用户：{{id}} -> {{username}}---{{pwd}}</div>"
    }

var myRouter = new VueRouter({
    //routes是路由规则数组
    routes: [
        //通过/:参数名  的形式传递参数 
        //如果props设置为函数，则通过函数的第一个参数获取路由对象
        //并可以通过路由对象的params属性获取传递的参数
        //
        { path: "/user/:id", component: User,props:(route)=>{
            return {username:"jack",pwd:123,id:route.params.id}
            } 
        },
    ]

})
```


###7.命名路由以及编程式导航

####A.命名路由：给路由取别名

案例：

```javascript
var myRouter = new VueRouter({
    //routes是路由规则数组
    routes: [
        //通过name属性为路由添加一个别名
        { path: "/user/:id", component: User, name:"user"},
    ]

})
```

//添加了别名之后，可以使用别名进行跳转
\<router-link to="/user”>User\</router-link>
\<router-link :to="{ name:'user' , params: {id:123} }”>User\</router-link>

//还可以编程式导航
myRouter.push( { name:'user' , params: {id:123} } )

####B.编程式导航(★★★)

页面导航的两种方式：
A.声明式导航：通过点击链接的方式实现的导航
B.编程式导航：调用js的api方法实现导航

Vue-Router中常见的导航方式：

```javascript
this.$router.push("hash地址");
this.$router.push("/login");
this.$router.push({ name:'user' , params: {id:123} });
this.$router.push({ path:"/login" });
this.$router.push({ path:"/login",query:{username:"jack"} });

this.$router.go( n );//n为数字，参考history.go
this.$router.go( -1 );
```

###8.实现后台管理案例(★★★)

案例效果：

![](/images/vue.assets/02后台管理系统-4006815.png)

点击左侧的"用户管理","权限管理","商品管理","订单管理","系统设置"都会出现对应的组件并展示内容

其中"用户管理"组件展示的效果如上图所示，在用户管理区域中的详情链接也是可以点击的，点击之后将会显示用户详情信息。

案例思路：
1).先将素材文件夹中的11.基于vue-router的案例.html复制到我们自己的文件夹中。
看一下这个文件中的代码编写了一些什么内容，
这个页面已经把后台管理页面的基本布局实现了
2).在页面中引入vue，vue-router
3).创建Vue实例对象，准备开始编写代码实现功能
4).希望是通过组件的形式展示页面的主体内容，而不是写死页面结构，所以我们可以定义一个根组件：

```javascript
//只需要把原本页面中的html代码设置为组件中的模板内容即可
const app = {
    template:`<div>
        <!-- 头部区域 -->
        <header class="header">传智后台管理系统</header>
        <!-- 中间主体区域 -->
        <div class="main">
          <!-- 左侧菜单栏 -->
          <div class="content left">
            <ul>
              <li>用户管理</li>
              <li>权限管理</li>
              <li>商品管理</li>
              <li>订单管理</li>
              <li>系统设置</li>
            </ul>
          </div>
          <!-- 右侧内容区域 -->
          <div class="content right">
            <div class="main-content">添加用户表单</div>
          </div>
        </div>
        <!-- 尾部区域 -->
        <footer class="footer">版权信息</footer>
      </div>`
  }
```

5).当我们访问页面的时候，默认需要展示刚刚创建的app根组件，我们可以
创建一个路由对象来完成这个事情,然后将路由挂载到Vue实例对象中即可

```javascript
const myRouter = new VueRouter({
    routes:[
        {path:"/",component:app}
    ]
})

const vm = new Vue({
    el:"#app",
    data:{},
    methods:{},
    router:myRouter
})
```

补充：到此为止，基本的js代码都处理完毕了，我们还需要设置一个路由占位符

```html
<body>
  <div id="app">
    <router-view></router-view>
  </div>
</body>
```

6).此时我们打开页面应该就可以得到一个VueRouter路由出来的根组件了
我们需要在这个根组件中继续路由实现其他的功能子组件
先让我们更改根组件中的模板：更改左侧li为子级路由链接，并在右侧内容区域添加子级组件占位符

```javascript
const app = {
    template:`<div>
        ........
        <div class="main">
          <!-- 左侧菜单栏 -->
          <div class="content left">
            <ul>
              <!-- 注意：我们把所有li都修改为了路由链接 -->
              <li><router-link to="/users">用户管理</router-link></li>
              <li><router-link to="/accesses">权限管理</router-link></li>
              <li><router-link to="/goods">商品管理</router-link></li>
              <li><router-link to="/orders">订单管理</router-link></li>
              <li><router-link to="/systems">系统设置</router-link></li>
            </ul>
          </div>
          <!-- 右侧内容区域 -->
          <div class="content right">
            <div class="main-content">
                <!-- 在 -->
                <router-view></router-view> 
            </div>
          </div>
        </div>
        .......
      </div>`
  }
```

然后，我们要为子级路由创建并设置需要显示的子级组件

```html
//建议创建的组件首字母大写，和其他内容区分
const Users = {template:`<div>
    <h3>用户管理</h3>
</div>`}
const Access = {template:`<div>
    <h3>权限管理</h3>
</div>`}
const Goods = {template:`<div>
    <h3>商品管理</h3>
</div>`}
const Orders = {template:`<div>
    <h3>订单管理</h3>
</div>`}
const Systems = {template:`<div>
    <h3>系统管理</h3>
</div>`}

//添加子组件的路由规则
const myRouter = new VueRouter({
    routes:[
        {path:"/",component:app , children:[
            { path:"/users",component:Users },
            { path:"/accesses",component:Access },
            { path:"/goods",component:Goods },
            { path:"/orders",component:Orders },
            { path:"/systems",component:Systems },
        ]}
    ]
})

const vm = new Vue({
    el:"#app",
    data:{},
    methods:{},
    router:myRouter
})
```

7).展示用户信息列表：
    A.为Users组件添加私有数据,并在模板中循环展示私有数据
    ​```
    const Users = {
    data(){
        return {
            userList:[
                {id:1,name:"zs",age:18},
                {id:2,name:"ls",age:19},
                {id:3,name:"wang",age:20},
                {id:4,name:"jack",age:21},
            ]
        }
    },
    template:`<div>

        <h3>用户管理</h3>
        <table>
            <thead>
                <tr>
                    <th>编号</th>
                    <th>姓名</th>
                    <th>年龄</th>
                    <th>操作</th>
                </tr>
            </thead>
            <tbody>
                <tr :key="item.id" v-for="item in userList">
                    <td>{{item.id}}</td>
                    <td>{{item.name}}</td>
                    <td>{{item.age}}</td>
                    <td><a href="javascript:;">详情</a></td>
                </tr>
            </tbody>
        </table>

​    </div>`}
​    ​```

8.当用户列表展示完毕之后，我们可以点击列表中的详情来显示用户详情信息，首先我们需要创建一个组件，用来展示详情信息

```
const UserInfo = {
    props:["id"],
    template:`<div>
      <h5>用户详情</h5>
      <p>查看 {{id}} 号用户信息</p>
      <button @click="goBack">返回用户详情页</button>
    </div> `,
    methods:{
      goBack(){
        //当用户点击按钮，后退一页
        this.$router.go(-1);
      }
    }
  }
```

然后我们需要设置这个组件的路由规则

```
const myRouter = new VueRouter({
    routes:[
        {path:"/",component:app , children:[
            { path:"/users",component:Users },
            //添加一个/userinfo的路由规则
            { path:"/userinfo/:id",component:UserInfo,props:true},
            { path:"/accesses",component:Access },
            { path:"/goods",component:Goods },
            { path:"/orders",component:Orders },
            { path:"/systems",component:Systems },
        ]}
    ]
})

const vm = new Vue({
    el:"#app",
    data:{},
    methods:{},
    router:myRouter
})
```

再接着给用户列表中的详情a连接添加事件

```javascript
const Users = {
    data(){
        return {
            userList:[
                {id:1,name:"zs",age:18},
                {id:2,name:"ls",age:19},
                {id:3,name:"wang",age:20},
                {id:4,name:"jack",age:21},
            ]
        }
    },
    template:`<div>
        <h3>用户管理</h3>
        <table>
            <thead>
                <tr>
                    <th>编号</th>
                    <th>姓名</th>
                    <th>年龄</th>
                    <th>操作</th>
                </tr>
            </thead>
            <tbody>
                <tr :key="item.id" v-for="item in userList">
                    <td>{{item.id}}</td>
                    <td>{{item.name}}</td>
                    <td>{{item.age}}</td>
                    <td><a href="javascript:;" @click="goDetail(item.id)">详情</a></td>
                </tr>
            </tbody>
        </table>
    </div>`,
    methods:{
        goDetail(id){
            this.$router.push("/userinfo/"+id);
        }
    }
}
```

## Vue工程化

###1.模块化的分类

####A.浏览器端的模块化

```txt
1).AMD(Asynchronous Module Definition,异步模块定义)
代表产品为：Require.js
2).CMD(Common Module Definition,通用模块定义)
代表产品为：Sea.js
```

####B.服务器端的模块化

    服务器端的模块化规范是使用CommonJS规范：
    1).使用require引入其他模块或者包
    2).使用exports或者module.exports导出模块成员
    3).一个文件就是一个模块，都拥有独立的作用域

####C.ES6模块化

    ES6模块化规范中定义：
        1).每一个js文件都是独立的模块
        2).导入模块成员使用import关键字
        3).暴露模块成员使用export关键字

小结：推荐使用ES6模块化，因为AMD，CMD局限使用与浏览器端，而CommonJS在服务器端使用。
      ES6模块化是浏览器端和服务器端通用的规范.

###2.在NodeJS中安装babel

####A.安装babel

    打开终端，输入命令：npm install --save-dev @babel/core @babel/cli @babel/preset-env @babel/node
    安装完毕之后，再次输入命令安装：npm install --save @babel/polyfill

####B.创建babel.config.js

    在项目目录中创建babel.config.js文件。
    编辑js文件中的代码如下：
        const presets = [
            ["@babel/env",{
                targets:{
                    edge:"17",
                    firefox:"60",
                    chrome:"67",
                    safari:"11.1"
                }
            }]
        ]
        //暴露
        module.exports = { presets }

####C.创建index.js文件

    在项目目录中创建index.js文件作为入口文件
    在index.js中输入需要执行的js代码，例如：
        console.log("ok");

####D.使用npx执行文件

    打开终端，输入命令：npx babel-node ./index.js

###3.设置默认导入/导出

####A.默认导出

    export default {
        成员A,
        成员B,
        .......
    },如下：
    let num = 100;
    export default{
        num
    }

####B.默认导入

    import 接收名称 from "模块标识符"，如下：
    import test from "./test.js"

注意：在一个模块中，只允许使用export default向外默认暴露一次成员，千万不要写多个export default。
如果在一个模块中没有向外暴露成员，其他模块引入该模块时将会得到一个**空对象**. 

###4.设置按需导入/导出

####A.按需导出

```javascript
export let num = 998;
export let myName = "jack";
export function fn = function(){ console.log("fn") }
```

####B.按需导入 

```javascript
import { num,fn as printFn ,myName } from "./test.js"
//同时导入默认导出的成员以及按需导入的成员
import test,{ num,fn as printFn ,myName } from "./test.js"
```

注意：一个模块中既可以按需导入也可以默认导入，一个模块中既可以按需导出也可以默认导出

###5.直接导入并执行代码

```javascript
import "./test2.js";
```

###6.webpack的概念

webpack是一个流行的前端项目构建工具，可以解决目前web开发的困境。
webpack提供了模块化支持，代码压缩混淆，解决js兼容问题，性能优化等特性，提高了开发效率和项目的可维护性

###7.webpack的基本使用

####A.创建项目目录并初始化

    创建项目，并打开项目所在目录的终端，输入命令：
    npm init -y

####B.创建首页及js文件

    在项目目录中创建index.html页面，并初始化页面结构：在页面中摆放一个ul，ul里面放置几个li
    在项目目录中创建js文件夹，并在文件夹中创建index.js文件

####C.安装jQuery

    打开项目目录终端，输入命令:
    npm install jQuery -S

####D.导入jQuery

```javascript
//打开index.js文件，编写代码导入jQuery并实现功能：
import $ from "jquery";
$(function(){
    $("li:odd").css("background","cyan");
    $("li:odd").css("background","pink");
})
```

注意：此时项目运行会有错误，因为import $ from "jquery";这句代码属于ES6的新语法代码，在浏览器中可能会存在兼容性问题
所以我们需要webpack来帮助我们解决这个问题。

####E.安装webpack

```javascript
1).打开项目目录终端，输入命令:
npm install webpack webpack-cli -D
2).然后在项目根目录中，创建一个 webpack.config.js 的配置文件用来配置webpack
在 webpack.config.js 文件中编写代码进行webpack配置，如下：
module.exports = {
    mode:"development"//可以设置为development(开发模式)，production(发布模式)
}
补充：mode设置的是项目的编译模式。
如果设置为development则表示项目处于开发阶段，不会进行压缩和混淆，打包速度会快一些
如果设置为production则表示项目处于上线发布阶段，会进行压缩和混淆，打包速度会慢一些
3).修改项目中的package.json文件添加运行脚本dev，如下：
"scripts":{
    "dev":"webpack"
}
注意：scripts节点下的脚本，可以通过 npm run 运行，如：
运行终端命令：npm run dev
将会启动webpack进行项目打包
4).运行dev命令进行项目打包，并在页面中引入项目打包生成的js文件
打开项目目录终端，输入命令:
npm run dev
等待webpack打包完毕之后，找到默认的dist路径中生成的main.js文件，将其引入到html页面中。
浏览页面查看效果。
```

###8.设置webpack的打包入口/出口

```javascript
在webpack 4.x中，默认会将src/index.js 作为默认的打包入口js文件
                 默认会将dist/main.js 作为默认的打包输出js文件
如果不想使用默认的入口/出口js文件，我们可以通过改变 webpack.config.js 来设置入口/出口的js文件，如下：
const path = require("path");
module.exports = {
    mode:"development",
    //设置入口文件路径
    entry: path.join(__dirname,"./src/xx.js"),
    //设置出口文件
    output:{
        //设置路径
        path:path.join(__dirname,"./dist"),
        //设置文件名
        filename:"res.js"
    }
}
```

###9.设置webpack的自动打包

```javascript
默认情况下，我们更改入口js文件的代码，需要重新运行命令打包webpack，才能生成出口的js文件
那么每次都要重新执行命令打包，这是一个非常繁琐的事情，那么，自动打包可以解决这样繁琐的操作。
实现自动打包功能的步骤如下：
    A.安装自动打包功能的包:webpack-dev-server
        npm install webpack-dev-server -D
    B.修改package.json中的dev指令如下：
        "scripts":{
            "dev":"webpack-dev-server"
        }
    C.将引入的js文件路径更改为：<script src="/bundle.js"></script>
    D.运行npm run dev，进行打包
    E.打开网址查看效果：http://localhost:8080

注意：webpack-dev-server自动打包的输出文件，默认放到了服务器的根目录中.
```

补充：
在自动打包完毕之后，默认打开服务器网页，实现方式就是打开package.json文件，修改dev命令：
    "dev": "webpack-dev-server --open --host 127.0.0.1 --port 9999"

###10.配置html-webpack-plugin

```javascript
使用html-webpack-plugin 可以生成一个预览页面。
因为当我们访问默认的 http://localhost:8080/的时候，看到的是一些文件和文件夹，想要查看我们的页面
还需要点击文件夹点击文件才能查看，那么我们希望默认就能看到一个页面，而不是看到文件夹或者目录。
实现默认预览页面功能的步骤如下：
    A.安装默认预览功能的包:html-webpack-plugin
        npm install html-webpack-plugin -D
    B.修改webpack.config.js文件，如下：
        //导入包
        const HtmlWebpackPlugin = require("html-webpack-plugin");
        //创建对象
        const htmlPlugin = new HtmlWebpackPlugin({
            //设置生成预览页面的模板文件
            template:"./src/index.html",
            //设置生成的预览页面名称
            filename:"index.html"
        })
    C.继续修改webpack.config.js文件，添加plugins信息：
        module.exports = {
            ......
            plugins:[ htmlPlugin ]
        }
```

###11.webpack中的加载器

```javascript
通过loader打包非js模块：默认情况下，webpack只能打包js文件，如果想要打包非js文件，需要调用loader加载器才能打包
    loader加载器包含：
        1).less-loader
        2).sass-loader
        3).url-loader:打包处理css中与url路径有关的文件
        4).babel-loader:处理高级js语法的加载器
        5).postcss-loader
        6).css-loader,style-loader

注意：指定多个loader时的顺序是固定的，而调用loader的顺序是从后向前进行调用

A.安装style-loader,css-loader来处理样式文件
    1).安装包
        npm install style-loader css-loader -D
    2).配置规则：更改webpack.config.js的module中的rules数组
    module.exports = {
        ......
        plugins:[ htmlPlugin ],
        module : {
            rules:[
                {
                    //test设置需要匹配的文件类型，支持正则
                    test:/\.css$/,
                    //use表示该文件类型需要调用的loader
                    use:['style-loader','css-loader']
                }
            ]
        }
    }
B.安装less,less-loader处理less文件
    1).安装包
        npm install less-loader less -D
    2).配置规则：更改webpack.config.js的module中的rules数组
    module.exports = {
        ......
        plugins:[ htmlPlugin ],
        module : {
            rules:[
                {
                    //test设置需要匹配的文件类型，支持正则
                    test:/\.css$/,
                    //use表示该文件类型需要调用的loader
                    use:['style-loader','css-loader']
                },
                {
                    test:/\.less$/,
                    use:['style-loader','css-loader','less-loader']
                }
            ]
        }
    }
C.安装sass-loader,node-sass处理less文件
    1).安装包
        npm install sass-loader node-sass -D
    2).配置规则：更改webpack.config.js的module中的rules数组
    module.exports = {
        ......
        plugins:[ htmlPlugin ],
        module : {
            rules:[
                {
                    //test设置需要匹配的文件类型，支持正则
                    test:/\.css$/,
                    //use表示该文件类型需要调用的loader
                    use:['style-loader','css-loader']
                },
                {
                    test:/\.less$/,
                    use:['style-loader','css-loader','less-loader']
                },
                {
                    test:/\.scss$/,
                    use:['style-loader','css-loader','sass-loader']
                }
            ]
        }
    }

    补充：安装sass-loader失败时，大部分情况是因为网络原因，详情参考：
    https://segmentfault.com/a/1190000010984731?utm_source=tag-newest

D.安装post-css自动添加css的兼容性前缀（-ie-,-webkit-）
1).安装包
    npm install postcss-loader autoprefixer -D
2).在项目根目录创建并配置postcss.config.js文件
const autoprefixer = require("autoprefixer");
module.exports = {
    plugins:[ autoprefixer ]
}
3).配置规则：更改webpack.config.js的module中的rules数组
module.exports = {
    ......
    plugins:[ htmlPlugin ],
    module : {
        rules:[
            {
                //test设置需要匹配的文件类型，支持正则
                test:/\.css$/,
                //use表示该文件类型需要调用的loader
                use:['style-loader','css-loader','postcss-loader']
            },
            {
                test:/\.less$/,
                use:['style-loader','css-loader','less-loader']
            },
            {
                test:/\.scss$/,
                use:['style-loader','css-loader','sass-loader']
            }
        ]
    }
}

E.打包样式表中的图片以及字体文件
在样式表css中有时候会设置背景图片和设置字体文件，一样需要loader进行处理
使用url-loader和file-loader来处理打包图片文件以及字体文件
1).安装包
    npm install url-loader file-loader -D
2).配置规则：更改webpack.config.js的module中的rules数组
module.exports = {
    ......
    plugins:[ htmlPlugin ],
    module : {
        rules:[
            {
                //test设置需要匹配的文件类型，支持正则
                test:/\.css$/,
                //use表示该文件类型需要调用的loader
                use:['style-loader','css-loader']
            },
            {
                test:/\.less$/,
                use:['style-loader','css-loader','less-loader']
            },
            {
                test:/\.scss$/,
                use:['style-loader','css-loader','sass-loader']
            },{
                test:/\.jpg|png|gif|bmp|ttf|eot|svg|woff|woff2$/,
                //limit用来设置字节数，只有小于limit值的图片，才会转换
                //为base64图片
                use:"url-loader?limit=16940"
            }
        ]
    }
}

F.打包js文件中的高级语法：在编写js的时候，有时候我们会使用高版本的js语法
有可能这些高版本的语法不被兼容，我们需要将之打包为兼容性的js代码
我们需要安装babel系列的包
A.安装babel转换器
    npm install babel-loader @babel/core @babel/runtime -D
B.安装babel语法插件包
    npm install @babel/preset-env @babel/plugin-transform-runtime @babel/plugin-proposal-class-properties -D
C.在项目根目录创建并配置babel.config.js文件
    
    module.exports = {
        presets:["@babel/preset-env"],
        plugins:[ "@babel/plugin-transform-runtime", "@babel/plugin-proposal-class-properties" ]
    }
D.配置规则：更改webpack.config.js的module中的rules数组
module.exports = {
    ......
    plugins:[ htmlPlugin ],
    module : {
        rules:[
            {
                //test设置需要匹配的文件类型，支持正则
                test:/\.css$/,
                //use表示该文件类型需要调用的loader
                use:['style-loader','css-loader']
            },
            {
                test:/\.less$/,
                use:['style-loader','css-loader','less-loader']
            },
            {
                test:/\.scss$/,
                use:['style-loader','css-loader','sass-loader']
            },{
                test:/\.jpg|png|gif|bmp|ttf|eot|svg|woff|woff2$/,
                //limit用来设置字节数，只有小于limit值的图片，才会转换
                //为base64图片
                use:"url-loader?limit=16940"
            },{
                test:/\.js$/,
                use:"babel-loader",
                //exclude为排除项，意思是不要处理node_modules中的js文件
                exclude:/node_modules/
            }
        ]
    }
}
```

###12.Vue单文件组件

传统Vue组件的缺陷：
全局定义的组件不能重名，字符串模板缺乏语法高亮，不支持css(当html和js组件化时，css没有参与其中)
没有构建步骤限制，只能使用H5和ES5，不能使用预处理器（babel）
解决方案：
使用Vue单文件组件，每个单文件组件的后缀名都是.vue
每一个Vue单文件组件都由三部分组成
1).template组件组成的模板区域
2).script组成的业务逻辑区域
3).style样式区域

代码如下：

```vue
<template>
    组件代码区域
</template>

<script>
    js代码区域
</script>

<style scoped>
    样式代码区域
</style>
```


补充：安装Vetur插件可以使得.vue文件中的代码高亮

配置.vue文件的加载器
A.安装vue组件的加载器
    npm install vue-loader vue-template-compiler -D
B.配置规则：更改webpack.config.js的module中的rules数组               

```javascript
const VueLoaderPlugin = require("vue-loader/lib/plugin");
const vuePlugin = new VueLoaderPlugin();
module.exports = {
    ......
    plugins:[ htmlPlugin, vuePlugin  ],
    module : {
        rules:[
            ...//其他规则
            { 
                test:/\.vue$/,
                loader:"vue-loader",
       			}
    		]
    }
}
```

###13.在webpack中使用vue

上一节我们安装处理了vue单文件组件的加载器，想要让vue单文件组件能够使用，我们必须要安装vue
并使用vue来引用vue单文件组件。
A.安装Vue
    `npm install vue -S`
B.在index.js中引入vue：`import Vue from "vue"`
C.创建Vue实例对象并指定el，最后使用render函数渲染单文件组件

```javascript
const vm = new Vue({
	el:"#first",
	render:h=>h(app)
})
```

###14.使用webpack打包发布项目

在项目上线之前，我们需要将整个项目打包并发布。
A.配置package.json

```json
"scripts":{
    "dev":"webpack-dev-server",
    "build":"webpack -p"
}
```

B.在项目打包之前，可以将dist目录删除，生成全新的dist目录

###15.Vue脚手架

Vue脚手架可以快速生成Vue项目基础的架构。
A.安装3.x版本的Vue脚手架：
    `npm install -g @vue/cli`
B.基于3.x版本的脚手架创建Vue项目：
    1).使用命令创建Vue项目
        命令：`vue create my-project`
        选择Manually select features(选择特性以创建项目)
        勾选特性可以用空格进行勾选。
        是否选用历史模式的路由：n
        ESLint选择：ESLint + Standard config
        何时进行ESLint语法校验：Lint on save
        babel，postcss等配置文件如何放置：In dedicated config files(单独使用文件进行配置)
        是否保存为模板：n
        使用哪个工具安装包：npm
    2).基于ui界面创建Vue项目
        命令：`vue ui`
        在自动打开的创建项目网页中配置项目信息。
    3).基于2.x的旧模板，创建Vue项目
        `npm install -g @vue/cli-init`
        `vue init webpack my-project`

C.分析Vue脚手架生成的项目结构
    node_modules:依赖包目录
    public：静态资源目录
    src：源码目录
    src/assets:资源目录
    src/components：组件目录
    src/views:视图组件目录
    src/App.vue:根组件
    src/main.js:入口js
    src/router.js:路由js
    babel.config.js:babel配置文件
    .eslintrc.js:

###16.Vue脚手架的自定义配置

```json
A.通过 package.json 进行配置 [不推荐使用]
    "vue":{
        "devServer":{
            "port":"9990",
            "open":true
        }
    }
B.通过单独的配置文件进行配置，创建vue.config.js
    module.exports = {
        devServer:{
            port:8888,
            open:true
        }
    }
```

## Vuex

###1.Vuex概述

Vuex是实现组件全局状态（数据）管理的一种机制，可以方便的实现组件之间的数据共享

使用Vuex管理数据的好处：
A.能够在vuex中集中管理共享的数据，便于开发和后期进行维护
B.能够高效的实现组件之间的数据共享，提高开发效率
C.存储在vuex中的数据是响应式的，当数据发生改变时，页面中的数据也会同步更新

###2.Vuex的基本使用

创建带有vuex的vue项目，打开终端，输入命令：vue ui
当项目仪表盘打开之后，我们点击页面左上角的项目管理下拉列表，再点击Vue项目管理器
点击创建项目，如下图所示
第一步，设置项目名称和包管理器
![](/images/vue.assets/创建vuex项目01.png)
第二步，设置手动配置项目
![](/images/vue.assets/创建vuex项目02.png)
第三步，设置功能项
![](/images/vue.assets/创建vuex项目03.png)
![创建vuex项目04(/images/vue.assets/创建vuex项目04(1).png)](17-21 Vue.js项目实战开发/20-21vue电商/10.vuex/笔记/images/创建vuex项目04(1).png)
第四步，创建项目
![](/images/vue.assets/创建vuex项目05.png)

###3.使用Vuex完成计数器案例

打开刚刚创建的vuex项目，找到src目录中的App.vue组件，将代码重新编写如下：

```vue
<template>
  <div>
    <my-addition></my-addition>
    <p>----------------------------------------</p>
    <my-subtraction></my-subtraction>
  </div>
</template>

<script>
import Addition from './components/Addition.vue'
import Subtraction from './components/Subtraction.vue'

export default {
  data() {
    return {}
  },
  components: {
    'my-subtraction': Subtraction,
    'my-addition': Addition
  }
}
</script>

<style>
</style>
```

在components文件夹中创建Addition.vue组件，代码如下：

```vue
<template>
    <div>
        <h3>当前最新的count值为：</h3>
        <button>+1</button>
    </div>
</template>

<script>
export default {
  data() {
    return {}
  }
}
</script>

<style>
</style>
```

在components文件夹中创建Subtraction.vue组件，代码如下：

```vue
<template>
    <div>
        <h3>当前最新的count值为：</h3>
        <button>-1</button>
    </div>
</template>

<script>
export default {
  data() {
    return {}
  }
}
</script>

<style>
</style>
```

最后在项目根目录(与src平级)中创建 .prettierrc 文件，编写代码如下：

```json
{
    "semi":false,
    "singleQuote":true
}
```

###4.Vuex中的核心特性

####A.State

State提供唯一的公共数据源，所有共享的数据都要统一放到Store中的State中存储
例如，打开项目中的store.js文件，在State对象中可以添加我们要共享的数据，如：count:0

在组件中访问State的方式：

1. `this.\$store.state.全局数据名称  如：this.$store.state.count`

2. 先按需导入mapState函数：` import { mapState } from 'vuex'`

   然后数据映射为计算属性： `computed:{ ...mapState(['全局数据名称']) }`

####B.Mutation

Mutation用于修改变更$store中的数据
使用方式：
打开store.js文件，在mutations中添加代码如下

```javascript
mutations: {
    add(state,step){
      //第一个形参永远都是state也就是$state对象
      //第二个形参是调用add时传递的参数
      state.count+=step;
    }
}
```

然后在Addition.vue中给按钮添加事件代码如下：

```javascript
<button @click="Add">+1</button>
methods:{
  Add(){
    //使用commit函数调用mutations中的对应函数，
    //第一个参数就是我们要调用的mutations中的函数名
    //第二个参数就是传递给add函数的参数
    this.$store.commit('add',10)
  }
}
```

使用mutations的第二种方式：

```javascript
import { mapMutations } from 'vuex'

methods:{
  ...mapMutations(['add'])
}
```

如下：

```javascript
import { mapState,mapMutations } from 'vuex'

export default {
  data() {
    return {}
  },
  methods:{
      //获得mapMutations映射的sub函数
      ...mapMutations(['sub']),
      //当点击按钮时触发Sub函数
      Sub(){
          //调用sub函数完成对数据的操作
          this.sub(10);
      }
  },
  computed:{
      ...mapState(['count'])
  }
}
```


####C.Action

在mutations中不能编写异步的代码，会导致vue调试器的显示出错。
在vuex中我们可以使用Action来执行异步操作。
操作步骤如下：
打开store.js文件，修改Action，如下：

```javascript
actions: {
  addAsync(context,step){
    setTimeout(()=>{
      context.commit('add',step);
    },2000)
  }
}
```

然后在Addition.vue中给按钮添加事件代码如下：

```vue
<button @click="AddAsync">...+1</button>

methods:{
  AddAsync(){
    this.$store.dispatch('addAsync',5)
  }
}
```

第二种方式：

```javascript
import { mapActions } from 'vuex'

methods:{
  ...mapActions(['subAsync'])
}
```

如下：

```javascript
import { mapState,mapMutations,mapActions } from 'vuex'

export default {
  data() {
    return {}
  },
  methods:{
      //获得mapMutations映射的sub函数
      ...mapMutations(['sub']),
      //当点击按钮时触发Sub函数
      Sub(){
          //调用sub函数完成对数据的操作
          this.sub(10);
      },
      //获得mapActions映射的addAsync函数
      ...mapActions(['subAsync']),
      asyncSub(){
          this.subAsync(5);
      }
  },
  computed:{
      ...mapState(['count'])
      
  }
}
```


####D.Getter

Getter用于对Store中的数据进行加工处理形成新的数据
它只会包装Store中保存的数据，并不会修改Store中保存的数据，当Store中的数据发生变化时，Getter生成的内容也会随之变化
打开store.js文件，添加getters，如下：

```javascript
export default new Vuex.Store({
  .......
  getters:{
    //添加了一个showNum的属性
    showNum : state =>{
      return '最新的count值为：'+state.count;
    }
  }
})
```

然后打开Addition.vue中

1. 添加插值表达式使用getters,`{{$store.getters.showNum}}`

2. 也可以在Addition.vue中，导入mapGetters，并将之映射为计算属性

   ```javascript
   import { mapGetters } from 'vuex'
   computed:{
     ...mapGetters(['showNum'])
   }
   ```

###5.vuex案例

####A.初始化案例

首先使用vue ui初始化一个使用vuex的案例
然后打开public文件夹，创建一个list.json文件，文件代码如下：

```json
[
    {
        "id": 0,
        "info": "Racing car sprays burning fuel into crowd.",
        "done": false
    },
    {
        "id": 1,
        "info": "Japanese princess to wed commoner.",
        "done": false
    },
    {
        "id": 2,
        "info": "Australian walks 100km after outback crash.",
        "done": false
    },
    {
        "id": 3,
        "info": "Man charged over missing wedding girl.",
        "done": false
    },
    {
        "id": 4,
        "info": "Los Angeles battles huge wildfires.",
        "done": false
    }
]
```

再接着，打开main.js,添加store.js的引入，如下：

```javascript
import Vue from 'vue'
import App from './App.vue'
import store from './store.js'

// 1. 导入 ant-design-vue 组件库
import Antd from 'ant-design-vue'
// 2. 导入组件库的样式表
import 'ant-design-vue/dist/antd.css'

Vue.config.productionTip = false
// 3. 安装组件库
Vue.use(Antd)

new Vue({
  store,
  render: h => h(App)
}).$mount('#app')
```

再接着打开store.js，添加axios请求json文件获取数据的代码，如下：

```javascript
import Vue from 'vue'
import Vuex from 'vuex'
import axios from 'axios'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    //所有任务列表
    list: [],
    //文本输入框中的值
    inputValue: 'AAA'
  },
  mutations: {
    initList(state, list) {
      state.list = list
    },
    setInputValue(state,value){
      state.inputValue = value
    }
  },
  actions: {
    getList(context) {
      axios.get('/list.json').then(({ data }) => {
        console.log(data);
        context.commit('initList', data)
      })
    }
  }
})
```

最后，代开App.vue文件，将store中的数据获取并展示：

```vue
<template>
  <div id="app">
    <a-input placeholder="请输入任务" class="my_ipt" :value="inputValue" @change="handleInputChange" />
    <a-button type="primary">添加事项</a-button>

    <a-list bordered :dataSource="list" class="dt_list">
      <a-list-item slot="renderItem" slot-scope="item">
        <!-- 复选框 -->
        <a-checkbox :checked="item.done">{{item.info}}</a-checkbox>
        <!-- 删除链接 -->
        <a slot="actions">删除</a>
      </a-list-item>

      <!-- footer区域 -->
      <div slot="footer" class="footer">
        <!-- 未完成的任务个数 -->
        <span>0条剩余</span>
        <!-- 操作按钮 -->
        <a-button-group>
          <a-button type="primary">全部</a-button>
          <a-button>未完成</a-button>
          <a-button>已完成</a-button>
        </a-button-group>
        <!-- 把已经完成的任务清空 -->
        <a>清除已完成</a>
      </div>
    </a-list>
  </div>
</template>

<script>
import { mapState } from 'vuex'

export default {
  name: 'app',
  data() {
    return {
      // list:[]
    }
  },
  created(){
    // console.log(this.$store);
    this.$store.dispatch('getList')
  },
  methods:{
    handleInputChange(e){
      // console.log(e.target.value)
      this.$store.commit('setInputValue',e.target.value)
    }
  },
  computed:{
    ...mapState(['list','inputValue'])
  }
}
</script>

<style scoped>
#app {
  padding: 10px;
}

.my_ipt {
  width: 500px;
  margin-right: 10px;
}

.dt_list {
  width: 500px;
  margin-top: 10px;
}

.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
```

####B.完成添加事项

首先，打开App.vue文件，给“添加事项”按钮绑定点击事件，编写处理函数

```javascript
//绑定事件
<a-button type="primary" @click="addItemToList">添加事项</a-button>

//编写事件处理函数
methods:{
    ......
    addItemToList(){
      //向列表中新增事项
      if(this.inputValue.trim().length <= 0){
        return this.$message.warning('文本框内容不能为空')
      }

      this.$store.commit('addItem')
    }
  }
```

然后打开store.js编写addItem

```javascript
export default new Vuex.Store({
  state: {
    //所有任务列表
    list: [],
    //文本输入框中的值
    inputValue: 'AAA',
    //下一个id
    nextId:5
  },
  mutations: {
    ........
    //添加列表项
    addItem(state){
      const obj = {
        id :state.nextId,
        info: state.inputValue.trim(),
        done:false
      }
      //将创建好的事项添加到数组list中
      state.list.push(obj)
      //将nextId值自增
      state.nextId++
      state.inputValue = ''
    }
  }
  ......
})
```

####C.完成删除事项

首先，打开App.vue文件，给“删除”按钮绑定点击事件，编写处理函数

```javascript
//绑定事件
<a slot="actions" @click="removeItemById(item.id)">删除</a>

//编写事件处理函数
methods:{
    ......
    removeItemById(id){
      //根据id删除事项
      this.$store.commit('removeItem',id)
    }
  }
```

然后打开store.js编写addItem

```javascript
export default new Vuex.Store({
  ......
  mutations: {
    ........
    removeItem(state,id){
      //根据id删除事项数据
      const index = state.list.findIndex( x => x.id === id )
      // console.log(index);
      if(index != -1) state.list.splice(index,1);
    }
  }
  ......
})
```

####D.完成选中状态的改变

首先，打开App.vue文件，给“复选”按钮绑定点击事件，编写处理函数

```javascript
//绑定事件
<a-checkbox :checked="item.done" @change="cbStateChanged(item.id,$event)">{{item.info}}</a-checkbox>

//编写事件处理函数
methods:{
    ......
    cbStateChanged(id,e){
      //复选框状态改变时触发
      const param = {
        id:id,
        status:e.target.checked
      }

      //根据id更改事项状态
      this.$store.commit('changeStatus',param)
    }
  }
```

然后打开store.js编写addItem

```javascript
export default new Vuex.Store({
  ......
  mutations: {
    ........
    changeStatus(state,param){
      //根据id改变对应事项的状态
      const index = state.list.findIndex( x => x.id === param.id )
      if(index != -1) state.list[index].done = param.status
    }
  }
  ......
})
```

####E.剩余项统计

打开store.js，添加getters完成剩余项统计

```javascript
getters:{
  unDoneLength(state){
    const temp = state.list.filter( x => x.done === false )
    console.log(temp)
    return temp.length
  }
}
```

打开App.vue，使用getters展示剩余项

```javascript
//使用映射好的计算属性展示剩余项
<!-- 未完成的任务个数 -->
<span>{{unDoneLength}}条剩余</span>

//导入getters
import { mapState,mapGetters } from 'vuex'
//映射
computed:{
  ...mapState(['list','inputValue']),
  ...mapGetters(['unDoneLength'])
}
```

####F.清除完成事项

首先，打开App.vue文件，给“清除已完成”按钮绑定点击事件，编写处理函数

```javascript
<!-- 把已经完成的任务清空 -->
<a @click="clean">清除已完成</a>

//编写事件处理函数
methods:{
  ......
  clean(){
    //清除已经完成的事项
    this.$store.commit('cleanDone')
  }
}
```

然后打开store.js编写addItem

```javascript
export default new Vuex.Store({
  ......
  mutations: {
    ........
    cleanDone(state){
      state.list = state.list.filter( x => x.done === false )
    }
  }
  ......
})
```

####G.点击选项卡切换事项

打开App.vue，给“全部”，“未完成”，“已完成”三个选项卡绑定点击事件，编写处理函数
并将列表数据来源更改为一个getters。

```javascript
<a-list bordered :dataSource="infoList" class="dt_list">
  ......
  <!-- 操作按钮 -->
  <a-button-group>
    <a-button :type="viewKey ==='all'?'primary':'default'" @click="changeList('all')">全部</a-button>
    <a-button :type="viewKey ==='undone'?'primary':'default'" @click="changeList('undone')">未完成</a-button>
    <a-button :type="viewKey ==='done'?'primary':'default'" @click="changeList('done')">已完成</a-button>
  </a-button-group>
  ......
</a-list>

//编写事件处理函数以及映射计算属性
methods:{
  ......
  changeList( key ){
    //点击“全部”，“已完成”，“未完成”时触发
    this.$store.commit('changeKey',key)
  }
},
computed:{
  ...mapState(['list','inputValue','viewKey']),
  ...mapGetters(['unDoneLength','infoList'])
}
```

打开store.js，添加getters，mutations，state

```javascript
export default new Vuex.Store({
  state: {
    ......
    //保存默认的选项卡值
    viewKey:'all'
  },
  mutations: {
    ......
    changeKey(state,key){
      //当用户点击“全部”，“已完成”，“未完成”选项卡时触发
      state.viewKey = key
    }
  },
  ......
  getters:{
    .......
    infoList(state){
      if(state.viewKey === 'all'){
        return state.list
      }
      if(state.viewKey === 'undone'){
        return state.list.filter( x => x.done === false )
      }
      if(state.viewKey === 'done'){
        return state.list.filter( x => x.done === true )
      }
    }
  }
})
```