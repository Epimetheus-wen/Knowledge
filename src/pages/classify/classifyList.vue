<template>
	<view class="classifyList">
		<Headertop :titleTop="titleTop"></Headertop>
		<view class="classifyList_main">
			<view class="classifyList_top">
				<view class="classifyList_top_center">
					<image class="img img1" src="../../static/home/search.png"></image>
					<input v-model="searchValue" class="input" @confirm="goSearch(searchValue)" confirm-type="search"
					 placeholder-class="input_s" placeholder="在全部知识中搜索" type="text">
					<image @tap="eliminate" class="img img2" src="../../static/seach/input_close.png"></image>
				</view>
			</view>
			<view class="classifyList_center">
				<view class="classifyList_center_top">
					<view class="left">
						<view class="txt" @tap="updateType('全部')" :class="{'on': type == ''}">全部</view>
						<view class="txt" @tap="updateType(1)" :class="{'on': type == 1}">文件</view>
						<view class="txt" @tap="updateType(2)" :class="{'on': type == 2}">常见问题</view>
					</view>
					<view class="right">
						<button v-if="type==1" class="filtrate" @tap="upPopup">
							<view :class="popup1.shade?'txt1':'txt'">{{popup1.name}}</view>
							<image v-if="popup1.shade" class="img" src="../../static/classify/up.png"></image>
							<image v-else class="img" src="../../static/classify/down.png"></image>
						</button>
						<button class="filtrate" @tap="upPopup1">
							<view :class="popup2.shade?'txt1':'txt'">{{popup2.name}}</view>
							<image v-if="popup2.shade" class="img" src="../../static/classify/up.png"></image>
							<image v-else class="img" src="../../static/classify/down.png"></image>
						</button>
						<view v-if="type==1" @tap="upPopup" :style="'top: calc('+((staHeight*2)+240)+'rpx);'" :class="popup1.shade?'shade_popup show_popup':'shade_popup'">
							<view class="popup_content">
								<view 
                                v-for="(item,index) in popup1.list" 
                                :key="index" 
                                :class="popup1.n==item.index?'select_item select_item_av':'select_item'"
                                @tap="getFormat(item.index,item.id)"
                                >
									<image mode="widthFix" class="img" :src="item.img"></image>
									<view class="txt">{{item.value}}</view>
								</view>
							</view>
						</view>
                        <view @tap="upPopup1" :style="'top: calc('+((staHeight*2)+240)+'rpx);'" :class="popup2.shade?'shade_popup show_popup':'shade_popup'">
							<view class="popup_content">
								<view 
                                v-for="(item,index) in popup2.list" 
                                :key="index" 
                                :class="popup2.n==item.index?'select_item select_item_av':'select_item'"
                                @tap="getFormat1(item.index,item.id)"
                                >
									<image mode="widthFix" class="img1" :src="item.img"></image>
									<view class="txt">{{item.value}}</view>
								</view>
							</view>
						</view>

					</view>
				</view>
                <view>
                    <loadingList v-if="list.length>0" :showLoad="showLoad" :list="list" :load="load" @updateCollect="updateCollect"></loadingList>
                    <view v-if="list.length<=0" class="preview_no">
                        <image class="img" src='../../static/seach/was.png'></image>
                        <view class="txt">没找到您想要的结果</view>
                        <view class="txt1">您来到了没有知识的荒原~</view>
                        <view class="txt2">可自行联系管理员添加知识</view>
                    </view>
                </view>
			</view>
		</view>
		<NavTab :index="2"></NavTab>
	</view>
</template>


