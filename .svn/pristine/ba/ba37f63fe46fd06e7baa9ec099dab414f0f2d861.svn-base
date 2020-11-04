<template>
    <view class="details">
        <Headertop :titleTop="titleTop"></Headertop>
        <view class="details_main">
            <!-- 信息图展 -->
            <view class="banner">
                <image class="img" src="../static/images/shangwuxiangqing.png"></image>
                <view class="banner_center">
                    <view :class="ranking<10?'ranking':ranking<100?'ranking ranking2':'ranking ranking3'"><view class="txt"><text>No.</text><text class="num">{{ranking}}</text></view></view>
                    <view class="info">
                        <view class="name">{{saleName}}</view>
                        <view class="depName">{{depName}}</view>
                        <view class="kehuNum">
                            客户数量：{{custNum}}家
                        </view>
                    </view>
                </view>
            </view>
            <view class="details_Table">
                <view class="details_Table_name">
                    <view class="name">业绩墙</view>
                    <view class="tishi">(每晚24点更新数据)</view>
                </view>
                <view @tap="showPop3()" class="details_Table_time">
                    <view class="time">{{customDate.value.start}} 到 {{customDate.value.end}}</view>
                    <image class="img" src="../static/images/riqi.png"></image>
                </view>
                <view @tap="offPop3" catchtouchmove='ture' :style="customDate.dom?'display:block;top: calc('+((staHeight*2)+80)+'rpx);':'display:none;top: calc('+((staHeight*2)+80)+'rpx);'" class="shade_popup show_popup">
                </view>
                <view :style="customDate.dom?'display:block':'display:none'" :class="customDate.shade?'popup_content popup_content3 popup_content_show3':'popup_content popup_content3 popup_content_no3'">
                    <view class="top">
                        <view class="title">日期选择</view>
                        <view @tap="offPop3" class="off_icon">
                            <image class="img" src="../../static/dataModule/icon_clone.png"></image>
                        </view>
                    </view>
                    <view class="main">
                        <uni-calendar
                        :range="true"
                        :insert="true"
                        :lunar="false"
                        :showMonth="false"
                        :start-date="customDate.startDate"
                        :end-date="customDate.endDate"
                        :date="customDate.dufData"
                        @change="change"
                        />
                        <view class="cal_data">
                            <view class="time_txt">
                                <view class="m1">
                                    <view class="title">开始：</view>
                                    <view class="t1">{{customDate.data.start}}</view>
                                </view>
                                <view class="m1">
                                    <view class="title">结束：</view>
                                    <view class="t1">{{customDate.data.end}}</view>
                                </view>
                            </view>
                            <button class="time" @tap="confirmTime">确定</button>
                        </view>
                    </view>
                </view>
                <view class="details_Table_list">
                    <view class="title">
                        <view class="title_item title_item1">客户名称</view>
                        <view class="title_item">营收（万）</view>
                        <view class="title_item">成本（万）</view>
                        <view :class="Permission==4?'title_item title_item4':'title_item'">{{Permission==4?'全口径毛利（万）':'毛利（万）'}}</view>
                    </view>
                    <view v-for="(item,index) in list" :key="index" class="list">
                        <view class="list_item list_item1">{{item.business_type}}</view>
                        <view class="list_item">{{item.revenue}}</view>
                        <view class="list_item">{{item.cost}}</view>
                        <view :class="Permission==4?'list_item list_item4':'list_item'">{{item.profit}}</view>
                    </view>
                    <view class="list_total">
                        <view class="list_item list_item1 list_item1_total">总计</view>
                        <view class="list_item">{{total.revenue}}</view>
                        <view class="list_item">{{total.cost}}</view>
                        <view :class="Permission==4?'list_item list_item4':'list_item'">{{total.profit}}</view>
                    </view>
                </view>
            </view>
        </view>
    </view>
</template>

