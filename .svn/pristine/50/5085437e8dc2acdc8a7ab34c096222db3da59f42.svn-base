<template>
	<view :class="fileList.count>0||problemList.count>0?'searchResult searchResult1':'searchResult'">
		<Headertop :titleTop="titleTop"></Headertop>
		<view class="searchResult_main">
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
            <view class="search_center">
                <view class="search_center_title">搜索结果</view>
                <view class="search_center_main">
                    <view class="search_center_item" v-if="fileList.count>0||problemList.count>0">
                        <view class="search_f_title">文件（<text>{{fileList.count}}</text>）</view>
                        <loadingList :showLoad="showLoad" :list='fileList.list' :load="load" @updateCollect="updateCollect1"></loadingList>
                        <view class="search_f_footer" v-if="fileList.count>3" @tap='toResult(1)'>
                            <view class="txt">全部相关文件</view>
                            <image class="img" src="../../static/seach/right_b.png"></image>
                        </view>
                    </view>
                    <view class="search_center_item" style="margin-top: 15rpx;" v-if="fileList.count>0||problemList.count>0">
                        <view class="search_f_title">常见问题（<text>{{problemList.count}}</text>）</view>
                        <loadingList  :showLoad="showLoad" :list='problemList.list' :load="load" @updateCollect="updateCollect2"></loadingList>
                        <view class="search_f_footer" v-if="problemList.count>3" @tap='toResult(2)'>
                            <view class="txt">全部相关问题</view>
                            <image class="img" src="../../static/seach/right_b.png"></image>
                        </view>
                    </view>

                    <view v-if="fileList.count<=0&&problemList.count<=0" class="preview_no">
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
                fileList:{
                    count:0
                },
                problemList:{
                    count:0
                },
                load:false,
                showLoad:false,
			};
		},
		components: {
			NavTab,
            Headertop,
            loadingList
		},
		onLoad: function(option) {
			console.log(option)
			this.searchValue = option.value

        },
        onShow(){
            this.getSearchList()
        },
		onReady() {
            
        },
		methods: {
            updateCollect1(index){
                console.log(index)
                if(this.fileList.list[index].fav==0){
                    this.fileList.list[index].fav=1
                }else{
                    this.fileList.list[index].fav=0
                }

            },
            updateCollect2(index){
                console.log(index)
                if(this.problemList.list[index].fav==0){
                    this.problemList.list[index].fav=1
                }else{
                    this.problemList.list[index].fav=0
                }

            },
            getSearchList(){
                let data={
                    "key":this.searchValue
                }
                this.$post("/apiIndex.html?f=indexSearchList",data)
                    .then((res) => {
                        console.log("getVisitHistory",res)
                        this.searchDefault=this.searchValue
                        this.searchBut=true
                        this.fileList=res.data.file_list
                        this.problemList=res.data.problem_list
                        this.fileList.list.map((item)=>{
                            item.tt=moment(item.update_time).format('YYYY-MM-DD HH:MM')
                            item.update_time=moment(item.update_time).format('YYYY-MM-DD')
                        })
                    })
            },
			eliminate() {
                this.searchValue = ""
                this.searchBut=false
			},
			goSearch(src) {
				let value=src
                if(value&&value!=''){
                    this.getSearchList()
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
            toResult(id){
                let data={
                    value:this.searchValue,
                    type:id
                }
                uni.navigateTo({
                    url: '/pages/search/searchList?data='+JSON.stringify(data)
                });
            }
		}
	};
</script>

<style lang='scss'>
	.searchResult {
		letter-spacing: 1rpx;
		min-height: 100vh;
		background: #EFEFF4;

		.searchResult_main {
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
                margin-top: 22rpx;
                .search_center_title{
                    font-size:26rpx;
                    color:rgba(0,0,0,0.3);
                    line-height:28rpx;
                    margin-left: 30rpx;
                }
                .search_center_main{
                    margin-top: 15rpx;
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
                            margin: 0 30rpx;
                            font-size:26rpx;
                            color:rgba(0,0,0,0.3);
                            line-height:51rpx;
                            padding-bottom: 7rpx;
                            border-bottom: 2rpx solid rgba(0,0,0,0.1);
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
     .searchResult1{
         background: #fff;
         .searchResult_main{
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
