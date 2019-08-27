<template>
	<view>
		<!-- banner -->
		<view class="banner-box">
			<view class="banner">
				<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="interval" duration="duration" indicator-active-color="#537deb" indicator-color="#dbdbdb">
					<block v-for="(item,index) in labelData" :key="index">
						<swiper-item class="swiper-item">
							<image :src="item" class="slide-image" />
						</swiper-item>
					</block>
				</swiper>
			</view>
		</view>
		
		<view class="msg pdlr4">
			<view class="tit">服务说明</view>
			<view>1、内容内容内容内容内容内容内容内容内容内容内容内容内容内容；</view>
			<view>2、内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容；</view>
			<view>3、内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容；</view>
		</view>
		
		<view class="submitbtn" style="margin: 100rpx auto 0 auto">
			<button type="submit">立即购买</button>
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
				labelData: [
					"../../static/images/vip-img1.png",
					"../../static/images/vip-img1.png",
					"../../static/images/vip-img1.png",
				]
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
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
	@import "../../assets/style/index.css";

	.swiper-box{ height: 400rpx; width: 100%; margin: 50rpx auto; border-radius: 10rpx; }
	.swiper-box .swiper-item image{ width: 251px;height: 324rpx;  display: block;margin: 0 auto;}
	.msg view{ padding-bottom: 30rpx; font-size: 26rpx; color: #666; line-height: 40rpx;}
	.msg .tit{ font-size: 30rpx; font-weight: bold;}
	

</style>