<script>
    import Headertop from "../../components/headertop.vue";
    import uniCalendar from '@/components/uni-calendar/uni-calendar.vue'
    import moment from "moment";
    
    export default {
        data() {
            return {
                titleTop:"商务详情",
                staHeight:"",
                rpx:1,
                saleName:'',
                ranking:"888",//排名
                depName:"",
                custNum:"",
                list:[
                    {
                        "business_type": "隐私号",      //业务线
                        "revenue": 69.36,               //营收
                        "cost": 38.21,                  //成本
                        "profit": 26.05                 //毛利
                    },
                    {
                        "business_type": "短信",
                        "revenue": 6.04,
                        "cost": 5.03,
                        "profit": 0.92
                    },
                    {
                        "business_type": "云线路",
                        "revenue": 6.26,
                        "cost": 4.17,
                        "profit": 2.08
                    }
                ],
                total: {                  //总计
                    "revenue": 81.66,       //总营收  
                    "cost": 47.42,          //总成本
                    "profit": 29.05         //总毛利
                },
                customDate:{
                    shade:false,
                    dom:false,
                    startDate:"2010-01-01",
                    endDate:"",
                    data:{
                        start:"----年--月--日 ---",
                        end:"----年--月--日 ---"
                    },
                    dateObj:{
                        start:"",
                        end:""
                    },
                    dufData:"",
                    value:{
                        start:"",
                        end:""
                    },
                },
                Permission:''
            }
        },
        components: {
            Headertop,
            uniCalendar
        },
        onLoad: function(option) {
            console.log(option)
            this.saleName=option.name
		},
        onShow(){
            console.log("状态栏高度");
			let that = this;
			wx.getSystemInfo({
				success: e => {
                    console.log("设备版本信息",e)
					console.log("状态栏高度", e.statusBarHeight);
					that.staHeight = e.statusBarHeight;
					that.rpx = e.windowWidth / 750;
				}
            });
            this.getDataAuth()
            .then((res)=>{
                console.log(res)
                this.Permission=res
            })
        },
        onReady(){
            let nowDate = new Date();
            let endTime=nowDate.getTime() - 24 * 60 * 60 * 1000
            let startTime=nowDate.getTime() -7 * 24 * 60 * 60 * 1000
            this.customDate.value.start=moment(startTime).format('YYYY.MM.DD')
            this.customDate.value.end=moment(endTime).format('YYYY.MM.DD')
            

            this.getDataDetail(moment(startTime).format('YYYY-MM-DD'),moment(endTime).format('YYYY-MM-DD'),this.saleName)
        
        },
        methods: {
                getDataAuth() {
                    return new Promise((resolve, reject) => {
                        this.$post("/apiBusiness.html?f=getDataAuth")
                            .then((res) => {
                                console.log("getDataAuth==>获取数据权限", res)
                                resolve(res.data);
                            })
                    })
                },
                getDataDetail(start,end,saleName) {//获取单个销售业务线数据
                    return new Promise((resolve, reject) => {
                        let data={
                            "start":start,
                            "end":end,
                            "sale_name":saleName
                        }
                        this.$post("/apiBusiness.html?f=getDataDetail",data)
                            .then((res) => {
                                console.log("getDataDetail==>获取单个销售业务线数据", res)
                                this.list=res.data.list
                                this.total=res.data.total
                                this.ranking=res.data.info.rank
                                this.custNum=res.data.info.cust_name
                                this.depName=res.data.info.dep_name
                            })
                    })
                },
                change(e) {
                console.log(e);
                if(e.range.data.length==0){
                    console.log("ssssssss")
                    if(e.range.before){
                        this.customDate.data.start=this.getTimeForm(e.range.before)
                    }else{
                        this.customDate.data.start="----年--月--日 ---"
                        this.customDate.data.end="----年--月--日 ---"
                    }
                }else{
                    this.customDate.dateObj.start=e.range.data[0]
                    this.customDate.dateObj.end=e.range.data[e.range.data.length-1]
                    this.customDate.data.start=this.getTimeForm(e.range.data[0])
                    this.customDate.data.end=this.getTimeForm(e.range.data[e.range.data.length-1])
                }
            },
            showPop3(id){
                setTimeout(() => {
                    this.customDate.dom = true
                    this.customDate.shade = true
				}, 100)
            },
            offPop3(){
                setTimeout(() => {
					this.customDate.shade = false		
                    setTimeout(() => {
                        this.customDate.dom = false
                    }, 200)
				}, 100)
            },
            getTimeForm(str){
                let t1= new Date(str)
                console.log("t1",t1)
                let day=""
                switch (t1.getDay()) {
                    case 0:
                        day = "星期天";
                        break;
                    case 1:
                        day = "星期一";
                        break;
                    case 2:
                        day = "星期二";
                        break;
                    case 3:
                        day = "星期三";
                        break;
                    case 4:
                        day = "星期四";
                        break;
                    case 5:
                        day = "星期五";
                        break;
                    case 6:
                        day = "星期六";
                }
               return moment(t1).format("YYYY")+"年"+ moment(t1).format("MM")+"月"+ moment(t1).format("DD")+"日 "+day;
            },
            confirmTime(){
                console.log("confirmTime")
                console.log(this.customDate.dateObj.start)
                console.log(this.customDate.dateObj.end)
                let t1=new Date(this.customDate.dateObj.start)
                let t2=new Date(this.customDate.dateObj.end)
                let days=t2.getTime() - t1.getTime()
                days= Math.floor(days / (24 * 3600 * 1000))
                if(this.customDate.data.start=="----年--月--日 ---"){
                    uni.showToast({
                        title: "请选择开始时间",
                        icon:'none',
                        duration: 2000
                    });
                    return
                }else if(this.customDate.data.end=="----年--月--日 ---"){
                    uni.showToast({
                        title: "请选择结束时间",
                        icon:'none',
                        duration: 2000
                    });
                    return
                }
                console.log("days",days)
                // if(days>30){
                //     uni.showToast({
                //         title: "时间范围不能超出一个月",
                //         icon:'none',
                //         duration: 2000
                //     });
                // }else{
                    this.customDate.value.start=moment(this.customDate.dateObj.start).format('YYYY.MM.DD')
                    this.customDate.value.end=moment(this.customDate.dateObj.end).format('YYYY.MM.DD')
                    this.offPop3()
                    this.getDataDetail(this.customDate.dateObj.start,this.customDate.dateObj.end,this.saleName)
                // }
                
            }
        },
    }
