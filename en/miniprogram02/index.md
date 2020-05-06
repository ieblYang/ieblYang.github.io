# 微信小程序函数节流，防止多次点击跳转


<!--more-->

## 1 使用场景

{{% admonition note "使用场景"%}}
* 在微信小程序中，点击“采集人脸”按钮后需要将相机采集到的照片上传到服务器，并进行人脸识别，在此过程存在一定时间的延迟，用户会误认为点击无效而进行多次点击，最后出现多次跳转页面的情况。
* 函数节流(throttle)：函数在一段时间内多次触发只会执行第一次，在这段时间结束前，不管触发多少次也不会执行函数。
{{% /admonition %}}

## 2 代码实现

### 2.1 /utils/util.js

```JavaScript
# /utils/util.js
function throttle(fn, gapTime) {
    if (gapTime == null || gapTime == undefined) {
        gapTime = 1500
    }

    let _lastTime = null

    // 返回新的函数
    return function () {
        let _nowTime = + new Date()
        if (_nowTime - _lastTime > gapTime || !_lastTime) {
            fn.apply(this, arguments)   //将this和参数传给原函数
            _lastTime = _nowTime
        }
    }
}
```

### 2.2 /pages/face/face_register.wxml

```Html
<view  class='moto-container-collection'>
    <text  class = 'moto' bindtap="open" type="primary" data-type="takePhoto">采集人脸</text>
</view>
```

### 2.3 /pages/face/face_register.js

```JavaScript
//引用util.js
const util = require('../../utils/util.js')

//1s触发一次
Page({
    open: util.throttle(function () {
        ...
    }, 1000)
})
```

## 3 参考资料
{{% admonition note "参考资料" %}}
* 该链接中有更加详细的讲解，可以前往学习参考
* <https://www.cnblogs.com/fps2tao/p/12186523.html>
{{% /admonition %}}
