<template>
	<view class="user">
		<Headertop :titleTop="titleTop"></Headertop>
		<view class="user_main">
			<view class="user_main_info">
				<image class="img" src="../../static/user/user_bg.png"></image>
				<view class="info">
                    <avatar style="position: absolute;top:0;" @upload="doUpload" @largeImg="getLargeImg" quality="1" ref="avatar" selWidth="250upx" selHeight="250upx"></avatar>
                    <view class="headPortrait">
                        <view class="skew-elm2">
                            <image class="con" :src="urls[1]" @tap="upPopup1"></image> 
                        </view>
                    </view>
                    <view class="name_h">
                        <view class="txt_name" v-text="userInfo.nickname?userInfo.nickname:'未填写'"></view>
                        <view class="lianjie">-</view>
                        <view class="txt_privname" v-text="userInfo.privname?userInfo.privname:'未填写'"></view>
                    </view>
					
					<view class="txt_department" v-text="userInfo.deptname?userInfo.deptname:'未填写'"></view>
					
					<view class="txt_job">
						<view class="txt">工号：</view>
						<view class="txt" v-text="userInfo.jnumber?userInfo.jnumber:'未填写'"></view>
					</view>
					<view class="txt_account">
						<view class="txt">账号：</view>
						<view class="txt" v-text="userInfo.phone?userInfo.phone:'未填写'"></view>
					</view>
				</view>
			</view>
			<view class="user_main_item" @tap="goCollect">
				<image class="img" src="../../static/user/sc.png"></image>
				<view class="txt">
					<view class="left">我的收藏</view>
					<image class="right" src="../../static/user/right.png"></image>
				</view>
			</view>
			<view class="user_main_item" @tap="goForget">
				<image class="img" src="../../static/user/anquan.png"></image>
				<view class="txt">
					<view class="left">修改密码</view>
					<image class="right" src="../../static/user/right.png"></image>
				</view>
			</view>
		</view>
		<button class="log_out" @tap="upPopup">退出登录</button>
		<view v-if="popup.dom" :style="'top: calc('+((staHeight*2)+80)+'rpx);'" :class="popup.shade?'shade_popup show_popup':'shade_popup no_popup'">
			<view class="popup_content">
				<view class="top">
					<view class="title">确定要退出当前账号？</view>
					<button class="out" @tap="logOut">退出登录</button>
				</view>
				<button class="off" @tap="upPopup">取消</button>
			</view>
		</view>
        <view v-if="popup1.dom" :style="'top: calc('+((staHeight*2)+80)+'rpx);'" :class="popup1.shade?'shade_popup show_popup':'shade_popup no_popup'">
			<view class="popup_content">
				<view class="top" style="height:112rpx">
					<button class="out" style="color:#2F7AF5" @tap="clk(1)">从手机相册中选择</button>
				</view>
				<button class="off" style="color:rgba(0,0,0,0.5)" @tap="upPopup1">取消</button>
			</view>
		</view>
		<NavTab :index="3"></NavTab>
	</view>
</template>

