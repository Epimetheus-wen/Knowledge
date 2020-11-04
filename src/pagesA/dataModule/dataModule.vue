<template>
	<view class="dataModule">
		<Headertop :titleTop="titleTop"></Headertop>
		<view class="dataModule_main">
			<view class="filtrate">
				<view class="filtrate_item">
					<view class="name">客户名称</view>
					<view v-if="BusinessDataType" class="right" @tap="showPop(customerName.n)">
						<view class="value">{{customerName.value}}</view>
						<image class="img" src="../../static/dataModule/right.png"></image>
					</view>
                    <view v-else class="right">
						<view class="value">{{customerName.value}}</view>
						<image class="img" src="../../static/dataModule/right.png"></image>
					</view>
                    <view v-if="customerName.dom" @tap="offPop" catchtouchmove='ture' :style="'top: calc('+((staHeight*2)+80)+'rpx);'" class="shade_popup show_popup">
                    </view>
                    <view v-if="customerName.dom" :class="customerName.shade?'popup_content popup_content_show':'popup_content popup_content_no'">
                        <view class="top">
                            <view @tap="offPop" class="off">取消</view>
                            <view @tap="noPop" class="on">确认</view>
                        </view>
                        <view class="search_top_center">
                            <image class="img img1" src="../../static/home/search.png"></image>
                            <input v-model="searchValue" class="input" @confirm="goSearch(searchValue)" confirm-type="search" placeholder-class="input_s" placeholder="输入客户名称" type="text">
                            <image @tap="eliminate" class="img img2" src="../../static/seach/input_close.png"></image>
                        </view>
                        <view class="main">
                            <view @tap="pitch(item.ID)" v-for="(item,index) in customerName.list" :key="index" :class="customerName.n==item.ID?'list list_action':'list'">{{item.CUST_NAME}}</view>
                        </view>
                    </view>
				</view>
				<view class="filtrate_item">
					<view class="name">Appkey</view>
					<view v-if="BusinessDataType" class="right" @tap="showPop2(appKey.n)">
						<view class="value">{{appKey.value}}</view>
						<image class="img" src="../../static/dataModule/right.png"></image>
					</view>
                    <view v-else class="right">
						<view class="value">{{appKey.value}}</view>
						<image class="img" src="../../static/dataModule/right.png"></image>
					</view>
                    <view v-if="appKey.dom" @tap="offPop2" catchtouchmove='ture' :style="'top: calc('+((staHeight*2)+80)+'rpx);'" class="shade_popup show_popup">
                    </view>
                    <view v-if="appKey.dom" :class="appKey.shade?'popup_content popup_content2 popup_content_show2':'popup_content popup_content2 popup_content_no2'">
                        <view class="top">
                            <view @tap="offPop2" class="off">取消</view>
                            <view @tap="noPop2" class="on">确认</view>
                        </view>
                        <view class="main">
                            <view @tap="pitch2(item.APP_KEY)" v-for="(item,index) in appKey.list" :key="index" :class="appKey.n==item.APP_KEY?'list list_action':'list'">{{item.APP_KEY}}</view>
                        </view>
                    </view>
				</view>
				<view class="filtrate_item">
					<view class="name">自定义日期</view>
					<view v-if="BusinessDataType" class="right" @tap="showPop3()">
						<view class="value">{{customDate.value.start}} 至 {{customDate.value.end}}</view>
						<image class="img" src="../../static/dataModule/right.png"></image>
					</view>
                    <view v-else class="right">
						<view class="value">{{customDate.value.start}} 至 {{customDate.value.end}}</view>
						<image class="img" src="../../static/dataModule/right.png"></image>
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
				</view>
			</view>
			<view class="dataModule_main_center">
				<view class="top">
					<view @tap="cutTab(1)" :class="dataType==1?'top_title top_title_action':'top_title'">业务数据</view>
					<view @tap="cutTab(2)" :class="dataType==2?'top_title top_title_action':'top_title'">消费数据</view>
				</view>
				<view v-if="dataType==1" class="center">
					<view class="info">
						<view class="info_time">数据更新于：{{uptime}}</view>
						<view class="info_list">
							<view class="info_item" hover-class="info_item_hover">
								<view class="title">号码总量</view>
								<view class="num">
                                    <view class="load" v-if="businessData.total_num=='号码总数'">
                                        <image src="../../static/user/load_04.png" class="load_icon"></image>
                                    </view>
                                    <text v-else>{{businessData.total_num}}</text>
                                    <text class="show">{{businessData.total_num}}</text>
                                </view>
								<view :style="businessData.number_rate=='号码利用率'?'opacity: 0;':'opacity: 1;'" class="txt">号码利用率：{{businessData.number_rate}}</view>
							</view>
							<view class="info_item" hover-class="info_item_hover">
								<view class="title">绑定关系数</view>
								<view class="num">
                                    <view class="load" v-if="businessData.bind_num=='绑定数'">
                                        <image src="../../static/user/load_04.png" class="load_icon"></image>
                                    </view>
                                    <text v-else>{{businessData.bind_num}}</text>
                                    <text class="show">{{businessData.bind_num}}</text>
                                </view>
								<view :style="businessData.average_bind=='平均绑定数'?'opacity: 0;':'opacity: 1;'" class="txt">平均绑定数：{{businessData.average_bind}}</view>
							</view>
							<view class="info_item" hover-class="info_item_hover">
								<view class="title">话单量</view>
								<view class="num">
                                    <view class="load" v-if="businessData.bill_num=='话单量'">
                                        <image src="../../static/user/load_04.png" class="load_icon"></image>
                                    </view>
                                    <text v-else>{{businessData.bill_num}}</text>
                                    <text class="show">{{businessData.bill_num}}</text>
                                </view>
								<view :style="businessData.bill_rate=='平均话单量'?'opacity: 0;':'opacity: 1;'" class="txt">平均接听率：{{businessData.bill_rate}}</view>
							</view>
							<view class="info_item" hover-class="info_item_hover">
								<view class="title">通话时长<text class="t">（分）</text></view>
								<view class="num">
                                    <view class="load" v-if="businessData.minute_num=='通话分钟数'">
                                        <image src="../../static/user/load_04.png" class="load_icon"></image>
                                    </view>
                                    <text v-else>{{businessData.minute_num}}</text>
                                    <text class="show">{{businessData.minute_num}}</text>
                                </view>
								<view :style="businessData.average_minute_num=='平均通话分钟数'?'opacity: 0;':'opacity: 1;'" class="txt">平均通话时长：{{businessData.average_minute_num}}</view>
							</view>
						</view>
					</view>
					<view class="answer">
						<view class="title_item">
							<view class="txt">接听率趋势</view>
							<!-- <view class="select">
								<view class="name">展示类型：</view>
								<view class="value">
									<text class="txt">按话单查看</text>
									<image class="img" src="../../static/dataModule/icon_xia.png"></image>
								</view>
							</view> -->
                            <view class="icon_nameId">
                                <view class="title_icon">
                                    <view class="i1"></view>
                                    <view class="n">接听率</view>
                                </view>
                                <view class="title_icon">
                                    <view class="i2"></view>
                                    <view class="n">话单量</view>
                                </view>
                            </view>
						</view>
						<uni-ec-canvas class="uni-ec-canvas " id="line-chart" canvas-id="multi-charts-line" :ec="ec"></uni-ec-canvas>
					</view>
					<view class="answer answer1">
						<view class="title_item">
							<view class="txt">话单统计分析</view>
						</view>
						<view class="centre" hover-class="centre_item_hover">
							<view class="centre_txt1">话单总量</view>
							<view class="centre_txt2">{{billAnalyzeGraph.bill_num}}</view>
                            <view class="show">{{billAnalyzeGraph.bill_num}}</view>
						</view>
						<view class="centre_right">
							<view class="centre_item">
								<view class="icon"></view>
								<view class="title">接听成功：</view>
								<view class="txt1 txt2">{{billAnalyzeGraph.success_rate}}</view>
								<view class="txt1">{{billAnalyzeGraph.success_num}}</view>
							</view>
							<view class="centre_item">
								<view class="icon1"></view>
								<view class="title">接听异常：</view>
								<view class="txt1 txt2">{{billAnalyzeGraph.fail_rate}}</view>
								<view class="txt1">{{billAnalyzeGraph.fail_num}}</view>
							</view>
						</view>
						<uni-ec-canvas class="uni-ec-canvas" id="pie-chart" canvas-id="multi-charts-pie" :ec="ec2"></uni-ec-canvas>
					</view>
                    <view class="answer answer5">
						<view class="title_item">
							<view class="txt">接听异常话单分析</view>
						</view>
						<uni-ec-canvas class="uni-ec-canvas " id="line-chart" canvas-id="multi-charts-line" :ec="ec5"></uni-ec-canvas>
					</view>
                    <view class="answer answer6">
						<view class="title_item">
							<view class="txt">通话时长占比</view>
						</view>
						<uni-ec-canvas class="uni-ec-canvas " id="line-chart" canvas-id="multi-charts-line" :ec="ec6"></uni-ec-canvas>
					</view>
				</view>
				<view v-if="dataType==2" class="center center2">
					<view class="info">
						<view class="info_top">
							<view class="info_time1">费用合计</view>
							<view class="info_time">数据更新于：{{uptime}}</view>
						</view>
						<view class="info_list info_list1">
							<view class="info_item" hover-class="info_item_hover">
								<view class="title">账户余额</view>
								<view class="num">
                                    <view class="load" v-if="costData.balance=='余额'">
                                        <image src="../../static/user/load_04.png" class="load_icon"></image>
                                    </view>
                                    <text v-else>{{costData.balance}}</text>
                                    <text class="show">{{costData.balance}}</text>
                                </view>
							</view>
							<view class="info_item" hover-class="info_item_hover">
								<view class="title">待结清金额</view>
								<view class="num">
                                    <view class="load" v-if="costData.arrearage=='带结清'">
                                        <image src="../../static/user/load_04.png" class="load_icon"></image>
                                    </view>
                                    <text v-else>{{costData.arrearage}}</text>
                                    <text class="show">{{costData.arrearage}}</text>
                                </view>
							</view>
							<view class="info_item" hover-class="info_item_hover">
								<view class="title">昨日消费</view>
								<view class="num">
                                    <view class="load" v-if="costData.yesterday_amount=='昨日消费'">
                                        <image src="../../static/user/load_04.png" class="load_icon"></image>
                                    </view>
                                    <text v-else>{{costData.yesterday_amount}}</text>
                                    <text class="show">{{costData.yesterday_amount}}</text>
                                </view>
							</view>
							<view class="info_item" hover-class="info_item_hover">
								<view class="title">本月总消费</view>
								<view class="num">
                                    <view class="load" v-if="costData.month_amount=='月消费'">
                                        <image src="../../static/user/load_04.png" class="load_icon"></image>
                                    </view>
                                    <text v-else>{{costData.month_amount}}</text>
                                    <text class="show">{{costData.month_amount}}</text>
                                </view>
							</view>
						</view>
					</view>
					<view class="answer">
						<view class="title_item">
							<view class="txt">消费趋势</view>
						</view>
						<uni-ec-canvas class="uni-ec-canvas " id="line-chart" canvas-id="multi-charts-line" :ec="ec3"></uni-ec-canvas>
					</view>
					<view class="answer answer2">
						<view class="title_item">
							<view class="txt">费用占比</view>
						</view>
						<uni-ec-canvas class="uni-ec-canvas " id="line-chart" canvas-id="multi-charts-line" :ec="ec4"></uni-ec-canvas>
					</view>
                    <view class="costTable">
                        <view class="costTable_main">
                            <view class="costTable_title">
                                <view class="txt txt1">消费类型</view>
                                <view class="txt txt2">金额</view>
                                <view class="txt txt3">占比</view>
                            </view>
                            <view class="costTable_list">
                                <view class="txt txt1">
                                    <view class="name">通话费</view>
                                    <!-- <view class="two">
                                        <view class="two_txt ">带录音</view>
                                        <view class="two_txt">不带录音</view>
                                    </view> -->
                                </view>
                                <view class="txt txt2">
                                    <view class="two">
                                        <view class="two_txt">￥{{costRateGraphList.call_money}}</view>
                                        <!-- <view class="two_txt">￥100.00</view> -->
                                    </view>
                                </view>
                                <view class="txt txt3">
                                    <view class="two">
                                        <view class="two_txt">{{costRateGraphRate.call_rate}}</view>
                                        <!-- <view class="two_txt">10%</view> -->
                                    </view>
                                </view>
                            </view>
                            <view class="costTable_list">
                                <view class="txt txt1">
                                    <view class="name">码号费</view>
                                    <!-- <view class="two">
                                        <view class="two_txt ">虚商号码</view>
                                        <view class="two_txt">联通号码</view>
                                    </view> -->
                                </view>
                                <view class="txt txt2">
                                    <view class="two">
                                        <view class="two_txt">￥{{costRateGraphList.number_fee}}</view>
                                        <!-- <view class="two_txt">￥100.00</view> -->
                                    </view>
                                </view>
                                <view class="txt txt3">
                                    <view class="two">
                                        <view class="two_txt">{{costRateGraphRate.number_rate}}</view>
                                        <!-- <view class="two_txt">10%</view> -->
                                    </view>
                                </view>
                            </view>
                            <view class="costTable_list">
                                <view class="txt txt1">
                                    <view class="name">短信费</view>
                                    <!-- <view class="two">
                                        <view class="two_txt ">虚商短信</view>
                                        <view class="two_txt">联通套外短信</view>
                                    </view> -->
                                </view>
                                <view class="txt txt2">
                                    <view class="two">
                                        <view class="two_txt">￥{{costRateGraphList.sms_money}}</view>
                                        <!-- <view class="two_txt">￥100.00</view> -->
                                    </view>
                                </view>
                                <view class="txt txt3">
                                    <view class="two">
                                        <view class="two_txt">{{costRateGraphRate.sms_rate}}</view>
                                        <!-- <view class="two_txt">10%</view> -->
                                    </view>
                                </view>
                            </view>
                            <view class="costTable_list">
                                <view class="txt txt1">
                                    <view class="name">其他费</view>
                                    <!-- <view class="two">
                                        <view class="two_txt ">虚商短信</view>
                                        <view class="two_txt">联通套外短信</view>
                                    </view> -->
                                </view>
                                <view class="txt txt2">
                                    <view class="two">
                                        <view class="two_txt">￥{{costRateGraphList.other_money}}</view>
                                        <!-- <view class="two_txt">￥100.00</view> -->
                                    </view>
                                </view>
                                <view class="txt txt3">
                                    <view class="two">
                                        <view class="two_txt">{{costRateGraphRate.other_rate}}</view>
                                        <!-- <view class="two_txt">10%</view> -->
                                    </view>
                                </view>
                            </view>
                        </view>
                    </view>
				</view>
			</view>
		</view>
		<NavTab :index="2"></NavTab>
	</view>
