
# DOM

## DOM事件流

事件冒泡： 事件冒泡(event bubbling)，即事件开始时由最具体的元素(文档中嵌套层次最深的那个节点)接收，然后逐级向上传播到较为不具体的节点。
```js
<!DOCTYPE HTML>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Document</title>
<body>
<div></div>
</body>    
</html>
```
事件的相应顺序为： div -> body->  html->  document

事件捕获：事件捕获的思想是不太具体的节点应该更早接收到事件，而最具体的节点应该最后接收到事件。事件捕获的用意在于在事件到达预定目标之前就捕获它。

事件的相应顺序为： document -> html ->  body -> div

## 事件委托? 
事件委托就是利用事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件.

在绑定大量事件的时候往往选择事件委托。




















































