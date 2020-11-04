<template>
	<view class="search">
		<Headertop :titleTop="titleTop"></Headertop>
		<view class="search_main">
			<view class="search_top">
				<view @tap="back" class="search_top_left">
					<image class="img" src="../../static/seach/left.png"></image>
				</view>
				<view class="search_top_center">
                    <image class="img img1" src="../../static/home/search.png"></image>
					<input v-model="searchValue" class="input" @confirm="goSearch(searchValue)" confirm-type="search" placeholder-class="input_s" placeholder="输入关键词，搜索知识或文档" type="text">
                    <image @tap="eliminate" class="img img2" src="../../static/seach/input_close.png"></image>
				</view>
				<view class="search_top_right" @tap="goSearch(searchValue)">
					搜索
				</view>
			</view>
            <view class="search_center">
                <view class="search_center_title">搜索历史</view>
                <view class="search_center_main">
                    <view v-if="searchHistory.length<=0" class="txt">暂无</view>
                    <button v-else v-for="(item,index) in searchHistory" :key="index" class="search_jg" @tap="goSearch(item.key)">{{item.key}}</button>

                </view>
            </view>
		</view>
		<!-- <NavTab :index="1"></NavTab> -->
	</view>
</template>

<script>
	import NavTab from "../../components/navTab.vue";
	import Headertop from "../../components/headertop.vue";
	export default {
		data() {
			return {
                titleTop: "易通商务助手-搜索",
                searchValue:'',
                searchHistory:[]
			};
		},
		components: {
			NavTab,
			Headertop
        },
        onShow(){
            this.getSearchHistory()
        },
        onLoad: function(option) {},
        onReady(){
            this.getSearchHistory()
        },
		methods: {
            getSearchHistory(){
                this.$post("/apiIndex.html?f=getSearchList")
                    .then((res) => {
                        console.log("getSearchList",res)
                        this.searchHistory=res.data
                      
                    })
            },
			back() {
				uni.switchTab({
					url: "/pages/index/index"
				});
            },
            eliminate(){
                this.searchValue=""
            },
            goSearch(src){
                let value=src
                if(value&&value!=''){
                    uni.navigateTo({
                        url: '/pages/search/searchResult?value='+value
                    });
                }
                
            }
		}
	};
</script>

<style lang="scss">
	.search {
		letter-spacing: 1rpx;
		height: 100vh;
		display: flex;
		flex-direction: column;
		justify-content: center;
		.search_main {
			flex: 1;
			background: #EFEFF4;

			.search_top {
				display: flex;
				align-items: center;
				justify-content: space-between;
				height: 92rpx;

				.search_top_left {
					display: flex;
					align-items: center;
					justify-content: center;
					width: 68rpx;
					height: 88rpx;

					>.img {
						width: 14rpx;
						height: 26rpx;
						display: block;
                        margin-left: 10rpx;
					}
				}

				.search_top_center {
					display: flex;
					align-items: center;
					justify-content: center;
					position: relative;
					width: 570rpx;
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

				.search_top_right {
					width: 112rpx;
                    font-size:28rpx;
                    color: #2F7AF5;
                    text-align: center;
				}

			}
            .search_center{
                margin: 27rpx 69rpx 0 69rpx;
                .search_center_title{
                    font-size:26rpx;
                    color:rgba(0,0,0,1);
                    line-height:51rpx;
                }
                .search_center_main{
                    display: flex;
                    flex-wrap: wrap;
                    >.txt{
                        font-size:24rpx;
                        color:rgba(0,0,0,0.3);
                        line-height:28rpx;
                        margin-top: 15rpx;
                    }
                    >.search_jg{
                        padding: 12rpx 24rpx;
                        margin: 20rpx 20rpx 0 0;
                        background:rgba(255,255,255,1);
                        box-shadow:0px 2rpx 6rpx 0px rgba(213,213,213,0.8);
                        border-radius:25rpx;
                        display: inline-block;
                        color:rgba(47,122,245,1);
                        line-height:26rpx;
                        font-size:24rpx;
                    }
                }
            }
		}
	}
</style>
