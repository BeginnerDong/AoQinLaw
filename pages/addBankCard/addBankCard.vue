<template>
	<view>

		<view class="caseSbmit">
			<form action="">
				<view class="eidt-line">
					<view class="ll">持卡人：</view>
					<view class="rr">
						<input type="text" placeholder="请输入持卡人姓名">
					</view>
				</view>
				<view class="eidt-line">
					<view class="ll">手机号:</view>
					<view class="rr">
						<input type="number" placeholder="请输入手机号码" maxlength="11" onkeyup="this.value=this.value.replace(/\D/g,'')" >
					</view>
				</view>
				<view class="eidt-line">
					<view class="ll">开户行：</view>
					<view class="rr">
						<input type="text" placeholder="请输入您的银行卡的开户行" >
					</view>
				</view>
				
				<view class="eidt-line">
					<view class="ll">银行卡号：</view>
					<view class="rr">
						<input type="number" placeholder="请输入您的银行卡号" maxlength="11" onkeyup="this.value=this.value.replace(/\D/g,'')" >
					</view>
				</view>
				
				<view class="submitbtn" style="margin-top: 300rpx;" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myCashOut/myCashOut'}})">
					<button type="submit" >确定</button>
				</view>
			</form>
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
	.caseSbmit .eidt-line .ll{ padding-right: 0;}
	.caseSbmit .eidt-line .rr input{font-size: 24rpx;}
</style> 