<script>
	import NavTab from "../../components/navTab.vue";
    import Headertop from "../../components/headertop.vue";
    import avatar from "../../components/yq-avatar/yq-avatar.vue";
	export default {
		data() {
			return {
				titleTop: "易通商务助手",
                userInfo: {},
                staHeight:'',
                largeImg:'',
				popup: {
					shade: false,
					dom: false
                },
                popup1: {
					shade: false,
					dom: false
                },
                urls: ["","",],
			};
		},
		components: {
			NavTab,
            Headertop,
            avatar
        },
        onShow(){
            console.log("状态栏高度");
            let that = this;
            wx.getSystemInfo({
                success: e => {
                    console.log("状态栏高度", e.statusBarHeight);
                    that.staHeight = e.statusBarHeight;
                }
            });
        },
		onLoad: function(option) {},
		onReady() {
			this.getUserInfo()
		},
		methods: {
			getUserInfo() {
				this.$post("/apiLogin.html?f=getInfo")
					.then((res) => {
						console.log("getInfo", res)
                        this.userInfo = res.data
                        this.urls[0]=res.data.head_pic
                        this.urls[1]=res.data.head_pic
					})
			},
			logOut() {
				uni.removeStorage({
					key: "token",
					success: function(res) {
						console.log("success");
					}
				});
				uni.reLaunch({
					url: "/pages/login/password/password",
					animationType: "none"
				});
			},
			upPopup() {
				setTimeout(() => {
					if (this.popup.shade) {

						this.popup.shade = false
						setTimeout(() => {
							this.popup.dom = false
						}, 300)
					} else {
						this.popup.dom = true
						this.popup.shade = true
					}
				}, 300)
            },
            upPopup1() {
				setTimeout(() => {
					if (this.popup1.shade) {

						this.popup1.shade = false
						setTimeout(() => {
							this.popup1.dom = false
						}, 300)
					} else {
						this.popup1.dom = true
						this.popup1.shade = true
					}
				}, 300)
			},
			goForget() {
				let mobile = this.userInfo.phone
				uni.navigateTo({
					url: '/pages/login/forgetPassword/forgetPassword?mobile=' + mobile
				});
            },
            goCollect(){
                uni.navigateTo({
					url: '/pages/user/collectList'
				});
            },
            clk(index){
                this.upPopup1()
                this.$refs.avatar.fChooseImg(index,{
                    selWidth: '638upx', selHeight: '700upx', inner: true
                });
            },
            getLargeImg(url){
  
                this.largeImg=url
            },
            doUpload(rsp) {
				console.log(rsp);
				this.$set(this.urls, rsp.index, rsp.path);
				// this.url = rsp.path;
                //rsp.avatar.imgSrc = rsp.path;
                
                // data:image/jpg;base64,
                console.log(this.largeImg)

                var FSM = wx.getFileSystemManager(); 
                let that=this
                FSM.readFile({
                    filePath: rsp.path,
                    encoding: "base64",
                    success: function(data) {
                        console.log(data)
                        console.log(data.data)//'data:image/jpg;base64,'+data.data
                        that.$file("/apiUserUpdate.html?f=updateHeadPic",{img:rsp.path})
                            .then((res) => {
                                
                                // this.updateHeadPic = res.data
                                res=JSON.parse(res);
                                console.log("updateHeadPic", res)
                                if(res.code==0){
                                    that.getUserInfo()
                                    uni.showToast({
                                        title: res.msg,
                                        icon: 'none',
                                        duration: 2000
                                    });
                                }
                            })
                    }
                });
			}
		}
	};
</script>

