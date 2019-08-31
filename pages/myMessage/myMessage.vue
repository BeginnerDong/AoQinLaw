<template>
	<view>

		<view class="caseSbmit">
			<form action="">
				<view class="eidt-line">
					<view class="ll"><text class="red">* </text>姓名:</view>
					<view class="rr">
						<input type="text" placeholder="请输入姓名">
					</view>
				</view>
				<view class="eidt-line">
					<view class="ll" style="width: 30%;"><text class="red">* </text>手机号码:</view>
					<view class="rr" style="width: 65%;">
						<input type="number" placeholder="请输入手机号码" maxlength="11" onkeyup="this.value=this.value.replace(/\D/g,'')" >
					</view>
				</view>
				<view class="eidt-line">
					<view class="ll"><text class="red">* </text>验证码:</view>
					<view class="rr" style="width: 45%;">
						<input type="number" placeholder="请输入验证码" onkeyup="this.value=this.value.replace(/\D/g,'')">
					</view>
					<view style="width:160rpx; height: 60rpx; line-height: 60rpx;text-align: center; border-radius: 10rpx; color: #fff;background: #537DEB; font-size: 24rpx;">获取验证码</view>
				</view>
				<view class="eidt-line">
					<view class="ll" style="width: 36%;"><text class="red">* </text>推荐人手机号码:</view>
					<view class="rr" style="width: 62%;">
						<input type="number" placeholder="请输入推荐人的手机号码(选填)" maxlength="11" onkeyup="this.value=this.value.replace(/\D/g,'')" >
					</view>
				</view>
				<view class="submitbtn" style="margin-top: 300rpx;">
					<button type="submit" style="margin-bottom: 30rpx;">确定</button>
				</view>
				<view class="textt" style="padding: 0px 15% 50px 15%;">首次注册时请写推荐人手机号，您的首次消费将给您的推荐人进行返佣</view>
			</form>
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
				wx_info:{},
				
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
	@import "../../assets/style/user.css";

</style> 
