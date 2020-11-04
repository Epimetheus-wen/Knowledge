<template>
  <view class="classify">
    <Headertop :titleTop="titleTop"></Headertop>
    <view class="classify_main">
        <view class="classify_top">
            <view class="classify_top_center">
                <image class="img img1" src="../../static/home/search.png"></image>
                <input v-model="searchValue" class="input" @confirm="goSearch(searchValue)" confirm-type="search" placeholder-class="input_s" placeholder="在全部知识中搜索" type="text">
                <image @tap="eliminate" class="img img2" src="../../static/seach/input_close.png"></image>
            </view>
        </view>
        <view class="classify_center">
            <ClassifyCatalog @tapIndex="tapIndex" :classifyList="classifyList"></ClassifyCatalog>
            <view v-for="(item,index) in classifyList" :key="index" :class="index==idx?'classify_center_right classify_center_right1':'classify_center_right'">
                <ClassifyGroup v-if='index==idx' :classifyGroup="item"></ClassifyGroup>
            </view>
        </view>
    </view>
    <NavTab :index="2"></NavTab>
  </view>
</template>


<script>
import NavTab from "../../components/navTab.vue";
import Headertop from "../../components/headertop.vue";
import ClassifyCatalog from "./classifyCatalog.vue";
import ClassifyGroup from "./classifyGroup.vue";
export default {
  data() { 
    return {
      titleTop: "易通商务助手-分类检索",
      classifyList:[],
      classifyGroup:[],
      searchValue:"",
      idx:0,
    };
  },
  components: {
    NavTab,
    Headertop,
    ClassifyCatalog,
    ClassifyGroup
  },
  onShow() {},
  onHide() {},
  onReady() {
      this.getCateList()
  },
  methods: {
        getCateList(){
            this.$post("/apiIndex.html?f=getCateList")
                .then((res) => {
                    console.log("getCateList",res)
                    this.classifyList=res.data
                })
        },
        tapIndex(index){
            console.log("index",index)
            this.idx=index

        },
        eliminate() {
            this.searchValue = ""
        },
        goSearch(src){
            console.log(src)
                let obj={
                    id:"",
                    indexName:src
                }
                let str=JSON.stringify(obj)
                uni.navigateTo({
                    url: '/pages/classify/classifyList?str='+str
                });
        }
  }
};
</script>

<style lang="scss">
.classify{
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    .classify_main{
        flex: 1;
        display: flex;
        flex-direction: column;
        .classify_top {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 92rpx;
            background: #EFEFF4;
            .classify_top_center {
                display: flex;
                align-items: center;
                justify-content: center;
                position: relative;
                width: 690rpx;
                height: 60rpx;
                background: rgba(255, 255, 255, 1);
                box-shadow: 0px 2rpx 10rpx 0px rgba(213, 213, 213, 1);
                border-radius: 6rpx;
                >.input{
                    width: 100%;
                    height: 100%;
                    padding-left:66rpx;
                    padding-right:54rpx;
                    font-size:26rpx;
                    position: relative;
                    z-index: 1;
                }
                .input_s{
                    font-size:26rpx;
                    color: #BBBBBB;
                }
                .img {
                    width: 30rpx;
                    height: 30rpx;
                    display: block;
                    z-index: 2;
                }
                >.img1{
                    position: absolute;
                    left: 24rpx;
                    top: 0;
                    bottom: 0;
                    margin: auto 0;
                }
                >.img2{
                    position: absolute;
                    padding: 0 24rpx;
                    right: 0;
                    top: 0;
                    bottom: 0;
                    margin: auto 0;
                }
                
            }
        }
        .classify_center{
            flex: 1;
            position: relative;
            display: flex;
            align-items: center;
            .classify_center_right{
                position:absolute;
                top: 0;
                bottom: 0;
                right: 0;
                left:185rpx;
                z-index: 1;
            }
            .classify_center_right1{
                z-index: 2;
            }
        }
        
    }
}

</style>
