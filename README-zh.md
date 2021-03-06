# Flutter 小程序

一个基于 Flutter 框架的小程序开发框架。通过解析 HTML 标签 和 CSS 样式，用于开发小程序应用，项目灵感来自 [FlutterHtmlView](https://github.com/PonnamKarthik/FlutterHtmlView)。

## 特性

- 将 html 标签转换为 Flutter 组件
- 支持使用 css 样式渲染 Flutter 组件
- 支持模板编译及数据绑定（TODO）

## 框架

### 视图层

框架的视图层由 html 与 css 编写，通过 .html 文件定义页面的视图层，由组件来进行展示。将逻辑层的数据反应成视图，同时将视图层的事件发送给逻辑层。组件(Component)是视图的基本组成单元。

```html
<config>
{
    "navigationBarTitleText": "Flutter MiniProgram"
}
</config>

<template>
    <view class="container">
         <!--Icon-->
         <icon type="threesixty" size="30"></icon>
         <icon type="home" size="40"></icon>

         <!--Heading-->
         <h1>Heading 1</h1>
         <h2>Heading 2</h2>
         <h3>Heading 3</h3>
         <h4>Heading 3</h4>
         <h5>Heading 3</h5>
         <h6>Heading 3</h6>

         <!--Text-->
         <text>普通文本</text>
         <text style="color: #0000FF;font-size: 20px;font-weight: bold;background-color:#ff0000;">加样式的文本</text>

         <!--Link-->
         <a href="https://flutter.io">https://flutter.io</a>

         <!--Image-->
         <text>Network Image：</text>
         <image src="https://avatars2.githubusercontent.com/u/13075561?s=460&v=4"></image>
         <text>Local Image：</text>
         <image src="../images/avatar.png"></image>

         <!--Button-->
         <button type="close">关闭类型的按钮</button>
         <button type="back">返回类型的按钮</button>
         <button type="floating-action" onclick="onFloatingButtonClick">Icon 类型的按钮</button>
         <button type="icon">Icon 类型的按钮</button>
         <button type="flat">扁平的 Material 类型的按钮</button>
         <button type="raised">阴影效果的按钮</button>
         <button type="raw-material">背景透明的按钮</button>

         <!--Video-->
         <video src="https://www.sample-videos.com/video123/mp4/720/big_buck_bunny_720p_20mb.mp4"></video>
    </view>
</template>

<style>
.container {
    margin: 10px;
    background-color: #eee;
}
.index-page {
    margin: 10px;
}
</style>
```

### 逻辑层

小程序开发框架的逻辑层使用 Dart 语法进行开发，通过 Dart / Flutter 插件完成视图逻辑的组织及原生插件调用。逻辑层将数据进行处理后发送给视图层，同时接受视图层的事件反馈。

```dart
class IndexPage extends Page {
  String url;
  BuildContext mContext;

  IndexPage({this.url});

  Map data = {
    "list": [
      {"title": "Icon", "subtitle": "图标", "routeName": "/icon"},
      {"title": "Text", "subtitle": "文本", "routeName": "/text"},
      {"title": "Button", "subtitle": "按钮", "routeName": "/button"},
      {"title": "Image", "subtitle": "图片", "routeName": "/image"},
      {"title": "Checkbox", "subtitle": "复选框", "routeName": "/checkbox"}
    ]
  };

  Map<String, Function> get methods => {
    "openPage": (String path) {
      Application.navigateTo(mContext, path);
    }
  };

  @override
  void onCreate(BuildContext context, Page page) {
    mContext = context;

    print(page);
    print(page.emitter);
    print(page.view);
    print(Application.router);

    page.emitter.on('lifecycle', () => print('监听'));
  }
}
```

## 组件

### 支持的组件

- view/div: 视图容器
- icon: 图标
- text: 文本
- br: 换行符
- hr: 水平分隔线
- p: 段落
- h1 ~ h6: 标题
- a: 链接
- button: 按钮
- input: 输入框
- checkbox: 复选框
- switch: 单选开关
- slider: 滑块
- image: 图片
- video: 视频
- table: 表格
- list-view: 列表
- web-view: 网页容器

### 视图

视图容器，相当于 Web 的 div 标签或者 React Native 的 View 组件。

API

| 属性名   |      类型      |  默认值 | 描述 |
|----------|:-------------:|------:|:-------------:|
| class |  String |  | 自定义样式名 |
| style |  String |  | 内联样式 |
| onTap | EventHandle |  | 点击事件 |
| onLongTap | EventHandle |  | 长按事件 |

### 文本输入框

文本输入框, 相当于 Web 的 input 标签 或者 iOS 中的 UITextField 和 Android 中的 EditText。

| 属性名   |      类型      |  默认值 | 描述 |
|----------|:-------------:|------:|:-------------:|
| type | String |  | input 的类型 |
| placeholder | String |  | 占位符 |
| focus | Boolean |  false | 获取焦点 |

```html
<view>
    <text>这是一个可以自动聚焦的input: </text>
    <input type="text" placeholder="这是一个可以自动聚焦的input" focus="true" />
</view>
<view>
    <text>数字键盘: </text>
    <input type="number" placeholder="数字键盘" />
</view>
```

### 图片

API

| 属性名   |      类型      |  默认值 | 描述 |
|----------|:-------------:|------:|:-------------:|
| class |  String |  | 自定义样式名 |
| style |  String |  | 内联样式 |
| src | String |  | 图片路径 |
| fit | String |  | 图片裁剪、缩放的模式, fill | contain | cover | fitWidth | fitHeight | none | scaleDown |

```html
<view>
    <h4>网络图片：</h4>
    <image src="https://avatars2.githubusercontent.com/u/13075561?s=460&v=4"
           style="width: 200px;height:200px"></image>
</view>
<view>
    <h4>本地图片：</h4>
    <image src="../images/avatar.png" style="width: 200px;height:200px"></image>
</view>
<view>
    <h4>图片裁剪模式 fit = "fill": </h4>
    <image src="../images/avatar.png" style="width: 200px;height:100px" fit="fill"></image>
</view>
```