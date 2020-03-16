移动端项目基础sass文件,包含常用混合,函数及变量.

## 目录
- [业务背景](#业务背景)
- [项目配置](#项目配置)
  - [vue.config.js](#vue.config.js)
   - [覆盖默认变量](#覆盖默认变量)
- [内容介绍](#内容介绍)
  - [语法约定](#语法约定)
  - [布局](#布局)
  - [尺寸](#尺寸)
  - [边框](#边框)
  - [背景](#背景)
  - [颜色](#颜色)
  - [文本](#文本)
  - [混合](#混合)
  - [函数](#函数)

## 业务背景

为了保证各个项目中的自己以及各个时期的自己按照统一的标准进行编码,所有项目都以这个sass库为基准进行开发.

在项目中,每个HTML节点尽量最多只使用一个语义化的css类名,所有操作都可以在该类名来通过引入公共的属性来解决,而非使用全局的基础类名

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
  margin-left: 8px;
}

// good
.title {
  @include text-truncate;
  width: px2vw(100);
  height: px2vw(100);
  margin-left: $s-xs;
}
```


## 项目配置
### vue.config.js

首先要安装全局自动引入公共sass的插件`style-resources-loader`
```
npm install style-resources-loader --save-dev 
```
或者
```
yarn add style-resources-loader -dev
```

然后在项目根目录下找到或创建`vue.config.js`,配置如下内容

```
module.exports = {
  chainWebpack: (config) => {
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

## 内容介绍

### 语法约定

所有变量,函数命名都是分为两部分.中间用`-`隔开
- 第1部分是css属性名简写
- 第2部分是属性值的语义化表达

如`$fz-md`
- $ -> 表示变量
- fz -> font-size
- `-` -> 分隔符
- md -> 中号middle的简写

简写完全参照emmet语法: https://docs.emmet.io/cheat-sheet/

### 布局

**弹性伸缩布局flex**
- @flex-grow // 横向伸缩
- @flex-column // 纵向伸缩

**定位层级z-index**
- $z-dropdown: 500
- $z-sticky: 520
- $z-fixed: 530
- $z-modal: 550
- $z-popover: 560
- $z-tooltip: 570
- $z-modal-backdrop: 1040

**绝对定位布局**
- @fixed-top // 顶部固定
- @fixed-bottom // 底部固定
- @sticky-top($top) // 底部吸附(顶部距离) 默认为0

**浮动布局**
- @clearfix // 清除浮动

### 边框
- $bd-base: 1px solid $bdc-base; // 内容分割线
- $bd-comp: 1px solid $bdc-comp; // 组件分割线
- @bd-hairline // 细边框


### 尺寸

所有尺寸默认情况下都是基于375设计稿,最终会换算成vw值以达到屏幕自适应的目的

可通过覆盖`$design-width`进行重置.

**设计稿尺寸**
- $design-width: 375;

**块间尺寸section**
- $s-xxl: 28;
- $s-xl: 24;
- $s-lg: 20;
- $s-md: 16; // 屏幕间隙
- $s-sm: 12;
- $s-xs: 8;
- $s-xxs: 4; // 行内元素间距

**字体大小font-size**
- $fz-xxl: 20;
- $fz-xl: 18;
- $fz-lg: 16;
- $fz-md: 14; // 普通文字
- $fz-sm: 12; // 备注文字
- $fz-xs: 10;

**边框圆角尺寸border-radius**
- $bdrs-round: 50%;
- $bdrs-lg: 8;
- $bdrs-sm: 4;
- $bdrs-none: 0;


### 颜色

**主题色**

一般一个项目只会有一个主题色,及相对次一点的搭配色.在引入项目时覆盖
- $c-theme: #f05b28;
- $c-theme-light: #fe8513;

**灰阶色**
- $white: #fff;
- $black: #000;
- $gray-3: #333;
- $gray-6: #666;
- $gray-9: #999;
- $gray-c: #ccc;
- $gray-e: #eee;
- $gray-f5: #f5f5f5;
- $gray-f8: #f8f8f8;

**透明色**
- $white-1: #ffffff1a;
- $white-4: #ffffff66;
- $black-1: #0000001a;
- $black-4: #00000066;

**文字色**
- $c-base: $gray-3; // 主文字颜色，用于页面标题/重要信息
- $c-gray: $gray-6; // 副文字颜色，用于常规信息/辅助信息/次要信息
- $c-light: $gray-9; // 副文字颜色，用于常规信息/辅助信息/次要信息
- $c-muted: $gray-c; // 暗文字色，用于暗文字/失效文字

**背景色**
- $bgc-base: $gray-f5; // 页面背景色，用于页面背景/默认图片背景/小标签背景
- $bgc-comp: $gray-f8; // 组件背景色，用于卡片/控件/按下背景

**边框色**
- $bdc-base: $gray-e; // 内容分割线色，用于内容/列表分割线
- $bdc-comp: $gray-c; // 组件分割线色，用于弹框/功能/按钮分割线

**阴影色**
- $bsc-base: $black-1; // 块之间的阴影,按钮的阴影

**骨架屏颜色**
$sk-base: #eee; // 元素方块背景色
$sk-light: #e2e2e2; // 扫光渐变色


### 背景
- @linear-gradient($from, $to, $deg) // 渐变(起始色, 终止色, 角度) 默认主题色

### 文本
- @text-truncate($rows) // 超出省略(行数) 默认1行

### 函数
- px2vw($size) // px转vw(px值)
