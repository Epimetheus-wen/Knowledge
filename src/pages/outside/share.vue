<template>
	<view class="previewWeb" @tap="dowInfo">
		<Headertop :titleTop="titleTop"></Headertop>
		<view class="previewWeb_main" :style="'min-height: calc(100vh - ('+((staHeight*2)+80)+'rpx) - 190rpx);'">
            <view class="previewWeb_main_center">
                <image mode="widthFix" v-if="obj.type==1&&(obj.accessory_type=='docx'||obj.accessory_type=='doc')" class="img" src="../../static/home/word.png"></image>
                <image mode="widthFix" v-if="obj.type==1&&(obj.accessory_type=='ppt'||obj.accessory_type=='pptx')" class="img" src="../../static/home/ppt.png"></image>
                <image mode="widthFix" v-if="obj.type==1&&obj.accessory_type=='pdf'" class="img" src="../../static/home/pdf.png"></image>
                <image mode="widthFix" v-if="obj.type==1&&(obj.accessory_type=='xls'||obj.accessory_type=='xlsx')" class="img" src="../../static/home/excel.png"></image>
				<image mode="widthFix" v-if="obj.type==1&&(obj.accessory_type=='rar'||obj.accessory_type=='zip')" class="img" src="../../static/home/zip.png"></image>
                <image mode="widthFix" v-if="obj.type==1&&(obj.accessory_type=='png'||obj.accessory_type=='jpg')" class="img" src="../../static/home/img.png"></image>
                <image mode="widthFix" v-if="obj.type==1&&obj.accessory_type=='txt'" class="img" src="../../static/home/txt.png"></image>
            </view>
            <view class="info">
                <view class="name">{{obj.accessory_name}}</view>
                <view class="size">
                    文件大小：{{obj.accessory_size}}M
                </view>
                <view class="load" v-if="againShow">
                    <image src="../../static/user/load_03.png" class="load_icon"></image>
                    <view class="txt">正在加载</view>
                </view>
                <button :style="zipShow?'display:none;':'display:block;'" v-else class="again" @tap="downloadFile(obj.filePath)">{{againTxt}}</button>
                <view v-if="zipShow" class="zipShow">请前往PC端操作</view>
            </view>
		</view>
        <!-- <view class="footer_but">
            <view class="information" @tap.stop="showInfo">
                <image v-if="information" class="img" src="../../static/user/shuxin.png"></image>
                <image v-else class="img" src="../../static/user/shuxin_no.png"></image>
                <view class="txt">信息</view>
                <view v-if="information" class="supernatant">
                    <view class="name">{{obj.accessory_name}}</view>
                    <view class="size">
                        文件大小：{{obj.accessory_size}}M
                    </view>
                    <view class="time">最近更新：{{obj.tt}}</view>
                    <view class="upname">更新人：{{obj.nickname}}</view>
                    <view class="triangleDiv"></view>
                </view>
            </view>
            <button v-if="imgCode==''&&obj.scope==1" open-type="getUserInfo" @getuserinfo="upImgIcon" withCredentials="true"  class="share">
                <image @tap="upImgIcon" class="img" src="../../static/user/fenxiang.png"></image>
                <view @tap="upImgIcon" class="txt">分享问题</view>
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
        </view> -->
        
	</view>
</template>


