<template>
	<view class="login">
		<view class="status_bar" :style="'height: calc('+((staHeight*2)+80)+'rpx);'">
			<view class="top_view" :style="'height:'+staHeight+'px;'"></view>
			<view class="top titles">
				<view class="titleText">易通商务助手</view>
			</view>
		</view>
		<view class="geduan" :style="'height: calc('+((staHeight*2)+80)+'rpx);'"></view>

		<view class="login_mian">
			<view class="title">员工登录</view>
			<view class="login_center">
				<view class="login_input">
					<view class="left">
						<image class="img img1" src="../../../static/login/user.png" mode=""></image>
					</view>
					<input v-model="mobile" class="uni-input srinput" placeholder="手机号码" />
				</view>
				<view class="login_input">
					<view class="left">
						<image class="img img2" src="../../../static/login/cloud.png" mode=""></image>
					</view>
					<input v-model="cloud" class="uni-input srinput srinput1" placeholder="短信验证码" />
					<view v-if="count&&countDown==60" class="cloud" @tap="getcloud">
                        <view class="txt">获取验证码</view>
					</view>
                    <view v-if="countDown!=60" class="cloud">
                        <view class="txt"><span>{{countDown}}</span><span>s后再次发送</span></view>
					</view>
                    <view v-if="!count&&countDown==60" class="cloud cloudS" @tap="getcloud">
                        <view class="txt">重发验证码</view>
					</view>
				</view>

			</view>
			<view class="login_footer">
				<view class="login_footer_skip">
					<view class="txt" @tap="goPassword">账号密码登录</view>
					<!-- <view class="txt">忘记密码</view> -->
				</view>
				<button class="login_but" @tap='login'>登录</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mobile: '',
                cloud: '',
                count:true,
                countDown:60
			};
        },
        onShow(){
            let that=this
            wx.getStorage({
				key: "phoneNum",
				success(res) {
					console.log(res.data);
					if(res.data.length==11){
                        that.mobile=res.data
                        return false;
                    }
				},
				fail(res) {

				}
            });
        },
		onLoad: function(option) {},
		methods: {
			getcloud() {
                console.log("获取验证码")
                let data={
                    'mobile':this.mobile,
                    'type':'login'
                }
                if(this.mobile==''){
                    uni.showToast({
                        title: "请输入手机号码",
                        icon:'none',
                        duration: 2000
                    });
                    return false; 
                }
                if(this.mobile.length!=11){
                    uni.showToast({
                        title: "请输入正确的手机号",
                        icon:'none',
                        duration: 2000
                    });
                    return false;
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
            login(){
                let data={
                    'mobile':this.mobile,
                    'verificationCode':this.cloud,
                }
                if(this.mobile==''){
                    uni.showToast({
                        title: "请输入手机号码",
                        icon:'none',
                        duration: 2000
                    });
                    return false; 
                }
                if(this.mobile.length!=11){
                    uni.showToast({
                        title: "请输入正确的手机号",
                        icon:'none',
                        duration: 2000
                    });
                    return false;
                }
                if(this.cloud==''){
                    uni.showToast({
                        title: "请输入验证码",
                        icon:'none',
                        duration: 2000
                    });
                    return false; 
                }
                this.$fetch("/apiLogin.html?f=verifLogin",data)
                    .then((response) => {
                        console.log(response)
                        uni.showToast({
                            title: response.msg,
                            icon:'none',
                            duration: 2000
                        });
                        if(response.code==0){
                            uni.setStorage({
                                key: "token",
                                data: response.data.token,
                                success: function() {
                                    console.log("success");
                                }
                            });
                            uni.setStorage({
                                key: "phoneNum",
                                data: data.mobile,
                                success: function() {
                                    console.log("success");
                                }
                            });
                            uni.reLaunch({
                                url: '/pages/index/index',
                                animationType: 'none',
                            });
                        }
                    })
                console.log(data)
            },
            goPassword(){
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

			.titleText {
				height: 80rpx;
				color: #000000;
				display: flex;
				align-items: center;
				font-size: 32rpx;
				margin-left: 30rpx;
				//   font-weight: 600;
			}
		}

		.login_mian {
			margin: 0 50rpx;

			>.title {
				font-size: 40rpx;
				font-weight: 600;
				color: rgba(0, 0, 0, 1);
				line-height: 56rpx;
				margin-top: 50rpx;
			}

			.login_center {
				margin-top: 50rpx;

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
                        
                        
                        font-size:28rpx;
                        color:rgba(0,0,0,0.45);
                        height: 115rpx;
                        line-height: 115rpx;
                        display: flex;
						align-items: center;
						justify-content: center;
						position: absolute;
						right: 0;
                        .txt{
                            padding: 0 10rpx;
                            height: 80rpx;
                            line-height: 80rpx;
                            border-left:1rpx solid #DDDDDD;
                        }
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
			}
		}
	}
</style>
