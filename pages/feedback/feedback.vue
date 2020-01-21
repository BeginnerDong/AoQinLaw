<template>
	<view class="pdlr4">
		<form>
			<view class="infor">
				<view class="tit">标题：</view>
				<view class="edit">
					<input type="text" placeholder="请输入您的称呼和联系方式" v-model="submitData.title">
				</view>
			</view>
			<view class="infor">
				<view class="tit">内容：</view>
				<view class="edit">
					<textarea value="" placeholder="请输入您反馈的内容" v-model="submitData.content"/>
				</view>
			</view>
			<view class="submitbtn" style="margin:150rpx auto 100rpx auto">
				<button type="button" @click="Utils.stopMultiClick(submit)">提交</button>
			</view>
			
		</form>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				submitData:{
					title:'',
					content:'',
					type:3
				}
			}
		},
		
		onLoad() {
			const self = this;
			uni.setStorageSync('canClick', true);
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {

			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				var phone = self.submitData.phone;
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {
					self.messageAdd();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
					
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				/* if(!wx.getStorageSync('user_info')||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				}; */
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						
						uni.showModal({
						    title: '提交成功',
						    content: '感谢您的宝贵意见，您的反馈我们将在研究之后做出相应提升。',
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
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.messageAdd(postData, callback);
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
