# 准备：浏览器

## 1. DOM 基本操作

* 查找节点
  * getElementById, getElementsByClassName, getElementsByTagName, getElementsByName                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      
  * document/element.querySelector, document/element.querySelectorAll
  * document.documentElement, document.body
* 新建节点
  * createElement
  * createAttribute, createTextNode, createComment, createDocumentFragment
* 添加新节点
  * appendChild, insertBefore, setAttribute, setAttributeNode
* 删除节点
  * removeChild, removeAttribute, removeAttributeNode
* 修改节点
  * replaceChild

## 2. 引起reflow的原因

1. 页面第一次渲染
2. DOM树变化
3. Render树变化
4. 浏览器窗口resize
5. 获取元素的某些属性

## 3. 引起repaint的原因

1. reflow
2. 背景色、颜色、字体改变

## 4. 减少以上两种回流的方法

1. 用transform做形变和位移
2. 避免逐个修改节点样式
3. 将需要多次修改的DOM设为display:none,即将其移出render tree，等修改后再加入
4. 避免多次读取某些属性

