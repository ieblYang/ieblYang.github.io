# 微信小程序实现长按触发某一事件


<!--more-->

## 1 实现代码

### 1.1 `message/message.js`

```JavaScript
  /**
   * 消息长按部分代码
   */
  //touch start
  handleTouchStart: function (e) {
    this.startTime = e.timeStamp; 
  },

  //touch end
  handleTouchEnd: function (e) {
    this.endTime = e.timeStamp;
  },

  handleLongPress: function (e) {

    var that = this;
    var id = e.currentTarget.dataset.id;
    wx.showModal({
      title: '提示',
      content: '是否删除这条消息？',
      success: function (res) {
        if (res.confirm) {
          wx.request({
            url: app.buildUrl("/message/delMess"),
            header: app.getRequestHeader(),
            data: {
              id: id,
            },
            success: function (res) {
              if (res.data.code == 200) {
                that.onLoad();
                app.showSuccess("消息删除成功！");
              }else{
                app.showFail("消息删除失败！");
              }
            }
          })
        }
      }
    })
  },

```

### 1.2 `message/message.wxml`

```Html
<view class="message-right-text-box" bindtouchstart="handleTouchStart" bindtouchend="handleTouchEnd" bindlongpress="handleLongPress" data-id="{{item.id}}">
  <text class="message-right-text">{{item.message}}</text>
</view>
```


## 2 效果展示
![效果展示](1.gif)


