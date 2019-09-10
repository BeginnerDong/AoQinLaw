<template>
	<view>
		<view class="vipCard">
			<image :src="cardData.mainImg&&cardData.mainImg[0]?cardData.mainImg[0].url:''" mode=""></image>
		</view>
		
		<view class="msg pdlr4">
			<view class="tit">服务说明</view>
			<view class="content ql-editor">
				<view class="content ql-editor" v-html="cardData.content">
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		<view class="pdlr4 seviceNum">
			<view class="tit">已使用服务次数</view>
			<view class="lis flexRowBetween" v-for="item in serviceData">
				<view class="">{{item.snap_product.title}}</view>
				<view class="nmb">{{item.hasUse}}次</view>
			</view>
			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				mainData:[],
				indicatorDots: true,
				autoplay: false,
				interval: 2000,
				duration: 500,
				currentIndex:0,
				cardData:{},
				serviceData:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			

			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id:2,
					type:2,
					pay_status:1
				};	
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						for (var i = 0; i < self.mainData.products.length; i++) {
							if(self.mainData.products[i].behavior==1){
								self.cardData = self.mainData.products[i].snap_product
								const regex = new RegExp('<img', 'gi');
								self.cardData.content = self.cardData.content.replace(regex, `<img style="max-width: 100%;"`);
							};
							if(self.mainData.products[i].behavior==2){
								self.serviceData.push(self.mainData.products[i])
							}
						};
						for (var i = 0; i < self.serviceData.length; i++) {
							self.serviceData[i].hasUse = self.serviceData[i].total - self.serviceData[i].rest
						}
						console.log('self.serviceData',self.serviceData)
					} else {
						self.$Utils.showToast('没有更多了', 'none');
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

	.vipCard{ width: 100%; margin: 50rpx auto; }
	.vipCard image{ width: 503rpx;height: 324rpx;  display: block;margin: 0 auto;}
	.msg view{ padding-bottom: 30rpx; font-size: 26rpx; color: #666; line-height: 40rpx;}
	view.tit{ font-size: 30rpx; font-weight: bold; color: #333; line-height: 60rpx;padding-bottom: 0;}
	.ql-editor{padding: 30rpx 0;}
	
	.seviceNum{padding: 20rpx 4%;}
	.seviceNum .lis{ height: 80rpx;border-bottom: 2rpx solid #e7e7e7; font-size: 26rpx; color: #666;}
	.seviceNum .nmb{ color: #537DEB;}

</style>
