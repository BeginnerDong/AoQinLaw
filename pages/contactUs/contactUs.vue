<template>
	<view>
		<view class="top">
			<image class="icon" src="../../static/images/contactUs-img1.png"></image>
		</view>
		<view class="f5H10"></view>

		<view class="contactLis">
			<view class="tit">电话：{{mainData.phone}}</view>
			<view class="icon" @click="phoneCall">
				<image src="../../static/images/us-icon1.png" mode=""></image>
			</view>
		</view>
		<view class="contactLis">
			<view class="tit">地址：{{mainData.address}}</view>
			<view class="icon" @click="intoMap">
				<image src="../../static/images/us-icon2.png" mode=""></image>
			</view>
		</view>
		<view class="pdlr4" style="color: #666; font-size: 26rpx; text-align: center;margin: 150rpx auto 60rpx auto;">如您对我们有兴趣，欢迎随时来电</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData: {
					phone: '',
					address: '',
					latitude: '',
					longitude: ''
				},			
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {

			getMainData() {
				const self = this;
				self.mainData.phone = wx.getStorageSync('user_info').thirdApp.phone;
				self.mainData.address = wx.getStorageSync('user_info').thirdApp.address;
				self.mainData.latitude = wx.getStorageSync('user_info').thirdApp.latitude;
				self.mainData.longitude = wx.getStorageSync('user_info').thirdApp.longitude;
			},
			
			phoneCall() {
				const self = this;
				wx.makePhoneCall({
					phoneNumber: self.mainData.phone,
				})
			},
			
			intoMap() {
				const self = this;
				wx.getLocation({
					type: 'gcj02', //返回可以用于wx.openLocation的经纬度
					success: function(res) { //因为这里得到的是你当前位置的经纬度
						var latitude = res.latitude
						var longitude = res.longitude
						wx.openLocation({ //所以这里会显示你当前的位置
							// longitude: 109.045249,
							// latitude: 34.325841,
							longitude: parseFloat(self.mainData.longitude),
							latitude: parseFloat(self.mainData.latitude),
							name: "澳秦法律",
							address: self.mainData.address,
							scale: 28
						})
					}
				})
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
