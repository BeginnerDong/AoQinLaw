<template>
	<view>
		<view class="myCace">
			<view class="item" style="margin-top: 0; border-radius: 0;">
				<view class="avoidOverflow">{{mainData.products[0].snap_product.product.title}}</view>
				<view class="lable flexRowBetween">
					<!-- <view class="left">刑事类</view> -->
					<view class="right">{{mainData.create_time}}</view>
				</view>
				<view class="info">{{mainData.description}}</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="pubTit2 pdlr4">服务律师</view>
		<view class="lawyerList" v-if="mainData.lawyer.length>0">
			<view class="info pr"  style="margin-top: 0;">
				<view class="info-left">
					<image :src="mainData.lawyer[0].mainImg[0].url"></image>
				</view>
				<view class="info-right" style="width:78%">
					<view class="name">{{mainData.lawyer[0].title}}<view class="lable">{{mainData.lawyer[0].passage1}}</view></view>
					<view class="two">
						<image class="icon" src="../../static/images/lvshi-icon2.png" ></image>
						执业{{mainData.lawyer[0].small_title}}年
						<view class="flexRowBetween starClass" style="margin-left: 10rpx;">
							<view class="starBox">
								<image v-for="item in stars" :src="mainData.averageScore > item ?(mainData.averageScore-item == 0.5?halfSrc:selectedSrc) : normalSrc" mode="">
								
								</image>
							</view>
							<view v-if="mainData.averageScore>0">{{mainData.averageScore*2}}分</view>
							<view v-if="mainData.averageScore==0">暂无评分</view>
						</view>
					</view>
					<view class="three">
						<view class="info-item" v-for="item in mainData.lawyer[0].keywords">{{item}}</view>		
					</view>
				</view>
			</view>
		</view>
		<view v-else class="pdlr4">暂未分配律师</view>
		<view class="f5H10"></view>
		
		<view class="pubTit2 pdlr4" >案件进度</view>
		<view class="CaseJindu">
			<view class="item" v-for="item in mainData.log" v-if="mainData.log.length>0">
				<image class="icon" src="../../static/images/lvshi-icon1.png" ></image>
				<view class="flexRowBetween">
					<view class="l-data">
						<view class="day">{{item.timeTwo}}</view>
						<view class="dt">{{item.timeOne}}</view>
					</view>
					<view style="color: #fdc125;">{{item.title}}</view>
				</view>
				<view class="cont">
					<view class="content ql-editor" style="padding: 0;" v-html="item.content">
					</view>
				</view>
			</view>	
			<view  class="pdlr4" v-if="mainData.log.length==0">没有新的进度~</view>
		</view>
	</view>
</template>

<script>
	
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				averageScore:0,
				stars: [0, 1, 2, 3, 4],
				normalSrc: '../../static/images/home-icon11.png',
				selectedSrc: '../../static/images/home-icon12.png',
				halfSrc: '../../static/images/home-icon13.png',
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {

			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.id
				};
				postData.getAfter = {
					lawyer:{
						tableName:'Article',
						middleKey:'lawyer',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'='
					},
					
					message: {
						tableName: 'Message',
						searchItem: {
							status:1,
							type:1
						},
						middleKey: ['lawyer',0,'id'],
						key: 'relation_id',
						condition: 'in',
						compute:{
						  score:[
							'sum',
							'score',
							{
							  status:1,
							}
						  ],
						  count:[
							'count',
							'count',
							{
							  status:1,
							}
						  ]
						},
					},
					
					log:{
						tableName:'OrderLog',
						middleKey:'order_no',
						key:'order_no ',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
						if(self.mainData.lawyer.length>0){
							self.mainData.lawyer[0].keywords = self.mainData.lawyer[0].keywords.split(',');	
						
								self.mainData.averageScore = self.mainData.message.score/self.mainData.message.count
							
						
						};
						
						if(self.mainData.log.length>0){
							for (var i = 0; i < self.mainData.log.length; i++) {
								self.mainData.log[i].timeOne = self.mainData.log[i].create_time.substring(0,7)
								self.mainData.log[i].timeTwo = self.mainData.log[i].create_time.substring(8,10)
							}
							
						}
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');

				};
				self.$apis.orderGet(postData, callback);
			},

		},
	};
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/lawyer.css";

</style> 
