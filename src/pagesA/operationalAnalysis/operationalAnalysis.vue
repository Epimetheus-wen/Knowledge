<template>
	<view class="opAnys">
		<Headertop :titleTop="titleTop"></Headertop>
		<view class="opAnys_main">
			<!-- 轮播图 -->
			<view class="banner">
				<view class="uni-padding-wrap">
					<view class="page-section swiper">
						<view class="page-section-spacing">
							<swiper class="swiper" :indicator-dots="indicatorDots" indicator-active-color="#076bff" :autoplay="autoplay"
							 :interval="interval" :duration="duration" :circular="true" @change="change">
								<swiper-item v-for="(item ,index) in list" :key="index">
									<view class="swiper-item">
										<img v-if="Permission==2" :src="item.imgUrl" alt />
                                        <img v-else :src="item.imgUrl2" alt />
										<view class="swiper-item_view">
											<view class="dateTitle">
												<view v-if="Permission==2" class="dateTitle_label">{{item.title}} {{item.uptime}}</view>
                                                <view v-else class="dateTitle_label1"><view class="toppp">{{item.title}}</view><view> {{item.uptime}}</view></view>
											</view>
											<view :class="Permission==2?'nameTitle':'nameTitle nameTitle1'">
												<view :class="Permission==2?'nameTitle_label':'nameTitle_label1'">{{Permission==4?item.nameTitlegl:item.nameTitle}}
													<image @tap="tishiOpen" class="xinxi" v-if="item.nameTitle!='昨日毛利'" src="../static/images/xinxi.png"></image>
												</view>
												<view :class="Permission==2?'nameTitle_sum':'nameTitle_sum1'"><text class="sum">{{item.sum}}</text><text class="danwei">万元</text></view>
											</view>
											<view v-if="Permission==2" @tap="goDetailsTop" :class="item.ranking<10?'ranking':item.ranking<100?'ranking2':'ranking3'"><text>No.</text><text class="num">{{item.ranking}}</text></view>
                                            <view class="dataSum" v-if="item.nameTitle=='昨日毛利'">
                                                <view class="dataSum_tiem">
                                                    <view>
                                                        <view class="dataSum_tiem_label">昨日营收 (万元)</view>
                                                        <view class="dataSum_tiem_sum">{{dataSum.day.revenue}}</view>
                                                    </view>
                                                </view>
                                                <view class="dataSum_tiem">
                                                    <view>
                                                        <view class="dataSum_tiem_label">昨日毛利营收比</view>
                                                        <view class="dataSum_tiem_sum">{{dataSum.day.rate}}</view>
                                                    </view>
                                                </view>
                                                <view class="dataSum_tiem">
                                                    <view>
                                                        <view class="dataSum_tiem_label">昨日成本 (万元)</view>
                                                        <view class="dataSum_tiem_sum">{{dataSum.day.cost}}</view>
                                                    </view>
                                                </view>
                                            </view>
                                            <view class="dataSum" v-if="item.nameTitle=='月累计毛利'">
                                                <view class="dataSum_tiem">
                                                    <view>
                                                        <view class="dataSum_tiem_label">月累计营收 (万元)</view>
                                                        <view class="dataSum_tiem_sum">{{dataSum.month.revenue}}</view>
                                                    </view>
                                                </view>
                                                <view class="dataSum_tiem">
                                                    <view>
                                                        <view class="dataSum_tiem_label">月毛利营收比</view>
                                                        <view class="dataSum_tiem_sum">{{dataSum.month.rate}}</view>
                                                    </view>
                                                </view>
                                                <view class="dataSum_tiem">
                                                    <view>
                                                        <view class="dataSum_tiem_label">月累计成本 (万元)</view>
                                                        <view class="dataSum_tiem_sum">{{dataSum.month.cost}}</view>
                                                    </view>
                                                </view>
                                            </view>
                                            <view class="dataSum" v-if="item.nameTitle=='年累计毛利'">
                                                <view class="dataSum_tiem">
                                                    <view>
                                                        <view class="dataSum_tiem_label">年累计营收 (万元)</view>
                                                        <view class="dataSum_tiem_sum">{{dataSum.year.revenue}}</view>
                                                    </view>
                                                </view>
                                                <view class="dataSum_tiem">
                                                    <view>
                                                        <view class="dataSum_tiem_label">年毛利营收比</view>
                                                        <view class="dataSum_tiem_sum">{{dataSum.year.rate}}</view>
                                                    </view>
                                                </view>
                                                <view class="dataSum_tiem">
                                                    <view>
                                                        <view class="dataSum_tiem_label">年累计成本 (万元)</view>
                                                        <view class="dataSum_tiem_sum">{{dataSum.year.cost}}</view>
                                                    </view>
                                                </view>
                                            </view>
                                        </view>
									</view>
								</swiper-item>
							</swiper>
							<swiper-dot class="dot" :current="current" :list="list"></swiper-dot>
						</view>
					</view>
				</view>
			</view>
            <!-- 经营趋势 -->
            <view class="trend">
                <view class="trend_title">
                    <view class="txt"><text>经营趋势</text><text class="danwei">（万元）</text></view>
                    <view class="filter_item_buttom">
                        <view
                            :class="ationFilter1?'filter_buttom_left':'filter_buttom_left filter_buttom_ation'"
                            @click="filterTime(1)"
                        >本月</view>
                        <view
                            :class="ationFilter2?'filter_buttom_center':'filter_buttom_center filter_buttom_ation'"
                            @click="filterTime(2)"
                        >本季</view>
                        <view
                            :class="ationFilter3?'filter_buttom_right':'filter_buttom_right filter_buttom_ation'"
                            @click="filterTime(3)"
                        >本年</view>
                    </view>
                </view>
                <view v-if="ecType">
                    <view class="trend_center">
                        <uni-ec-canvas class="uni-ec-canvas " id="line-chart" canvas-id="multi-charts-line" :ec="ec"></uni-ec-canvas>
                    </view>
                    <view class="trend_bottom" @tap="gooPanel">
                        <view class="txt">查看数据详情</view><image class="img" src="../static/images/right.png"></image>
                    </view>
                </view>
                <view v-if="!ecType" class="nodata">
                    <image class="img" src="../static/images/nodata.png"></image>
                    <view class="txt">- 暂无数据权限 -</view>
                </view>
            </view>
            <!-- 业务占比 -->
            <view class="trend trend2">
                <view class="trend_title">
                    <view class="txt"><text>业务占比</text><text class="danwei">（万元）</text></view>
                    <view class="filter_item_buttom">
                        <view
                            :class="ationFilter4?'filter_buttom_left':'filter_buttom_left filter_buttom_ation'"
                            @tap="filterTime2(4)"
                        >本月</view>
                        <view
                            :class="ationFilter5?'filter_buttom_center':'filter_buttom_center filter_buttom_ation'"
                            @tap="filterTime2(5)"
                        >本季</view>
                        <view
                            :class="ationFilter6?'filter_buttom_right':'filter_buttom_right filter_buttom_ation'"
                            @tap="filterTime2(6)"
                        >本年</view>
                    </view>
                </view>
                <view class="trend_filter">
                    <view :class="ationClass1?'trend_filter_item':'trend_filter_item trend_filter_item_action'" @tap="filterClass(1)">按营收</view>
                    <view :class="ationClass2?'trend_filter_item':'trend_filter_item trend_filter_item_action'"  @tap="filterClass(2)">按成本</view>
                    <view :class="ationClass3?'trend_filter_item':'trend_filter_item trend_filter_item_action'"  @tap="filterClass(3)">{{Permission==4?'按全口径毛利':'按毛利'}}</view>
                </view>
                <view v-if="ecType2">
                    <view class="trend_center">
                        <uni-ec-canvas class="uni-ec-canvas " id="line-chart" canvas-id="multi-charts-line" :ec="ec2"></uni-ec-canvas>
                    </view>
                    <view class="trend_bottom" @tap="gooPanel">
                        <view class="txt">查看数据详情</view><image class="img" src="../static/images/right.png"></image>
                    </view>
                </view>
                <view v-if="!ecType2" class="nodata">
                    <image class="img" src="../static/images/nodata.png"></image>
                    <view class="txt">- 暂无数据权限 -</view>
                </view>
            </view>
            <!-- 商务排名 -->
            <view :class="Permission==2?'trend trend3':'trend trend3 trend4'">
                <view class="trend_title">
                    <view class="txt"><view>商务排名</view><view class="danwei">(每晚24点更新数据)</view></view>
                    <view class="filter_item_buttom">
                        <view
                            :class="ationFilter7?'filter_buttom_left':'filter_buttom_left filter_buttom_ation'"
                            @tap="filterTime3(7)"
                        >本月</view>
                        <view
                            :class="ationFilter8?'filter_buttom_center':'filter_buttom_center filter_buttom_ation'"
                            @tap="filterTime3(8)"
                        >本季</view>
                        <view
                            :class="ationFilter9?'filter_buttom_right':'filter_buttom_right filter_buttom_ation'"
                            @tap="filterTime3(9)"
                        >本年</view>
                    </view>
                </view>
                <view class="trend_filter">
                    <view :class="ationClass4?'trend_filter_item':'trend_filter_item trend_filter_item_action'" @tap="filterClass2(4)">按营收</view>
                    <view :class="ationClass5?'trend_filter_item':'trend_filter_item trend_filter_item_action'"  @tap="filterClass2(5)">{{Permission==4?'按全口径毛利':'按毛利'}}</view>
                </view>
                <view v-if="rankingType">
                    <view v-if="Permission==2" class="trend_center">
                        <view v-if="!ationClass4">
                            <view class="trend_ranking_item">
                                <view class="trend_ranking_left">
                                    <view :class="rankingList.per_revenue.per_rank>=100?'trend_ranking_num3 trend_ranking_num':'trend_ranking_num'">{{rankingList.per_revenue.per_rank}}</view>
                                    <view class="trend_ranking_title">
                                        <view class="trend_ranking_name">我的排名</view>
                                        <view class="trend_ranking_groupName">{{rankingList.per_revenue.dep_name}}</view>
                                    </view>
                                </view>
                                <view class="trend_ranking_right">
                                    <view class="trend_ranking_txt">营收 (万)</view>
                                    <view class="trend_ranking_sum">{{rankingList.per_revenue.val}}</view>
                                </view>
                            </view>
                            <view  v-for="(item,index) in rankingList.revenue" :key="index"  class="trend_ranking_item trend_ranking_item1">
                                <view class="trend_ranking_left">
                                    <view class="trend_ranking_num">{{index+1}}</view>
                                    <view class="trend_ranking_title">
                                        <view class="trend_ranking_name">{{item.sale_name}}</view>
                                        <view class="trend_ranking_groupName">{{item.dep_name}}</view>
                                    </view>
                                </view>
                                <view class="trend_ranking_right">
                                    <view v-if="rankingList.per_revenue.nickname==item.sale_name" class="trend_ranking_txt">营收 (万)</view>
                                    <view v-if="rankingList.per_revenue.nickname==item.sale_name" class="trend_ranking_sum">{{item.revenue}}</view>
                                    <view v-if="rankingList.per_revenue.nickname!=item.sale_name" style="font-size: 30rpx;color: #232323;">*******</view>
                                </view>
                            </view>
                        </view>
                        <view v-if="!ationClass5">
                            <view class="trend_ranking_item">
                                <view class="trend_ranking_left">
                                    <view :class="rankingList.per_profit.per_rank>=100?'trend_ranking_num3 trend_ranking_num':'trend_ranking_num'">{{rankingList.per_profit.per_rank}}</view>
                                    <view class="trend_ranking_title">
                                        <view class="trend_ranking_name">我的排名</view>
                                        <view class="trend_ranking_groupName">{{rankingList.per_profit.dep_name}}</view>
                                    </view>
                                </view>
                                <view class="trend_ranking_right">
                                    <view class="trend_ranking_txt">{{Permission==4?'全口径毛利 (万)':'毛利 (万)'}}</view>
                                    <view class="trend_ranking_sum">{{rankingList.per_profit.val}}</view>
                                </view>
                            </view>
                            <view  v-for="(item,index) in rankingList.profit" :key="index"  class="trend_ranking_item trend_ranking_item1">
                                <view class="trend_ranking_left">
                                    <view class="trend_ranking_num">{{index+1}}</view>
                                    <view class="trend_ranking_title">
                                        <view class="trend_ranking_name">{{item.sale_name}}</view>
                                        <view class="trend_ranking_groupName">{{item.dep_name}}</view>
                                    </view>
                                </view>
                                <view class="trend_ranking_right">
                                    <view v-if="rankingList.per_profit.nickname==item.sale_name" class="trend_ranking_txt">{{Permission==4?'全口径毛利 (万)':'毛利 (万)'}}</view>
                                    <view v-if="rankingList.per_profit.nickname==item.sale_name" class="trend_ranking_sum">{{item.profit}}</view>
                                    <view v-if="rankingList.per_profit.nickname!=item.sale_name" style="font-size: 30rpx;color: #232323;">*******</view>
                                </view>
                            </view>
                        </view>
                    </view>
                    <view v-else class="trend_center">
                        <view v-if="!ationClass4">
                            <view @tap="goDetails(item.sale_name)" v-for="(item,index) in rankingList.revenue" :key="index"  class="trend_ranking_item trend_ranking_item1">
                                <view class="trend_ranking_left">
                                    <view :class="index>2?'trend_ranking_num trend_ranking_num4':'trend_ranking_num'">{{index+1}}</view>
                                    <view class="trend_ranking_title">
                                        <view class="trend_ranking_name">{{item.sale_name}}</view>
                                        <view class="trend_ranking_groupName">{{item.dep_name}}</view>
                                    </view>
                                </view>
                                <view class="trend_ranking_right">
                                    <view class="trend_ranking_txt">营收 (万)</view>
                                    <view class="trend_ranking_sum">{{item.revenue}}</view>
                                </view>
                            </view>
                        </view>
                        <view v-if="!ationClass5">
                            <view @tap="goDetails(item.sale_name)"  v-for="(item,index) in rankingList.profit" :key="index"  class="trend_ranking_item trend_ranking_item1">
                                <view class="trend_ranking_left">
                                    <view :class="index>2?'trend_ranking_num trend_ranking_num4':'trend_ranking_num'">{{index+1}}</view>
                                    <view class="trend_ranking_title">
                                        <view class="trend_ranking_name">{{item.sale_name}}</view>
                                        <view class="trend_ranking_groupName">{{item.dep_name}}</view>
                                    </view>
                                </view>
                                <view class="trend_ranking_right">
                                    <view class="trend_ranking_txt">{{Permission==4?'全口径毛利 (万)':'毛利 (万)'}}</view>
                                    <view class="trend_ranking_sum">{{item.profit}}</view>
                                </view>
                            </view>
                        </view>
                    </view>
                    <view v-if="Permission!=2" @tap="goPreList" class="trend_bottom">
                        <view class="txt">查看全部员工排行</view><image class="img" src="../static/images/right.png"></image>
                    </view>
                </view>
                <view v-if="!rankingType" class="nodata">
                    <image class="img" src="../static/images/nodata.png"></image>
                    <view class="txt">- 暂无数据排名 -</view>
                </view>
            </view>
		</view>
        <uni-popup class="pop" ref="popup" type="center" @change='handelChange'>
            <view class="center">
                <view class="title">经营月报、年报数据</view>
                <view class="text">从今日起往前追溯至本月（年）初的每日数据累计之和，不代表真实月（年度）账单，仅供参考决策（真实计提在后续版本上线～）</view>
                <view class="butGather">
                    <button class="generate" type="primary" @tap="backgen">我了解了</button>
                </view>
            </view>
        </uni-popup>
		<NavTab :index="4"></NavTab>
	</view>
