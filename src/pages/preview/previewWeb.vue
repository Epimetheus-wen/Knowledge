<template>
	<view class="previewWeb">
		<Headertop :titleTop="titleTop"></Headertop>
		<view class="previewWeb_main" :style="'min-height: calc(100vh - ('+((staHeight*2)+80)+'rpx));'">
            <view class="previewWeb_main_top">
                <view class="top_title">{{obj.title}}</view>
                <view class="top_item">
                    <view class="top_item_bq">解决方案</view>
                    <view class="top_item_time">创建<text>{{obj.tt}}</text></view>
                </view>
                <view v-if="loginType" class="top_item_but" @tap="toClassifyList(obj.name)">
                    <view class="txt">进入<text>{{obj.name}}</text>阅读更多</view>
                </view>
            </view>
            <view class="previewWeb_main_center">
                <rich-text class="center_main" :nodes="obj.answer"></rich-text>
            </view>
            <view class="previewWeb_main_bottom">
                <view class="item">
                    <view class="item_txt">最近修改：</view>
                    <view class="item_num">{{obj.tt}}</view>
                </view>
                <view class="item">
                    <view class="item_txt">更新人：</view>
                    <view class="item_num">{{obj.nickname}}</view>
                </view>
            </view>
		</view>
        <view class="footer_but" v-if="loginType">
            <button v-if="imgCode==''&&obj.scope==1" open-type="getUserInfo"  class="share">
                <image class="img" src="../../static/user/fenxiang.png"></image>
                <view class="txt">分享问题</view>
            </button>
            <button v-if="!imgCode==''&&obj.scope==1" open-type="share" class="share">
                <image class="img" src="../../static/user/fenxiang.png"></image>
                <view class="txt">分享问题</view>
            </button>
            <button v-if="obj.scope==2" @tap="shareNo" class="share">
                <image class="img" src="../../static/user/fenxiang.png"></image>
                <view class="txt">分享问题</view>
            </button>
             <view class="collect" @tap="collect(obj.id)">
                <image v-if="obj.fav==1" class="img" src='../../static/home/star_true.png'></image>
                <image v-if="obj.fav==0" class="img" src='../../static/home/star_false.png'></image>
                <view class="txt">收藏</view>
            </view>
            <view class="haibao">
                <canvas id="code-canvas" canvas-id="codecanvas" :style="'width:'+(218*rpx)+'px;height:'+(175*rpx)+'px;'"></canvas>
            </view>
        </view>
    </view>
</template>


<script>
	import Headertop from "../../components/headertop.vue";
    import moment from "moment"
	export default {
		data() {
			return {
                titleTop: "常见问题详情",
                staHeight:'',
                haibaoUrl:"",
                imgCode: "",
                nickName:"",
                loginType:false,
                rpx:1,
				obj: {
                    id:''
                },
			};
		},
		components: {
			Headertop
		},
		onLoad: function(option) {
			this.obj = JSON.parse(decodeURIComponent(option.str))
            console.log(option)
            console.log(this.obj)
         
		},
		onShow() {
            console.log("状态栏高度");
            let that = this;
            wx.getSystemInfo({
                success: e => {
                    console.log("状态栏高度", e.statusBarHeight);
                    that.staHeight = e.statusBarHeight;
                    that.rpx = e.windowWidth / 375;
                }
            });
            console.log(this.imgCode)
            // wx.getStorage({
            //     key: "loginType",
            //     success(res) {
            //         that.loginType=true
            //         console.log(res.data)
            //         if(res.data==1007){
            //             that.loginType=false
            //         }else{
            //             that.loginType=true
            //         }

            //     },
            //     fail(res) {
            //          that.loginType=false
            //     }
            // })
            wx.getStorage({
                key: "token",
                success(res) {
                    if(res.data){
                        that.loginType=true
                    }else{
                        that.loginType=false
                    }
                },
                fail(res) {
                    that.loginType=false
                }
            })
            this.getUserInfo1()
        },
        onHide() {},
        onShareAppMessage(res) {

            if(this.obj.scope==2){
                
                return {
                    title: "易通商务助手",
                    path: '/pages/index/index',
                    imageUrl:this.haibaoUrl,
                    success: function(ress) {
                        console.log(ress)
                    }
                }
            }else{
                console.log('转发',this.haibaoUrl)
                let str=JSON.stringify(this.obj)
                let users = wx.getStorageSync('str')
                console.log(users)
                return {
                    title: this.obj.title,
                    path: '/pages/preview/previewWeb?str='+encodeURIComponent(str),
                    imageUrl:this.haibaoUrl,
                    success: function(ress) {
                        console.log(ress)
                    }
                }
            }
            
		},
		onReady() {
            let centertxt=this.obj.answer
            this.obj.answer=centertxt.replace(/<img/g, "<img class='imgs'");
            console.log(this.obj.answer)
            

        },
		methods: {
            shareNo(){
                uni.showToast({
                    title: "该文档无法分享",
                    icon:'none',
                    duration: 2000
                });
            },
            collect(id){
                console.log(id)
                if(this.obj.fav==0){
                    this.obj.fav=1
                }else{
                    this.obj.fav=0
                }
                this.$post("/apiIndex.html?f=collect",{"id":id})
                    .then((res) => {
                        console.log("collect",res)
                    })
            },
            toClassifyList(value){
                console.log(value)
                let obj={
                    id:"",
                    indexName:value
                }
                let str=JSON.stringify(obj)
                uni.navigateTo({
                    url: '/pages/classify/classifyList?str='+str
                });
            },
            getUserInfo1(){
                let that=this
                wx.getUserInfo({
                    success: function (res) {
                     var userInfo = res.userInfo
                     that.nickName=userInfo.nickName
                     console.log(userInfo)
                    //  that.imgCode=userInfo.avatarUrl
                        wx.downloadFile({
                            url: userInfo.avatarUrl, 
                            success (res) {
                                if (res.statusCode === 200) {
                                    console.log(res)
                                    that.imgCode=res.tempFilePath
                                    that.share()
                                }
                            }
                        })
                    },
                    fail:function(res){
                        console.log(res)
                    }
                })
            },
            share() {
				console.log("share")
                let that=this
                const codecanvas = wx.createCanvasContext("codecanvas", this);
                codecanvas.drawImage("../../static/user/fx_bg.png", 0, 0, (218*that.rpx), (175*that.rpx))
                codecanvas.drawImage("../../static/home/rtf.png", (93*that.rpx), (30*that.rpx), (40*that.rpx), (48*that.rpx))
                codecanvas.drawImage(that.imgCode, (20*that.rpx), (132*that.rpx), (30*that.rpx), (30*that.rpx))
                codecanvas.fillStyle="#242424"
                if(that.rpx<=1){
                    console.log(that.rpx*10)
                    let num=Math.round(that.rpx*10)
                    codecanvas.font=num+'px SimHei'
                }else{
                    console.log(that.rpx*10)
                    let num=Math.round(that.rpx*10)
                    codecanvas.font=num+'px SimHei'
                }
                codecanvas.fillStyle="#fff"
                console.log(that.rpx)
                codecanvas.fillText("来自"+that.nickName+"的分享",(60*that.rpx),(150*that.rpx))
                    codecanvas.draw(
                        false,
                        setTimeout(function() {
                        wx.canvasToTempFilePath({
                            canvasId: "codecanvas",
                            success: function(res) {
                                console.log(res)
                                that.haibaoUrl = res.tempFilePath;
                            },
                            fail: function (e) {
                                console.log("AAAA", e)
                            }
                        },that);
                        }, 1000)
                    );

			},
        }
	};
