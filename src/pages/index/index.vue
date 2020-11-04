<template>
	<view class="home">
		<view class="status_bar" :style="'height: calc('+((staHeight*2)+80)+'rpx);'">
			<view class="top_view" :style="'height:'+staHeight+'px;'"></view>
			<view class="top titles">
				<view class="titleText">易通商务助手</view>
			</view>
		</view>
		<view class="geduan" :style="'height: calc('+((staHeight*2)+80)+'rpx);'"></view>
		<view class="home_main">
			<!-- 头部搜索 -->
			<view class="home_main_top">
				<image class="banner" src="../../static/home/banner.png"></image>
				<view class="banner_text">
					<view class="banner_text_1">
						易通·产品知识平台
					</view>
					<view class="banner_text_2">
						高效检索，轻松转发，
					</view>
					<view class="banner_text_2">
						随时随地云办公
					</view>
				</view>
				<button class="search_but" @tap="goSearch">
					<image class="img" src='../../static/home/search.png'></image>
					<view class="txt">搜索知识、文档</view>
				</button>
			</view>
			<!-- 广告栏 -->
			<view class="home_main_gg">
				<view class="textBox" v-if="TrumpetList.length>0">
					<view class="trumpet">
						<image class="img" src="../../static/home/trumpet.png" alt></image>
					</view>
					<view class="list">
						<view class="text slide-enter-active slide-enter" v-if="TrumpetList.length>0" :key="text.id" @click="goTrumpet(TrumpetList[text.id])">
							<view class="span">《<text class="txt">{{TrumpetList[text.id].accessory_name}}</text>》有更新，点击查看</view>
						</view>
						<view class="text slide-enter-active slide-enter" v-if="TrumpetList.length>0" :key="text1.id" @click="goTrumpet(TrumpetList[text1.id])">
							<view class="span">《<text class="txt">{{TrumpetList[text1.id].accessory_name}}</text>》有更新，点击查看</view>
						</view>
					</view>

					<view class="particulars">
						详情
					</view>
				</view>
			</view>

            <!-- 快捷前往 -->
            <view class="shortcut">
                <view class="shortcut_title">快捷前往</view>
                <view class="shortcut_list">
                    <view class="shortcut_card" @tap="toClassifyList('数据概览')">
                        <image class="img" src='../../static/home/datum.png'></image>
                        <view class="txt">数据概览</view>
                    </view>
                    <view class="shortcut_card" @tap="toClassifyList('经营分析')">
                        <image class="img" src='../../static/home/introduce.png'></image>
                        <view class="txt">经营分析</view>
                    </view>
                    <view class="shortcut_card" @tap="toClassifyList('解决方案')">
                        <image class="img" src='../../static/home/scheme.png'></image>
                        <view class="txt">解决方案</view>
                    </view>
                    <view class="shortcut_card" @tap="toClassifyList('接口文档')">
                        <image class="img" src='../../static/home/port.png'></image>
                        <view class="txt">接口文档</view>
                    </view>
                </view>
            </view>
            <!-- 最近预览 -->
            <view class="recent">
                <view class="recent_title">最近预览</view>
                <loadingList v-if="list.length>0" :showLoad="showLoad" :list='list' :load="load" @updateCollect="updateCollect"></loadingList>
                <view v-if="list.length<=0" class="preview_no">
                    <image class="img" src='../../static/home/preview_no.png'></image>
                    <view class="txt">-暂无阅览-</view>
                </view>
            </view>
		</view>
        <NavTab :index="1"></NavTab>
	</view>
</template>


