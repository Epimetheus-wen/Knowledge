<template>
    <view class="nav">
        <view class="navTab">
            <view class="navTab_item">
                <view v-if="index==1" class="item">
                    <image class="img" src="../static/home/home.png"></image>
                    <view class="txt">首页</view>
                </view>
                <view v-if="index!=1" @tap="toNavTab(1)" class="item item_no">
                    <image class="img" src="../static/home/home_no.png"></image>
                    <view class="txt">首页</view>
                </view>
            </view>
            <view class="navTab_item">
                <view v-if="index==2" class="item">
                    <image class="img" src="../static/home/ss.png"></image>
                    <view class="txt">分类检索</view>
                </view>
                <view v-if="index!=2" @tap="toNavTab(2)" class="item item_no">
                    <image class="img" src="../static/home/ss_no.png"></image>
                    <view class="txt">分类检索</view>
                </view>
            </view>
            <view class="navTab_item">
                <view v-if="index==4" class="item">
                    <image class="img" src="../static/home/jingyin.png"></image>
                    <view class="txt">经营面板</view>
                </view>
                <view v-if="index!=4" @tap="toNavTab(4)" class="item item_no">
                    <image class="img" src="../static/home/jingyin_no.png"></image>
                    <view class="txt">经营面板</view>
                </view>
            </view>
            <view class="navTab_item">
                <view v-if="index==3" class="item">
                    <image class="img" src="../static/home/user.png"></image>
                    <view class="txt">个人设置</view>
                </view>
                <view v-if="index!=3" @tap="toNavTab(3)" class="item item_no">
                    <image class="img" src="../static/home/user_no.png"></image>
                    <view class="txt">个人设置</view>
                </view>
            </view>
            
        </view>
        <view class="navTab_z"></view>
    </view>
</template>

<script>
	export default {
		props: [ 'index'],
		data() {
			return {
	
			};
        },
        methods:{
            toNavTab(id){
                if(id==1){
                    uni.switchTab({
                        url: '/pages/index/index',
                        animationType: 'none',
                    });
                }
                if(id==2){
                    uni.switchTab({
                        url: '/pages/classify/classify',
                        animationType: 'none',
                    });
                }
                if(id==3){
                    uni.switchTab({
                        url: '/pages/user/user',
                        animationType: 'none',
                    });
                }
                if(id==4){
                    uni.navigateTo({
                        url: '/pagesA/operationalAnalysis/operationalAnalysis',
                        animationType: 'none',
                    });
                }
            }
        }
	}
</script>

<style lang='scss'>
.navTab_z{
    height:100rpx;
}
.navTab{
    height:100rpx;
    position:fixed;
    bottom: 0;
    left: 0;
    right: 0;
    background: #fff;
    z-index: 3;
    display: flex;
    align-items: center;
    justify-content: space-around;
    border-top: 1px solid #DDDDDD;
    .navTab_item{
        width: 20%;
        .item{
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            >.img{
                width: 44rpx;
                height: 44rpx;
                display: block;
            }
            >.txt{
                color: #2F7AF5;
                font-size:20rpx;
            }
        }
        .item_no{
            >.txt{
                color: #888888;
                font-size:20rpx;
            }
        }
    }
}
</style>