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
所有变量,函数命名都是分为两部分.中间用`-`隔开
- 第1部分是css属性名简写
- 第2部分是属性值的语义化表达

如`$fz-md`
- $ -> 表示变量
- fz -> font-size
- - -> 分隔符
- md -> middle中号

简写完全参照emmet语法: https://docs.emmet.io/cheat-sheet/

## 内容介绍

### 尺寸

所有尺寸默认情况下都是基于375设计稿,最终会换算成vw值以达到屏幕自适应的目的

**块间尺寸section**
- $s-xxl 28
- $s-xl 24
- $s-lg 20
- $s-md 16
- $s-sm 12
- $s-xs 8
- $s-xxs 4

**字体大小font-size**
- $fz-xxl 20
- $fz-xl 18
- $fz-lg 16
- $fz-md 14
- $fz-sm 12
- $fz-xs 10

**边框圆角尺寸border-radius**
- $bdrs-round 50%
- $bdrs-lg 8
- $bdrs-sm 4
- $bdrs-none 0


### 颜色

**主题色**

一般一个项目只会有一个主题色,及相对次一点的搭配色.在引入项目时覆盖
- $c-theme #f05b28
- $c-theme-light #fe8513

**灰阶色**
- $white #fff
- $black #000
- $gray-3 #333
- $gray-6 #666
- $gray-9 #999
- $gray-c #ccc
- $gray-e #eee
- $gray-f5 #f5f5f5
- $gray-f8 #f8f8f8

**透明色**
- $white-1 #ffffff1a
- $white-4 #ffffff66
- $black-1 #0000001a
- $black-4 #00000066

## 项目配置
### vue.config.js

首先要安装全局自动引入公共sass的插件`style-resources-loader`
```
npm install style-resources-loader --save-dev 
```
或者
```
yarn add style-resources-loader --save-dev
```

其实在项目根目录下找到或创建`vue.config.js`,配置如下内容

```
module.exports = {
  chainWebpack: (config) => {
    // 引入公共scss
    [
      'vue',
      'normal',
      'vue-modules',
      'normal-modules',
    ].forEach((type) => {
      const rule = config
        .module
        .rule('scss')
        .oneOf(type);
      rule
        .use('style-resource')
        .loader('style-resources-loader')
        .options({
          patterns: ['node_modules/rww-sass/_index.scss'],
        });
    });
  })
}
```

### 覆盖默认变量