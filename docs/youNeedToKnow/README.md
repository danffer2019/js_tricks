# you need to kow

## insertAdjacentHTML

`insertAdjacentHTML()`将指定的文本解析为HTML或XML，并将结果节点插入到DOM树中的指定位置。它不会重新解析它正在使用的元素，因此它不会破坏元素内的现有元素。这避免了额外的序列化步骤，使其比直接innerHTML操作更快。

```javascript
element.insertAdjacentHTML(position, text)
```

> position是相对于元素的位置，并且必须是以下字符串之一：

- 'beforebegin'
    元素自身的前面。
- 'afterbegin'
    插入元素内部的第一个子节点之前。
- 'beforeend'
    插入元素内部的最后一个子节点之后。
- 'afterend'
    元素自身的后面。

---

## document.implementation

implementation 属性可返回处理该文档的 DOMImplementation 对象。

```javascript
function parseHTML(str) {
  var tmp = document.implementation.createHTMLDocument()
  tmp.body.innerHTML = str
  return tmp.body.children
}

parseHTML(htmlString)
```
---

## 函数节流 throttle

指某些代码不可以在没有间断的情况下连续重复执行

```javascript
function throttle (method,wait=1000,context){
    clearTimeout(method.tid)
    method.tid = setTimeout(function(){
        method.call(context)
    },wait)
}
```
---

##  函数防抖 debounce

指一个事件如果频繁触发，会隔一段时间执行一次

```javascript
function debounce(fn,delay,mustRunDelay){
    var timer = null
    var t_start
    return function(){
        var context = this
        var args = arguments
        var t_curr = +new Date()
        clearTimeout(timer)
        if(!t_start){
            t_start = t_curr
        }
        if(t_curr - t_start >= mustRunDelay) {
            fn.apply(context,args)
            t_start = t_curr
        } else {
            timer = setTimeout(function(){
                fn.apply(context,args)
            },delay)
        }
    }
}
```
---

## MessageChannel
> `MessageChannel`接口允许我们创建一个新的消息通道，并通过它的两个MessagePort 属性发送数据。

`MessageChannel`创建了两个可通信的频道，每个频道可以通过`postMessage`方法发送信息，而另一个频道通过`onmessage`回调方法接受信息

```javascript
   const mc = new MessageChannel()
   let p1 = mc.port1
   let p2 = mc.port2
   p1.onmessage = function(e){console.log("port1 receive " + e.data)}
   p2.onmessage = function(e){console.log("port2 receive " + e.data)}
   p1.postMessage("hello, world")
   p2.postMessage("hi,world")
```
### 用途
- html和iframe之间的通讯
- web worker之间的通讯
---

## DocumentFragment
`DocumentFragment`接口表示没有父级的最小文档对象
`document.createDocumentFragment()`

因为`DocumentFragment`不是真实 DOM 树的其中一部分，它的变化不会引起 DOM 树的重新渲染操作(reflow) ，因此不会导致性能问题。

当我们需要修改多个节点时，可以创建一个`DocumentFragment`，在此节点进行添加(append)或被插入(inserted)操作。因为所有的节点会被一次性插入到文档中，而这个操作仅发生一个重渲染的操作，而不是每个节点分别被插入到文档中，因为后者会发生多次重渲染的操作。

```javascript
var frag = document.createDocumentFragment()
for (var i = 0; i < 100; i++) {
  var p = document.createElement("P")
  var text = document.createTextNode(i)
  p.appendChild(text)
  frag.appendChild(p)
}
document.body.appendChild(frag)
```
---

## 深拷贝&&浅拷贝
中所周知，Object是引用数据类型，存储在内存的堆中，浅拷贝只是拷贝了另一个对象的内存地址，所以在修改时会同时修改另一个对象，而深拷贝会开辟新的内存地址，所以不会影响另一个对象

### 浅拷贝
```javascript
let obj1 = {a:'1',b:{c:1}}
// 直接赋值
let obj2 = obj1
// 使用Object.assign
//Object.assign()拷贝的是属性值。假如源对象的属性值是一个指向对象的引用，它也只拷贝那个引用值
let obj3 = Object.assign({},obj1)
```

### 深拷贝
```javascript
let obj1 = {a:'1',b:{c:1}}
```
#### JSON.parse
```javascript
let obj2 = JSON.parse(JSON.stringify(obj1))
```
#### 递归
```javascript
function deepClone(initalObj, finalObj ={}) {       
  for (let i in initalObj) {        
    let prop = initalObj[i]
    if(prop === finalObj) {            
      continue
    }        
    if (typeof prop === 'object') {
      finalObj[i] = (prop.constructor === Array) ? [] : {}          
      arguments.callee(prop, finalObj[i]);
    } else {
      finalObj[i] = prop
    }
  }    
  return finalObj
}
deepClone(obj1) 
```
#### Object.create()
```javascript
function deepClone(initalObj, finalObj) {    
  let obj = finalObj || {}   
  for (let i in initalObj) {        
    let prop = initalObj[i]
    if(prop === obj) {            
      continue
    }        
    if (typeof prop === 'object') {
      obj[i] = (prop.constructor === Array) ? [] : Object.create(prop)
    } else {
      obj[i] = prop
    }
  }    
  return obj
}
```



