<template>
	<view>
		<view class="orderNav pdlr4">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">未支付</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">已支付</view>
			<view class="tt" :class="current==3?'on':''" @click="change('3')">已完成</view>
		</view>
		
		<view class="prolisbox">
			
			<view>
				<view class="prolis"  v-for="(item,index) in mainData">
					<view class="twoCt">
						<view class="leftbox">
							<image :src="item.products&&item.products[0]&&item.products[0].snap_product&&item.products[0].snap_product.product&&item.products[0].snap_product.product.mainImg&&item.products[0].snap_product.product.mainImg[0]?item.products[0].snap_product.product.mainImg[0].url:''"></image>
						</view>
						<view class="cont">
							<view class="title avoidOverflow2">{{item.products&&item.products[0]&&item.products[0].snap_product&&item.products[0].snap_product.product&&item.products[0].snap_product.product?item.products[0].snap_product.product.title:''}}</view>
							<!-- <view class="charge">会员免费</view> -->
						</view>
					</view>
					<view class="explain" v-for="c_item in item.products">
						<view>{{c_item.snap_product?c_item.snap_product.title:''}}</view>
						<view class="price">{{c_item.snap_product?c_item.snap_product.price:''}}</view>
					</view>
					<view class="bBtn red">
						<view>总金额：</view>
						<view class="price">{{item.price}}</view>
						<view class="btn gopay" v-if="item.pay_status==0" @click="pay(index)">去支付</view>
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
				mainData:[],
				current:1,
				searchItem:{
					pay_status:0
				}
			}
		},
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			if(options.current){
				self.change(options.current)
			}else{
				self.$Utils.loadAll(['getMainData'], self);
			}
			
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			change(current) {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(current!=self.current){
					self.searchItem = {};
					self.current = current
					if (current == '1') {
						self.searchItem.pay_status = '0';
					} else if (current == '2') {
						self.searchItem.pay_status = '1';
								
					} else if (current == '3') {
						self.searchItem.pay_status = '1';
						self.searchItem.transport_status  = '2';
					} 
					self.getMainData(true)
				}
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.$Utils.clearPageIndex(self);
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.thirdapp_id = 2;
				postData.searchItem.type = 1;
				postData.searchItem.passage1 = 'user';
				postData.order = {
					create_time: 'desc'
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info.data.length > 0) {
							self.mainData.push.apply(self.mainData, res.info.data);
						} else {
							self.$Utils.showToast('没有更多了', 'none');
						};
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('网络故障', 'none')
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
				
			
			
			pay(index) {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {
					tokenFuncName: 'getProjectToken',
					searchItem: {
						id: self.mainData[index].id,
					},
					/* score:self.data.mainData[index].price, */
					wxPay: {
						price: self.mainData[index].price,
					},
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								if (payData == 1) {
									self.$Utils.showToast(res.msg, 'none')
									self.getMainData(true);
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						}
					} else {
						self.$Utils.showToast(res.msg, 'none')
						self.getMainData(true);
					}
				};
				self.$apis.pay(postData, callback);
			},
			
			
			
			

		},
	};
</script>

<style>
	@import "../../assets/style/common.css";
	page{ background: #f5f5f5;padding-bottom: 100rpx;}
		
	.prolisbox .prolis{width: 92%;margin: 30rpx auto 0 auto;background: #fff; padding: 30rpx;box-sizing: border-box;}

	.prolisbox .prolis .leftbox{ width:220rpx ; height: 147rpx; overflow: hidden;}
	.prolisbox .prolis .leftbox image{ width: 100%; height: 100%; display: block;}
	.prolisbox .prolis .cont{ width:380rpx; height: 147rpx; position: relative;}
	.twoCt{ display: flex;justify-content: space-between;}
	.prolis .cont .title{ font-size:28rpx ; line-height: 40rpx;}
	.prolis .cont .charge{ position: absolute; bottom: 0;left: 0; font-size: 26rpx; color: rgba(255,59,59);}
	.prolis .explain{padding: 20rpx; background: #f9f9f9; border:2rpx solid #e7e7e7; font-size: 24rpx; color: #666; margin: 30rpx 0;box-sizing: border-box;}
	.prolis .explain .price{ margin-top: 16rpx; font-size: 26rpx;}
	.bBtn{padding-top: 10rpx; display: flex; justify-content: flex-end; align-items: center;}
	.bBtn .btn{ padding: 0 40rpx; line-height: 50rpx; height: 50rpx; border-radius: 30rpx; color: #666; border:2rpx solid #e7e7e7;font-size: 24rpx; margin-left: 30rpx;}
</style> 
