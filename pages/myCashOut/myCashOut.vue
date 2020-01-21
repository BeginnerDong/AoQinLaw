<template>
	<view>
		<view v-if="userInfoData.bank_id==''" class="center addBtn"  @click="Router.navigateTo({route:{path:'/pages/addBankCard/addBankCard'}})">
			<image src="../../static/images/withdrawal-icon.png" mode=""></image>
			添加银行卡
		</view>
		
		<!-- 添加银行卡后显示 -->
		<view class="flexRowBetween bankmsg addBtn" v-else  style="justify-content: space-between;">
			<view class="lft">
				{{userInfoData.bank}}
				<view class="color2 num">**** **** **** *** {{userInfoData.bank_id}}</view>
			</view>
			<view @click="Router.navigateTo({route:{path:'/pages/addBankCard/addBankCard'}})">
				<image style="width: 14rpx; height: 28rpx;" src="../../static/images/arrow-icon1.png" mode=""></image>
			</view>
		</view>
		
		<view class="editMoney mglr4">
			<view class="tit">提现金额</view>
			<view class="input">
				<input type="number" placeholder="请输入提现金额" v-model="submitData.count">
			</view>
			<view class="flexRowBetween font12 color3">
				<view>本次可提现金额{{userInfoData.balance}}元</view>
				<view style="color: #537DEB;" @click="allOut">全部提现</view>
			</view>
			<view class="submitbtn">
				<button type="button"  @click="Utils.stopMultiClick(submit)"  style="margin: 100rpx auto 30rpx auto;border-radius: 50rpx;">提现</button>
			</view>
		</view>


		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				userInfoData:{},
				submitData:{
					count:''
				}
			}
		},
		onLoad() {
			const self = this;
			
		},
		
		onShow() {
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
						self.userInfoData.bank_id = self.userInfoData.bank_id.substring(self.userInfoData.bank_id.length-4);
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getUserInfoData');
			
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			allOut() {
				const self = this;
				self.submitData.count = self.userInfoData.balance;
			},
			
			flowLogAdd() {
				const self = this
				const postData = {
					data: {
						count: -self.submitData.count,
						trade_info: '提现',
						status: 0,
						type: 2,					
						user_no: wx.getStorageSync('user_info').user_no,
						relation_user:wx.getStorageSync('user_info').user_no
					}
				};
				postData.tokenFuncName = 'getProjectToken';	
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						uni.showModal({
						    title: '申请成功',
						    content: '您的提现申请已提交，佣金将在3-5个工作日到账，请注意查收。',
							showCancel:false,
						    success: function (res) {
						        if (res.confirm) {
						            
									uni.navigateBack({
										delta:1
									})
						         
						        } else if (res.cancel) {
						            console.log('用户点击取消');
						        }
						    }
						});
					} else {
						self.$Utils.showToast(res.msg, 'none');
					}
				};
				self.$apis.flowLogAdd(postData, callback)
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass)
				console.log('self.submitData',self.submitData)
				if (pass) {
					if (self.userInfoData.bank_id=='') {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请先绑定银行卡', 'none');
						return
					};
					if (parseFloat(self.submitData.count) > parseFloat(self.userInfoData.balance)) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('佣金不足', 'none');
						return
					}
					if(parseFloat(self.submitData.count)==0){
						uni.setStorageSync('canClick', true);
						
						self.$Utils.showToast('提现金额有误', 'none');
						return
					};
					self.flowLogAdd()
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入提现金额', 'none')
				};
			},
		},
	};
</script>

<style>
	@import "../../assets/style/common.css";
	page{ background: #f5f5f5;}
	
	.bankmsg .lft{ display: flex; justify-content: flex-start; align-items: center; font-size: 30rpx;}
	.bankmsg .lft .num{ margin-left: 20rpx; font-size: 26rpx;}
	
	.addBtn{ margin: 50rpx 4% 140rpx 4%; background: #fff;border-radius: 10rpx; display: flex; align-items: center;justify-content: center; height: 100rpx; font-size: 30rpx;padding: 0 30rpx;}
	.addBtn image{ width: 40rpx; height: 40rpx; margin-right: 20rpx;}
	
	.editMoney{padding: 30rpx;background: #fff;}
	.editMoney .tit{ font-size: 28rpx; line-height: 80rpx;}
	.editMoney .input{ display: flex; justify-content: flex-start; line-height: 100rpx; margin-bottom: 10rpx;}
	.editMoney .input input{ font-size: 15px; line-height: 100rpx; display: block; width: 80%; height: 100rpx; box-sizing: border-box;}
	.editMoney .input::before{content: "￥"; font-size: 50rpx; margin-right: 10rpx;}
</style> 