<script>
import loadingList from "../../components/loadingList.vue"
import NavTab from "../../components/navTab.vue"
import moment from "moment"
	import md5 from "js-md5";
	export default {
		data() {
			return {
				staHeight: "",
				textArr: [{
					title: ""
				}, {
					title: ""
				}, {
					title: ""
				}],
				number: 0,
                number1: 0,
                list:[],
                load:false,
                showLoad:true,
                tabScrollTop:'',
                TrumpetList:[]
			};
		},
		components: {
            loadingList,
            NavTab
        },
		computed: {
			text() {
				return {
					id: this.number,
					val: this.textArr[this.number]
				};
			},
			text1() {
				return {
					id: this.number1,
					val: this.textArr[this.number1]
				};
			},
		},
		onShow() {
			wx.getStorage({
				key: "token",
				success(res) {
					console.log(res.data);
					if (!res.data) {
						console.log("token", res.data);
						wx.redirectTo({
							url: "/pages/login/password/password"
						});
					}
				},
				fail(res) {
					console.log("no");
					wx.redirectTo({
						url: "/pages/login/password/password"
					});
					console.log("no");
				}
            });
            this.getVisitHistory()
		},
		onHide() {},
		onLoad() {
            uni.hideTabBar();
        },
        onShareAppMessage(res) {
            console.log('转发')
			return {
				title: '易通·产品知识平台',
				path: '/pages/index/index',
				// imageUrl:"../../static/user/fx_bg.png",
				success: function(ress) {
                    console.log(ress)
                }
			}
		},
		onReady() {
            console.log("状态栏高度");
            this.$post("/apiIndex.html?f=getDocList")
                .then((res) => {
                    console.log("getDocList",res)
                    this.TrumpetList=res.data
                    this.TrumpetList.map((item)=>{
                            item.tt=moment(item.update_time).format('YYYY-MM-DD HH:MM')
                            item.update_time=moment(item.update_time).format('YYYY-MM-DD')
                        })
                    this.startMove();
                })
			let that = this;
			wx.getSystemInfo({
				success: e => {
					console.log("状态栏高度", e.statusBarHeight);
					that.staHeight = e.statusBarHeight;
				}
            });
            
        
        },
		methods: {
            getVisitHistory(){
                this.$post("/apiIndex.html?f=getVisitHistory")
                    .then((res) => {
                        console.log("getVisitHistory",res)

                    this.list=res.data
                        this.list.map((item)=>{
                            item.tt=moment(item.update_time).format('YYYY-MM-DD HH:MM')
                            item.update_time=moment(item.update_time).format('YYYY-MM-DD')
                        })
                    })
            },
            updateCollect(index){
                console.log(index)
                if(this.list[index].fav==0){
                    this.list[index].fav=1
                }else{
                    this.list[index].fav=0
                }

            },
            goSearch(){
                uni.switchTab({
                    url: '/pages/search/search'
                });
            },
			startMove() {
				this.textArr = this.TrumpetList;
				let timer = setTimeout(() => {
					if (this.number == this.TrumpetList.length - 1) {
						this.number = 0;
					} else {
						this.number += 1;
					}
					if (this.number1 == this.TrumpetList.length - 1) {
						this.number1 = 0;
					} else {
						this.number1 = this.number + 1;
					}
					this.startMove();
				}, 6000);
			},
			goTrumpet(src) {
                console.log(src);
                this.recordHistory(src)
            },
            toClassifyList(value){
                console.log(value)
                let obj={
                    id:"",
                    indexName:value
                }
                let str=JSON.stringify(obj)
                if(value=="数据概览"){
                    uni.navigateTo({
                        url: '/pagesA/dataModule/dataModule'
                    });
                }else if(value=="经营分析"){
                    uni.navigateTo({
                        url: "/pagesA/operationalAnalysis/operationalAnalysis",
                    })
                }else{
                    uni.navigateTo({
                        url: '/pages/classify/classifyList?str='+str
                    });
                }

            },
            recordHistory(obj){
                this.$post("/apiIndex.html?f=recordHistory",{"id":obj.id})
                    .then((res) => {
                        console.log("recordHistory",res)
                        console.log("添加记录")
                    })

                    console.log(obj)
                    obj.type=1
                    let str=JSON.stringify(obj)
                    console.log(str)

                    uni.navigateTo({
                        url: '/pages/preview/preview?str='+encodeURIComponent(str)
                    });
                    // if(obj.type==2){
                    //     uni.navigateTo({
                    //         url: '/pages/preview/previewWeb?str='+encodeURIComponent(str)
                    //     });
                    // }
                    // if(obj.type==1){
                    //     uni.navigateTo({
                    //         url: '/pages/preview/preview?str='+encodeURIComponent(str)
                    //     });
                    // }
            }
		}
	};
</script>

