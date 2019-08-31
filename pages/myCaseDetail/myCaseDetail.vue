<template>
	<view>
		<view class="myCace">
			<view class="item" style="margin-top: 0; border-radius: 0;">
				<view>标题标题标题标题标题标题标题标题</view>
				<view class="lable flexRowBetween">
					<view class="left">刑事类</view>
					<view class="right">2019年8月27日</view>
				</view>
				<view class="info">内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="pubTit2 pdlr4" >服务律师</view>
		<view class="lawyerList">
			<view class="info pr"  style="margin-top: 0;">
				<view class="info-left">
					<image src="../../static/images/lvshi-img1.png"></image>
				</view>
				<view class="info-right" style="width:78%">
					<view class="name">张胜男<view class="lable">诉讼律师</view></view>
					<view class="two">
						<image class="icon" src="../../static/images/lvshi-icon2.png" ></image>
						执业2年
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
						<block>
							<view class="info-item">刑事诉讼</view>
							<view class="info-item">债权债务</view>
						</block>
					</view>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="pubTit2 pdlr4" >案件进度</view>
		<view class="CaseJindu">
			<view class="item">
				<image class="icon" src="../../static/images/lvshi-icon1.png" ></image>
				<view class="flexRowBetween">
					<view class="l-data">
						<view class="day">03</view>
						<view class="dt">2019-08</view>
					</view>
					<view style="color: #fdc125;">进度状态</view>
				</view>
				<view class="cont">内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。</view>
			</view>
			
			<view class="item">
				<image class="icon" src="../../static/images/lvshi-icon1.png" ></image>
				<view class="flexRowBetween">
					<view class="l-data">
						<view class="day">03</view>
						<view class="dt">2019-08</view>
					</view>
					<view style="color: #fdc125;">进度状态</view>
				</view>
				<view class="cont">内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。</view>
			</view>
			
			<view class="item">
				<image class="icon" src="../../static/images/lvshi-icon1.png" ></image>
				<view class="flexRowBetween">
					<view class="l-data">
						<view class="day">03</view>
						<view class="dt">2019-08</view>
					</view>
					<view style="color: #fdc125;">进度状态</view>
				</view>
				<view class="cont">内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。</view>
			</view>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {

			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';

				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data;
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
