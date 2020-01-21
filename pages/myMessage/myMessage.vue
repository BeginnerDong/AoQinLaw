<template>
	<view>

		<view class="caseSbmit">
			<form action="">
				<view class="eidt-line">
					<view class="ll"><text class="red">* </text>姓名:</view>
					<view class="rr">
						<input type="text" placeholder="请输入姓名" v-model="submitData.name">
					</view>
				</view>
				<view class="eidt-line">
					<view class="ll" style="width: 30%;"><text class="red">* </text>手机号码:</view>
					<view class="rr" style="width: 65%;">
						<input type="number" v-model="submitData.phone" placeholder="请输入手机号码" maxlength="11" onkeyup="this.value=this.value.replace(/\D/g,'')" >
					</view>
				</view>
				<view class="eidt-line" v-if="!isRegister">
					<view class="ll"><text class="red">* </text>验证码:</view>
					<view class="rr" style="width: 45%;">
						<input type="number" v-model="submitData.code" placeholder="请输入验证码" onkeyup="this.value=this.value.replace(/\D/g,'')">
					</view>
					<view @click="sendCode" v-if="!hasSend" style="width:160rpx; height: 60rpx; line-height: 60rpx;text-align: center; border-radius: 10rpx; color: #fff;background: #537DEB; font-size: 24rpx;">
						{{text}}
					</view>
					<view  v-else style="width:160rpx; height: 60rpx; line-height: 60rpx;text-align: center; border-radius: 10rpx; color: #fff;background: #537DEB; font-size: 24rpx;">
						{{text}}
					</view>
				</view>
				<view class="eidt-line">
					<view class="ll" style="width: 36%;"><text class="red">* </text>推荐人手机号码:</view>
					<view class="rr" style="width: 62%;">
						<input type="number" v-model="submitData.passage1" placeholder="请输入推荐人的手机号码(选填)" maxlength="11" onkeyup="this.value=this.value.replace(/\D/g,'')" >
					</view>
				</view>
				<view class="submitbtn" style="margin-top: 300rpx;" v-if="!isRegister">
					<button type="submit" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)" style="margin-bottom: 30rpx;">确定</button>
				</view>
				<view v-if="!isRegister" class="textt" style="padding: 0px 15% 50px 15%;">首次注册时请写推荐人手机号，您的首次消费将给您的推荐人进行返佣</view>
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
					name:'',
					phone:'',
					code:'',
					passage1:'',
				},
				currentTime:61,
				text:'获取验证码',
				hasSend:false,
				isRegister:true
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			
			getMainData() {
				const self = this;
				const postData = {
					searchItem: {
						thirdapp_id: 2,
					},
					tokenFuncName:'getProjectToken'
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.submitData.name = res.info.data[0].name,
						self.submitData.phone = res.info.data[0].phone,
						self.submitData.passage1 = res.info.data[0].passage1
						if(self.submitData.passage1==''){
							self.isRegister = false
						}
					};
					self.$Utils.finishFunc('getMainData');		
				};
				self.$apis.userInfoGet(postData, callback);
			},
		
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				var phone = self.submitData.phone;
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.passage1;
				const pass = self.$Utils.checkComplete(newObject);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {
					if (phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(phone)) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('手机格式不正确', 'none')			
					} else {
						if(self.submitData.phone==self.submitData.passage1){
							uni.setStorageSync('canClick', true);
							self.$Utils.showToast('禁止设置自己为上级', 'none');
							return
						};
						const callback = (user, res) => {
							self.userInfoUpdate();
						};
						self.$Utils.getAuthSetting(callback);
						
					}
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			sendCode(){
				var self = this;
				console.log(111)
				if(self.hasSend){
					return;
				};
				var phone = self.submitData.phone;
				
				if (phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(phone)) {
					
					self.$Utils.showToast('请输入正确的手机号', 'none');
					return;
				}
				var postData = {
					params:{
					  PhoneNumbers:self.submitData.phone,
					  TemplateCode:"SMS_173476603",
						SignName:"澳秦功成小法宝法律服务"
					 }
					
				};
				var callback = function(res){
					if(res.solely_code==100000){
						self.hasSend = true;
						var interval = setInterval(function() {
							self.currentTime--; //每执行一次让倒计时秒数减一
						
							self.text=self.currentTime + 's';//按钮文字变成倒计时对应秒数
							
							//如果当秒数小于等于0时 停止计时器 且按钮文字变成重新发送 且按钮变成可用状态 倒计时的秒数也要恢复成默认秒数 即让获取验证码的按钮恢复到初始化状态只改变按钮文字
							if (self.currentTime <= 0) {
								clearInterval(interval)
								
								self.hasSend = false;
								self.text='重新发送';
								self.currentTime= 61;
								
							}
							
						}, 1000);
					}else{
						self.$Utils.showToast('发送失败', 'none');
					};
				};
				self.$apis.codeGet(postData, callback);
			},
					
			userInfoUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				/* if(!wx.getStorageSync('user_info')||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				}; */
				postData.data = {
					phone:self.submitData.phone,
					name:self.submitData.name,
					passage1:self.submitData.passage1
				};
				/* if(self.isRegister){
					delete postData.data.phone
				}; */
				postData.smsAuth = {
					phone:self.submitData.phone,						
					code:self.submitData.code					,
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('提交成功', 'none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 800);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
		},
	};
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/user.css";

</style> 
