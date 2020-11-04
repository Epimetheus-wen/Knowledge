<template>
	<view class="login">
		<view class="status_bar" :style="'height: calc('+((staHeight*2)+80)+'rpx);'">
			<view class="top_view" :style="'height:'+staHeight+'px;'"></view>
			<view class="top titles">
				<uni-icons @tap="getBack" type="arrowleft" class="arrowleft"></uni-icons><view class="titleText">易通商务助手-修改密码</view>
			</view>
		</view>
		<view class="geduan" :style="'height: calc('+((staHeight*2)+80)+'rpx);'"></view>

		<view class="login_mian">
			<view class="title">验证码已发送至手机</view>
            <view class="mobile">{{mobile}}</view>
			<view class="login_center">
				<view class="login_input">
					<view class="left">
						<image class="img img2" src="../../../static/login/cloud.png" mode=""></image>
					</view>
					<input v-model="cloud" class="uni-input srinput srinput1" placeholder="输入短信验证码" />
					<view v-if="count&&countDown==60" class="cloud" @tap="getcloud">
                        获取验证码
					</view>
                    <view v-if="countDown!=60" class="cloud">
                        <span>{{countDown}}</span><span>s后再次发送</span>
					</view>
                    <view v-if="!count&&countDown==60" class="cloud cloudS" @tap="getcloud">
                        重发验证码 
					</view>
				</view>

			</view>
			<view class="login_footer">
				<button class="login_but login_but_back" @tap='getNewPW'>下一步</button>
			</view>
		</view>
	</view>
</template>

<script>
import uniIcons from "@dcloudio/uni-ui/lib/uni-icons/uni-icons.vue"
	export default {
		data() {
			return {
				mobile: '',
                cloud: '',
                count:true,
                countDown:60
			};
		},
		onLoad: function(option) {
            this.mobile=option.mobile
        },
        onReady(){
            this.getcloud()
        },
        components:{
            uniIcons
        },
		methods: {
			getcloud() {
                console.log("获取验证码")
                let data={
                    'mobile':this.mobile,
                    'type':'laterPassword'
                }
                this.$fetch("/apiVerification.html",data)
                    .then((response) => {
                        console.log(response)
                        uni.showToast({
                            title: response.msg,
                            icon:'none',
                            duration: 2000
                        });
                        if(response.code==0){
                            let countDownTime = setInterval(() => { //倒计时
                                this.countDown--;
                                if (this.countDown == 0) {
                                    this.countDown = 60
                                    this.count=false
                                    clearInterval(countDownTime);
                                }
                            }, 1000)
                        }
                    })
				console.log(data)
            },
            goPassword(){
                uni.navigateBack({
                    delta: 1
                });
            },
            getNewPW(){//验证找回修改密码
                let data={
                    'mobile':this.mobile,
                    'verification':this.cloud
                }
                let mobile=this.mobile
                this.$fetch("/apiUserUpdate.html?f=verifUpdatePwd",data)
                    .then((response) => {
                        console.log(response)
                        uni.showToast({
                            title: response.msg,
                            icon:'none',
                            duration: 2000
                        });
                        if(response.code==0){
                            uni.navigateTo({
                                url: '/pages/login/newPassword/newPassword?mobile='+mobile
                            });
                        }
                    })
				console.log(data)
                
            },
            getBack(){
                uni.navigateBack({
                    delta: 1
                });
            }
		}
	};
</script>

<style lang="scss">
	.login {
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
            display: flex;
            align-items: center;
            >.img{
                width: 21rpx;
                height: 32rpx;
                display: block;
                margin-left: 42rpx;
                margin-right: 12rpx;
            }
            .arrowleft{
                font-size: 22rpx;
                color: #000000;
                margin-left: 20rpx;
                margin-right: 5rpx;
                .uni-icons{
                    font-size: 38rpx !important;
                    color: #000000 !important;
                }
            }
			.titleText {
				height: 80rpx;
				color: #000000;
				display: flex;
				align-items: center;
				font-size: 32rpx;
				
				//   font-weight: 600;
			}
		}

		.login_mian {
			margin: 0 50rpx;

			>.title {
				font-size: 40rpx;
				font-weight: 600;
				color: rgba(0, 0, 0, 0.85);
				line-height: 56rpx;
				margin-top: 50rpx;
			}
            >.mobile{
                margin-top: 8rpx;
                font-size:28rpx;
                font-family:HelveticaNeue;
                color:rgba(0,0,0,1);
                line-height:51rpx;
            }
			.login_center {
				margin-top: 40rpx;

				.login_input {
					display: flex;
					align-items: center;
					height: 114rpx;
					position: relative;
					border-bottom: 1rpx solid #e9e9e9;

					//   padding-top: 50rpx;
					.center {
						display: flex;
						align-items: center;
						height: 114rpx;
					}

					.left {
						display: flex;
						align-items: center;
						font-size: 24rpx;
						color: #5d5d5d;
						width: 54rpx;

						.img {
							width: 22rpx;
							height: 32rpx;
							display: block;
							margin: 0 10rpx;
						}

						.img1 {
							width: 32rpx;
							margin: 0 5rpx;
						}
                        .img2{
                            width: 32rpx;
                            height: 22rpx;
                            margin: 0 2rpx;
                        }
					}

					.srinput {
						font-size: 28rpx;
						color: #242424;
						z-index: 1;
						width: 70%;
					}
                    .srinput1{
                        width: 50%;
                    }

                    .cloud{
                        padding: 0 10rpx;
                        border-left:1rpx solid #DDDDDD;
                        font-size:28rpx;
                        color:rgba(0,0,0,0.45);
                        height: 80rpx;
                        line-height: 80rpx;
                        display: flex;
						align-items: center;
						justify-content: center;
						position: absolute;
						right: 0;
                    }
                    .cloudS{
                        color: #2F7AF5;
                    }
				}
			}

			.login_footer {
				.login_footer_skip {
					display: flex;
					justify-content: space-between;
					padding: 27rpx 0;
					.txt {
						font-size: 28rpx;
						color: rgba(47, 122, 245, 1);
						line-height: 36rpx;
					}
				}

				.login_but {
					width: 600rpx;
					height: 86rpx;
					background: rgba(47, 122, 245, 1);
					color: #fff;
                    font-size:32rpx;
                    line-height: 86rpx;
                    margin-top: 50rpx;
                    border-radius: 43rpx;
                }
                .login_but_back{
                    margin-top: 80rpx;
                }
			}
		}
	}
</style>
