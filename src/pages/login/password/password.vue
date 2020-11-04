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
						<image class="img img1" src="../../../static/login/password.png" mode=""></image>
					</view>
					<input v-if="eyes" v-model="password" class="uni-input srinput" placeholder="登录密码" />
					<input v-if="!eyes" v-model="password" password class="uni-input srinput" placeholder="登录密码" />
					<view class="eyes" @tap="eyesPassword">
						<image v-if="!eyes" class="eyes_img" src="../../../static/login/close.png" alt></image>
						<image v-if="eyes" class="eyes_img" src="../../../static/login/open.png" alt></image>
					</view>
				</view>

			</view>
			<view class="login_footer">
				<view class="login_footer_skip">
					<view class="txt" @tap="goCloud">验证码登录</view>
					<view class="txt" @tap="goForget">忘记密码</view>
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
				eyes: false,
				mobile: '',
				password: ''
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
			eyesPassword() {
				console.log("切换")
				if (this.eyes) {
					this.eyes = false
				} else {
					this.eyes = true
				}
				console.log(this.eyes)
            },
            login(){
                let data={
                    'mobile':this.mobile,
                    'password':this.password,
                }
                console.log(data)
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
                if(this.password==''){
                    uni.showToast({
                        title: "请输入登录密码",
                        icon:'none',
                        duration: 2000
                    });
                    return false; 
                }
                
                this.$fetch("/apiLogin.html?f=pwdLogin",data)
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
                            uni.switchTab({
                                url: '/pages/index/index',
                                animationType: 'none',
                            });
                        }
                        
                    })
            },
            goCloud(){
                uni.navigateTo({
                    url: '/pages/login/cloud/cloud'
                });
            },
            goForget(){
                let mobile=this.mobile
                if(mobile==''){
                    uni.showToast({
                        title: "请输入手机号码",
                        icon:'none',
                        duration: 2000
                    });
                    return false; 
                }
                if(mobile.length!=11){
                    uni.showToast({
                        title: "请输入正确的手机号",
                        icon:'none',
                        duration: 2000
                    });
                    return false;
                }
                uni.navigateTo({
                    url: '/pages/login/forgetPassword/forgetPassword?mobile='+mobile
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
					}

					.srinput {
						font-size: 28rpx;
						color: #242424;
						z-index: 1;
						width: 70%;
					}

					.eyes {
						width: 100rpx;
						height: 100%;
						display: flex;
						align-items: center;
						justify-content: center;
						position: absolute;
						right: 0;

						.eyes_img {
							display: block;
							width: 32rpx;
							height: 22rpx;
						}
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
