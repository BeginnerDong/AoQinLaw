<template>
	<view class="pdlr4">
		<form>
			<view class="infor">
				<view class="tit">标题：</view>
				<view class="edit">
					<input type="text" placeholder="请输入姓名">
				</view>
			</view>
			<view class="infor">
				<view class="tit">内容：</view>
				<view class="edit">
					<textarea value="" placeholder="请输入您想要反馈的内容" />
				</view>
			</view>
			<view class="submitbtn" style="margin:150rpx auto 100rpx auto">
				<button type="button">提交</button>
			</view>
			
		</form>
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
	.infor{padding: 30rpx 0;}
	.infor .tit{ font-size:28rpx; line-height: 50rpx; padding-bottom: 20rpx;}
	.infor input{ width: 100%;height: 60rpx; padding: 0rpx 20rpx; line-height: 60rpx; font-size: 24rpx; display: block;background: #f2f2f2;box-sizing: border-box;}
	.infor textarea{ width: 100%;height: 390rpx; padding: 10rpx 20rpx; line-height: 44rpx; font-size: 24rpx; display: block;background: #f2f2f2;box-sizing: border-box;}
</style> 