</template>

<script>
	import NavTab from "../../components/navTab.vue";
	import uniEcCanvas from "../components/uni-ec-canvas/uni-ec-canvas";
    import Headertop from "../../components/headertop.vue";
    import uniCalendar from '@/components/uni-calendar/uni-calendar.vue'
	import moment from "moment";
	export default {
		data() {
			return {
				titleTop: "数据概览",
				staHeight: "",
				dataType: 1,
                rpx: 1,
                searchValue:"",
                customerName:{
                    shade:false,
                    dom:false,
                    value:"全部客户",
                    n:"全部客户",
                    old:"全部客户",
                    list:[{
                        CUST_NAME:"全部客户",
                        ID:"全部客户"
                    }]
                },
                customerList:[],
                appKey:{
                    shade:false,
                    dom:false,
                    value:"全部AppKey",
                    n:"全部AppKey",
                    old:"全部AppKey",
                    list:[{
                        APP_KEY:"全部AppKey",
                    }]
                },
                appKeyList:[],
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
                    }
                },
                uptime:'',
                businessData:{
                    "total_num": "号码总数",             //号码总数
                    "number_rate": "号码利用率",        //号码利用率
                    "bind_num": '绑定数',              //绑定数
                    "average_bind": '平均绑定数',          //平均绑定数
                    "bill_num": '话单量',              //话单量
                    "bill_rate": "平均话单量",          //平均话单量
                    "minute_num": "通话分钟数",          //通话分钟数
                    "average_minute_num": "平均通话分钟数"  //平均通话分钟数
                },
                costData:{
                    "yesterday_amount": "昨日消费",                //昨日消费
                    "month_amount": "月消费",                    //月消费
                    "arrearage": "带结清",                   //带结清
                    "balance": "余额"                          //余额
                },
				ec4: {
					option: {
                        color:["#5AD8A6","#FF6E5F","#476FD4","#6698FF","#FFD351","#59937C","#FF9302","#7775ED","#BDC3C7",],
						legend: {
                            icon: "circle",
                            itemHeight: 12,
                            itemWidth: 12,
                            selectedMode:false,
                            bottom: 10,
                            itemGap:40,
                            left:60,
                            right:60,
							data: ["通话费","码号费","短信费","其他费用"]
                        },
                        tooltip: {
							trigger: "item",
							formatter: "{b}：{c0}\r\n占比：{d}%",
							backgroundColor: "rgba(255,255,255,1)",
							extraCssText: 'box-shadow: 0 0 3px rgba(0, 0, 0, 0.3);',
							axisPointer: { //去掉移动的指示线
								type: 'line',
								lineStyle: {
									color: "#E0E0E0"
								}
							},
							borderRadius: 4,
							textStyle: {
								color: "#333333",
								fontSize: 18
							}
						},
                        textStyle: {
							fontSize: 18,
							fontStyle: 'normal',
							fontWeight: 'normal',
							color: "#666666"
						},
						series: [
                            // {
							// 	name: '访问来源',
							// 	type: 'pie',
							// 	selectedMode: 'single',
                            //     radius: [0, '30%'],
                            //     center:["50%","40%"],
							// 	label: {
                            //         position: 'inner',
                            //         fontSize: 18
                            //     },
                            //     itemStyle: {
                            //         borderWidth: 2, //设置border的宽度有多大
                            //         borderColor: '#fff',
                            //     },
                            //     hoverAnimation: false,
                            //     selectedOffset:5,
							// 	labelLine: {
							// 		show: false
							// 	},
							// 	data: [{
							// 			value: 679,
							// 			name: '短信费',
							// 			selected: true
							// 		},
							// 		{
							// 			value: 535,
							// 			name: '码号费'
							// 		},
							// 		{
							// 			value: 1548,
							// 			name: '通话费'
							// 		}
							// 	]
							// },
							{
								name: '访问来源',
								type: 'pie',
                                // radius: ['40%', '58%'],
                                radius: [0, '58%'],
                                center:["50%","40%"],
                                selectedOffset:5,
                                selectedMode: 'single',
                                labelLine: {
                                    show: true,
                                    normal: {
                                        length: 20,
                                        length2: 50,
                                        lineStyle: {
                                            color: '#979797'
                                        }
                                    }
                                },
                                itemStyle: {
                                    borderWidth: 2, //设置border的宽度有多大
                                    borderColor: '#fff',
                                },
								label: {
									// formatter: '{a|{a}}{abg|}\n{hr|}\n  {b|{b}：}{c}  {per|{d}%}  ',
                                    formatter:'\n\n\n{b|{b}：}\n{c|{c}}',
                                    padding: [30, -70],
                                    position: 'outer',
                                    alignTo: 'labelLine',
									rich: {
										a: {
											color: '#999',
											lineHeight: 22,
											align: 'center'
										},
										// abg: {
										//     backgroundColor: '#333',
										//     width: '100%',
										//     align: 'right',
										//     height: 22,
										//     borderRadius: [4, 4, 0, 0]
										// },
										hr: {
											borderColor: '#aaa',
											width: '100%',
											borderWidth: 0.5,
											height: 0
										},
										b: {
											fontSize: 18,
                                            lineHeight: 25,
                                            align:"center",
                                            color:"#999999",
                                            width:70
                                        },
                                        c:{
											fontSize: 18,
                                            lineHeight: 25,
                                            color:"#333333",
                                            align:"center",
                                            width:70
                                            // verticalAlign:""
                                        },
										per: {
											color: '#eee',
											backgroundColor: '#334455',
											padding: [2, 4],
											borderRadius: 2
										}
									}
								},
								data: [
                                    {
										value: 0,
										name: '通话费'
									},
									{
										value: 0,
										name: '码号费'
									},
									{
										value: 0,
										name: '短信费'
									},
									{
										value: 0,
										name: '其他费用'
									}
								]
							}
						]
					}
                },
                costRateGraphList:{
                    call_money:"",
                    number_fee:"",
                    other_money:"",
                    sms_money:""
                },
                costRateGraphRate:{
                    call_rate: "%",
                    number_rate: "%",
                    other_rate: "%",
                    sms_rate: "%"
                },
				ec3: {
					option: {
						color: ["#357DF5"],
						title: {
							text: ""
						},
						tooltip: {
							trigger: "axis",
							formatter: "{b}\r\n消费额：{c0}",
							backgroundColor: "rgba(255,255,255,1)",
							extraCssText: 'box-shadow: 0 0 3px rgba(0, 0, 0, 0.3);',
							axisPointer: { //去掉移动的指示线
								type: 'line',
								lineStyle: {
									color: "#E0E0E0"
								}
							},
							borderRadius: 4,
							textStyle: {
								color: "#333333",
								fontSize: 18
							}
						},
						grid: {
							top: '15%',
							left: '3.2%',
							right: '3.2%',
							bottom: '10%',
							containLabel: true
						},
						xAxis: {
							type: "category",
							boundaryGap: true,
							data: ["2-12", "2-14", "2-16", "2-18", "2-20", "2-22", "2-24"],
							axisTick: {
								// x轴刻度线
								show: true,
								alignWithLabel: true,
							},

							axisLabel: {
								color: "#999999",
								fontSize: 18
							},
							axisLine: {
								lineStyle: {
									color: ['#C2C2C2']
								}
							}
						},
						yAxis: {
							type: "value",
							axisLine: {
								// y轴
								show: false
							},
							axisTick: {
								// y轴刻度线
								show: false
							},
							splitLine: {
								// 网格线
								show: true,
								lineStyle: {
									// 使用深浅的间隔色
									color: ['#ECECEC']
								}
							},
							axisLabel: {
								color: "#999999",
								fontSize: 18
							}
						},
						series: [{
							name: "浏览量",
							type: "line",
							// smooth: true,
							symbol: null, // 拐点图形类型
							symbolSize: 5,
							hoverAnimation: false,
							itemStyle: {
								normal: {
									color: '#357DF5', //拐点颜色
									borderColor: '#fff', //拐点边框颜色
									borderWidth: 1 //拐点边框大小
								},
								emphasis: {
									color: '#357DF5', //hover拐点颜色定义
									borderColor: '#333',
								}
							},
							areaStyle: {
								color: {
									type: "linear",
									x: 0,
									y: 0,
									x2: 0,
									y2: 1,
									colorStops: [{
											offset: 0,
											color: "#E1EBFF" // 0% 处的颜色
										},
										{
											offset: 1,
											color: "#fff" // 100% 处的颜色
										}
									],
									// global: false // 缺省为 false
								}
							},
							lineStyle: {
								color: "#357DF5"
							},
							data: [120, 132, 101, 134, 90, 230, 210]
						}]
					}
				},
				ec2: {
					option: {
						textStyle: {
							fontSize: 20,
							fontStyle: 'normal',
							fontWeight: 'normal',
							color: "#333333"
						},
						series: [{
							name: "访问来源",
							type: "pie",
							radius: ["40%", "70%"],
							center: ["25%", "50%"],
							label: {
								position: "outside",
								color: "#19193E",
								normal: {
									show: false
								},
								emphasis: {
									show: false
								},
								fontSize: 10
							},
							labelLine: {
								normal: {
									show: false
								},
								emphasis: {
									show: false
								}
							},
							itemStyle: {
								borderWidth: 5, //设置border的宽度有多大
								borderColor: '#fff',
							},
							data: [{
									value: 3321,
									name: "接听成功",
									itemStyle: {
										normal: {
											color: "#6395FA"
										},
										emphasis: {
											color: "#6395FA"
										}
									}
								},
								{
									value: 3048,
									name: "接听异常",
									itemStyle: {
										normal: {
											color: "#FF9500"
										},
										emphasis: {
											color: "#FF9500"
										}
									}
								}
							]
						}]
					}
                },
                billAnalyzeGraph:{
                    "bill_num": "0",            //总量
                    "success_num": "0",         //成功数
                    "fail_num": 0,              //异常数
                    "success_rate": "0%",       //成功率
                    "fail_rate": "0%"           //异常率
                },
				ec: {
					option: {
						color: ["#5B8FF9", "#FF9500"],
						tooltip: {
                            trigger: 'axis',
                            formatter: "{b}\r\n  接听率：{c1}%\r\n  话单量：{c0}",
							backgroundColor: "rgba(255,255,255,1)",
							extraCssText: 'box-shadow: 0 0 3px rgba(0, 0, 0, 0.3);',
							axisPointer: { //去掉移动的指示线
								type: 'none'
							},
							borderRadius: 4,
							textStyle: {
								color: "#333333",
                                fontSize: 18
                            },
						},
						// legend: {
						// 	selectedMode: false,
						// 	data: [{
						// 		name: "接听率",
						// 		icon: 'rect',
                        //     },
                        //     {
						// 		name: "话单量",
						// 		icon: "rect"
						// 	}],
						// 	itemHeight: 12,
						// 	itemWidth: 12,
						// 	top: '5%',
						// 	left: "3.2%",
                        // },
                        dataZoom: [
                            {
                                type: 'inside',
                                xAxisIndex:"0",
                                startValue:0,
                                endValue:6
                            }
                        ],
						xAxis: [{
							type: "category",
							data: [
								"06.07",
								"06.08",
								"06.09",
								"06.10",
								"06.11",
								"06.12",
								"06.13"
							],
							axisTick: {
								// x轴刻度线
								show: false
							},
							axisLabel: {
								color: "#999999",
								fontSize: 18
							},
							axisLine: {
								lineStyle: {
									color: ['#C2C2C2']
								}
							}
						}],
						yAxis: [{
								type: "value",
								min: 0,
								max: 100,
								interval: 20,
								axisLine: {
									// y轴
									show: false
								},
								axisTick: {
									// y轴刻度线
									show: false
								},
								splitLine: {
									// 网格线
									show: true,
									lineStyle: {
										// 使用深浅的间隔色
										color: ['#ECECEC']
									}
								},
								axisLabel: {
									formatter: "{value} %",
									color: "#999999",
									fontSize: 18
								}
							},
							{
								type: "value",
								max: 5,
								axisLine: {
									// y轴
									show: false,
								},
								axisTick: {
									// y轴刻度线
									show: false
								},
								splitLine: {
									// 网格线
									show: false
								},
								axisLabel: {
									formatter: "{value}",
									color: "#999999",
									fontSize: 18
								}
							}
						],
						textStyle: {
							fontSize: 18,
							fontStyle: 'normal',
							fontWeight: 'normal',
							color: "#474747"
						},
						grid: {
							top: '20%',
							left: '3.2%',
							right: '3.2%',
							bottom: '10%',
							containLabel: true
						},
						series: [{
								name: "话单量",
								type: "bar",
								yAxisIndex: 1,
								barWidth: 40,
								data: [0, 0, 0, 0, 0, 0, 0]
							},
							{
								name: "接听率",
								type: "line",
								symbol: null, // 拐点图形类型
								symbolSize: 0,
								data: [0, 0, 0, 0, 0, 0, 0]
							}
						]
					}
                },
                BusinessDataType:true,
                ec5:{
                    option:{
                        // tooltip: {
                        //     trigger: 'axis',
                        //     formatter: "{b}\r\n话单量：{c}",
						// 	backgroundColor: "rgba(255,255,255,1)",
						// 	extraCssText: 'box-shadow: 0 0 3px rgba(0, 0, 0, 0.3);',
						// 	axisPointer: { //去掉移动的指示线
						// 		type: 'none'
						// 	},
						// 	borderRadius: 4,
						// 	textStyle: {
						// 		color: "#333333",
                        //         fontSize: 18
                        //     },
						// },
                        grid: {
                            top:"8%",
                            left: '3.2%',
                            right: '7.2%',
                            bottom: '6.2%',
                            width:"auto",
                            containLabel: true
                        },
                        xAxis: {
                            type: 'value',
                            max: 16,
                            interval:4,
                            axisLine: {
                                // X轴
                                show: true,
                                symbol:['none', 'round'],
                                symbolSize:[1, 8],
                                symbolOffset:[0,4],
                                lineStyle: {
                                    color: ['#D9D9D9']
                                }
                            },
                            axisTick: {
                                // X轴刻度线
                                show: true,
                                lineStyle: {
                                    color: ['#D9D9D9']
                                }
                            },
                            axisLabel: {
                                formatter: "{value}",
                                color: "#999999",
                                fontSize: 18
                            }
                        },
                        yAxis:[ {
                            type: 'category',
                            axisLine: {
                                // y轴
                                show: false,
                                lineStyle: {
                                    color: ['#D9D9D9']
                                }
                            },
                            axisTick: {
                                // y轴刻度线
                                show: false
                            },
                            axisLabel: {
                                formatter: "{value}",
                                color: "#999999",
                                fontSize: 18
                            },
                            data: ['无绑定关系', '被叫关机', '被叫停机', '被叫空号', '被叫拒接', '被叫忙', '被叫无应答',"被叫无法接通","运营商故障","主叫提前挂机"]
                        },
                        {
                            type: 'category',
                            position:"left",
                            z:10,
                            axisLine: {
                                // y轴
                                show: false,
                                lineStyle: {
                                    color: ['#D9D9D9']
                                }
                            },
                            axisTick: {
                                // y轴刻度线
                                show: false
                            },
                            axisLabel: {
                                inside: true,
                                margin:12,
                                formatter: "{value}",
                                color: "#fff",
                                fontSize: 18
                            },
                            data: [0,0,0,0,0,0,0,0,0,0]
                        }],
                        color:["#5B8FF9",],
                        series: [
                            {
                                name: '直接访问',
                                type: 'bar',
                                barWidth: 25,
                                label: {
                                    show: true,
                                    distance:-30,
                                    align:"left",
                                    color: "#333",
                                    position: 'insideRight',
                                    fontSize:18,
                                    lineHeight:25
                                },
                                data: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
                            }
                        ]
                    }
                },
                ec6:{
                    option:{
                        color:["#73A0FA","#5AD8A6","#F7C739","#FF99C3","#945FB9"],
                        legend: {
                            icon: "circle",
                            itemHeight: 12,
                            itemWidth: 12,
                            selectedMode:false,
                            top:"center",
                            itemGap:30,
                            right:99,
                            orient:"vertical",
                            data: ["3分钟以上","3分钟内","2分钟内","1分钟内","0通话"]
                        },
                        tooltip: {
                            trigger: "item",
                            z:50,
                            formatter: "{b}：{c}",
                            backgroundColor: "rgba(255,255,255,1)",
                            extraCssText: 'box-shadow: 0 0 3px rgba(0, 0, 0, 0.3);',
                            axisPointer: { //去掉移动的指示线
                                type: 'line',
                                lineStyle: {
                                    color: "#E0E0E0"
                                }
                            },
                            borderRadius: 4,
                            textStyle: {
                                color: "#333333",
                                fontSize: 18
                            }
                        },
                        textStyle: {
                            fontSize: 18,
                            fontStyle: 'normal',
                            fontWeight: 'normal',
                            color: "#666666"
                        },
                        series: [
                            
                            {
                                name: '访问来源',
                                type: 'pie',
                                radius: ['30%', '68%'],
                                center:["33%","50%"],
                                selectedOffset:5,
                                selectedMode: 'single',
                                hoverAnimation: false,
                                labelLine: {
                                    show: true,
                                    normal: {
                                        length: 20,
                                        length2: 50,
                                        lineStyle: {
                                            color: '#333'
                                        },
                                    }
                                },
                                itemStyle: {
                                    borderWidth: 2, //设置border的宽度有多大
                                    borderColor: '#fff',
                                },
                                label: {show:false},
                                data: [
                                    {
                                        value: 0,
                                        name: '3分钟以上'
                                    },
                                    {
                                        value: 0,
                                        name: '3分钟内'
                                    },
                                    {
                                        value: 0,
                                        name: '2分钟内'
                                    },
                                    {
                                        value: 0,
                                        name: '1分钟内'
                                    },
                                    {
                                        value: 0,
                                        name: '0通话'
                                    }
                                ]
                            },
                            {
                                name: '访问来源',
                                type: 'pie',
                                radius: ['60%', '60%'],
                                center:["33%","50%"],
                                selectedOffset:5,
                                selectedMode: 'single',
                                hoverAnimation: false,
                                z:48,
                                labelLine: {
                                    show: true,
                                    normal: {
                                        length: 40,
                                        length2: 0,
                                        lineStyle: {
                                            color: '#333'
                                        }
                                    }
                                },
                                itemStyle: {
                                    opacity:0,
                                    // borderWidth: 2, //设置border的宽度有多大
                                    // borderColor: '#fff',
                                },
                                label: {
                                    formatter:"{d}%",
                                    color:'#333'
                                    
                                },
                                data: [
                                    {
                                        value: 0,
                                        name: '3分钟以上'
                                    },
                                    {
                                        value: 0,
                                        name: '3分钟以内'
                                    },
                                    {
                                        value: 0,
                                        name: '2分钟以内'
                                    },
                                    {
                                        value: 0,
                                        name: '1分钟以内'
                                    },
                                    {
                                        value: 0,
                                        name: '0通话'
                                    }
                                ]
                            }
                        ]
                    }
                }
			}
		},
		onShow() {

                

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
            this.getCustName()
                .then((res)=>{
                    this.customerName.list=this.customerName.list.concat(res)
                })
            this.getAppKey()
                .then((res)=>{
                    this.appKey.list=this.appKey.list.concat(res)
                })

        },
		onReady() {
            let nowDate = new Date();
            let endTime=nowDate.getTime() - 24 * 60 * 60 * 1000
            // let startTime=nowDate.getTime() -31 * 24 * 60 * 60 * 1000
            this.uptime = moment(endTime).format('YYYY-MM-DD 00:00')
            let t2=nowDate.getTime() -7 * 24 * 60 * 60 * 1000
            this.customDate.dufData = moment(endTime).format('YYYY-MM-DD')
            console.log("当前时间",this.customDate.dufData)
            this.customDate.endDate=this.customDate.dufData
            // this.customDate.startDate=moment(startTime).format('YYYY-MM-DD')
            this.customDate.value.start=moment(t2).format('YYYY-MM-DD')
            this.customDate.value.end=moment(endTime).format('YYYY-MM-DD')

            this.getBusinessData('',this.customDate.value.start,this.customDate.value.end,'')
            this.getCostData('',this.customDate.value.start,this.customDate.value.end,'')
            this.getBillGraph('',this.customDate.value.start,this.customDate.value.end,'')
            this.getBillAnalyzeGraph('',this.customDate.value.start,this.customDate.value.end,'')
            this.getCostGraph('',this.customDate.value.start,this.customDate.value.end,'')
            this.getCostRateGraph('',this.customDate.value.start,this.customDate.value.end,'')
            this.getAnswerAnalyzeGraph('',this.customDate.value.start,this.customDate.value.end,'')
            this.getCallDurationGraph('',this.customDate.value.start,this.customDate.value.end,'')

			this.ec.option.yAxis[0].axisLabel.fontSize = 18 * this.rpx;
			this.ec.option.yAxis[1].axisLabel.fontSize = 18 * this.rpx;
			this.ec.option.xAxis[0].axisLabel.fontSize = 18 * this.rpx;
			// this.ec.option.legend.itemHeight = 12 * this.rpx;
			// this.ec.option.legend.itemWidth = 12 * this.rpx;
			this.ec.option.textStyle.fontSize = 18 * this.rpx;
			this.ec.option.series[0].barWidth = 40 * this.rpx;

			this.ec3.option.yAxis.axisLabel.fontSize = 18 * this.rpx;
            this.ec3.option.xAxis.axisLabel.fontSize = 18 * this.rpx;
            
            this.ec4.option.legend.itemHeight=12* this.rpx;
            this.ec4.option.legend.itemWidth=12* this.rpx;
            this.ec4.option.legend.itemGap=40* this.rpx;
            this.ec4.option.legend.left=40* this.rpx;
            this.ec4.option.legend.right=40* this.rpx;
            this.ec4.option.textStyle.fontSize = 18 * this.rpx;
            this.ec4.option.series[0].label.fontSize = 18 * this.rpx;
            // this.ec4.option.series[1].label.rich.b.fontSize = 18 * this.rpx;
            // this.ec4.option.series[1].label.rich.b.lineHeight = 25 * this.rpx;
            // this.ec4.option.series[1].label.rich.c.fontSize = 18 * this.rpx;
            // this.ec4.option.series[1].label.rich.c.lineHeight = 25 * this.rpx;
            this.ec4.option.series[0].label.rich.b.fontSize = 18 * this.rpx;
            this.ec4.option.series[0].label.rich.b.lineHeight = 25 * this.rpx;
            this.ec4.option.series[0].label.rich.c.fontSize = 18 * this.rpx;
            this.ec4.option.series[0].label.rich.c.lineHeight = 25 * this.rpx;
            
            this.ec5.option.yAxis[0].axisLabel.fontSize = 18 * this.rpx;
            this.ec5.option.yAxis[1].axisLabel.fontSize = 18 * this.rpx;
            this.ec5.option.xAxis.axisLabel.fontSize = 18 * this.rpx;
            this.ec5.option.xAxis.axisLabel.margin = 12 * this.rpx;
            this.ec5.option.series[0].barWidth = 25 * this.rpx;
            this.ec5.option.series[0].label.fontSize = 18 * this.rpx;
            this.ec5.option.series[0].label.lineHeight = 25 * this.rpx;
            this.ec5.option.series[0].label.distance = -12 * this.rpx;


            this.ec6.option.legend.itemHeight=12* this.rpx;
            this.ec6.option.legend.itemWidth=12* this.rpx;
            this.ec6.option.legend.itemGap=30* this.rpx;
            this.ec6.option.legend.right=99* this.rpx;
            this.ec6.option.textStyle.fontSize = 18 * this.rpx;
            this.ec6.option.series[0].label.fontSize = 18 * this.rpx;
            this.ec6.option.series[1].labelLine.normal.length = 40 * this.rpx;
        },
		components: {
			uniEcCanvas,
			Headertop,
            NavTab,
            uniCalendar
        }, 
		methods: {
            upChart(key,start,end,custId){
                
                if(key=="全部AppKey"){
                    key=""
                }
                if(custId=="全部客户"){
                    custId=""
                }
                let businessData={
                    "total_num": "号码总数",             //号码总数
                    "number_rate": "号码利用率",        //号码利用率
                    "bind_num": '绑定数',              //绑定数
                    "average_bind": '平均绑定数',          //平均绑定数
                    "bill_num": '话单量',              //话单量
                    "bill_rate": "平均话单量",          //平均话单量
                    "minute_num": "通话分钟数",          //通话分钟数
                    "average_minute_num": "平均通话分钟数"  //平均通话分钟数
                }
                let costData={
                    "yesterday_amount": "昨日消费",                //昨日消费
                    "month_amount": "月消费",                    //月消费
                    "arrearage": "带结清",                   //带结清
                    "balance": "余额"                          //余额
                }
                
                this.businessData=businessData
                this.costData=costData
                this.getBusinessData(key,start,end,custId)
                this.getCostData(key,start,end,custId)
                this.getBillGraph(key,start,end,custId)
                this.getBillAnalyzeGraph(key,start,end,custId)
                this.getCostGraph(key,start,end,custId)
                this.getCostRateGraph(key,start,end,custId)
                this.getAnswerAnalyzeGraph(key,start,end,custId)
                this.getCallDurationGraph(key,start,end,custId)
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
            getBusinessData(key,start,end,custId){
                this.BusinessDataType=false
                let data={
                    key:key,
                    start:start,
                    end:end,
                    cust_id:custId
                }

                 this.$post("/apiPrivacy.html?f=businessData",data)
                    .then((res) => {
                        console.log("businessData", res)
                        this.businessData=res.data
                        this.BusinessDataType=true
                    })
            },
            getCostData(key,start,end,custId){
                let data={
                    key:key,
                    start:start,
                    end:end,
                    cust_id:custId
                }
                 this.$post("/apiPrivacy.html?f=costData",data)
                    .then((res) => {
                        console.log("costData", res)
                        this.costData=res.data
                    })
            },
            getCustName() {
                return new Promise((resolve, reject) => {
                    this.$post("/apiPrivacy.html?f=getCustName")
                        .then((res) => {
                            console.log("getCustName", res)
                            this.customerList= res.data
                            resolve(res.data);
                        })
                })
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
            getAppKey() {
                return new Promise((resolve, reject) => {
                    let idName=""
                    if(this.customerName.n=="全部客户"){
                        idName=''
                    }else{
                        idName=this.customerName.n
                    }
                    let data={
                        id:idName
                    }
                    this.$post("/apiPrivacy.html?f=getAppKey",data)
                        .then((res) => {
                            console.log("getAppKey", res)
                            this.appKeyList= res.data
                            resolve(res.data);
                        })
                })
            },
            getBillGraph(key,start,end,custId){
                //接听率趋势图
                let data={
                    key:key,
                    start:start,
                    end:end,
                    cust_id:custId
                }
                 this.$post("/apiPrivacy.html?f=billGraph",data)
                    .then((res) => {
                        console.log("billGraph", res)
                        this.ec.option.xAxis[0].data=res.data.x
                        this.ec.option.series[0].data=res.data.list[1].data
                        this.ec.option.series[1].data=res.data.list[0].data
                        // this.ec.option.series[1].data=[50, 0, 0, 0, 0, 0, 0, 0, 60, 40, 0, 0, 0, 0, 2, 0, 0, 10, 0, 0, 0, 55, 0, 0, 0, 0, 0, 0, 0, 0]
                        // this.ec.option.series[0].data=[5000, 6000, 10000,  12000, 16000, 16000, 0, 0, 6000, 4000, 14000, 0, 200000, 0, 2, 0, 0, 10, 0, 0, 0, 55, 0, 0, 0, 0, 0, 0, 0, 0]
                        this.ec.option.yAxis[1].max=res.data.max
                    })
            },
            getBillAnalyzeGraph(key,start,end,custId){
                //话单统计分析图
                let data={
                    key:key,
                    start:start,
                    end:end,
                    cust_id:custId
                }
                 this.$post("/apiPrivacy.html?f=billAnalyzeGraph",data)
                    .then((res) => {
                        console.log("billAnalyzeGraph", res)
                        this.ec2.option.series[0].data[0].value=res.data.success_num
                        this.ec2.option.series[0].data[1].value=res.data.fail_num


                        if(res.data.success_num>0&&res.data.fail_num==0){
                            this.ec2.option.series[0].itemStyle.borderWidth=0
                        }else if(res.data.success_num==0&&res.data.fail_num>0){
                            this.ec2.option.series[0].itemStyle.borderWidth=0
                        }else{
                            this.ec2.option.series[0].itemStyle.borderWidth=5
                        }

                        this.billAnalyzeGraph=res.data
                    })
            },
            getCostGraph(key,start,end,custId){
                //消费数据-消费趋势图
                let data={
                    key:key,
                    start:start,
                    end:end,
                    cust_id:custId
                }
                 this.$post("/apiPrivacy.html?f=costGraph",data)
                    .then((res) => {
                        console.log("costGraph", res)
                        this.ec3.option.xAxis.data=res.data.x

                        this.ec3.option.series[0].data=res.data.list[0].data
                    })
            },
            getCostRateGraph(key,start,end,custId){
                //消费数据-消费趋势图
                let data={
                    key:key,
                    start:start,
                    end:end,
                    cust_id:custId
                }
                 this.$post("/apiPrivacy.html?f=costRateGraph",data)
                    .then((res) => {
                        console.log("costRateGraph", res)
                        this.ec4.option.series[0].data=res.data.list
                        // this.ec4.option.series[0].data[0].value=res.data.list[0].value
                        // this.ec4.option.series[0].data[1].value=res.data.list[1].value
                        // this.ec4.option.series[0].data[2].value=res.data.list[2].value
                        // this.ec4.option.series[0].data[3].value=res.data.list[3].value
                        this.costRateGraphList.call_money=res.data.list[0].value
                        this.costRateGraphList.number_fee=res.data.list[1].value
                        this.costRateGraphList.other_money=res.data.list[3].value
                        this.costRateGraphList.sms_money=res.data.list[2].value
                        this.costRateGraphRate=res.data.rate
                        let num=0
                        res.data.list.map((item)=>{
                            if(item.value>0){
                                num=num+1
                            }
                        })
                        if(num==1){
                            this.ec4.option.series[0].itemStyle.borderWidth=0
                        }else{
                            this.ec4.option.series[0].itemStyle.borderWidth=2
                        }

                    })
            },
            getAnswerAnalyzeGraph(key,start,end,custId){
                //消费数据-接听异常话单分析图
                let data={
                    key:key,
                    start:start,
                    end:end,
                    cust_id:custId
                }
                 this.$post("/apiPrivacy.html?f=answerAnalyzeGraph",data)
                    .then((res) => {
                        console.log("answerAnalyzeGraph", res)
                        this.ec5.option.yAxis[0].data=res.data.x
                        this.ec5.option.xAxis.max=res.data.max
                        this.ec5.option.series[0].data=res.data.list
                        this.ec5.option.yAxis[1].data=res.data.ratio
                        this.ec5.option.xAxis.interval=res.data.interval
                        
                        


                        let maxArr=Math.max(...this.ec5.option.series[0].data)
                        if((maxArr/this.ec5.option.xAxis.max)>(13/16)){
                            console.log("大于")
                            let item = parseInt((this.ec5.option.xAxis.max+this.ec5.option.xAxis.interval)/4)
                            this.ec5.option.xAxis.interval=item
                            this.ec5.option.xAxis.max=item*4
                        }
                        this.ec5.option.series[0].data.map((item,index)=>{
                            if(item==0||item<(this.ec5.option.xAxis.interval/2)){
                                console.log(item)
                                this.ec5.option.yAxis[1].data[index]=''
                            }else{
                                this.ec5.option.yAxis[1].data[index]=this.ec5.option.yAxis[1].data[index]+"%"
                            }
                        })
                        console.log(this.ec5.option.yAxis[1].data)
                    })
            },
            getCallDurationGraph(key,start,end,custId){
                //消费数据-通话时长占比
                let data={
                    key:key,
                    start:start,
                    end:end,
                    cust_id:custId
                }
                 this.$post("/apiPrivacy.html?f=callDurationGraph",data)
                    .then((res) => {
                        console.log("callDurationGraph", res)
                        this.ec6.option.series[0].data=res.data
                        this.ec6.option.series[1].data=res.data
                    })
            },
			cutTab(id) {
				this.dataType = id
            },
            goSearch(str){
                console.log(str)
                str = str.replace(/(^\s*)|(\s*$)/g, "")
                let arrSearch = []
                if (str != "") {
                    for (var i = 0; i < this.customerList.length; i++) {
                        if (this.customerList[i].CUST_NAME.indexOf(str) >= 0) {
                            arrSearch.push(this.customerList[i]);
                        }
                    }
                } else {
                    console.log("空")
                    arrSearch=[{
                        CUST_NAME:"全部客户",
                        ID:"全部客户"
                    }]
                    arrSearch = arrSearch.concat(this.customerList)
                }
                this.customerName.list=arrSearch
            },
            eliminate(){
                this.searchValue=""
                this.customerName.list=[{
                        CUST_NAME:"全部客户",
                        ID:"全部客户"
                    }]
                this.customerName.list=this.customerName.list.concat(this.customerList)
            },
            pitch(id){
                console.log(id)
                this.customerName.n=id
            },
            pitch2(id){
                console.log(id)
                this.appKey.n=id
            },
            offPop(){
                setTimeout(() => {
					this.customerName.shade = false		
                    setTimeout(() => {
                        this.customerName.dom = false
                        this.customerName.n=this.customerName.old
                    }, 200)
				}, 100)
            },
            offPop2(){
                setTimeout(() => {
					this.appKey.shade = false		
                    setTimeout(() => {
                        this.appKey.dom = false
                        this.appKey.n=this.appKey.old
                    }, 200)
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
            noPop(){
                setTimeout(() => {
					this.customerName.shade = false		
                    setTimeout(() => {
                        this.customerName.dom = false
                        
                        this.customerName.list.map(item=>{
                            if(item.ID==this.customerName.n){
                                this.customerName.value=item.CUST_NAME
                                this.getAppKey()
                                    .then((res)=>{
                                        this.appKey.list=[{
                                                APP_KEY:"全部AppKey",
                                            }]
                                        this.appKey.value="全部AppKey"
                                        this.appKey.n="全部AppKey"
                                        this.appKey.old="全部AppKey"
                                        this.appKey.list=this.appKey.list.concat(res)
                                        this.upChart(this.appKey.n,this.customDate.value.start,this.customDate.value.end,this.customerName.n)
                                    })
                            }
                        })
                    }, 200)
				}, 100)
            },
            noPop2(){
                setTimeout(() => {
					this.appKey.shade = false		
                    setTimeout(() => {
                        this.appKey.dom = false
                        this.appKey.value=this.appKey.n
                        this.upChart(this.appKey.n,this.customDate.value.start,this.customDate.value.end,this.customerName.n)
                    }, 200)
				}, 100)
            },
            showPop(id){
                this.customerName.old=id
                setTimeout(() => {
                    this.customerName.dom = true
                    this.customerName.shade = true
				}, 100)
            },
            showPop2(id){
                this.appKey.old=id
                setTimeout(() => {
                    this.appKey.dom = true
                    this.appKey.shade = true
				}, 100)
            },
            showPop3(id){
                setTimeout(() => {
                    this.customDate.dom = true
                    this.customDate.shade = true
				}, 100)
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
                if(days>30){
                    uni.showToast({
                        title: "时间范围不能超出一个月",
                        icon:'none',
                        duration: 2000
                    });
                }else{
                    this.customDate.value.start=this.customDate.dateObj.start
                    this.customDate.value.end=this.customDate.dateObj.end
                    this.offPop3()
                    this.upChart(this.appKey.n,this.customDate.value.start,this.customDate.value.end,this.customerName.n)
                }
                
            }
		},
	};
</script>

<style lang="scss">
	.uni-ec-canvas {
		width: 750upx;
		height: 400upx;
		display: block;
	}

	.dataModule_main {
		background: rgb(239, 239, 244);

		.filtrate {
			background: #fff;

			.filtrate_item {
				height: 96rpx;
				display: flex;
				justify-content: space-between;
				align-items: center;
				margin-left: 24rpx;
				border-bottom: 1rpx solid #EEEEEE;
				padding-right: 24rpx;

				.name {
					font-size: 30rpx;
					color: #999999;
				}

				.right {
					display: flex;
					justify-content: space-between;
					align-items: center;
                    height: 100%;
					.value {
						font-size: 30rpx;
						color: #333333;
						margin-right: 8rpx;
					}

					>.img {
						width: 36rpx;
						height: 36rpx;
						display: block;
					}
				}
			}
		}

		.dataModule_main_center {
			margin-top: 24rpx;

			.top {
				background: #fff;
				display: flex;
				align-items: center;
				padding: 0 80rpx;
				justify-content: space-around;
				height: 84rpx;

				.top_title {
					font-size: 34rpx;
					color: #333333;
					text-align: center;
					height: 80rpx;
					line-height: 80rpx;
					font-size: 34rpx;
					border-bottom: 4rpx solid #fff;
				}

				.top_title_action {
					color: #357DF5;
					font-weight: 600;
					border-bottom: 4rpx solid #357DF5;
				}
			}

			.center {
				// padding-bottom: 24rpx;

				.info {
					background: #fff;
					border-top: 1rpx solid #EEEEEE;
					padding-bottom: 33rpx;

					.info_time {
						padding: 32rpx 24rpx 38rpx 0;
						font-size: 24rpx;
						color: #666666;
						text-align: right;
					}

					.info_top {
						display: flex;
						align-items: center;
						justify-content: space-between;

						.info_time1 {
							margin-left: 24rpx;
							font-size: 34rpx;
							color: #333333;
						}
					}

					.info_list {
						display: flex;
						align-items: center;
						justify-content: space-between;
                        padding: 0 4rpx;
						.info_item {
							padding: 0 20rpx;
							border-right: 1rpx solid #D8D8D8;
                            
							.title {
								font-size: 22rpx;
								color: #999999;
								line-height: 30rpx;
								margin-bottom: 8rpx;

								.t {
									font-size: 18rpx;
								}
							}

							.num {
								font-size: 40rpx;
								color: #357DF5;
								font-weight: 600;
								line-height: 56rpx;
								margin-bottom: 8rpx;
                                max-width: 130rpx;
                                overflow: hidden;
                                white-space: nowrap;
                                text-overflow: ellipsis;
                                min-width: 130rpx;
							}

							.txt {
								font-size: 18rpx;
								color: #333333;
								line-height: 25rpx;
							}
						}

						.info_item:last-child {
							border: none;
						}
					}

					.info_list1 {
						.info_item {
							width: 100%;
                            padding-left:40rpx;
						}
                        .info_item:first-child{
                            padding-left:20rpx;
                        }
                        .info_item:last-child{
                            padding-left:20rpx;
                        }
					}
				}

				.answer {
					height: 468rpx;
					margin-top: 24rpx;
					background: #fff;

					.title_item {
						display: flex;
						justify-content: space-between;
						align-items: center;

						//   margin-bottom: 32rpx;
						>.txt {
							padding-left: 24rpx;
							padding-top: 24rpx;
							font-size: 34rpx;
							color: #333333;
							font-weight: 600;
							line-height: 48rpx;
						}

						.select {
							padding-right: 24rpx;
							padding-top: 28rpx;
							display: flex;
							align-items: center;

							.name {
								font-size: 22rpx;
								color: #666666;
							}

							.value {
								display: flex;
								align-items: center;
								background: #F8F8F8;
								border: 1px solid #D8D8D8;
								border-radius: 4px;
								padding: 5rpx 8rpx 5rpx 12rpx;

								>.txt {
									font-size: 22rpx;
									color: #333333;
									margin-right: 26rpx;
								}

								>.img {
									width: 24rpx;
									height: 24rpx;
									display: block;
								}
							}
						}
					}

					.uni-ec-canvas {
						height: 396rpx;
					}
				}

				.answer1 {
					height: 420rpx;
					position: relative;

					.centre {
						position: absolute;
						top: 50%;
						left: 17.5%;
                        width: 115rpx;
                        overflow: hidden;
                        white-space: nowrap;
                        text-overflow: ellipsis;
						.centre_txt1 {
							font-size: 20rpx;
							color: #999999;
							text-align: center;
						}

						.centre_txt2 {
							font-size: 36rpx;
							color: #333333;
							font-weight: 600;
							text-align: center;
                            overflow: hidden;
                            white-space: nowrap;
                            text-overflow: ellipsis;
						}
					}

					.centre_right {
						position: absolute;
						top: 50%;
						left: 53.2%;

						.centre_item {
							display: flex;
							align-items: center;
							margin-bottom: 24rpx;

							.icon {
								width: 12rpx;
								height: 12rpx;
								border-radius: 50%;
								background: #6395FA;
								margin-right: 8rpx;
							}

							.icon1 {
								width: 12rpx;
								height: 12rpx;
								border-radius: 50%;
								background: #FF9500;
								margin-right: 8rpx;
							}

							.title {
								font-size: 20rpx;
								color: #333333;
								margin-right: 20rpx;
								line-height: 28rpx;
							}

							.txt1 {
								font-size: 20rpx;
								color: #666666;
								margin-right: 20rpx;
							}
                            .txt2{
                                width: 45rpx;
                            }
						}
					}
					.uni-ec-canvas {
						height: 348rpx;
					}
				}

                .answer2{
                    height: 727rpx;
                    .uni-ec-canvas {
						height: 655rpx;
					}
                }
                .costTable{
                    background: #fff;
                    padding-bottom: 41rpx;
                    padding-top: 30rpx;
                    .costTable_main{
                        border-top:1rpx solid #DDDDDD;
                        border-left:1rpx solid #DDDDDD;
                        margin: 0 24rpx;
                        .costTable_title{
                            height: 68rpx;
                            background: #F5F8FE;
                            font-size: 24rpx;
                            color: #333333;
                            display: flex;
                            .txt{
                                border-bottom: 1rpx solid #DDDDDD;
                                border-right: 1rpx solid #DDDDDD;
                                width: 33.3%;
                                text-align: center;
                                line-height: 68rpx;
                            }
                            .txt1{
                                width: 362rpx;
                            }
                            .txt2{
                                width: 240rpx;
                            }
                            .txt3{
                                width:100rpx;
                            }
                        }
                        .costTable_list{
                            // height: 136rpx;
                            height: 68rpx;
                            display: flex;
                            align-items: center;
                            text-align: center;
                            line-height: 68rpx;
                            font-size: 20rpx;
                            color: #333333;
                            .txt1{
                                width: 362rpx;
                                display: flex;
                                .name{
                                    // width: 162rpx;
                                    width: 362rpx;
                                    display: flex;
                                    align-items: center;
                                    justify-content: center;
                                    border-bottom: 1rpx solid #DDDDDD;
                                    border-right: 1rpx solid #DDDDDD;
                                }
                                >.two{
                                    width: 197rpx;
                                }
                            }
                            .txt2{
                                width: 241rpx;
                                
                            }
                            .txt3{
                                width:100rpx;
                            }
                            .two{
                                .two_txt{
                                    border-bottom: 1rpx solid #DDDDDD;
                                    border-right: 1rpx solid #DDDDDD;
                                }
                            }
                        }
                    }

                }

                .answer5{
                    height: 650rpx;
                    .uni-ec-canvas {
						height: 570rpx;
					}
                }
                .answer6{
                    height: 480rpx;
                    .uni-ec-canvas {
						height: 400rpx;
					}
                }
			}
            .center2{
                padding: 0;
            }
		}
	}

	.tooltip_1 {
		background: #fff;
		width: 180rpx;
		height: 108rpx;
	}


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
        height: 1036rpx;
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
        bottom: -1036rpx;
        animation: myfirst12 0.2s linear;
    }

    .cal_data{
        height: 156rpx;
        display: flex;
        background: #F5F8FE;
        justify-content: space-between;
        align-items: center;
        padding: 0 24rpx;
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
			bottom: -1036rpx;
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
			bottom: -1036rpx;
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

    .load{
        position: relative;
        width: 36rpx;
        display: flex;
        align-items: center;
        margin: 0 auto;
        height: 36rpx;
        padding: 14rpx;
        .load_icon{
            position: absolute;
            left: 0;
            top: 0;
            bottom: 0;
            right: 0;
            width: 36rpx;
            height: 36rpx;
            display: block;
            margin: auto;
            animation:myfirst1 1s linear infinite; 
        }
        >.txt{
            padding-left: 50rpx;
            line-height:36rpx;
            font-size:24rpx;
            color:rgba(0,0,0,0.25);
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
.info_item{
    position: relative;
    .show{
        position:absolute;
        top: -27px;
        left: 24rpx;
        font-size: 23rpx;
        padding:0 12rpx;
        box-shadow: 0 0 3px rgba(0, 0, 0, 0.3);
        line-height: 45rpx;
        background: #fff;
        display: none;
    }
}
.info_item_hover{
    .show{
        display: block;
    }
}
.centre{
    z-index: 2;
    .show{
        position:absolute;
        top: 0px;
        left: 0rpx;
        width: 116rpx;
        text-align: center;
        font-size: 20rpx;
        box-shadow: 0 0 3px rgba(0, 0, 0, 0.3);
        background: #fff;
        display: none;

    }
}
.centre_item_hover{
    .show{
        display: block;
    }
}
.title_item{
    position: relative;
    .icon_nameId{
        position: absolute;
        bottom: -45rpx;
        left: 24rpx;
        display: flex;
        justify-content: space-between;
        align-items: center;
        .title_icon{
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 18rpx;
            color: rgba(0,0,0,0.85);
            margin-right: 32rpx;
            .i1{
                height: 2rpx;
                background: #FF9500;
                width: 16rpx;
                margin-right: 10rpx;
            }
            .i2{
                height: 12rpx;
                background: #5B8FF9;
                width: 12rpx;
                margin-right: 10rpx;
            }
        }
}
}

</style>
