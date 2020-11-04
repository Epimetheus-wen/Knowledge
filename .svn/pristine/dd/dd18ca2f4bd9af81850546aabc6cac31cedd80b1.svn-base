<script>
import md5 from "js-md5";
export default {
  onLaunch: function(e) {
    console.log("App onLaunch");
    // wx.setStorage({
    //   key: "token",
    //   data: ""
    // });
    
    uni.hideTabBar()

  },
  onShow: function(e) {
      console.log("App onShow");
      console.log("App场景",e)
        uni.setStorage({
            key: "loginType",
            data: e.scene,
            success: function() {
                console.log("success");
            }
        });


    const updateManager = wx.getUpdateManager()

    updateManager.onCheckForUpdate(function (res) {
    // 请求完新版本信息的回调
    console.log(res.hasUpdate)
    })

    updateManager.onUpdateReady(function () {
        wx.showModal({
            title: '更新提示',
            content: '新版本已经准备好，是否重启应用？',
            success(res) {
                if (res.confirm) {
                    // 新的版本已经下载好，调用 applyUpdate 应用新版本并重启
                    updateManager.applyUpdate()
                }
            }
        })
    })

    updateManager.onUpdateFailed(function () {
    // 新版本下载失败
    })
  },
  onHide: function() {
    console.log("App onHide");
  }
};
</script>

<style lang="scss">
/*每个页面公共css */
* {
  margin: 0;
  padding: 0;
  font-family:PingFangSC-Medium,PingFang SC;
}
.arrowleft {
      font-size: 22rpx;
      color: #000000;
      margin-right: 12rpx;
      .uni-icons {
        font-size: 38rpx !important;
        color: #000000 !important;
      }
    }

button::after{
    border: none;
}

</style>
