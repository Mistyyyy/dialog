## 因为最近开始学vue，做项目的时候，遇到需要弹出层组件，自己很早就想实现弹出层的简单插件，所以，现在先占个坑，先从几种方式来实现弹出层的实现。
### 今天做的第一种方式，就是需要添加额外的div标签来充当个遮罩层

### 先捋一下思路

- 需求是弹出一个弹框，外侧有遮罩，先给定弹框的宽高。后期会通过用户自己传参改变，点击按钮，弹出弹框，点击关闭或者遮罩层，关闭弹框
- 第一步：弹出弹框和遮罩，为提高用户体验，使用过渡效果来实现。弹出框是决定定位来达到水平垂直居中，相对父元素是body元素。遮罩层是fixed定位，相对视口来显示
- 第二步：先把弹框和遮罩层的z轴顺序搞清楚，弹出层显示的时候，弹框在遮罩层之上，不显示的时候，弹框和遮罩层在body之下就可以了。
- 第三步：考虑过渡效果，我是使用opacity属性来实现平滑的过渡效果，所以transition-property都为opacity。未显示时，opacity都置为0，这是为了关闭弹框而考虑的。然后，点击显示，opacity置为1，z-index同时置位10和8.当关闭弹框的时候，为了平滑的过渡效果。先把opacity置为0，等到过渡效果结束，再把z-index设置为body之下。

### 这就是整体的思路

### 8.11 目前有个小bug,为了不让用户在页面上设置任何关于弹出层的代码，在js中直接调用，关于弹出层和遮罩层的代码通过button按钮，触发事件自动生成。

- 第一次点击弹出，会生成dom元素，采用document.createDocumentFragment来插入dom节点，为了减少内存开销。之后这个DOM节点会存在于页面上并且在之后的需要的时候进行复用。
- 实现：利用绝对定位和固定定位的特性，让他们处于文档流之下。

- 问题：第一次进行弹出的时候，因为生成dom外加又要过渡效果。浏览器优化策略会忽略过渡效果。

### 解决方法：

- 将弹出层和遮罩全部包裹在一个div元素中，将其设置为fixed定位元素，平铺整个屏幕。这时候，可以根据该div元素来实现弹出过渡效果和关闭渐入效果。同时，采用DOM生成和移除DOM元素来实现。

- 同样，也有难点，就是DOM碎片生成，插入到文档流时，不执行过渡效果。 


