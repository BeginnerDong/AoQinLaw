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
		<view class="lawyerList">
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
								<image src="../../static/images/home-icon12.png" mode=""></image>
								<image src="../../static/images/home-icon12.png" mode=""></image>
								<image src="../../static/images/home-icon12.png" mode=""></image>
								<image src="../../static/images/home-icon13.png" mode=""></image>
								<image src="../../static/images/home-icon11.png" mode=""></image>
							</view>
							<view>9.5分</view>
						</view>
					</view>
					<view class="three">
						<view class="info-item" v-for="item in mainData.lawyer[0].keywords">{{item}}</view>		
					</view>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="pubTit2 pdlr4" >案件进度</view>
		<view class="CaseJindu">
			<view class="item" v-for="item in mainData.log">
				<image class="icon" src="../../static/images/lvshi-icon1.png" ></image>
				<view class="flexRowBetween">
					<view class="l-data">
						<view class="day">03</view>
						<view class="dt">2019-08</view>
					</view>
					<view style="color: #fdc125;">{{item.title}}</view>
				</view>
				<view class="cont">
					<view class="content ql-editor" style="padding: 0;" v-html="item.content">
					</view>
				</view>
			</view>		
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{}
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
						}
						console.log(self.mainData.lawyer[0].keywords)
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