</template>

<script>
	import NavTab from "../../components/navTab.vue";
	import Headertop from "../../components/headertop.vue";
    import swiperDot from "./seiperDot.vue";
    import uniPopup from '../components/uni-popup/uni-popup.vue'
    import uniEcCanvas from "../components/uni-ec-canvas/uni-ec-canvas";
	import moment from "moment";

	export default {
		data() {
			return {
                titleTop: "经分概览",
                rpx: 1,
                Permission:"1",
                staHeight:"",
				list: [{
						name: "swiperSlide1",
                        imgUrl: require("../static/images/banner.png"),
                        imgUrl2: require("../static/images/banner2.png"),
						title: "经营日报",
						uptime: "",
                        nameTitle: "昨日毛利",
                        nameTitlegl: "昨日全口径毛利",
						ranking: '',
						sum: "10,000.00",
						text: "新用户立即注册可享短信免费体验！",
						to: "register",
					},
					{
						name: "swiperSlide2",
                        imgUrl: require("../static/images/banner.png"),
                        imgUrl2: require("../static/images/banner2.png"),
						title: "经营月报",
						uptime: "",
						ranking: '',
                        nameTitle: "月累计毛利",
                        nameTitlegl: "本月全口径毛利",
						sum: "10,000.00",
						text: "最大效率触达客户，确保用户知晓信息",
						to: "register",
					},
					{
						name: "swiperSlide2",
                        imgUrl: require("../static/images/banner.png"),
                        imgUrl2: require("../static/images/banner2.png"),
						title: "经营年报",
						uptime: "",
						ranking: '',
                        nameTitle: "年累计毛利",
                        nameTitlegl: "本年全口径毛利",
						sum: "10,000.00",
						text: "最大效率触达客户，确保用户知晓信息",
						to: "register",
					},
                ],
                dataSum:{
                    day:{
                        cost: 0,//成本
                        profit: 0,//毛利
                        rate: "0%",//毛利率
                        revenue: 0,//营收
                    },
                    month:{
                        cost: 0,
                        profit: 0,
                        rate: "0%",
                        revenue: 0,
                    },
                    year:{
                        cost: 0,
                        profit: 0,
                        rate: "0%",
                        revenue: 0,
                    }
                },
				current: 0,
				indicatorDots: false, //是否显示面板指示点
				autoplay: false, //是否自动切换
				interval: 5000, //自动切换时间间隔
                duration: 500, //滑动动画时长
                ationFilter1:false,
                ationFilter2:true,
                ationFilter3:true,
                ationFilter4:false,
                ationFilter5:true,
                ationFilter6:true,
                filterType:1,
                ationFilter7:false,
                ationFilter8:true,
                ationFilter9:true,
                ationClass1:false,
                ationClass2:true,
                ationClass3:true,
                classType:1,
                ationClass4:false,
                ationClass5:true,
                ec: {
					option: {
						color: ["#3AA0FF","#4DCB73"],
						title: {
							text: ""
						},
						tooltip: {
							trigger: "axis",
							formatter: "{b}\r\n营收：{c0}\r\n毛利：{c1}",
							backgroundColor: "rgba(255,255,255,1)",
							extraCssText: 'box-shadow: 0 0 3px rgba(0, 0, 0, 0.3);',
                            axisPointer: { //去掉移动的指示线
								type: 'none',
                            },
							borderRadius: 4,
							textStyle: {
								color: "#333333",
								fontSize: 18
							}
                        },
                        legend: {
                        	selectedMode: false,
                        	data: [{
                        		name: "营收",
                        		icon: 'rect',
                            },
                            {
                        		name: "毛利",
                        		icon: "rect"
                        	}],
                        	itemHeight: 6,
                        	itemWidth: 30,
                            bottom: '4%',
                            textStyle:{
                                fontSize:20,
                                padding:[0, 25, 0, 0]
                            }
                        },
						grid: {
							top: '10%',
							left: '3.2%',
							right: '3.2%',
							bottom: '14%',
							containLabel: true
						},
						xAxis: {
							type: "category",
							boundaryGap: false,//是否前后留白
							data: ["    8.3", "8.4", "8.5", "8.6", "8.7", "8.8", "8.9    "],
							axisTick: {
								// x轴刻度线
								show: false,
								alignWithLabel: true,
							},
							axisLabel: {
								color: "#999999",
								fontSize: 18
							},
							axisLine: {
								lineStyle: {
                                    color: ['#F1F1F1'],
                                    width:0.3
								}
							}
						},
						yAxis: {
                            type: "value",
                            splitNumber:6,//预估间隔，不准确
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
                                    color: ['#F1F1F1'],
                                    width:0.3
								}
							},
							axisLabel: {
								color: "#999999",
								fontSize: 18
							}
                        },
                        series: [{
							name: "营收",
							type: "line",
							// smooth: true,
							symbol: null, // 拐点图形类型
							symbolSize: 5,
							hoverAnimation: false,
							itemStyle: {
								normal: {
									color: '#3AA0FF', //拐点颜色
									borderColor: '#fff', //拐点边框颜色
									borderWidth: 1 //拐点边框大小
								},
								emphasis: {
								    borderWidth: 1,
								    color: '#3AA0FF', //拐点颜色
                                    borderColor: "#fff",
                                    shadowBlur:3,
                                    shadowColor:"#3AA0FF"
								}
							},
							lineStyle: {
								color: "#3AA0FF"
							},
							data: [0, 0, 0, 0, 0, 0, 0]
                        },
                        {
							name: "毛利",
							type: "line",
							// smooth: true,
							symbol: null, // 拐点图形类型
							symbolSize: 5,
							hoverAnimation: false,
							itemStyle: {
								normal: {
									color: '#4DCB73', //拐点颜色
									borderColor: '#fff', //拐点边框颜色
									borderWidth: 1 //拐点边框大小
								},
								emphasis: {
								    borderWidth: 1,
								    color: '#4DCB73', //拐点颜色
                                    borderColor: "#fff",
                                    shadowBlur:3,
                                    shadowColor:"#4DCB73"
								}
							},
							lineStyle: {
								color: "#4DCB73"
							},
							data: [0, 0, 0, 0, 0, 0, 0]
						}]
					}
                },
                ec2:{
                    option: {
						color: ["#178FFF","#2FC15B","#F9CB13"],
						title: {
							text: ""
						},
						tooltip: {
							trigger: "axis",
							formatter: "",
							backgroundColor: "rgba(255,255,255,1)",
							extraCssText: 'box-shadow: 0 0 3px rgba(0, 0, 0, 0.3);',
                            axisPointer: { //去掉移动的指示线
								type: 'none',
                            },
							borderRadius: 4,
							textStyle: {
								color: "#333333",
								fontSize: 18
							}
                        },
                        legend: {
                        	selectedMode: false,
                        	data: [{
                        		name: "隐私号",
                        		icon: 'rect',
                            },
                            {
                        		name: "短信",
                        		icon: "rect"
                        	},
                            {
                        		name: "云线路",
                        		icon: "rect"
                        	}],
                        	itemHeight: 6,
                        	itemWidth: 6,
                            bottom: '4%',
                            textStyle:{
                                fontSize:20,
                                padding:[0, 25, 0, 0]
                            }
                        },
                        dataZoom: [
                            {
                                type: 'inside',
                                xAxisIndex:"0",
                                startValue:0,
                                endValue:6
                            }
                        ],
						grid: {
							top: '10%',
							left: '3.2%',
							right: '3.2%',
							bottom: '14%',
							containLabel: true
						},
						xAxis: {
							type: "category",
							boundaryGap: true,
							data: ["8.3", "8.4", "8.5", "8.6", "8.7", "8.8", "8.9"],
							axisTick: {
								// x轴刻度线
								show: false,
								alignWithLabel: true,
							},
							axisLabel: {
								color: "#999999",
								fontSize: 18
							},
							axisLine: {
								lineStyle: {
                                    color: ['#F1F1F1'],
                                    width:0.3
								}
							}
						},
						yAxis: {
                            type: "value",
                            // splitNumber:6,//预估间隔，不准确
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
                                    color: ['#F1F1F1'],
                                    width:0.3
								}
							},
							axisLabel: {
								color: "#999999",
								fontSize: 18
							}
                        },
                        series: [
                        ]
					}
                },
                rankingList:{
                    "revenue": [        //营收数据
                        {
                            "sale_name": "张三",    //销售
                            "dep_name": "未知",//部门
                            "revenue":"56,982.00"//金额
                        },
                        {
                            "sale_name": "吕超源",
                            "dep_name": "未知",
                            "revenue":"56,982.00"//金额
                        },
                        {
                            "sale_name": "邱杰",
                            "dep_name": "未知",
                            "revenue":"56,982.00"//金额
                        }
                        ,
                        {
                            "revenue": "203.35",
                            "sale_name": "易芬芬",
                            "dep_name": "未知"
                        },
                        {
                            "revenue": "185.69",
                            "sale_name": "黄杰",
                            "dep_name": "未知"
                        }
                    ],
                    "profit": [             //毛利数据
                        {
                            "sale_name": "易芬芬",
                            "dep_name": "未知",
                            "profit":"56,982.00"//金额
                        },
                        {
                            "sale_name": "欧阳俊勋",
                            "dep_name": "未知",
                            "profit":"56,982.00"//金额
                        },
                        {
                            "sale_name": "房平阳",
                            "dep_name": "北京商务一组",
                            "profit":"56,982.00"//金额
                        }
                        ,
                        {
                            "profit": "28.85",
                            "sale_name": "吕超源",
                            "dep_name": "未知"
                        },
                        {
                            "profit": "16.53",
                            "sale_name": "刘晓丹",
                            "dep_name": "未知"
                        }
                    ],
                    "per_revenue": {            //营收个人排名
                        "nickname": "房平阳",   //名字
                        "dep_name": "北京商务一组", //部门
                        "per_rank": 1,//个人排名
                        "val":"56,982.00"
                    },
                    "per_profit": {             //毛利个人排名
                        "nickname": "房平阳",
                        "dep_name": "北京商务一组",
                        "per_rank": 1,
                        "val":"56,982.00"
                    }
                },
                rankingType:true,
                ecType2:true,
                ecType:true
			};
		},
		components: {
			NavTab,
			Headertop,
            swiperDot,
            uniPopup,
            uniEcCanvas
		},
		onLoad() {
			uni.hideTabBar();
        },
        onShow(){
            let that=this
            wx.getSystemInfo({
				success: e => {
                    console.log("设备版本信息",e)
					console.log("状态栏高度", e.statusBarHeight);
					that.staHeight = e.statusBarHeight;
					that.rpx = e.windowWidth / 750;
				}
            });
            // 获取权限标识
            this.getDataAuth()
            .then((res)=>{
                console.log(res)
                this.Permission=res
                // 个人毛利数据排名
                if(this.Permission==2){
                    this.getRank()
                    .then((res)=>{
                        console.log(res)
                        this.list[0].ranking=res.yesterdayRank
                        this.list[1].ranking=res.monthRank
                        this.list[2].ranking=res.yearRank
                    })
                }
                if(this.Permission==4){
                    this.ec.option.tooltip.formatter='{b}\r\n营收：{c0}\r\n全口径毛利：{c1}'
                    this.ec.option.legend.data[1].name='全口径毛利'
                    this.ec.option.series[1].name='全口径毛利'
                }
            })

            // 年/月/日，营收，成本，毛利，毛利率
            this.getDayData()
            .then((res)=>{
                console.log(res)
                this.list[0].sum=res.profit
                this.dataSum.day=res

            })
            this.getMonthData()
            .then((res)=>{
                console.log(res)
                this.list[1].sum=res.profit
                this.dataSum.month=res
            })
            this.getYearData()
            .then((res)=>{
                console.log(res)
                this.list[2].sum=res.profit
                this.dataSum.year=res
            })


            
        },
		onReady() {
            console.log("测试")
			let nowDate = new Date();
			this.list[0].uptime = moment(nowDate).format("MM") + "月" + moment(nowDate).format(
				"DD") + "日"
			this.list[1].uptime = moment(nowDate).format("YYYY") + "年" + moment(nowDate).format("MM") + "月"
            this.list[2].uptime = moment(nowDate).format("YYYY") + "年度"
        
            //图表
            this.operateGraph(1)
            this.businessGraph(1,this.classType)
            this.businessRank(1)

            this.ec.option.yAxis.axisLabel.fontSize = 18 * this.rpx;
            this.ec.option.xAxis.axisLabel.fontSize = 18 * this.rpx;
            this.ec.option.legend.itemHeight = 6 * this.rpx;
            this.ec.option.legend.itemWidth = 30 * this.rpx;
            this.ec.option.legend.textStyle.fontSize = 20 * this.rpx;
            this.ec.option.legend.textStyle.padding[1] = 25 * this.rpx;

            

            this.ec2.option.yAxis.axisLabel.fontSize = 18 * this.rpx;
            this.ec2.option.xAxis.axisLabel.fontSize = 18 * this.rpx;
            this.ec2.option.legend.itemHeight = 6 * this.rpx;
            this.ec2.option.legend.itemWidth = 6 * this.rpx;
            this.ec2.option.legend.textStyle.fontSize = 20 * this.rpx;
            this.ec2.option.legend.textStyle.padding[1] = 25 * this.rpx;
            // this.ec2.option.series[0].barWidth = 40 * this.rpx;
        },
		methods: {
            getRank() {
                return new Promise((resolve, reject) => {
                    this.$post("/apiBusiness.html?f=getRank")
                        .then((res) => {
                            console.log("getRank==>个人毛利数据排名", res)
                            resolve(res.data);
                        })
                })
            },
            getDataAuth() {
                return new Promise((resolve, reject) => {
                    this.$post("/apiBusiness.html?f=getDataAuth")
                        .then((res) => {
                            console.log("getDataAuth==>获取数据权限", res)
                            resolve(res.data);
                        })
                })
            },
            getMonthData() {//本月营收额，成本，毛利，毛利率
                return new Promise((resolve, reject) => {
                    this.$post("/apiBusiness.html?f=getMonthData")
                        .then((res) => {
                            console.log("getMonthData==>本月数据", res)
                            resolve(res.data);
                        })
                })
            },
            getDayData() {//昨日营收额，成本，毛利，毛利率
                return new Promise((resolve, reject) => {
                    this.$post("/apiBusiness.html?f=getDayData")
                        .then((res) => {
                            console.log("getDayData==>昨日数据", res)
                            resolve(res.data);
                        })
                })
            },
            getYearData() {//本年营收额，成本，毛利，毛利率
                return new Promise((resolve, reject) => {
                    this.$post("/apiBusiness.html?f=getYearData")
                        .then((res) => {
                            console.log("getYearData==>本年数据", res)
                            resolve(res.data);
                        })
                })
            },
            operateGraph(type){//经营趋势图
                let data={
                    "type":type,
                }
                 this.$post("/apiBusiness.html?f=operateGraph",data)
                    .then((res) => {
                        console.log("operateGraph==>经营趋势图", res)
                        res.data.x[0]="     "+res.data.x[0]
                        res.data.x[res.data.x.length-1]=res.data.x[res.data.x.length-1]+"     "
                        if(type==3){
                            res.data.x[0]="      "+res.data.x[0]
                            res.data.x[res.data.x.length-1]=res.data.x[res.data.x.length-1]+"      "
                        }
                        this.ec.option.xAxis.data=res.data.x
                        this.ec.option.series[0].data=res.data.list[0].data
                        this.ec.option.series[1].data=res.data.list[1].data
                        if(this.ec.option.series.length==0){
                            this.ecType=false
                        }else{
                            this.ecType=true
                        }
                    })
            },
            businessGraph(type,statisticsType){//业务占比图
                let data={
                    "type":type,
                    "statistics_type":statisticsType
                }
                 this.$post("/apiBusiness.html?f=businessGraph",data)
                    .then((res) => {
                        console.log("businessGraph==>业务占比图", res)
                        this.ec2.option.xAxis.data=res.data.x
                        this.ec2.option.dataZoom[0].endValue=res.data.x.length-1
                        this.ec2.option.tooltip.formatter="{b}"
                        res.data.list.map((item,index)=>{
                            item["type"]="bar"
                            item["barWidth"]=40 * this.rpx;
                            this.ec2.option.tooltip.formatter=this.ec2.option.tooltip.formatter+"\r\n"+item.name+"：{c"+(res.data.list.length-(index+1))+"}"
                            if(res.data.list.length==1){
                                this.ec2.option.color=["#178FFF"]
                            }else if(res.data.list.length==2){
                                this.ec2.option.color=["#2FC15B","#178FFF"]
                            }else{
                                this.ec2.option.color=["#F9CB13","#2FC15B","#178FFF"]
                            }
                        })
                        this.ec2.option.series=res.data.list.reverse()
                        if(this.ec2.option.series.length==0){
                            this.ecType2=false
                        }else{
                            this.ecType2=true
                        }

                        console.log(this.ec2.option.series)
                    })
            },
            businessRank(type) {//商务排名
                return new Promise((resolve, reject) => {
                    this.$post("/apiBusiness.html?f=businessRank",{"type":type})
                        .then((res) => {
                            console.log("businessRank==>商务排名", res)
                            this.rankingList=res.data
                            if(this.Permission!=2){
                                if(this.rankingList.revenue.length==0){
                                    this.rankingType=false
                                }else{
                                    this.rankingType=true
                                }
                            }
                            resolve(res.data);
                        })
                })
            },
			change(e) {
				this.current = e.detail.current;
            },
            tishiOpen(){
                this.$refs.popup.open()
            },
            handelChange(e){
                console.log(e)
            },
            backgen(){
                this.$refs.popup.close()
            },
            filterTime(id){
                if (id == 1) {
                    if (this.ationFilter1) {
                        this.ationFilter1 = false
                        this.operateGraph(1)
                    }else{
                        this.ationFilter1 = false
                    }
                }else{
                    this.ationFilter1 = true
                }
                if (id == 2) {
                    if (this.ationFilter2) {
                        this.ationFilter2 = false
                        this.operateGraph(2)
                    }else{
                        this.ationFilter2 = false
                    }
                }else{
                    this.ationFilter2 = true
                }
                if (id == 3) {
                    if (this.ationFilter3) {
                        this.ationFilter3 = false
                        this.operateGraph(3)
                    }else{
                        this.ationFilter3 = false
                    }
                }else{
                    this.ationFilter3 = true
                }
            },
            filterTime2(id){
                if (id == 4) {
                    this.filterType=1
                    if (this.ationFilter4) {
                        this.ationFilter4 = false
                        this.businessGraph(1,this.classType)
                    }else{
                        this.ationFilter4 = false
                    }     
                }else{
                    this.ationFilter4 = true
                }
                if (id == 5) {
                    this.filterType=2
                    if (this.ationFilter5) {
                        this.ationFilter5 = false
                        this.businessGraph(2,this.classType)
                    }else{
                        this.ationFilter5 = false
                    }
                }else{
                    this.ationFilter5 = true
                }
                if (id == 6) {
                    this.filterType=3
                    if (this.ationFilter6) {
                        this.ationFilter6 = false
                        this.businessGraph(3,this.classType)
                    }else{
                        this.ationFilter6 = false
                    }
                }else{
                    this.ationFilter6 = true
                }
            },
            filterTime3(id){
                if (id == 7) {
                    if(this.ationFilter7){
                        this.ationFilter7 = false
                        this.businessRank(1)
                    }else{
                        this.ationFilter7 = false
                    }
                }else{
                    this.ationFilter7 = true
                }
                if (id == 8) {
                    if(this.ationFilter8){
                        this.ationFilter8 = false
                        this.businessRank(2)
                    }else{
                        this.ationFilter8 = false
                    }
                }else{
                    this.ationFilter8 = true
                }
                if (id == 9) {
                    if(this.ationFilter9){
                        this.ationFilter9 = false
                        this.businessRank(3)
                    }else{
                        this.ationFilter9 = false
                    }
                }else{
                    this.ationFilter9 = true
                }
            },
            filterClass(id){
                if (id == 1) {
                    this.classType=1
                    if (this.ationClass1) {
                        this.ationClass1 = false
                        this.businessGraph(this.filterType,1)
                    }else{
                        this.ationClass1 = false
                    }
                }else{
                    this.ationClass1 = true
                }
                if (id == 2) {
                    this.classType=2
                    if (this.ationClass2) {
                        this.ationClass2 = false
                        this.businessGraph(this.filterType,2)
                    }else{
                        this.ationClass2 = false
                    }
                }else{
                    this.ationClass2 = true
                }
                if (id == 3) {
                    this.classType=3
                    if (this.ationClass3) {
                        this.ationClass3 = false
                        this.businessGraph(this.filterType,3)
                    }else{
                        this.ationClass3 = false
                    }
                }else{
                    this.ationClass3 = true
                }
            },
            filterClass2(id){
                if (id == 4) {
                    this.ationClass4 = false
                }else{
                    this.ationClass4 = true
                }
                if (id == 5) {
                    this.ationClass5 = false
                }else{
                    this.ationClass5 = true
                }
            },
            goDetails(name){
                uni.navigateTo({
                    url: "/pagesA/details/details?name="+name,
                })
            },
            goDetailsTop(){
                console.log("排名跳转")
                uni.navigateTo({
                    url: "/pagesA/details/details?name="+this.rankingList.per_profit.nickname,
                })
            },
            gooPanel(){
                uni.navigateTo({
                    url: "/pagesA/oaPanel/oaPanel",
                })
            },
            goPreList(){
                uni.navigateTo({
                    url: "/pagesA/preList/preList",
                })
            }
		},
	};
