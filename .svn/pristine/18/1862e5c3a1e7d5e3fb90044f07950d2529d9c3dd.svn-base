<template>
  <view class="web-main">
    <web-view :webview-styles="webviewStyles" :src="id"></web-view>
  </view>
</template>
<style>
</style>
<script>
export default {
  data() {
    return {
        id:"",
        obj:{}
    };
  },
  onLoad: function(option) {
    //option为object类型，会序列化上个页面传递的参数
    this.obj = JSON.parse(option.txtObj)
    wx.setNavigationBarTitle({
        title: this.obj.name
        })

    this.id=this.obj.url
  }
};
</script>