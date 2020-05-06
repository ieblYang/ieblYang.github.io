# 微信小程序自定义showToast图片


<!--more-->

## 1 引言

### 1.1 使用场景

{{% admonition note "使用场景"%}}
* 在自己制作小程序的过程中，发现小程序自带的`wx.showToast`很适合做一些提示信息，只是`icon`仅支持"success","loading"两个图标，在查找官方文档后我发现，其实`wx.showToast`是支持自定义图片的，这样一下就方便很多了
{{% /admonition %}}

### 1.2 官方文档

{{% admonition success "官方文档"%}}
* 官方文档 -> API -> 界面 -> 交互 -> wx.showToast  
* `wx.showToast(Object object)` 显示消息提示框
* 参数 `Object object`     

|属性|类型|说明|
|----|----|----|
|title|string|提示的内容|   
|icon|string|图标，默认 'success'|    
|image|string| 自定义图标的本地路径，image 的优先级高于 icon| 
|duration|number|提示的延迟时间,默认1500| 
|mask|boolean|是否显示透明蒙层，防止触摸穿透 ，默认：false| 
|success|function|接口调用成功的回调函数 | 
|fail|function|接口调用失败的回调函数 | 
|complete|function|接口调用结束的回调函数（调用成功、失败都会执行）|  

* object.icon 的合法值

|值|说明|
|----|----|
|success|显示成功图标，此时 title 文本最多显示 7 个汉字长度|
|loading|显示加载图标，此时 title 文本最多显示 7 个汉字长度|
|none|不显示图标，此时 title 文本最多可显示两行|  

* 注意
    * `wx.showLoading` 和 `wx.showToast` 同时只能显示一个
    * `wx.showToast` 应与 `wx.hideToast` 配对使用
{{% /admonition %}}

## 2 代码实现

### 2.1 app.js

```JavaScript
// 为方便管理，我们先在 app.js 中定义一个函数
 demoToast:function(title,image){
    wx.showToast({
      title: title,
      image:'/images/' + image,
      duration: 2000,
      mask:true
    })
  },
```

### 2.2 /pages/face/face_login.js

```JavaScript
// 日后使用中，我们只需要调用 app.js 中的 demoToast 函数就可以使用
const app = getApp();

// 加载中,使用 wx.showLoading
wx.showLoading
({  
    title: "正在登录",   
})

//成功
app.demoToast('测试成功','success.png');

// 失败
app.demoToast('测试失败','fail.png');
```

## 3 效果演示

### 3.1 加载中

![Minion](/images/miniprogram/miniprogram03/1.jpg)

### 3.2 失败

![Minion](/images/miniprogram/miniprogram03/2.jpg)

### 3.3 成功

![Minion](/images/miniprogram/miniprogram03/3.jpg)

## 4 参考资料
{{% admonition note "参考资料" %}}
* 微信小程序官方文档
* <https://developers.weixin.qq.com/miniprogram/dev/api/ui/interaction/wx.showToast.html>
{{% /admonition %}}
