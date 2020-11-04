<template>
  <view class="header_top">
    <view class="status_bar">
      <!-- <view class="top_view" ></view> -->
      <view class="top titles">
        <uni-icons @tap="getBack" type="arrowleft" class="arrowleft1"></uni-icons>
        <view  class="arrowleft_ba" @tap="getBack"></view>
        <view class="titleText">{{titleTop}}</view>
      </view>
    </view>
    <view class="geduan"></view>
  </view>
</template>

<script>
import uniIcons from "@dcloudio/uni-ui/lib/uni-icons/uni-icons.vue";
export default {
  props: ["titleTop"],
  data() {
    return {
      staHeight: "",
    };
  },
  components: {
    uniIcons
  },
  methods: {
    getBack() {
        uni.navigateBack({
            delta: 1
        });
    }
  }
};
</script>


<style lang="scss">
    //css函数
@function tovmin($rpx){//$rpx为需要转换的字号
    @return #{$rpx * 100 / 750}vmin; 
}

.header_top {
  letter-spacing: tovmin(1);
  .status_bar {
    height: tovmin(80);
    width: 100%;
    position: fixed;
    top: 0;
    background: #fff;
    z-index: 10;
  }
//   .top_view {
//     height: var(--status-bar-height);
//     width: 100%;
//   }


  .geduan {
    height: tovmin(80);
  }

  .titles {
    height: tovmin(80);
    padding-left: tovmin(30);
    display: flex;
    align-items: center;
    position: relative;
    .arrowleft1 {
      font-size: tovmin(22);
      color: #000000;
      margin-right: tovmin(3);
      margin-left: tovmin(-10);
      .uni-icons {
        font-size: tovmin(38) !important;
        color: #000000 !important;
      }
    }
    .arrowleft_ba{
        position:absolute;
        left: 0;
        width: tovmin(60);
        height: tovmin(80);
    }
    .titleText {
      height: tovmin(80);
      color: #000000;
      display: flex;
      align-items: center;
      font-size: tovmin(32);
      font-weight: 500;
    }
  }
}
</style>