<template>
	<view>
		<view class="top">
			<image class="icon" src="../../static/images/contactUs-img1.png"></image>
		</view>
		<view class="f5H10"></view>

		<view class="contactLis">
			<view class="tit">电话：029-8931246</view>
			<view class="icon">
				<image src="../../static/images/us-icon1.png" mode=""></image>
			</view>
		</view>
		<view class="contactLis">
			<view class="tit">地址：陕西省西安市雁塔区大都荟</view>
			<view class="icon">
				<image src="../../static/images/us-icon1.png" mode=""></image>
			</view>
		</view>
		<view class="pdlr4" style="color: #666; font-size: 26rpx; text-align: center;margin: 150rpx auto 60rpx auto;">如您对我们有兴趣，欢迎随时来电</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				webSelf: this,
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
	.top image{ width: 403rpx; height: 369rpx; display: block;margin: 50rpx auto;}
	.contactLis{ padding:30rpx 4%; font-size: 28rpx; display: flex; justify-content: space-between;border-bottom: 2rpx solid #e7e7e7;}
	.contactLis .tit{ width: 85%; line-height: 40rpx;}
	.contactLis .icon{ width: 15%;}
	.contactLis .icon image{width: 40rpx; height: 40rpx; display: block;float: right;}
</style>
