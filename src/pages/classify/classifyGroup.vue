<template>
  <view class="classify_Group">
    <view v-if="!classifyGroup.depth" class="top">
      <view class="classify_item_top">全部</view>
    </view>
    <view v-if="classifyGroup.depth" class="top">
      <view :class="idx=='全部'?'on_a':'classify_item_top'">全部</view>
      <view
        v-for="(item,index) in classifyGroup.child"
        :key="index"
        :class="idx==index?'on_a':'classify_item_top'"
        @tap="tapSelect(index)"
      >{{item.name}}</view>
    </view>
    <view v-if="!classifyGroup.depth" class="center">
        <view 
        class="classify_item_center"
         v-for="(item,index) in classifyGroup.child"
        :key="index"
        @tap="toSearch(item)">
            <image class="img" src="../../static/classify/wenjianja.png"></image>
            <view class="txt">{{item.name}}</view>
        </view>
    </view>
    <view v-if="classifyGroup.depth" class="center">
        <view 
        class="classify_item_center"
         v-for="(item,index) in classifyGroup.child[idx].child"
        :key="index"
         @tap="toSearch(item)">
            <image class="img" src="../../static/classify/wenjianja.png"></image>
            <view class="txt">{{item.name}}</view>
        </view>
    </view>
  </view>
</template>

<script>
export default {
  props: ["classifyGroup"],
  data() {
    return {
      idx: 0
    };
  },
  components: {},
  mounted() {},
  methods: {
    tapSelect(index) {
      this.idx = index;
      console.log(index);
      console.log(this.idx);
      // this.$emit('tapIndex', index);
    },
    toSearch(obj){
        console.log(obj)
        let str=JSON.stringify(obj)
        uni.navigateTo({
            url: '/pages/classify/classifyList?str='+str
        });
    }
  }
};
</script>

<style lang='scss' scoped>
.classify_Group {
  margin: 10rpx 0 0 40rpx;
  .top {
    line-height: 100rpx;
    font-size: 24rpx;
    color: rgba(67, 82, 106, 0.45);
    padding-bottom: 6rpx;
    display: flex;
    flex-wrap: wrap;
    border-bottom: 2rpx solid rgba(0, 0, 0, 0.1);
    .classify_item_top {
      margin-right: 20rpx;
    }
    .on_a {
      color: rgba(67, 82, 106, 1);
      margin-right: 20rpx;
    }
  }
  .center{
      margin-top:19rpx;
      .classify_item_center{
          display: flex;
          align-items: center;
          margin-bottom:18rpx;
          >.img{
              width: 43rpx;
              height: 38rpx;
              display: block;
              margin-right: 23rpx;
          }
          >.txt{
                font-size:26rpx;
                color:rgba(67,82,106,1);
                line-height:51rpx;
          }
      }
  }
}
</style>