<script>
    import Headertop from "../../components/headertop.vue";
    import moment from "moment"
	export default {
		data() {
			return {
                titleTop: "文件预览",
                staHeight:'',
                againShow:true,
                againTxt:"点击再次阅览文件",
                platform:"",
                haibaoUrl:"",
                imgIcon:"",
                imgCode: "",
                nickName:"",
                loginType:false,
                rpx:1,
                zipShow:false,
				obj: {
                    id:''
                },
                information:false
			};
		},
		components: {
            Headertop,
		},
		onLoad: function(option) {
			this.obj = JSON.parse(decodeURIComponent(option.str))
            console.log(option)
            console.log(this.obj)
            
		},
		onShow() {
            
            this.information=false
            // this.againShow=true
            console.log("状态栏高度");
                let that = this;
            wx.getSystemInfo({
                success: e => {
                    console.log("状态栏高度", e.statusBarHeight);
                    that.staHeight = e.statusBarHeight;
                    that.rpx = e.windowWidth / 375;
                }
            });
            if(this.obj.accessory_type=="docx"||this.obj.accessory_type=="doc"){
                this.imgIcon="../../static/home/word.png"
            }
            if(this.obj.accessory_type=="ppt"||this.obj.accessory_type=='pptx'){
                this.imgIcon="../../static/home/ppt.png"
            }
            if(this.obj.accessory_type=="xls"||this.obj.accessory_type=='xlsx'){
                this.imgIcon="../../static/home/excel.png"
            }
            if(this.obj.accessory_type=="pdf"){
                this.imgIcon="../../static/home/pdf.png"
            }
            if(this.obj.accessory_type=="zip"||this.obj.accessory_type=='rar'){
                this.imgIcon="../../static/home/zip.png"
            }
            if(this.obj.accessory_type=="jpg"||this.obj.accessory_type=='png'){
                this.imgIcon="../../static/home/img.png"
            }
            if(this.obj.accessory_type=="txt"){
                this.imgIcon="../../static/home/txt.png"
            }
        },
        onHide() {},
		onReady() {
            
            let that=this
            wx.getSystemInfo({
                success: function (res) {
                    console.log("设备",res)
                    that.platform=res.platform
                }
            })
            this.downloadFile(this.obj.filePath)
        },
		methods: {
            showInfo(){
                if(this.information==false){
                    this.information=true
                }else{
                    this.information=false
                }
                console.log(this.information)
            },
            dowInfo(){
                    this.information=false
                console.log(this.information)
            },
            downloadFile(obj) {
                let that=this
                if(this.platform=="ios"){
                    wx.downloadFile({
                        // 示例 url，并非真实存在
                        url: obj.file,
                        success: function(res) {
                            console.log(res);
                            let fileManager = wx.getFileSystemManager();
                            const filePath = res.tempFilePath;
                            console.log(fileManager)
                            if(that.obj.accessory_type=="jpg"||that.obj.accessory_type=="png"){
                                // 预览图片
                                wx.previewImage({
                                    current: filePath,
                                    urls: [filePath],
                                    success: function (res) {
                                        console.log(res);
                                        setTimeout(()=>{
                                            that.againShow=false
                                        },300)
                                    },
                                    fail: function () {
                                        //console.log('fail')
                                    },
                                })
                            }else if(that.obj.accessory_type=="rar"||that.obj.accessory_type=="zip"){
                                that.againShow=false
                                that.zipShow=true
                            }else if(that.obj.accessory_type=="txt"){
                                    wx.getFileSystemManager().readFile({  //读取文件
                                        filePath: filePath,
                                        encoding: 'utf-8',
                                        success: res => {
                                            console.log("txt",res.data)
                                        },
                                        fail: console.error
                                    })
                                    let txtObj={
                                        url:obj.file,
                                        name:obj.fileName
                                    }
                                    setTimeout(()=>{
                                            that.againShow=false
                                        },300)
                                    txtObj=JSON.stringify(txtObj)
                                    uni.navigateTo({
                                        url: '/pages/outside/outside?txtObj='+txtObj
                                    });

                            }else{
                                wx.openDocument({
                                    filePath: filePath,
                                    success: function(res) {
                                        console.log("打开文件",res);
                                        setTimeout(()=>{
                                            that.againShow=false
                                        },300)
                                    }
                                });
                            }
                            

                        }
                    });
                }else{
                    wx.downloadFile({
                        // 示例 url，并非真实存在
                        url: obj.file,
                        success: function(res) {
                            console.log(res);
                            let fileManager = wx.getFileSystemManager();
                            const filePath = res.tempFilePath;
                            let newPath = wx.env.USER_DATA_PATH + "/" + obj.fileName;
                            wx.getFileSystemManager().renameSync(filePath, newPath);
                            console.log(fileManager)
                            if(that.obj.accessory_type=="rar"||that.obj.accessory_type=="zip"){
                                uni.showToast({
                                    title: "内部存储/tencent/MicroMsg/wxanewfiles/.../"+obj.fileName,
                                    icon:'none',
                                    duration: 5000
                                });
                                setTimeout(()=>{
                                    that.againTxt="点击再次下载文件"
                                    that.againShow=false
                                },300)
                            }else{
                                wx.openDocument({
                                    filePath: newPath,
                                    success: function(res) {
                                        console.log("打开文档成功");
                                        setTimeout(()=>{
                                            that.againShow=false
                                        },300)
                                    }
                                });
                            }
                            
                        }
                    });
                }
            },
        }
	};
