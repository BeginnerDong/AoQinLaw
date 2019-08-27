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
				webSelf: this,
				showView: false,
				score:'',
				wx_info:{},
				current:1
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current
				}
			},
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
