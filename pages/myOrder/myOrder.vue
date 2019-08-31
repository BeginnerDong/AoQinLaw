<template>
	<view>
		<view class="orderNav pdlr4">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">未支付</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">已支付</view>
			<view class="tt" :class="current==3?'on':''" @click="change('3')">已完成</view>
		</view>
		
		<view class="prolisbox">
			
			<view v-if="current==1">
				<view class="prolis">
					<view class="twoCt">
						<view class="leftbox">
							<image src="../../static/images/details-img.png"></image>
						</view>
						<view class="cont">
							<view class="title avoidOverflow2">标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="charge">会员免费</view>
						</view>
					</view>
					<view class="explain">
						<view>不涉及财产关系100元/小时；(不足1小时按1小时计算)</view>
						<view class="price">56.00</view>
					</view>
					<view class="bBtn red">
						<view>总金额：</view>
						<view class="price">56.00</view>
						<view class="btn gopay">去支付</view>
					</view>
				</view>
				<view class="prolis">
					<view class="twoCt">
						<view class="leftbox">
							<image src="../../static/images/details-img.png"></image>
						</view>
						<view class="cont">
							<view class="title avoidOverflow2">标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="charge">会员免费</view>
						</view>
					</view>
					<view class="explain">
						<view>不涉及财产关系100元/小时；(不足1小时按1小时计算)</view>
						<view class="price">56.00</view>
					</view>
					<view class="bBtn red">
						<view>总金额：</view>
						<view class="price">56.00</view>
						<view class="btn gopay">去支付</view>
					</view>
				</view>
			</view>
			<view v-if="current==2">
				<view class="prolis">
					<view class="twoCt">
						<view class="leftbox">
							<image src="../../static/images/details-img.png"></image>
						</view>
						<view class="cont">
							<view class="title avoidOverflow2">标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="charge">会员免费</view>
						</view>
					</view>
					<view class="explain">
						<view>不涉及财产关系100元/小时；(不足1小时按1小时计算)</view>
						<view class="price">56.00</view>
					</view>
					<view class="bBtn red">
						<view>总金额：</view>
						<view class="price">56.00</view>
					</view>
				</view>
			</view>
			<view v-if="current==3">
				<view class="prolis">
					<view class="twoCt">
						<view class="leftbox">
							<image src="../../static/images/details-img.png"></image>
						</view>
						<view class="cont">
							<view class="title avoidOverflow2">标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="charge">会员免费</view>
						</view>
					</view>
					<view class="explain">
						<view>不涉及财产关系100元/小时；(不足1小时按1小时计算)</view>
						<view class="price">56.00</view>
					</view>
					<view class="bBtn red">
						<view>总金额：</view>
						<view class="price">56.00</view>
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
				current:1
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			change(current) {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(current!=self.current){
					self.current = current
					if (current == '1') {
						self.data.searchItem.pay_status = '0';
					} else if (current == '2') {
						self.data.searchItem.pay_status = '1';
								
					} else if (current == '3') {
						self.data.searchItem.pay_status = '1';
						self.data.searchItem.transport_status  = '2';
					} 
					self.getMainData(true)
				}
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.$Units.clearPageIndex(self);
				};
				const postData = {};
				postData.paginate = self.$Units.cloneForm(self.paginate);
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = self.$Units.cloneForm(self.searchItem);
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
							self.$Units.showToast('没有更多了', 'none');
						};
					} else {
						uni.setStorageSync('canClick', true);
						self.$Units.showToast('网络故障', 'none')
					};
			
					
					console.log('getMainData', self.data.mainData)
				};
				self.$Units.orderGet(postData, callback);
			},
				
			
			
			pay(e) {
				const self = this;
				var index = api.getDataSet(e,'index');
				const postData = {
					tokenFuncName: 'getProjectToken',
					searchItem: {
						id: self.data.mainData[index].id,
					},
					/* score:self.data.mainData[index].price, */
					wxPay: {
						price: self.data.mainData[index].price,
					},
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						api.buttonCanClick(self, true);
						if (res.info) {
							const payCallback = (payData) => {
								if (payData == 1) {
									api.showToast(res.msg, 'none')
									self.getMainData(true);
								};
							};
							api.realPay(res.info, payCallback);
						}
					} else {
						api.showToast(res.msg, 'none')
						self.getMainData(true);
					}
				};
				api.pay(postData, callback);
			},
			
			
			
			menuClick: function(e) {
				const self = this;
				api.buttonCanClick(self);
				const num = e.currentTarget.dataset.num;
				self.changeSearch(num);
			},
			
			changeSearch(num) {
				const self = this;
				this.setData({
					num: num
				});
				self.data.searchItem = {}
				if (num == '0') {
					self.data.searchItem.pay_status = '0';
				} else if (num == '1') {
					self.data.searchItem.pay_status = '1';
			
				} else if (num == '2') {
					self.data.searchItem.pay_status = '1';
					self.data.searchItem.transport_status  = '2';
				} 
				self.setData({
					web_mainData: [],
				});
				self.getMainData(true);
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
