<template>
	<view :class="count>0?'searchFile searchFile1':'searchFile'">
		<Headertop :titleTop="titleTop"></Headertop>
		<view class="searchFile_main">
			<view class="search_top">
				<view class="search_top_center">
					<image class="img img1" src="../../static/home/search.png"></image>
					<input v-model="searchValue" class="input" @input="bindInput" @confirm="goSearch(searchValue)" confirm-type="search" placeholder-class="input_s" placeholder="输入关键词，搜索知识或文档" type="text">
					<image @tap="eliminate" class="img img2" src="../../static/seach/input_close.png"></image>
				</view>
				<view v-if="searchBut" class="search_top_right" @tap="backSearch(searchValue)">
					取消
				</view>
                <view v-else class="search_top_right" @tap="goSearch(searchValue)">
					搜索
				</view>
			</view>
            <view  class="search_center">
                <view class="search_center_main">
                    <view class="search_center_item">
                        <view class="search_f_title">{{typeName}}（<text>{{count}}</text>）</view>
                        <loadingList v-if="count>0" :showLoad="showLoad" :list="list" :load="load" @updateCollect="updateCollect"></loadingList>
                    </view>
                    <view v-if="count<=0" class="preview_no">
                        <image class="img" src='../../static/seach/was.png'></image>
                        <view class="txt">没找到您想要的结果</view>
                        <view class="txt1">您来到了没有知识的荒原~</view>
                        <view class="txt2">可自行联系管理员添加知识</view>
                    </view>
                </view>
            </view>
            
		</view>
        <NavTab :index="1"></NavTab>
	</view>
</template>

<script>
    import loadingList from "../../components/loadingList.vue"
	import NavTab from "../../components/navTab.vue";
    import Headertop from "../../components/headertop.vue";
    import moment from "moment"
	export default {
		data() {
			return {
				titleTop: "搜索结果",
                searchValue: "",
                searchDefault:"",
                searchBut:true,
                list:[],
                type:"",
                count:"",
                typeName:'',
                load:true,
                showLoad:true,
                totalPage:0,
                page:1
			};
		},
		components: {
			NavTab,
            Headertop,
            loadingList
		},
		onLoad: function(option) {
            console.log(option.data)
            let data=JSON.parse(option.data)
            this.searchValue = data.value
            this.type=data.type
            if(this.type==1){
                this.typeName="文件"
                this.titleTop=this.titleTop+"-文件"
            }
            if(this.type==2){
                this.typeName="常见问题"
                this.titleTop=this.titleTop+"-常见问题"
            }
        },
        onShow(){
            this.getSearchList(this.searchValue,this.page)
                .then((res) => {
                    console.log(res)
                    this.list=res
                    this.searchDefault=this.searchValue
                    if(this.page>=this.totalPage){
                        this.load=false 
                    }
                })
        },
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
                
                this.getSearchList(this.searchDefault,this.page+1)
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
            getSearchList(Value,page){
                return new Promise((resolve, reject) => {
                    let data={
                        "key":Value,
                        "type":this.type,
                        "page":page
                    }
                    this.$post("/apiIndex.html?f=indexSearchTypeList",data)
                        .then((res) => {
                            console.log("ndexSearchTypeList",res)
                            this.count=res.data.count
                            this.totalPage=res.data.totalPage
                            res.data.list.map((item)=>{
                                item.tt=moment(item.update_time).format('YYYY-MM-DD HH:MM')
                                item.update_time=moment(item.update_time).format('YYYY-MM-DD')
                            })
                            resolve(res.data.list)
                        })
                })
            },
			eliminate() {
				this.searchValue = ""
			},
			goSearch(src) {
                let value=src
                this.page=1
                if(value&&value!=''){
                    this.getSearchList(this.searchDefault,this.page)
                        .then((res) => {
                            console.log(res)
                            this.list=res
                            this.searchDefault=this.searchValue
                            this.searchBut=true
                        })
                }
            },
            backSearch(src){
                console.log("取消")
                uni.switchTab({
                    url: '/pages/search/search'
                });
            },
            bindInput(e){
                console.log(e.detail.value)
                if(e.detail.value==this.searchDefault){
                    this.searchBut=true
                }else{
                    this.searchBut=false
                }
            },
		}
	};
</script>

<style lang='scss'>
	.searchFile {
		letter-spacing: 1rpx;
		min-height: 100vh;
		background: #EFEFF4;

		.searchFile_main {
			.search_top {
				display: flex;
				align-items: center;
				justify-content: space-between;
				height: 92rpx;

				.search_top_center {
					margin-left: 30rpx;
					display: flex;
					align-items: center;
					justify-content: center;
					position: relative;
					width: 608rpx;
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

				.search_top_right {
					width: 112rpx;
					font-size: 28rpx;
					color: #2F7AF5;
					text-align: center;
				}

			}
            .search_center{
                margin-top: 10rpx;
                .search_center_title{
                    font-size:26rpx;
                    color:rgba(0,0,0,0.3);
                    line-height:28rpx;
                    margin-left: 30rpx;
                }
                .search_center_main{
                    margin-top: 10rpx;
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
                    
                    .search_center_item{
                        .search_f_title{
                            margin-left: 30rpx;
                            font-size:26rpx;
                            color:rgba(0,0,0,0.3);
                            line-height:51rpx;
                            padding-bottom: 7rpx;
                            border-bottom: 1rpx solid rgba(0,0,0,0.1);
                        }
                        .search_f_footer{
                            display: flex;
                            align-items: center;
                            justify-content: space-between;
                            margin: 4rpx 0 13rpx;
                            .txt{
                                margin-left: 146rpx;
                                color:rgba(47,122,245,1);
                                line-height:51rpx;
                                font-size:24rpx;
                            }
                            .img{
                                width:14rpx;
                                height: 26rpx;
                                display: block;
                                margin-right: 50rpx;
                            }
                        }
                    }
                    
                }
            }
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
