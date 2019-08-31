<template>
	<view>

	<view class="caseSbmit">
		<form action="">
			<view class="eidt-line">
				<view class="ll">选择类型：</view>
				<view class="rr">
					<picker mode="selector" :range="typeData" range-key="title" @change="changeType" style="text-align: right;color:#999">
						{{submitData.description!=''?submitData.description:'请选择'}}
					</picker>
				</view>
			</view>
			<view class="eidt-line">
				<view class="ll">问题详情：</view>
				<view class="rr">
					<textarea placeholder="请输入你要咨询的问题" v-model="submitData.content"></textarea>
				</view>
			</view>
			<view class="eidt-line">
				<view class="ll">个人姓名：</view>
				<view class="rr">
					<input type="text" placeholder="请输入姓名" v-model="submitData.keywords">
				</view>
			</view>
			<view class="eidt-line">
				<view class="ll">手机号码：</view>
				<view class="rr">
					<input type="number" v-model="submitData.phone"  placeholder="请输入手机号码" maxlength="11" onkeyup="this.value=this.value.replace(/\D/g,'')" >
				</view>
			</view>
			<view class="textt">您提交的案件，律师将在24小时之内给您解答，请在法律咨询中查看，如您需要紧急处理，请进入在线咨询页面，与律师及时沟通。</view>
			<view class="submitbtn">
				<button type="submit"  @click="Utils.stopMultiClick(submit)">立即发布</button>
			</view>
		</form>
	</view>

		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">小法宝</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/caseSubmit/caseSubmit'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar2-a.png" />
				</view>
				<view class="text this-text">案件提交</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				submitData: {
					content: '',
					description: '',
					keywords: '',
					phone: '',
					type:2
				},
				typeData:[]
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			changeType(e){
				const self = this;
				console.log(e);
				self.submitData.description = self.typeData[e.detail.value].title
			},
			
			getMainData() {
				const self = this;
				const postData = {
					searchItem: {
						thirdapp_id: 2,
						title:'咨询类型'
					},
				};
				postData.getAfter= {
					child:{
						order:{
							create_time:'asc'
						},
						tableName:'Label',
						middleKey:'id',
						key:'parentid',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.typeData = res.info.data[0].child
					};
					console.log('self.typeData', self.typeData)
					self.$Utils.finishFunc('getMainData');		
				};
				self.$apis.labelGet(postData, callback);
			},

			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				var phone = self.submitData.phone;
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {
					if (phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(phone)) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('手机格式不正确', 'none')			
					} else {					
						/* const callback = (user, res) => {
							
						};
						self.$Utils.getAuthSetting(callback); */
						self.messageAdd();
					}
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
				postData.data.behavior = 1;
				postData.saveAfter = [{
					tableName: 'UserInfo',
					FuncName: 'update',
					searchItem: {
						user_no:wx.getStorageSync('user_info').user_no
					},
					data: {
						phone: self.submitData.phone,
						name:self.submitData.keywords,
					}
				}];
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('提交成功', 'none');
						self.submitData ={
							content: '',
							description: '',
							keywords: '',
							phone: '',
							type:2
						};
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
	@import "../../assets/style/user.css";

</style> 