</script>

<style lang="scss">
    .previewWeb{
        .previewWeb_main{
            min-height: calc(100vh - (var(--status-bar-height) + 80rpx) - 190rpx);
            background: #EFEFF4;
            padding-top:190rpx;
            .previewWeb_main_center{
                width:360rpx;
                height:280rpx;
                background:rgba(255,255,255,1);
                border-radius:6rpx;
                border:2rpx solid rgba(0,0,0,0.1);
                margin: 0 auto 30rpx auto;
                display: flex;
                align-items: center;
                justify-content: center;
                .img{
                    width:80rpx;
                    height:96rpx;
                    display: block;
                }
            }
            .info{
                margin: 0 auto;
                width: 680rpx;
                .name{
                    text-align: center;
                    line-height:51rpx;
                    font-size:30rpx;
                    color:rgba(0,0,0,0.6);
                    margin-bottom: 8rpx;
                }
                .size{
                     text-align: center;
                    line-height:28rpx;
                    color:rgba(0,0,0,0.3);
                    font-size:26rpx;
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
            justify-content: space-around;
            height: 100rpx;
            border-top: 1rpx solid #DDDDDD;
            background: #fff;
            .share,.collect,.information{
                display: flex;
                align-items: center;
                height: 100rpx;
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
                margin: 0;
                height: 98rpx;
            }
            .information{
                position: relative;
                .supernatant{
                    background: #A4B3FF;
                    position:absolute;
                    left: -12rpx;
                    bottom: 85rpx;
                    width: 393rpx;
                    padding: 30rpx 52rpx 36rpx 36rpx;
                    line-height:40rpx;
                    color:rgba(255,255,255,1);
                    font-size:26rpx;
                    border-radius:6rpx;
                    z-index: 1;

                    .triangleDiv{
                        position: absolute;
                        bottom: -10rpx;
                        left: 20rpx;
                        border-color: #A4B3FF transparent transparent transparent;
                        border-style: solid;
                        border-width: 12rpx 10rpx 0 10rpx;
                        height: 0;
                        width: 0;
                    }
                }
            }
        }
    }
    .load{
        position: relative;
        width: 160rpx;
        display: flex;
        align-items: center;
        margin: 0 auto;
        margin-top:62rpx;
        height: 36rpx;
        .load_icon{
            position: absolute;
            left: 0;
            top: 0;
            bottom: 0;
            width: 36rpx;
            height: 36rpx;
            display: block;
            animation:myfirst1 1s linear infinite; 
        }
        >.txt{
            padding-left: 50rpx;
            line-height:36rpx;
            font-size:24rpx;
            color:rgba(0,0,0,0.25);
        }
    }
    .zipShow{
        text-align: center;
        margin-top:100rpx;
        line-height:36rpx;
        font-size:24rpx;
        color:rgba(0,0,0,0.25);
    }
    .again{
        width:510rpx;
        height:82rpx;
        margin: 0 auto;
        background: #2F7AF5;
        margin-top:100rpx;
        color: #fff;
        border-radius: 6rpx;
        font-size:32rpx;
    }
    .searchFile1{
        background: #fff;
        .searchFile_main{
            .search_top{
                background: #EFEFF4;
            }
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
    .haibao{
        position: absolute;
        top: 800rpx;
        left: 0rpx;
        right: 0;
        bottom: 100rpx;
        margin: auto;
        z-index: 0;
        opacity:0;
        #code-canvas{
            margin: 0 auto;
            opacity:0;
        }
    }
</style>
