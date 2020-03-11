### 业务背景

移动端项目基础sass文件,包含常用混合,函数及变量.在项目中,每个HTML节点尽量最多只使用一个语义化的css类名,所有操作都可以在该类名来通过引入公共的属性来解决,而非使用全局的基础类名

```html
<!-- bad -->
<div class="title text-truncate ml-2"></div>

<!-- good -->
<div class="title"></div>
```

```scss
// bad
.title {
  width: 100px;
  height: 100px
}

.text-truncate {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.ml-2 {
  margin-left: 10px;
}

// good
.title {
  @include text-truncate;
  width: 100px;
  height: 100px;
  margin-left: 10px;
}
```

### 语法约定

## 内容介绍
### 尺寸

所有尺寸默认情况下都是基于375设计稿,最终会换算成vw值以达到屏幕自适应的目的

**块间尺寸section**
- xxl 28
- xl 24
- lg 20
- md 16
- sm 12
- xl 8
- xxl 4

**字体大小size**
- 


## 项目配置
### vue.config.js