<script>
	import NavTab from "../../components/navTab.vue";
	import Headertop from "../../components/headertop.vue";
    import loadingList from "../../components/loadingList.vue"
    import moment from "moment"
	export default {
		data() {
			return {
                titleTop: "分类检索-结果",
                searchValue:"",
                staHeight:'',
				obj: {
                    id:''
                },
				type: '',
				arrows: false,
				popup1: {
                    shade: false,
                    name:'格式',
					n: 0,
					list: [{
							value: '全部格式',
							id: '',
							img: '../../static/classify/format.png',
							index: 0
						},
						{
							value: 'Word',
							id: '1',
							img: '../../static/home/word.png',
							index: 1
						},
						{
							value: 'PPT',
							id: '2',
							img: '../../static/home/ppt.png',
							index: 2
						},
						{
							value: 'PDF',
							id: '3',
							img: '../../static/home/pdf.png',
							index: 3
						},
						{
							value: 'Excel',
							id: '4',
							img: '../../static/home/excel.png',
							index: 4
						},
						{
							value: 'TXT',
							id: '7',
							img: '../../static/home/txt.png',
							index: 5
						},
						{
							value: 'Rar/Zip',
							id: '6',
							img: '../../static/home/zip.png',
							index: 6
						},
						{
							value: 'JPEG/PNG',
							id: '5',
							img: '../../static/home/img.png',
							index: 7
                        }
					]
				},
                popup2: {
                    shade: false,
                    name:'排序',
					n: 0,
					list: [{
							value: '按更新时间顺序',
							id: '2',
							img: '../../static/classify/sheng.png',
							index: 0
						},
						{
							value: '按更新时间倒序',
							id: '1',
							img: '../../static/classify/dao.png',
							index: 1
						},
						{
							value: '按名称排序（A~Z）',
							id: '3',
							img: '../../static/classify/minchen.png',
							index: 2
						}
					]
                },
                list:[],
                load:true,
                showLoad:true,
                totalPage:0,
                page:1
			};
		},
		components: {
			NavTab,
			Headertop,
			loadingList,
		},
		onLoad: function(option) {

			this.obj = JSON.parse(option.str)
            console.log(this.obj)
            if(this.obj.id==""){
                this.searchValue=this.obj.indexName
            }

            
		},
		onShow() {
            console.log("状态栏高度");
                let that = this;
            wx.getSystemInfo({
                success: e => {
                    console.log("状态栏高度", e.statusBarHeight);
                    that.staHeight = e.statusBarHeight;
                }
            });

            this.getCateRepositoryList(this.popup2.list[this.popup2.n].id,this.type,this.popup1.list[this.popup1.n].id,this.obj.id,this.searchValue,this.page)
                .then((res) => {
                    console.log(res)
                    this.list=res
                    if(this.page>=this.totalPage){
                        this.load=false 
                    }
                })
        },
		onHide() {},
		onReady() {

        },
        onReachBottom(ev){
            console.log(ev)
            let totalPage=this.totalPage
            console.log(this.page)
            if(this.page>=totalPage){
                this.load=false 
            }
            if((this.page+1)<=totalPage){
                
                this.getCateRepositoryList(this.popup2.list[this.popup2.n].id,this.type,this.popup1.list[this.popup1.n].id,this.obj.id,this.searchValue,this.page)
                    .then((res) => {
                        console.log(res)
                        this.list=this.list.concat(res)
                    })

                this.page+=1
                 var query = wx.createSelectorQuery();
                    query.select('.list_footer').boundingClientRect()
                    query.selectViewport().scrollOffset()
                    query.exec((res) => {
                        console.log(res)
                        uni.pageScrollTo({
                            scrollTop: res[1].scrollTop,
                            duration: 300
                        });
                    })
            }
        },
		methods: {
             updateCollect(index){
                console.log(index)
                if(this.list[index].fav==0){
                    this.list[index].fav=1
                }else{
                    this.list[index].fav=0
                }
            },
            getCateRepositoryList(sort,type,ftype,cateid,value,page){
                return new Promise((resolve, reject) => {
                    let data={
                        "sort":sort,
                        "type":type,
                        "ftype":ftype,
                        "cate_id":cateid,
                        "key":value,
                        "page":page
                    }
                    this.$post("/apiIndex.html?f=getCateRepositoryList",data)
                        .then((res) => {
                            console.log("ndexSearchTypeList",res)
                            this.totalPage=res.data.totalPage
                            res.data.list.map((item)=>{
                                item.tt=moment(item.update_time).format('YYYY-MM-DD HH:MM')
                                item.update_time=moment(item.update_time).format('YYYY-MM-DD')
                            })
                            resolve(res.data.list)
                        })
                })
            },
            goSearch(src) {
                this.page=1
                this.updateList()
            },
            eliminate() {
				this.searchValue = ""
			},
			updateType(id) {
				console.log(id)
				if (id == '全部') {
					this.type = ''
				} else {
					this.type = id
                }
                this.updateList()
            },
            updateList(){
                this.page=1
                this.getCateRepositoryList(this.popup2.list[this.popup2.n].id,this.type,this.popup1.list[this.popup1.n].id,this.obj.id,this.searchValue,this.page)
                .then((res) => {
                    console.log(res)
                    this.list=res
                    if(this.page>=this.totalPage){
                        this.load=false 
                    }
                })
            },
			upPopup() {
                if(this.popup2.shade == false){
                    setTimeout(()=>{
                        if (this.popup1.shade) {
                            this.popup1.shade = false
                        } else {
                            this.popup1.shade = true
                        }
                    },100) 
                }
                 
            },
            getFormat(index,id){
                this.popup1.n=index
                if(index==0){
                    this.popup1.name='格式'
                }else{
                    this.popup1.name=this.popup1.list[index].value
                }
                this.updateList()
                console.log(id)
            },
            upPopup1() {
                if(this.popup1.shade == false){
                    setTimeout(()=>{
                        if (this.popup2.shade) {
                            this.popup2.shade = false
                        } else {
                            this.popup2.shade = true
                        }
                    },100) 
                }
 
            },
            getFormat1(index,id){
                this.popup2.n=index
                if(index==0){
                    this.popup2.name='顺序'
                }else if(index==1){
                    this.popup2.name='倒序'
                }else if(index==2){
                    this.popup2.name='A~Z'
                }
                this.updateList()
                console.log(id)
            }

		}
	};
