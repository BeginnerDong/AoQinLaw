<template>
	<view>

		<view class="caseSbmit">
			<form action="">
				<view class="eidt-line">
					<view class="ll">持卡人：</view>
					<view class="rr">
						<input type="text" placeholder="请输入持卡人姓名" v-model="submitData.bank_name">
					</view>
				</view>
				<view class="eidt-line">
					<view class="ll">手机号:</view>
					<view class="rr">
						<input type="number" placeholder="请输入手机号码"  v-model="submitData.bank_phone" maxlength="11" onkeyup="this.value=this.value.replace(/\D/g,'')" >
					</view>
				</view>
				<view class="eidt-line">
					<view class="ll">开户行：</view>
					<view class="rr">
						<input type="text" placeholder="请输入您的银行卡的开户行" v-model="submitData.bank">
					</view>
				</view>
				
				<view class="eidt-line">
					<view class="ll">银行卡号：</view>
					<view class="rr">
						<input type="number" placeholder="请输入您的银行卡号" v-model="submitData.bank_id" 
						onkeyup="this.value=this.value.replace(/\D/g,'')" >
					</view>
				</view>
				
				<view class="submitbtn" style="margin-top: 300rpx;" @click="Utils.stopMultiClick(submit)">
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
				Router:this.$Router,
				Utils:this.$Utils,
				submitData:{
					bank:'',
					bank_id:'',
					bank_name:'',
					bank_phone:''
				}
				
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		methods: {

			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
						self.submitData.bank = self.userInfoData.bank;
						self.submitData.bank_id = self.userInfoData.bank_id;
						self.submitData.bank_name = self.userInfoData.bank_name;
						self.submitData.bank_phone = self.userInfoData.bank_phone;
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getUserInfoData');	
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass)
				console.log('self.submitData',self.submitData)
				if (pass) {
					
					self.userInfoUpdate()
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			userInfoUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.$Utils.showToast('操作成功', 'none')
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 800);
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
				};
				self.$apis.userInfoUpdate(postData, callback);
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
