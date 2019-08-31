<template>
	<view>
		<view class="purchs_head flexRowBetween">
			<view class="xian"></view>
			<view class="item flexRowBetween">
				<image src="../../static/images/xiadan-icon1.png" alt=""></image>
				<view class="tit">提交订单</view>
			</view>
			<view class="item flexRowBetween">
				<image class="twoIcon" src="../../static/images/xiadan-icon2.png" alt=""></image>
				<view class="tit">支付金额</view>
			</view>
			<view class="item flexRowBetween">
				<image src="../../static/images/xiadan-icon3.png" alt=""></image>
				<view class="tit">律师回电服务</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="purch_indList">
			<view class="purchTitle flexRowBetween pdlr4">
				<view class="tit">服务项目</view>
			</view>
			<view class="fwxm mglr4">
				<view class="text">不涉及财产关系100元/小时；(不足1小时按1小时计算)</view>
				<view class="price">56.00</view>
			</view>
			<view class="f5H10"></view>
			<view class="purchTitle flexRowBetween pdlr4" style="border-bottom:2rpx solid #E7E7E7 ;">
				<view class="tit">个人姓名</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入姓名" placeholder-style="color:#999" />
				</view>
			</view>
			
			<view class="purchTitle flexRowBetween pdlr4">
				<view class="tit">问题详情</view>
			</view>
			<view class="pdlr4">
				<textarea value="" placeholder="请输入您想要的问题一遍律师提前准备" placeholder-style="color:#999;" />
			</view>
			<view class="Detai_BfixBox">
				总金额：
				<span class="price" style="font-size: 30rpx;">56.00</span> 
				<view class="Rbtn" >立即下单</view>
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
	@import "../../assets/style/service.css";
	page{padding-bottom: 140rpx;}
	.purchs_head{padding: 40rpx 4%; position: relative;}
	.purchs_head .xian{ border-top: 2rpx dashed #537DEB; width: 370rpx; position: absolute; top: 64rpx; left: 50%; transform: translateX(-50%);}
	.purchs_head .item{ width: 33.3%; flex-direction: column; font-size: 26rpx;}
	.purchs_head .item image{ width: 48rpx; height: 48rpx; margin: 0 auto; display: block; margin-bottom: 30rpx;}
	.purchs_head .item .twoIcon{ background: #fff; padding: 0 20rpx;}
	
	.purchTitle{ height: 100rpx; font-size:30rpx;}
	.purchTitle .tit{ width: 30%; font-weight: bold;}
	.purchTitle .rr{ width: 50%; color: #333; text-align: right; display: block;}
	.purchTitle .rr input{ width: 100%; display: block; text-align: right; font-size: 24rpx;}
	
	.purch_indList .fwxm{ padding-bottom:40rpx; border: 2rpx solid #e7e7e7; background: #F9F9F9;padding: 30rpx; font-size: 24rpx; color: #666; margin-bottom: 40rpx;}
	.purch_indList .fwxm .price{ font-size: 26rpx; margin-top: 20rpx;}
	.purch_indList textarea{ width: 100%; box-sizing: border-box; padding: 20rpx;border:2rpx solid #e7e7e7; background: #f2f2f2; font-size: 26rpx; line-height: 40rpx;}
	
</style>