</script>

<style lang="scss">
	.classifyList {
		display: flex;
		flex-direction: column;
		min-height: 100vh;

		.classifyList_main {
			flex: 1;
			display: flex;
			flex-direction: column;

			.classifyList_top {
				display: flex;
				align-items: center;
				justify-content: center;
				height: 92rpx;
				background: #EFEFF4;

				.classifyList_top_center {
					display: flex;
					align-items: center;
					justify-content: center;
					position: relative;
					width: 690rpx;
					height: 60rpx;
					background: rgba(255, 255, 255, 1);
					box-shadow: 0px 2rpx 10rpx 0px rgba(213, 213, 213, 1);
					border-radius: 6rpx;

					>.input {
						width: 100%;
						height: 100%;
						padding-left: 66rpx;
						padding-right: 54rpx;
						font-size: 26rpx;
						position: relative;
						z-index: 1;
					}

					.input_s {
						font-size: 26rpx;
						color: #BBBBBB;
					}

					.img {
						width: 30rpx;
						height: 30rpx;
						display: block;
						z-index: 2;
					}

					>.img1 {
						position: absolute;
						left: 24rpx;
						top: 0;
						bottom: 0;
						margin: auto 0;
					}

					>.img2 {
						position: absolute;
						padding: 0 24rpx;
						right: 0;
						top: 0;
						bottom: 0;
						margin: auto 0;
					}

				}
			}

			.classifyList_center {
				.classifyList_center_top {
					height: 68rpx;
					margin: 0 30rpx;
					display: flex;
					justify-content: space-between;
                    border-bottom: 2rpx solid rgba(0,0,0,0.1);
					.left {
						display: flex;
                        align-items: center;

						>.txt {
							color: rgba(0, 0, 0, 0.3);
							line-height: 51rpx;
							font-size: 26rpx;
							margin-right: 30rpx;
						}

						.on {
							color: #2F7AF5;
						}
					}

					.right {
						display: flex;

						.filtrate {
							background: #fff;
							display: flex;
							align-items: center;
							padding-left: 30rpx;
							padding-right: 0;

							.img {
								width: 16rpx;
								height: 10rpx;
								display: block;
								margin-left: 12rpx;
							}

							.txt {
								font-size: 22rpx;
								color: rgba(0, 0, 0, 0.3);
								line-height: 30px;

							}
                            .txt1{
                                font-size: 22rpx;
								color: rgba(0, 0, 0, 0.5);
								line-height: 30px;
                            }
						}
					}
				}
			}

		}
	}

	.shade_popup {
		position: absolute;
		top: calc(var(--status-bar-height) + 240rpx);
		right: 0;
		left: 0;
		bottom: 0;
		z-index: 100;
		background: rgba(0, 0, 0, 0.4);
		display: none;

		.popup_content {
			background: #fff;

			.select_item_av {
				background: rgba(239, 239, 244, 1);
                >.txt {
					color: rgba(0, 0, 0, 0.85);
				}
			}

			.select_item {
				height: 82rpx;
				display: flex;
				align-items: center;
				padding-left: 36rpx;

				>.img {
					width: 46rpx;
					height: 54rpx;
					display: block;
					margin-right: 23rpx;
				}
                >.img1 {
					width: 29rpx;
					height: 35rpx;
					display: block;
					margin-right: 23rpx;
				}

				>.txt {
					line-height: 51rpx;
					font-size: 26rpx;
					color: rgba(0, 0, 0, 0.45);
				}
			}
		}
	}

.show_popup {
    display: block !important;
}
.preview_no{
    .img{
        width:210rpx;
        height:210rpx;
        display: block;
        margin: 226rpx 270rpx 0 270rpx;
    }
    .txt{
        margin-top: 40rpx;
        margin-bottom: 23rpx;
        text-align: center;
        font-size:30rpx;
        color:rgba(0,0,0,0.45);
        line-height:51rpx;
    }
    .txt1,.txt2{
        text-align: center;
        line-height:40rpx;
        color:rgba(0,0,0,0.3);
        font-size:28rpx;
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
</style>