</script>

<style lang="scss">
    .previewWeb{
        .previewWeb_main{
            min-height: calc(100vh - (var(--status-bar-height) + 80rpx));
            position: relative;
            
            .previewWeb_main_top{
                padding-top: 9rpx;
                position: relative;
                .top_title{
                    margin-left: 30rpx;
                    line-height:51rpx;
                    color:rgba(0,0,0,0.85);
                    font-size:30rpx;
                }
                .top_item{
                    display: flex;
                    align-items: center;
                    margin: 0 30rpx;
                    padding: 7rpx 0 8rpx 0;
                    border-bottom: 2rpx solid rgba(0,0,0,0.1);
                    .top_item_bq{
                        width:106rpx;
                        height:38rpx;
                        background:rgba(193,202,220,1);
                        border-radius:19rpx;
                        color: #fff;
                        font-size:22rpx;
                        line-height: 38rpx;
                        text-align: center;
                        margin-right: 13rpx;
                    }
                    .top_item_time{
                        font-size:20rpx;
                        color:rgba(0,0,0,0.3);
                        line-height:51rpx;
                    }
                }
                .top_item_but{
                    position:absolute;
                    right: 0;
                    bottom: 24rpx;
                    height:40rpx;
                    background-image: url("../../static/user/yindaotiao.png");
                    background-size: 100% 100%;
                    padding: 0 46rpx 0 24rpx;
                    .img{
                        width: 100%;
                        height: 100%;
                        display: block;
                    }
                    .txt{
                        font-size:24rpx;
                        color: #FFFFFF;
                        line-height: 40rpx;
                        display: flex;
                        align-items: center;
                        text{
                            max-width: 130rpx;
                            color: #FFD200;
                            display: block;
                            overflow: hidden;
                            white-space: nowrap;
                            text-overflow: ellipsis;
                        }
                    }
                }
            }
            .previewWeb_main_center{
                margin: 44rpx 30rpx 0rpx 30rpx;
                padding-bottom:313rpx ;
                .center_main{
                    font-size:26rpx;
                    color:rgba(0,0,0,0.65);
                    line-height:40rpx;
                    .imgs{
                       margin: 20rpx auto;
                        display: block;
                        width: 100%;
                        height: 100%;
                    }
                }
            }
            .previewWeb_main_bottom{
                position:absolute;
                bottom: 168rpx;
                left: 36rpx;
                
                .item{
                    display: flex;
                    align-items: center;
                    font-size:22rpx;
                    color:rgba(0,0,0,0.45);
                    line-height:38rpx;
                }
            }
        }
        .footer_but{
            position:fixed;
            left: 0;
            right: 0;
            bottom: 0;
            display: flex;
            align-items: center;
            justify-content: flex-end;
            height: 100rpx;
            border-top: 1rpx solid #DDDDDD;
            background: #fff;
            .share,.collect{
                display: flex;
                align-items: center;
                margin-right: 49rpx;
                .img{
                    width: 34rpx;
                    height: 32rpx;
                    display: block;
                    margin-right: 15rpx;
                }
                .txt{
                    font-size:28rpx;
                    color:rgba(0,0,0,0.45);
                }
            }
            .share{
                padding: 0;
                background: #fff;
            }
        }
        
    }
    .haibao{
        position: absolute;
        top: 100000rpx;
        left: 0rpx;
        right: 0;
        bottom: 100rpx;
        margin: auto;
        z-index: -1;
        opacity:0;
        #code-canvas{
            margin: 0 auto;
            opacity:0;
        }
    }
    .haibao1{
        position: absolute;
        top: 800rpx;
        left: 0rpx;
        right: 0;
        bottom: 100rpx;
        margin: auto;
        z-index: 1;
        opacity:0;
        background: #fff;
    }
</style>