<style lang="scss">
	.user {
		height: 100vh;
		position: relative;

		.user_main {
			.user_main_info {
				width: 750rpx;
				height: 382rpx;
				// margin: 0 20rpx;
				position: relative;
				// margin-bottom: 12rpx;

				.img {
					width: 750rpx;
					height: 382rpx;
					display: block;
				}

				.info {
					position: absolute;
					top: 0;
					bottom: 0;
					right: 0;
					left: 0;
					color: #fff;
                    .headPortrait{
                        position:absolute;
                        top: 12rpx;
                        left: 20rpx;
                        width: 319rpx;
                        height: 350rpx;
                        transform-origin: top;
                        transform: skew(-11deg, 0deg);
                        -ms-transform: skew(-11deg, 0deg);
                        -moz-transform: skew(-11deg, 0deg);
                        -webkit-transform: skew(-11deg, 0deg);
                        -o-transform: skew(-11deg, 0deg);
                        overflow: hidden;
                        // border-radius: 0px 10px 20px 0px;

                        .skew-elm2{    
                            width: 319rpx;
                            height: 350rpx;
                            transform-origin: top;
                            transform: skew(0deg, 0deg);
                            -ms-transform: skew(0deg, 0deg);
                            -moz-transform: skew(0deg, 0deg);
                            -webkit-transform: skew(0deg, 0deg);
                            -o-transform: skew(0deg, 0deg);
                            overflow: hidden;
                            border-radius: 10rpx 0rpx 0rpx 10rpx;
                        }
                        
                        .con{ 
                            width: 319rpx;
                            height: 350rpx;
                            transform-origin: top;
                            transform: skew(11deg, 0deg); 
                            -ms-transform: skew(11deg, 0deg);
                            -moz-transform: skew(11deg, 0deg);
                            -webkit-transform: skew(11deg, 0deg);
                            -o-transform: skew(11deg, 0deg);
                            border-radius: 10rpx 0rpx 0rpx 10rpx;
                            // background-image: url("../../static/user/fx_bg.png");
                        }

                    }
                    .name_h{
                        display: flex;
                        align-items: center;
                        margin-left: 370rpx;
						margin-top: 123rpx;
                        font-size:28rpx;
                        line-height:40rpx;
                        font-weight: 600;
                        .lianjie{
                            padding: 0 12rpx;
                        }
                    }
                    
					.txt_department {
						margin-left: 370rpx;
						margin-top: 56rpx;
						line-height: 38rpx;
                        color: #010204;
						font-size: 22rpx;
					}

					.txt_job,
					.txt_account {
                        color: #010204;
						display: flex;
						align-items: center;
						margin-left: 370rpx;
						line-height: 38rpx;
						font-size: 22rpx;
					}
				}
			}

			.user_main_item {
				height: 115rpx;
				display: flex;
				align-items: center;

				>.img {
					margin-left: 52rpx;
					width: 42rpx;
					height: 40rpx;
					display: block;
					margin-right: 25rpx;
				}

				>.txt {
					height: 100%;
					border-bottom: 1rpx solid #DDDDDD;
					flex: 1;
					line-height: 115rpx;
					display: flex;
					align-items: center;
					justify-content: space-between;

					.right {
						width: 14rpx;
						height: 26rpx;
						display: block;
						margin-right: 50rpx;
					}
				}
			}
		}

		.log_out {
			position: absolute;
			bottom: 200rpx;
			left: 0;
			right: 0;
			margin: 0 auto;
			width: 304rpx;
			height: 80rpx;
			border-radius: 6rpx;
			
			font-size: 26rpx;
			color: rgba(0, 0, 0, 0.85);
			line-height: 80rpx;
			background: #fff;
            overflow:visible;
            padding:0;
		}
        .log_out::after{
            border: 2rpx solid rgba(221, 221, 221, 1);
            border-radius: 20rpx;
        }

		.show_popup {
			display: block !important;

			.popup_content {
				bottom: 0;
				animation: myfirst5 0.2s linear;
			}
		}

		.no_popup {
			display: block !important;

			.popup_content {
				bottom: -400rpx;
				animation: myfirst6 0.2s linear;
			}

		}

		.shade_popup {
			position: absolute;
			top: calc(var(--status-bar-height) + 80rpx);
			right: 0;
			left: 0;
			bottom: 0;
			z-index: 100;
			background: rgba(0, 0, 0, 0.4);

			.popup_content {
				position: absolute;
				left: 0;
				right: 0;

				.top {
					height: 240rpx;
					margin: 0 30rpx;
					border-radius: 12rpx;
					background: #fff;

					.title {
						height: 126rpx;
						border-bottom: 2rpx solid rgba(0, 0, 0, 0.1);
						font-size: 28rpx;
						text-align: center;
						line-height: 127rpx;
						color: rgba(0, 0, 0, 0.5);
					}

					.out {
						height: 112rpx;
						font-size: 34rpx;
						text-align: center;
						line-height: 112rpx;
						color: #FA5151;
						background: #fff;
					}
				}

				.off {
					margin: 16rpx 30rpx 30rpx;
					border-radius: 12rpx;
					height: 112rpx;
					font-size: 34rpx;
					text-align: center;
					line-height: 112rpx;
					color: #2F7AF5;
					background: #fff;
				}
			}
		}
	}

	.button-hover {
		background-color: #f8f8f8 !important;
	}

	@keyframes myfirst5 {
		0% {
			bottom: -400rpx;
		}

		100% {
			bottom: 0;
		}
	}

	@keyframes myfirst6 {
		0% {
			bottom: 0;
		}

		100% {
			bottom: -400rpx;
		}
	}
</style>