<style lang="scss">
	.home {
        letter-spacing: 1rpx;
		.status_bar {
			height: calc(var(--status-bar-height) + 80rpx);
			width: 100%;
			position: fixed;
			top: 0;
			background: #fff;
			z-index: 10;
		}

		.top_view {
			height: var(--status-bar-height);
			width: 100%;
			color: #000000;
		}

		.geduan {
			height: calc(var(--status-bar-height) + 80rpx);
		}

		.titles {
			height: 80rpx;

			.titleText {
				height: 80rpx;
				color: #000000;
				display: flex;
				align-items: center;
				font-size: 32rpx;
				margin-left: 30rpx;
				font-weight: 500;
			}
		}

		// 头部banner与搜索
		.home_main_top {
			display: flex;
			align-items: center;
			justify-content: center;
			position: relative;
			margin-bottom: 92rpx;

			.banner {
				width: 710rpx;
				height: 280rpx;
				position: absolute;
				top: 0;
				bottom: 0;
				left: 0;
				right: 0;
				margin: auto;
				display: block;
				z-index: 1;
			}

			.banner_text {
				width: 710rpx;
				height: 280rpx;
				position: relative;
				z-index: 2;
				color: #fff;

				.banner_text_1 {
					font-size: 34rpx;
					line-height: 48rpx;
					font-weight: 600;
					margin: 60rpx 0 15rpx 48rpx;
				}

				.banner_text_2 {
					font-size: 24rpx;
					margin-left: 48rpx;
				}
			}

			.search_but {
				position: absolute;
				bottom: -42rpx;
				z-index: 3;
				width: 650rpx;
				height: 68rpx;
				background: rgba(255, 255, 255, 1);
				box-shadow: 0px 2rpx 10rpx 0px rgba(213, 213, 213, 1);
				border-radius: 6rpx;
				display: flex;
				align-items: center;
				justify-content: center;

				>.img {
					width: 30rpx;
					height: 30rpx;
					display: block;
					margin-right: 14rpx;
				}

				>.txt {
					font-size: 24rpx;
					line-height: 30rpx;
					color: #BBBBBB;
				}
			}
		}
// 公告
		.home_main_gg {
            background: #FDFCEE;
			.textBox {
				position: relative;
				overflow: hidden;
				height: 80rpx;
				.trumpet {
					width: 30rpx;
					height: 26rpx;
					position: absolute;
					left: 35rpx;
					bottom: 0;
					top: 0;
					margin: auto 0;
					.img {
						display: block;
						width: 30rpx;
						height: 26rpx;
					}
				}

				.particulars {
					width: 100rpx;
					height: 50rpx;
					position: absolute;
					bottom: 0;
					top: 0;
					right: 30rpx;
					margin: auto 0;
                    background:rgba(255,255,255,1);
                    box-shadow:0px 2rpx 6rpx 0px rgba(213,213,213,0.8);
                    border-radius:25rpx;
                    text-align: center;
                    font-size:24rpx;
                    color:rgba(47,122,245,1);
                    line-height: 50rpx;
				}

				.list {
					z-index: 9;
					position: relative;
				}

				.text {
					padding-left: 83rpx;
					font-size: 24rpx;
					line-height: 80rpx;
					z-index: 9;
					padding-right: 150rpx;
					.span {
                        color: #F86E21;
						display: flex;
						height: 100%;
						// overflow: hidden;
						// white-space: nowrap;
						// text-overflow: ellipsis;
                        .txt{
                            display: block;
                            max-width: 250rpx;
                            overflow: hidden;
                            white-space: nowrap;
                            text-overflow: ellipsis;
                        }
					}
				}

				.slide-enter-active,
				.slide-leave-active {
					//   transition: all 0.5s linear;
					animation: myfirst 6s infinite;
				}

				.slide-enter {
					transform: translateY(40rpx);
					opacity: 1;
				}

				.slide-leave-to {
					transform: translateY(-40rpx);
					opacity: 0;
				}
			}
		}
        // 快捷前往
        .shortcut{
            margin-top:38rpx;
            .shortcut_title{
                font-size:28rpx;
                color:rgba(0,0,0,1);
                line-height:51rpx;
                margin-left: 30rpx;
                margin-bottom: 16rpx;
            }
            .shortcut_list{
                display: flex;
                flex-wrap: wrap;
                justify-content: center;
                .shortcut_card{
                    width:355rpx;
                    height:195rpx;
                    position: relative;
                    .img{
                        width: 100%;
                        height: 100%;
                        display: block;
                    }
                    .txt{
                        position: absolute;
                        top: 22rpx;
                        left: 33rpx;
                        z-index: 2;
                        font-weight: 600;
                        line-height:48rpx;
                        font-size:27rpx;
                        color:rgba(255,255,255,1);
                    }
                }
            }
        }
        .recent{
            margin-top:38rpx;
            .recent_title{
                font-size:28rpx;
                color:rgba(0,0,0,1);
                line-height:51rpx;
                margin-left: 30rpx;
                margin-bottom: 16rpx;
            }
        }
        .preview_no{
            height: 250rpx;
            display: flex;
            align-items: center;
            flex-direction: column;

            >.img{
                width: 180rpx;
                height: 132rpx;
                display: block;
                margin-bottom: 20rpx;
                margin-top: 10rpx;
            }
            >.txt{
                font-size: 22rpx;
                color: #D8DBE5;
            }
        }
	}

	@keyframes myfirst {
		0% {
			transform: translateY(10rpx);
			opacity: 0;
		}

		10% {
			transform: translateY(0rpx);
			opacity: 1;
		}

		90% {
			transform: translateY(0rpx);
			opacity: 1;
		}

		100% {
			transform: translateY(-40rpx);
			opacity: 0;
		}
    }
    .load_icon{
         position: absolute;
         left: 0;
         top: -4rpx;
         animation:myfirst1 1.5s linear infinite; 
        .uni-icons{
            font-size: 38rpx !important;
            color: rgba(0,0,0,0.4) !important;
        }
    }
     
@keyframes myfirst1 {
	0% {
        transform:rotate(0deg);
    }
    50% {
        transform:rotate(180deg);
    }
    100% {
        transform:rotate(360deg);
    }
}
</style>
