# Node 节点

文档对象模型(DOM, Document Object Model)是 HTML 和 XML 文档的编程接口，将其用**节点**构成的带层级结构的树表示。

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202403091135376.png)

## 节点类型

1. **Node.ELEMENT_NODE**
2. Node.ATTRIBUTE_NODE
3. **Node.TEXT_NODE**
4. Node.CDATA_SECTION_NODE
5. Node.ENTITY_REFERENCE_NODE
6. Node.ENTITY_NODE
7. Node.PROCESSING_INSTRUCTION_NODE
8. **Node.COMMENT_NODE**
9. **Node.DOCUMENT_NODE**
10. Node.DOCUMENT_TYPE_NODE
11. Node.DOCMENT_FRAGMENT_NODE
12. Node.NOTATION_NODE

`node.nodeName`

`node.nodeType`

`node.normalize()`

`node.hasChildNodes()`

### document


## 操作节点
### 查询节点

**根据所有节点关系**

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202403091144890.png)

**根据元素节点关系**

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202403091144718.png)

### 创建节点

- `document.createElement(tag)`：用给定标签创建一个元素节点
- `document.createTextNode(value)`：创建一个文本节点
- `element.cloneNode(deep)`：克隆元素，deep => true/false

### 插入节点
- `node.append(...nodes or strings)`：在 node 最后一个子元素后插入
- `node.prepend(...nodes or strings)`：在 node 第一个子元素前插入
- `node.before(...nodes or strings)`：在 node 之前插入
- `node.after(...nodes or strings)`：在 node 之后插入
- `parent.appendChild(node)`
- `parent.insertBefore(node, nextSibling)`

### 替换节点
- `node.replaceWith(...nodes or string)`：替换 node
- `parent.replaceChild(newElem, node)`

### 移除节点
- `node.remove()`：移除节点
- `parent.removeChild(node)`

### `insertAdjacentHTML/Text/Element`
- `element.insertAdjacentHTML(where, html)`
  - where => `beforebegin`：将 html 插入 element 之前
  - where => `afterbegin`：将 html 插入 element 第一个子元素之前
  - where => `beforeend`：将 html 插入到 element 最后一个子元素之后
  - where => `afterend`：将 html 插入到 element 之后


## 样式

`element.style` 是只读的对象

**重制 style**
- `element.style.cssText = strings`：用 strings 对整个 style 重写
- `element.setAttribute('style', strings)`：用 strings 对整个 style 重写

**`getComputedStyle(element[, pseudo])`**

读取已经完成 计算(computed)、解析(resolved)的样式，pseudo 指定伪元素，默认为 element 自身

返回只读对象

## class

`element.className`：返回 element 的类名集合组成的字符串

`element.classList`：获取 element 类名组成的 iterator 对象
  - `element.classList.add(class)`：添加类
  - `element.classList.remove(class)`：移除类
  - `element.classList.toggle(class)`：类存在就删除，不存在就添加
  - `element.classList.contains(class)`：类存在返回 true

## 
### size and scroll

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202403071526804.png)

`contentWidth = width - scrollbarWidth`

除 `scrollTop/Left` 外都是只读的

**offsetParent**

最近的满足以下条件的祖先元素(用于渲染期间计算坐标)
1. CSS 定位(Position 为 absolute、relative、fixed、sticky)
2. 或 `<td>` `<th>` `<table>`
3. 或 `<body>`

**offsetTop/Left**

相对 `offsetParent` 左上角边缘的距离

**offsetWidth/Height**

`offset(Width/Height) = border + pending + contentWidth[ + scrollbar]`

**clientTop/Left**

左上角内角到左上角外角的距离

`client(Top/Left) = border[ + scrollbar]`

**clientWidth/Height**

`client(Width/Height) = pending + contentWidth`

**scrollTop/Left**

左上角内角滚出元素的距离

**scrollWidth/Height**

不含 scrollbar、border，含超出隐藏部分

### 整个文档

**document 可见部分 width/height (内容区域)**

`document.documentElement.clientWidth/clientHeight`

**document 的 width/height (包括滚出去的部分)**

```
let scrollHeight = Math.max(
  document.body.scrollHeight, document.documentElement.scrollHeight,
  document.body.offsetHeight, document.documentElement.offsetHeight,
  document.body.clientHeight, document.documentElement.clientHeight
);
```

**当前 document 滚动位置**

`window.pageYOffset/pageXOffset`

**更改当前 document 的滚动位置**

`window.scrollTo(pageX, pageY)`：绝对坐标

`window.scrollBy(x, y)`：相对当前位置进行滚动 (x, y) 的距离

`element.scrollIntoView(top)`：滚动到 element 可见(`top:true` element 与窗口顶部对齐，`top:false` element 与窗口底部对齐)

### 坐标

**相对于窗口**

`element.getBoundingClientRect()`

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202403091437270.png)

`document.elementFromPoint(x, y)`：获取窗口坐标 (x, y) 处元素

**相对于文档**

`element.getBoundingClientRect() + 当前页面滚动距离`

## 参考

[jsvascript-guidebook](https://tsejx.github.io/javascript-guidebook/document-object-model)

[javascript.info](https://zh.javascript.info/document)