<template>
	<view>

	<view class="caseSbmit">
		<form action="">
			<view class="eidt-line">
				<view class="ll">选择类型：</view>
				<view class="rr">
					<input type="text" placeholder="请选择">
				</view>
			</view>
			<view class="eidt-line">
				<view class="ll">问题详情：</view>
				<view class="rr">
					<textarea placeholder="请输入你要咨询的问题"></textarea>
				</view>
			</view>
			<view class="eidt-line">
				<view class="ll">个人姓名：</view>
				<view class="rr">
					<input type="text" placeholder="请输入姓名">
				</view>
			</view>
			<view class="eidt-line">
				<view class="ll">手机号码：</view>
				<view class="rr">
					<input type="number" placeholder="请输入手机号码" maxlength="11" onkeyup="this.value=this.value.replace(/\D/g,'')" >
				</view>
			</view>
			<view class="textt">您提交的案件，律师将在24小时之内给您解答，请在法律咨询中查看，如您需要紧急处理，请进入在线咨询页面，与律师及时沟通。</view>
			<view class="submitbtn">
				<button type="submit">立即发布</button>
			</view>
		</form>
	</view>

		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">小法宝</view>
			</view>
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/caseSubmit/caseSubmit'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar2-a.png" />
				</view>
				<view class="text this-text">案件提交</view>
			</view>
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/user/user'}})">
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

</style> 
