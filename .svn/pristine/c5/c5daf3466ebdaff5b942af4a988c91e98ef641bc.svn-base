<template>
  <view class="classify_Catalog">
    <view
      v-for="(item,index) in classifyList"
      :key="index"
      class="classify_item"
      :class="{'on': index == idx}"
      @tap="tapSelect(index)"
    >
      <view v-text="item.name" class="txt"></view>
    </view>
  </view>
</template>

<script>
export default {
  props: ["classifyList"],
  data() {
    return {
      idx: 0
    };
  },
  components: {},
  methods: {
      tapSelect(index){
            this.idx = index
            this.$emit('tapIndex', index);
      }
  }
};
</script>

<style lang='scss' scoped>
.classify_Catalog{
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    width:185rpx;
    background: #F6F6FC;
    .classify_item{
        width:185rpx;
        height:90rpx;
        display: flex;
        align-items: center;
        justify-content: center;
        .txt{
            font-size:26rpx;
            color:#43526A;
            line-height:48rpx;
        }
    }
    .on{
        width:185rpx;
        height:90rpx;
        background:rgba(255,255,255,1);
        .txt{
            color:rgba(47,122,245,1);
        }
    }
}
</style>