</script>

<style lang='scss' >
    .details{
        .details_main{
            .banner{
                width: 750rpx;
                height: 306rpx;
                position: relative;
                .img{
                    width: 750rpx;
                    height: 306rpx;
                    display: block;
                }
                .banner_center{
                    position: absolute;
                    top: 0;
                    bottom: 0;
                    left: 0;
                    right: 0;
                    display: flex;
                    .ranking{
                        width: 120rpx;
                        height: 120rpx;
                        padding-left:66rpx;
                        padding-top: 46rpx;
                        text-align: center;
                        display: flex;
                        justify-content: center;
                        align-items: center;
                        font-size: 36rpx;
                        color: #FFFFFF;
                        .txt{
                            line-height: 60rpx;
                            .num{
                                font-size: 60rpx;
                                line-height: 60rpx;
                            }
                        }
                    }
                    .ranking2{
                        font-size: 30rpx;
                        .txt{
                            line-height: 60rpx;
                            .num{
                                font-size: 46rpx;
                                line-height: 60rpx;
                            }
                        }
                    }
                    .ranking3{
                        font-size: 26rpx;
                        .txt{
                            line-height: 60rpx;
                            .num{
                                font-size: 38rpx;
                                line-height: 60rpx;
                            }
                        }
                    }
                    .info{
                        padding-left:65rpx;
                        padding-top: 88rpx;
                        color: #FFFFFF;
                        .name{
                            font-size: 40rpx;
                            line-height: 40rpx;
                            margin-bottom: 19rpx;
                        }
                        .depName{
                            font-size: 26rpx;
                            line-height: 26rpx;
                            margin-bottom: 30rpx;
                        }
                        .kehuNum{
                            font-size: 22rpx;
                            line-height: 22rpx;
                        }
                    }
                }
            }
            .details_Table{
                height: 825rpx;
                margin: 0 20rpx;
                margin-top: 10rpx;
                border-radius:6rpx;
                box-shadow:0rpx 0rpx 9rpx 0rpx rgba(213,213,213,0.4);
                .details_Table_name{
                    padding-top: 47rpx;
                    margin:0 19rpx;
                    display: flex;
                    justify-content: space-between;
                    align-items: center;
                    .name{
                        font-size:32rpx;
                        line-height: 32rpx;
                        color: #232323;
                        font-weight: 600;
                    }
                    .tishi{
                        font-size:20rpx;
                        line-height: 20rpx;
                        color: #959595;
                    }
                }
                .details_Table_time{
                    margin-top: 33rpx;
                    margin-left: 19rpx;
                    margin-bottom: 19rpx;
                    display: flex;
                    align-items: center;
                    height: 26rpx;
                    .time{
                        font-size: 24rpx;
                        line-height: 24rpx;
                        color: #797979;
                        margin-right: 14rpx;
                    }
                    .img{
                        width: 30rpx;
                        height: 26rpx;
                        display: block;
                    }
                }
                .details_Table_list{
                    margin: 0 20rpx;
                    .title{
                        display: flex;
                        justify-content: space-between;
                        align-items: center;
                        height: 90rpx;
                        font-size:25rpx;
                        line-height: 25rpx;
                        color: #939393;
                        background: #F7F7FD;
                        .title_item{
                            width: 27%;
                            text-align: center;
                        }
                        .title_item1{
                            padding-left: 29rpx;
                            text-align: left;
                            width: 19%;
                        }
                        .title_item4{
                            width: 35%;
                            text-align: center;
                        }
                    }
                    .list,.list_total{
                        display: flex;
                        justify-content: space-between;
                        align-items: center;
                        height: 90rpx;
                        font-size:24rpx;
                        line-height: 24rpx;
                        color: #232323;
                        background: #fff;
                        .list_item{
                            width: 27%;
                            text-align: center;
                        }
                        .list_item1{
                            padding-left: 29rpx;
                            text-align: left;
                            width: 19%;
                        }
                        .list_item1_total{
                            color: #FF8D13;
                        }
                        .list_item4{
                            width: 35%;
                            text-align: center;
                        }
                    }
                    .list:nth-child(2n+1){
                        background: #F7F7FD;
                    }
                }
            }
        }
    }


