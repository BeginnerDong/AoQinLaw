<template>
	<view>

		<view class="userHead">
			<view class="flexRowBetween">
				<view class="left">
					<view style="width: 120rpx;height: 120rpx;border-radius:50%;overflow: hidden;margin-right: 20rpx;">
						<open-data type="userAvatarUrl"></open-data>
					</view>
					<open-data type="userNickName"></open-data>
				</view>
				<view class="rr" @click="Router.navigateTo({route:{path:'/pages/myMessage/myMessage'}})">我的信息 &gt;</view>
			</view>
		</view>
		
		<view class="userBox2 boxShaow">
			<view class="flexRowBetween tit">
				<view>我的订单</view>
				<view class="more" @click="Router.navigateTo({route:{path:'/pages/myOrder/myOrder'}})">全部订单 &gt;</view>
			</view>
			<view class="menu flexRowBetween">
				<view class="child" @click="Router.navigateTo({route:{path:'/pages/myOrder/myOrder?current=1'}})">
					<image src="../../static/images/about-icon1.png"></image>
					<view>未支付</view>
				</view>
				<view class="child" @click="Router.navigateTo({route:{path:'/pages/myOrder/myOrder?current=2'}})">
					<image src="../../static/images/about-icon2.png"></image>
					<view>已支付</view>
				</view>
				<view class="child" @click="Router.navigateTo({route:{path:'/pages/myOrder/myOrder?current=3'}})">
					<image src="../../static/images/about-icon3.png"></image>
					<view>已完成</view>
				</view>
			</view>
		</view>
		
		<view class="XlineNav">
			<view class="info bg1" v-if="mainData.order&&mainData.order.length==0"  @click="Router.navigateTo({route:{path:'/pages/myVIP/myVIP'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon4.png"></image>
				</view>
				<view class="ilblock">我的会员</view>
				<image class="arrow" src="../../static/images/arrow-icon1.png" ></image>
			</view>
			<view class="info bg1" v-if="mainData.order&&mainData.order.length>0"  @click="Router.navigateTo({route:{path:'/pages/myVIPcard/myVIPcard'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon4.png"></image>
				</view>
				<view class="ilblock">我的会员</view>
				<image class="arrow" src="../../static/images/arrow-icon1.png" ></image>
			</view>
			<view class="info bg1" @click="Router.navigateTo({route:{path:'/pages/myConsult/myConsult'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon5.png"></image>
				</view>
				<view class="ilblock">我的咨询</view>
				<image class="arrow" src="../../static/images/arrow-icon1.png" ></image>
			</view>
			<view class="info bg1" @click="Router.navigateTo({route:{path:'/pages/myCase/myCase'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon6.png"></image>
				</view>
				<view class="ilblock">我的案件</view>
				<image class="arrow" src="../../static/images/arrow-icon1.png" ></image>
			</view>
			<view class="info bg1" @click="Router.navigateTo({route:{path:'/pages/feedback/feedback'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon7.png"></image>
				</view>
				<view class="ilblock">意见反馈</view>
				<image class="arrow" src="../../static/images/arrow-icon1.png" ></image>
			</view>
			<view class="info bg1" @click="Router.navigateTo({route:{path:'/pages/myTeam/myTeam'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon8.png"></image>
				</view>
				<view class="ilblock">我的团队</view>
				<image class="arrow" src="../../static/images/arrow-icon1.png" ></image>
			</view>
		</view>

		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">小法宝</view>
			</view>
			<view class="navbar_item"  @click="Router.redirectTo({route:{path:'/pages/caseSubmit/caseSubmit'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">案件提交</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">我的</view>
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
				mainData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {

			getMainData() {
				const self = this;
				console.log('852369')
				var now = Date.parse(new Date())/1000;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.getAfter = {
					order:{
						tableName:'Order',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1,
							invalid_time:['>',now],
							pay_status: 1,
							type:2
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');		
				};
				self.$apis.userInfoGet(postData, callback);
			},

		},
	};
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/user.css";
	

</style>
