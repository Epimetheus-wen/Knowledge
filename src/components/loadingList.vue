<template>
	<view class="list_loading">
		<view class="list">
			<view class="list_item" v-for="(item,index) in list" :key="item.id">
				<image mode="widthFix" @tap="recordHistory(item.id)" v-if="item.type==1&&(item.accessory_type=='docx'||item.accessory_type=='doc')" class="img" src="../static/home/word.png"></image>
                <image mode="widthFix" @tap="recordHistory(item.id)" v-if="item.type==1&&(item.accessory_type=='ppt'||item.accessory_type=='pptx')" class="img" src="../static/home/ppt.png"></image>
                <image mode="widthFix" @tap="recordHistory(item.id)" v-if="item.type==1&&item.accessory_type=='pdf'" class="img" src="../static/home/pdf.png"></image>
                <image mode="widthFix" @tap="recordHistory(item.id)" v-if="item.type==1&&(item.accessory_type=='xls'||item.accessory_type=='xlsx')" class="img" src="../static/home/excel.png"></image>
				<image mode="widthFix" @tap="recordHistory(item.id)" v-if="item.type==1&&(item.accessory_type=='rar'||item.accessory_type=='zip')" class="img" src="../static/home/zip.png"></image>
                <image mode="widthFix" @tap="recordHistory(item.id)" v-if="item.type==1&&(item.accessory_type=='png'||item.accessory_type=='jpg')" class="img" src="../static/home/img.png"></image>
                <image mode="widthFix" @tap="recordHistory(item.id)" v-if="item.type==1&&item.accessory_type=='txt'" class="img" src="../static/home/txt.png"></image>
                <image mode="widthFix" @tap="recordHistory(item.id)" v-if="item.type==2" class="img" src="../static/home/rtf.png"></image>
                <view class="list_item_center">
					<view class="left" @tap="recordHistory(item)">
						<view class="left_top"><view v-if="item.type==2" class="bq">常见问题</view> <text class="txt" v-text="item.type==1?item.accessory_name:item.type==2?item.title:''">《隐私号接口文档》</text></view>
						<view class="left_bottom">
							<text v-text="item.name+' '">隐私号产品</text><text> | 最近更新：</text><text v-text="item.update_time">2020-06-01</text>
						</view>
					</view>
					<view class="right" @tap="collect(item.id,index)">
						<image v-if="item.fav==1" class="img" src='../static/home/star_true.png'></image>
                        <image v-if="item.fav==0" class="img" src='../static/home/star_false.png'></image>
					</view>
				</view>
			</view>
		</view>
		<view class="list_footer" v-if="showLoad">
            <view v-if="load" class="load_footer">
                <view class="txt">
                    <uni-icons class="load_icon" type="spinner-cycle" size="30"></uni-icons>
                    <text>正在加载中......</text>
                </view>
            </view>
			<image v-if="!load" class="img" src='../static/home/list_footer.png'></image>
		</view>
	</view>
</template>

<script>
import uniIcons from "@dcloudio/uni-ui/lib/uni-icons/uni-icons.vue"
	export default {
		props: ["list","load","showLoad"],
		data() {
			return {
                loading:"loading",
                iconSize:16,
                rpx:1
            };
        },
        components:{
            uniIcons
        },
        onReady(){
            console.log(this.list)
        },
        methods:{
            collect(id,index){
                console.log(id)
                this.$post("/apiIndex.html?f=collect",{"id":id})
                    .then((res) => {
                        console.log("collect",res)
                        this.$emit('updateCollect', index);
                        console.log(this.list)
                    })     
            },
            recordHistory(obj){
                this.$post("/apiIndex.html?f=recordHistory",{"id":obj.id})
                    .then((res) => {
                        console.log("recordHistory",res)
                        console.log("添加记录")
                    })

                    console.log(obj)
                    let str=JSON.stringify(obj)
                    console.log(str)

                    if(obj.type==2){
                        uni.navigateTo({
                            url: '/pages/preview/previewWeb?str='+encodeURIComponent(str)
                        });
                    }
                    if(obj.type==1){
                        uni.navigateTo({
                            url: '/pages/preview/preview?str='+encodeURIComponent(str)
                        });
                    }
            }
        }
	};
</script>

<style lang='scss' >
	.list_loading {
		>.list {
			.list_item {
				display: flex;
				align-items: center;
				margin: 0 30rpx;
				padding: 20rpx 0;
				border-bottom: 2rpx solid rgba(0, 0, 0, 0.1);

				>.img {
					width: 80rpx;
					height: 96rpx;
					display: block;
					margin-right: 15rpx;
				}

				.list_item_center {
					display: flex;
					flex: 1;
					justify-content: space-between;

					.left {
						.left_top {
                            padding-left: 15rpx;
							font-size: 32rpx;
							line-height: 51rpx;
							max-width: 520rpx;
							overflow: hidden;
							white-space: nowrap;
							text-overflow: ellipsis;
                            display: flex;
                            align-items: center;
                            .bq{
                                width:126rpx;
                                height:45rpx;
                                background:rgba(187,198,219,1);
                                border-radius:23rpx;
                                margin-right: 10rpx;
                                font-size:24rpx;
                                color: #fff;
                                text-align: center;
                                line-height: 45rpx;
                            }
                            >.txt{
                                overflow: hidden;
                                white-space: nowrap;
                                text-overflow: ellipsis;
                            }
						}

						.left_bottom {
							padding-left: 15rpx;
							font-size: 26rpx;
							line-height: 51rpx;
							color: rgba(0, 0, 0, 0.3);
							max-width: 520rpx;
							overflow: hidden;
							white-space: nowrap;
							text-overflow: ellipsis;
						}
					}

					.right {
						display: flex;
						align-items: center;
						margin-right: 11rpx;

						>.img {
							width: 33rpx;
							height: 32rpx;
							display: block;
						}
					}
				}
			}
		}
        >.list_footer{
            .img{
                width: 750rpx;
                height: 126rpx;
            }
            .load_footer{
                height: 126rpx !important;
                display: flex;
                align-items: center;
                justify-content: center;
                >.txt{
                    padding-left:45rpx;
                    position: relative;
                    font-size: 26rpx;
                    color: rgba(0,0,0,0.4);
                }
                // .load_icon{
                //     animation:turn 1s linear infinite;  
                // }
            }
            
        }
    }

    
</style>
