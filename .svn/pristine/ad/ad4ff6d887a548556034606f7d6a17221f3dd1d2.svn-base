<template>
	<view class="login">
		<view class="status_bar" :style="'height: calc('+((staHeight*2)+80)+'rpx);'">
			<view class="top_view" :style="'height:'+staHeight+'px;'"></view>
			<view class="top titles">
				<uni-icons @tap='getBack' type="arrowleft" class="arrowleft"></uni-icons><view class="titleText">易通商务助手</view>
			</view>
		</view>
		<view class="geduan" :style="'height: calc('+((staHeight*2)+80)+'rpx);'"></view>

		<view class="login_mian">
			<view class="title">设置新密码</view>
            <view class="text">登录密码用于登录移动端与PC网站</view>
			<view class="login_center">
				<view class="login_input">
					<view class="left">
						<image class="img img1" src="../../../static/login/password.png" mode=""></image>
					</view>
					<input v-if="eyes1" v-model="newpassword1" class="uni-input srinput" placeholder="设置新密码" />
					<input v-if="!eyes1" v-model="newpassword1" password class="uni-input srinput" placeholder="设置新密码" />
					<view class="eyes" @tap="eyesPassword1">
						<image v-if="!eyes1" class="eyes_img" src="../../../static/login/close.png" alt></image>
						<image v-if="eyes1" class="eyes_img" src="../../../static/login/open.png" alt></image>
					</view>
				</view>
                <view class="login_input">
					<view class="left">
						<image class="img img1" src="../../../static/login/password.png" mode=""></image>
					</view>
					<input v-if="eyes2" v-model="newpassword2" class="uni-input srinput" placeholder="再次输入密码" />
					<input v-if="!eyes2" v-model="newpassword2" password class="uni-input srinput" placeholder="再次输入密码" />
					<view class="eyes" @tap="eyesPassword2">
						<image v-if="!eyes2" class="eyes_img" src="../../../static/login/close.png" alt></image>
						<image v-if="eyes2" class="eyes_img" src="../../../static/login/open.png" alt></image>
					</view>
				</view>

			</view>
			<view class="login_footer">
				<view class="login_footer_skip">
					<view class="text">* 至少8位，不能全是英文或数字，可含特殊字符</view>
					<!-- <view class="txt">忘记密码</view> -->
				</view>
				<button class="login_but" @tap='goLogin'>前往登录</button>
			</view>
		</view>
	</view>
</template>

<script>
import uniIcons from "@dcloudio/uni-ui/lib/uni-icons/uni-icons.vue"
	export default {
		data() {
			return {
                mobile:'',
                newpassword1: '',
                newpassword2: '',
                eyes1:false,
                eyes2:false,
			};
        },
        components:{
            uniIcons
        },
		onLoad: function(option) {
            this.mobile=option.mobile
        },
		methods: {
            eyesPassword1() {
				console.log("切换")
				if (this.eyes1) {
					this.eyes1 = false
				} else {
					this.eyes1 = true
				}
				console.log(this.eyes1)
            },
            eyesPassword2() {
				console.log("切换")
				if (this.eyes2) {
					this.eyes2 = false
				} else {
					this.eyes2 = true
				}
				console.log(this.eyes2)
            },
            goLogin(){
                let data={
                    'mobile':this.mobile,
                    'password':this.newpassword2,
                }
                if (this.newpassword1 != this.newpassword2) {
                    uni.showToast({
                        title: '两次输入密码不同',
                        icon:'none',
                        duration: 2000
                    });
                    return false;
                }
                if (this.newpassword2.length < 8 || this.newpassword2.length > 16) {
                    uni.showToast({
                        title: '请输入8-16位密码',
                        icon:'none',
                        duration: 2000
                    });
                    return false;
                }

                let str = this.newpassword2
                // var reg = /^(?=.*[a-zA-Z])(?=.*\d)[a-zA-Z\d]{8,16}$/
                // var reg = /((?=.*[a-z])(?=.*\d)|(?=[a-z])(?=.*[#@!~%^&*])|(?=.*\d)(?=.*[#@!~%^&*]))[a-z\d#@!~%^&*]{8,16}/i
                var reg = /^(?=.*[a-zA-Z])(?=.*\d)[a-zA-Z0-9\-\!\@\#\$\%\*\.]{8,15}$/
                if (!reg.test(str)) {
                    uni.showToast({
                        title: '请输入8-16位密码，字母加数字的组合',
                        icon:'none',
                        duration: 2000
                    });
                    return false;
                }
                
                this.$fetch("/apiUserUpdate.html?f=updatePwd",data)
                    .then((response) => {
                        console.log(response)
                        uni.showToast({
                            title: response.msg,
                            icon:'none',
                            duration: 2000
                        });
                        if(response.code==0){
                            uni.reLaunch({
                                url: '/pages/login/password/password'
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
			.titleText {
				height: 80rpx;
				color: #000000;
				display: flex;
				align-items: center;
				font-size: 32rpx;
				// margin-left: 42rpx;
				//   font-weight: 600;
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
            >.text{
                font-size:28rpx;
                line-height:51rpx;
                color:rgba(0,0,0,0.65);
                margin-top: 8rpx;
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
					padding: 25rpx 0;
					.txt {
						font-size: 28rpx;
						color: rgba(47, 122, 245, 1);
						line-height: 36rpx;
                    }
                    >.text{
                        font-size:24rpx;
                        color:rgba(0,0,0,0.45);
                        line-height:36rpx;
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
