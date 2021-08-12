# DOM事件

我是从点击事件开始了解事件模型，2002年， W3C发布了一个标准：

* 文档名为DOM Level 2 Events Specification

* 规定浏览器应该同时支持两种调用顺序

* 首先按爷爷>爸爸>儿子顺序看有没有函数监听

* 然后按儿子>爸爸>爷爷顺序看有没有函数监听

* 有监听函数就调用，并提供是事件信息，没有就跳过

## 捕获：从外向内找监听函数，就叫事件捕获

## 冒泡：从内向外找监听函数，就叫事件冒泡

这里有张图片，从中可以清晰的理解捕获和冒泡的执行顺序

![冒泡与捕获示意图](./2021-08-11_203444.png)

## addEventListener

addEventListener()的工作原理是将实现EventListener的函数或对象添加到调用它的EventTarget上的指定事件类型的事件侦听器列表中。


通俗点说就是绑定一个事件API

```javascript

//第三个参数bool默认为冒泡，如果想使用捕获，可以把boll改为true
e.addEventListener('click', fn, bool)

```

## target与currentTarget

### 区别

* e.target-用户操作的元素

* e.currentTarget-程序员监听的元素

## 【总结】

以上是我对DOM事件一点简单了解，后续学到新的知识再进行补充！！！

## 事件委托

事件委托就是利用事件冒泡，指定一个事件处理程序，就可以管理某一类型的所有事件

## 事件委托原理

不用给每个子节点单独设置事件监听，而是把事件监听设置在父节点上，利用冒泡原理，影响设置每个字节点

案例一：你要给10个按钮添加点击事件，如何操作

代码：

```html

<!-- div#div1>button{click $}*10 -->  
<div id="div1">  
<button>click 1</button>  
<button>click 2</button>  
<button>click 3</button>  
<button>click 4</button>  
<button>click 5</button>  
<button>click 6</button>  
<button>click 7</button>  
<button>click 8</button>  
<button>click 9</button>  
<button>click 10</button>  
</div>

```

监听它们的祖先，等冒泡的时候判断这10个里面有没有（target）被操的的元素

代码：

```javascript

div1.addEventListener('click', (e) => {  
const t = e.target  
if(t.tagName.toLowerCase()==='button'){  console.log('button 被点击了')  
console.log('button 被点击了' + t.textContent)   
}
})

```

案例二：监听目前不存在的元素的点击事件

```javascript

setTimeout(()=>{  
const button = document.createElement('button')  
button.textContent = 'click 1'  
div1.appendChild(button) },1000)  

div1.addEventListener('click', 
(e)=>{  const t = e.target  
if(t.tagName.toLowerCase() === 'button'){  
console.log('button click')     
}   
} )

```

监听祖先元素，等典籍的时候看看是不是我想要监听的元素即可

## 优点

* 省内存

* 可以监听动态元素