//日历_first
    .shade_popup {
        position: fixed;
        top: calc(var(--status-bar-height) + 80rpx);
        right: 0;
        left: 0;
        bottom: 0;
        z-index: 100;
        background: rgba(0, 0, 0, 0.4);	
    }
    .popup_content {
        position: fixed;
        left: 0;
        right: 0;
        background: #fff;
        height: 608rpx;
        z-index: 101;
        border-radius: 20rpx 20rpx 0 0;
        padding-bottom: 60rpx;
        .top{
            background: rgba(53,125,245,0.05);
            display: flex;
            justify-content: space-between;
            padding: 0 24rpx;
            height: 100rpx;
            align-items: center;
            .off{
                font-size: 30rpx;
                color: #999999;
            }
            .on{
                font-size: 30rpx;
                color: #357DF5; 
            }
        }
        .search_top_center {
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            height: 60rpx;
            background: rgba(255, 255, 255, 1);
            box-shadow: 0px 2rpx 10rpx 0px rgba(213, 213, 213, 1);
            border-radius: 6rpx;
            margin: 10rpx 24rpx;
            >.input{
                width: 100%;
                height: 100%;
                padding-left:66rpx;
                padding-right:54rpx;
                font-size:26rpx;
                position: relative;
                z-index: 1;
            }
            .input_s{
                font-size:26rpx;
                color: #BBBBBB;
            }
            .img {
                width: 30rpx;
                height: 30rpx;
                display: block;
                z-index: 2;
            }
            >.img1{
                position: absolute;
                left: 24rpx;
                top: 0;
                bottom: 0;
                margin: auto 0;
            }
            >.img2{
                position: absolute;
                padding: 0 24rpx;
                right: 0;
                top: 0;
                bottom: 0;
                margin: auto 0;
            }
            
        }
        .main{
            height: 428rpx;
            overflow: scroll;
            .list{
                background: #FFFFFF;
                border: 1rpx solid #CCCCCC;
                border-radius: 4rpx;
                font-size: 30rpx;
                color: #999999;
                padding: 15rpx 0 15rpx 24rpx;
                margin: 32rpx 24rpx;
            }
            .list_action{
                background: rgba(53,125,245,0.10);
                border: 1rpx solid #357DF5;
                border-radius: 4rpx;
                color: #357DF5;
            }
        }
    }
    .popup_content_show{
        bottom: 0;
        animation: myfirst7 0.2s linear;
    }
    .popup_content_no{
        bottom: -668rpx;
        animation: myfirst8 0.2s linear;
    }

    .popup_content2{
        height: 548rpx;
    }
    .popup_content_show2{
        bottom: 0;
        animation: myfirst9 0.2s linear;
    }
    .popup_content_no2{
        bottom: -608rpx;
        animation: myfirst10 0.2s linear;
    }

    .popup_content3{
        height: 1056rpx;
        padding-bottom: 0;
        .top{
            justify-content: center;
            position: relative;
            .title{
                font-size: 30rpx;
                color: #666666;
            }
            .off_icon{
                position: absolute;
                right: 0;
                top: 0;
                bottom: 0;
                display: flex;
                align-items: center;
                padding: 0 24rpx;
                .img{
                    width: 40rpx;
                    height: 40rpx;
                    display: block;
                }
            }
        }
        .main{
            height: 100%;
        }
    }
    .popup_content_show3{
        bottom: 0;
        animation: myfirst11 0.2s linear;
    }
    .popup_content_no3{
        bottom: -1056rpx;
        animation: myfirst12 0.2s linear;
    }

    .cal_data{
        height: 156rpx;
        display: flex;
        background: #F5F8FE;
        justify-content: space-between;
        align-items: center;
        padding: 0 24rpx;
        padding-bottom: 20rpx;
        .time_txt{
            .m1{
                display: flex;
                align-items: center;
                font-size: 28rpx;
                color: #666666;
                line-height: 52rpx;
            }
        }
        .time{
            background: #357DF5;
            border-radius: 12rpx;
            width: 200rpx;
            height: 90rpx;
            color: #fff;
            font-size: 40rpx;
            line-height: 90rpx;
            margin: 0;

        }
        .button-hover{
            background: rgba(106, 156, 243, 0.9);
        }
    }
    @keyframes myfirst7 {
		0% {
			bottom: -668rpx;
		}
		100% {
			bottom: 0;
		}
	}
    @keyframes myfirst8 {
		0% {
			bottom: 0;
		}

		100% {
			bottom: -668rpx;
		}
	}
    @keyframes myfirst9 {
		0% {
			bottom: -608rpx;
		}
		100% {
			bottom: 0;
		}
	}
    @keyframes myfirst10 {
		0% {
			bottom: 0;
		}

		100% {
			bottom: -608rpx;
		}
	}
    @keyframes myfirst11 {
		0% {
			bottom: -1056rpx;
		}
		100% {
			bottom: 0;
		}
	}
    @keyframes myfirst12 {
		0% {
			bottom: 0;
		}

		100% {
			bottom: -1056rpx;
		}
	}
.uni-calendar__backtoday{
    display: none;
    
}

.uni-calendar-item--multiple{
    background:#DEE9FF !important;
    color: #357DF5 !important;
}

.uni-calendar-item--checked{
    background-color: #DEE9FF !important;
    color: #357DF5 !important;
}
.uni-calendar-item--after-checked .uni-calendar-item__weeks-box-item,
.uni-calendar-item--before-checked .uni-calendar-item__weeks-box-item{
    background: #357DF5 !important;
    .uni-calendar-item--checked{
        background-color: #357DF5 !important;
    }
    text{
        background-color: #357DF5 !important;
        color: #fff !important;
    }
}
.uni-calendar-item--before-checked,.uni-calendar-item--after-checked{
    background-color: #357DF5 !important;
}
//日历_end
</style>