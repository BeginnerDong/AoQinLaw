<template>
	<view>
		<!-- banner -->
		<view class="banner-box">
			<view class="banner">
				<swiper class="swiper-box" @change="changeIndex" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" indicator-active-color="#537deb" indicator-color="#dbdbdb">
					<block v-for="(item,index) in mainData" :key="index">
						<swiper-item class="swiper-item">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="slide-image" />
						</swiper-item>
					</block>
				</swiper>
			</view>
		</view>
		
		<view class="msg pdlr4">
			<view class="tit">服务说明</view>
			<view class="content ql-editor" v-html="mainData[currentIndex].content">
			</view>
		</view>
		<view class="f5H10"></view>
		<view class="pdlr4 seviceNum">
			<view class="tit">已使用服务次数</view>
			<view class="lis flexRowBetween">
				<view class="">会员咨询</view>
				<view class="nmb">8次</view>
			</view>
			<view class="lis flexRowBetween">
				<view class="">撰写合同</view>
				<view class="nmb">8次</view>
			</view>
			<view class="lis flexRowBetween">
				<view class="">民事诉讼</view>
				<view class="nmb">8次</view>
			</view>
			<view class="lis flexRowBetween">
				<view class="">标准化催收服务</view>
				<view class="nmb">8次</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin: 100rpx auto 0 auto">
			<button type="submit" @click="Utils.stopMultiClick(submit)">立即购买</button>
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
				currentIndex:0
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			changeIndex(e){
				const self = this;
				self.currentIndex = e.detail.current;
				console.log(e)
			},

			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2,
					type:2,
				};	
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				/* const callback = (user, res) => {
					self.addOrder();
				};
				api.getAuthSetting(callback); */
				self.addOrder();
			},
			
			
			addOrder() {
				const self = this;
				const orderList = [{
					product: [],
					type: 2
				}];
			
				orderList[0].product.push({
					id: self.mainData[self.currentIndex].id,
					count: 1
				}, );
				if (!self.order_id) {
					const postData = {
						tokenFuncName: 'getProjectToken',
						orderList: orderList,
					};
					console.log('addOrder', self.addressData)
			
					const callback = (res) => {
						if (res && res.solely_code == 100000) {
							self.order_id = res.info.id
							self.pay(self.order_id);
						}else{
							uni.setStorageSync('canClick', true);
							self.$Units.showToast(res.msg,'none')
						}
					};
					self.$apis.addOrder(postData, callback);
				} else {
					self.pay(self.order_id)
				}
			},
			
			
			
			
			pay() {
				const self = this;
				var order_id = self.order_id;
				const postData = {
					tokenFuncName: 'getProjectToken',
					searchItem: {
						id: order_id,
					},
					/* score:self.mainData[self.currentIndex].price */
					wxPay: {
						price: self.mainData[self.currentIndex].price
					}
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								if (payData == 1) {
									self.$Units.showToast('购买成功', 'none');
									setTimeout(function() {
										Router.reLaunch({route:{path:'/pages/indx/indx'}})
									}, 800)
								};
							};
							self.$apis.realPay(res.info, payCallback);
						}
					} else {
						self.$Units.showToast('支付失败', 'none')
					}
				};
				self.$apis.pay(postData, callback);
			},
			
			

		},
	};
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/index.css";

	.swiper-box{ height: 400rpx; width: 100%; margin: 50rpx auto; border-radius: 10rpx; }
	.swiper-box .swiper-item image{ width: 251px;height: 324rpx;  display: block;margin: 0 auto;}
	.msg view{ padding-bottom: 30rpx; font-size: 26rpx; color: #666; line-height: 40rpx;}
	view.tit{ font-size: 30rpx; font-weight: bold; color: #333; line-height: 60rpx;padding-bottom: 0;}
	.ql-editor{padding: 30rpx 0;}
	
	.seviceNum{padding: 20rpx 4%;}
	.seviceNum .lis{ height: 80rpx;border-bottom: 2rpx solid #e7e7e7; font-size: 26rpx; color: #666;}
	.seviceNum .nmb{ color: #537DEB;}

</style>