</script>

<style lang='scss' scoped>
.opAnys{
    .opAnys_main{
        //  轮播图
        .banner {
            padding-bottom: 24rpx;

            .swiper {
                height: 382rpx;

                swiper-item {
                    display: flex;
                    justify-content: center;
                    align-items: center;
                }

                .swiper-item {
                    width: 750rpx;
                    height: 382rpx;
                    position: relative;

                    img {
                        width: 100%;
                        height: 100%;
                        display: block;
                    }

                    .swiper-item_view {
                        position: absolute;
                        top: 0;
                        bottom: 0;
                        left: 0;
                        right: 0;

                        .dateTitle_label {
                            font-size: 22rpx;
                            color: #FFFFFF;
                            line-height: 22rpx;
                            margin: 36rpx 0 0 49rpx;
                        }
                        .dateTitle_label1{
                            font-size: 22rpx;
                            color: #FFFFFF;
                            line-height: 22rpx;
                            margin: 36rpx 49rpx 0 49rpx;
                            text-align: right;
                            .toppp{
                                text-align: right;
                                margin-bottom: 10rpx;
                            }
                        }

                        .nameTitle {
                            margin-top: 36rpx;

                            .nameTitle_label {
                                display: flex;
                                align-items: center;
                                font-size: 26rpx;
                                line-height: 34rpx;
                                margin-left: 70rpx;
                                color: #F8B551;

                                .xinxi {
                                    display: block;
                                    width: 34rpx;
                                    height: 34rpx;
                                    margin-left: 10rpx;
                                }
                            }
                            .nameTitle_label1{
                                display: flex;
                                align-items: center;
                                font-size: 26rpx;
                                line-height: 34rpx;
                                margin-left: 70rpx;
                                margin-right: 70rpx;
                                justify-content: center;
                                color: #F8B551;

                                .xinxi {
                                    display: block;
                                    width: 34rpx;
                                    height: 34rpx;
                                    margin-left: 10rpx;
                                }
                            }

                            .nameTitle_sum {
                                font-size: 50rpx;
                                line-height: 50rpx;
                                margin-left: 69rpx;
                                margin-top: 22rpx;
                                color: #FFFFFF;
                                display: flex;
                                align-items: flex-end;

                                .danwei {
                                    font-size: 26rpx;
                                    line-height: 26rpx;
                                    margin-left: 18rpx;
                                }
                            }
                            
                            .nameTitle_sum1 {
                                font-size: 50rpx;
                                line-height: 50rpx;
                                margin-left: 69rpx;
                                margin-right: 69rpx;
                                margin-top: 22rpx;
                                color: #FFFFFF;
                                display: flex;
                                align-items: flex-end;
                                justify-content: center;
                                .sum{
                                    margin-left: 30rpx;
                                }
                                .danwei {
                                    font-size: 26rpx;
                                    line-height: 26rpx;
                                    margin-left: 18rpx;
                                }
                            }
                        }
                        .nameTitle1{
                            margin-top: -10rpx;
                            padding-bottom: 14rpx;
                        }
                        .ranking {
                            position: absolute;
                            top: 70rpx;
                            right: 60rpx;
                            width: 98rpx;
                            line-height: 48rpx;
                            font-size: 29rpx;
                            text-align: center;
                            color: #FFFFFF;
                            .num{
                                font-size: 48rpx;
                                line-height: 48rpx;
                            }
                        }
                        .ranking2{
                            position: absolute;
                            top: 74rpx;
                            right: 60rpx;
                            width: 98rpx;
                            line-height: 48rpx;
                            font-size: 25rpx;
                            text-align: center;
                            color: #FFFFFF;
                            .num{
                                font-size: 35rpx;
                                line-height: 48rpx;
                            }
                        }
                        .ranking3{
                            position: absolute;
                            top: 76rpx;
                            right: 60rpx;
                            width: 98rpx;
                            line-height: 48rpx;
                            font-size: 22rpx;
                            text-align: center;
                            color: #FFFFFF;
                            .num{
                                font-size: 30rpx;
                                line-height: 48rpx;
                            }
                        }
                        .dataSum{
                            display: flex;
                            justify-content: space-around;
                            align-items: center;
                            margin: 0 20rpx;
                            margin-top: 76rpx;
                            .dataSum_tiem{
                                width: 236rpx;
                                display: flex;
                                justify-content: center;
                                align-items: center;
                                .dataSum_tiem_label{
                                    font-size:22rpx;
                                    line-height: 22rpx;
                                    color: #797979;
                                }
                                .dataSum_tiem_sum{
                                    margin-top: 13rpx;
                                    line-height: 24rpx;
                                    color: #2F3B53;
                                    font-size:24rpx;
                                }
                            }
                        }
                    }
                }
            }

            .page-section-spacing {
                position: relative;

                .dot {
                    position: absolute;
                    bottom: -15rpx;
                }
            }
        }
        //经营趋势
        .trend{
            width:710rpx;
            margin: 0 20rpx;
            height:720rpx;
            box-shadow:0rpx 0rpx 9rpx 0rpx rgba(213,213,213,0.4);
            border-radius:6rpx;
            background:#FFFFFF;
            margin-top:24rpx;
            .trend_title{
                padding-top: 47rpx;
                padding-left: 19rpx;
                display: flex;
                justify-content: space-between;
                .txt{
                    display: flex;
                    align-items: flex-end;
                    font-size: 32rpx;
                    color: #232323;
                    font-weight: 600;
                    line-height: 32rpx;
                    height: 32rpx;
                    .danwei{
                        margin-bottom: 3rpx;
                        font-size: 22rpx;
                        line-height: 22rpx;
                    }
                }
                .filter_item_buttom{
                    display: flex;
                    align-items: center;
                    font-size: 26rpx;
                    color: #242424;
                    .filter_buttom_left {
                        width: 118rpx;
                        height: 46rpx;
                        text-align: center;
                        line-height: 46rpx;
                        border: 1rpx solid #E9E9E9;
                        border-radius: 24rpx 0px 0px 24rpx;
                        background: #fff;
                    }
                    .filter_buttom_center {
                        width: 120rpx;
                        height: 46rpx;
                        text-align: center;
                        line-height: 46rpx;
                        border-top: 1rpx solid #E9E9E9;
                        border-bottom: 1rpx solid #E9E9E9;
                        border-right: 1rpx solid #fff;
                        border-left: 1rpx solid #fff;
                        background: #fff;
                    }
                    .filter_buttom_right {
                        width: 118rpx;
                        height: 46rpx;
                        text-align: center;
                        line-height: 46rpx;
                        border: 1rpx solid #E9E9E9;
                        border-radius: 0rpx 24rpx 24rpx 0rpx;
                        background: #fff;
                    }
                    .filter_buttom_ation {
                        border-color: #2F79F5;
                        color: #2F79F5;
                    }
                }
            }
            .trend_center{
                height: 543rpx;
            }
            .trend_bottom{
                display: flex;
                justify-content: center;
                align-items: center;
                height: 81rpx;
                margin: 0 36rpx;
                border-top:1px solid #F1F1F1;
                .txt{
                    color: #1890FF;
                    font-size:20rpx; 
                }
                .img{
                    width: 8rpx;
                    height: 12rpx;
                    display: block;
                    margin-left: 8rpx;
                }
            }
        }
        .trend2{
            height:727rpx;
            .trend_filter{
                display: flex;
                align-items: center;
                height: 48rpx;
                margin-top: 24rpx;
                padding-left: 20rpx;
                .trend_filter_item{
                    // width:120rpx;
                    padding: 0 21rpx;
                    height:48rpx;
                    border-radius:6rpx;
                    font-size:26rpx;
                    color: #232323;
                    text-align: center;
                    line-height: 48rpx;
                }
                .trend_filter_item_action{
                    background: #A4B3FF;
                    color: #fff;
                }
            }
            .trend_center{
                height: 466rpx;
            }
        }
        .trend3{
            height:788rpx;
            margin-bottom: 80rpx;
            .trend_title{
                .txt{
                    display: block;
                    height: 65rpx;
                    .danwei{
                        margin-top: 13rpx;
                        font-size:20rpx;
                        color: #959595;
                        line-height: 20rpx;
                    }
                }

            }
            .trend_filter{
                display: flex;
                align-items: center;
                height: 48rpx;
                margin-top: 29rpx;
                padding-left: 20rpx;
                .trend_filter_item{
                    // width:120rpx;
                    padding: 0 21rpx;
                    height:48rpx;
                    border-radius:6rpx;
                    font-size:26rpx;
                    color: #232323;
                    text-align: center;
                    line-height: 48rpx;
                }
                .trend_filter_item_action{
                    background: #A4B3FF;
                    color: #fff;
                }
            }
            .trend_center{
                height: 520;
                margin: 0 20rpx;
                margin-top: 30rpx;
                .trend_ranking_item{
                    height: 130rpx;
                    background: #F7F7FD;
                    padding: 0 20rpx;
                    display: flex;
                    align-items: center;
                    justify-content: space-between;
                    .trend_ranking_left{
                        display: flex;
                        .trend_ranking_num{
                            width:46rpx;
                            height:46rpx;
                            background: #2F79F5;
                            color: #fff;
                            border-radius: 50%;
                            font-size: 28rpx;
                            line-height:46rpx;
                            text-align: center;
                            margin-right: 20rpx;
                        }
                        .trend_ranking_num3{
                            font-size: 22rpx !important;
                        }
                        .trend_ranking_num4{
                            background: #EFEFF3 !important;
                            color: #232323 !important;
                        }
                        .trend_ranking_title{
                            .trend_ranking_name{
                                font-size:30rpx;
                                color: #232323;
                                line-height: 30rpx;
                                margin-bottom: 18rpx;
                            }
                            .trend_ranking_groupName{
                                font-size:20rpx;
                                line-height: 20rpx;
                                color: #959595;
                            }
                        }
                    }
                    .trend_ranking_right{
                        text-align: right;
                        .trend_ranking_txt{
                                font-size:20rpx;
                                color: #797979;
                                line-height: 20rpx;
                                margin-bottom: 16rpx;
                        }
                        .trend_ranking_sum{
                                font-size:30rpx;
                                color: #232323;
                                line-height: 30rpx;
                        }
                    }
                }
                .trend_ranking_item1{
                    background: #fff;
                    .trend_ranking_left{
                        .trend_ranking_num{
                            background:#F7B551;
                        }
                    }
                }
            }
        }
        .trend4{
            height:999rpx !important;
            .trend_center{
                height: 650rpx !important;
            }
            .trend_bottom{
                margin-top:55rpx;
            }
        }
    }
}

    //占位图
    .nodata{
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        height: 466rpx;
        .img{
            width: 270rpx;
            height: 198rpx;
            display: block;
        }
        .txt{
            margin-top: 24rpx;
            font-size: 30rpx;
            color: #D8DBE5;
        }
    }
    //弹出层
    .pop{
        z-index: 999999999;
        .center{
            background: #fff;
            border-radius: 18rpx;
            width: 680rpx;
            position: relative;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding-bottom: 90rpx;
            z-index: 99999;
            .title{
                font-size:36rpx;
                line-height: 36rpx;
                color: #232323;
                margin-top: 99rpx;
                margin-bottom: 55rpx;
                letter-spacing:3rpx;
            }
            .text{
                margin: 0 54rpx;
                color: #8F8F8F;
                font-size:26rpx;
                line-height:40rpx;
                margin-bottom: 62rpx;
                text-align: center;
            }
            .butGather{
                width: 580rpx;
                height: 90rpx;
                .generate{
                    width: 100%;
                    height: 100%;
                    background:rgba(47,121,245,1);
                    border-radius:45px;
                }
            }
        }
    }
</style>
