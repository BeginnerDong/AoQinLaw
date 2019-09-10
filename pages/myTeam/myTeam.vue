<template>
	<view>
		<view class="teamTop">
			<view class="money">{{userInfoData.balance}}</view>
			<view class="yuan">总金额(元)</view>
			<view class="txBtn" @click="Router.navigateTo({route:{path:'/pages/myCashOut/myCashOut'}})">提现</view>
			<!-- myCashOut -->
		</view>
		
		<view class="orderNav pdlr4" style="background: #f5f5f5;">
			<view class="tt" :class="num==1?'on':''" @click="change('1')">奖励记录</view>
			<view class="tt" :class="num==2?'on':''" @click="change('2')">团队成员</view>
		</view>
		
		<view class="teamBox1" v-if="num==1">
			<view class="item flexRowBetween" v-for="(item,index) in rewardData" :key="index">
				<view class="left flexRowBetween" style="width: 50%;">
					<view class="photo">
						<image :src="item.user.headImgUrl"></image>
					</view>
					<view class="cont">
						<view>{{item.user.nickname}}</view>
						<view class="data red">{{item.create_time}}</view>
					</view>
				</view>
				<view class="right red">{{item.count}}</view>
			</view>
		</view>
		<view class="teamBox1" v-if="num==2">
			<view class="item flexRowBetween" v-for="(item,index) in teamData" :key="index">
				<view class="left flexRowBetween">
					<view class="photo">
						<image :src="item.user.headImgUrl"></image>
					</view>
					<view class="cont">
						<view>{{item.user.nickname}}</view>
					</view>
				</view>
				<view class="right" style="color: #666; font-size: 26rpx;">{{item.create_time}}</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				userInfoData:{},
				num:1,
				rewardData:[],
				teamData:[],
				rewardData1:[
					{
						photoUrl:"../../static/images/tean-img.png",
						name:"昵称昵称昵称",
						time:"2019.08.27",
						price:"+23"
					},
					{
						photoUrl:"../../static/images/tean-img.png",
						name:"昵称昵称昵称",
						time:"2019.08.27",
						price:"+23"
					},
					{
						photoUrl:"../../static/images/tean-img.png",
						name:"昵称昵称昵称",
						time:"2019.08.27",
						price:"+23"
					}
				],
				memberDta:[
					{
						photoUrl:"../../static/images/tean-img.png",
						name:"昵称昵称昵称",
						time:"2019.08.27"
					},
					{
						photoUrl:"../../static/images/tean-img.png",
						name:"昵称昵称昵称",
						time:"2019.08.27"
					},
					{
						photoUrl:"../../static/images/tean-img.png",
						name:"昵称昵称昵称",
						time:"2019.08.27"
					}
				]
			}
		},
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.paginateTwo = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getUserInfoData','getRewardData','getTeamData'], self);
		},
		methods: {
			
			change(num){
				const self = this;
				if(num!= self.num){
					self.num = num
				}
			},
			
			onReachBottom() {
				console.log('onReachBottom')
				const self = this;
				if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {				
					if(self.num==1){
						self.paginate.currentPage++;
						self.getRewardData()
					}else{
						self.paginateTwo.currentPage++;
						self.getTeamData()
					}
				};
			},
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getUserInfoData');

				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			
			
			getTeamData() {
				const self = this;
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginateTwo);
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					parent_no:wx.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					user:{
						tableName:'User',
						middleKey:'child_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['nickname','headImgUrl']
					}
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {			
						if (res.info.data.length > 0) {
							self.teamData.push.apply(self.teamData, res.info.data);
						}
					} else {
						self.$Utils.showToast('网络故障', 'none')
					};
					self.$Utils.finishFunc('getTeamData');
				};
				self.$apis.distriGet(postData, callback);
			},
			
			getRewardData() {
				const self = this;
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					type:2
				};
				postData.getAfter = {
					user:{
						tableName:'User',
						middleKey:'relation_user',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['nickname','headImgUrl']
					}
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {			
						if (res.info.data.length > 0) {
							self.rewardData.push.apply(self.rewardData, res.info.data);
						}
					} else {
						self.$Utils.showToast('网络故障', 'none')
					};
					self.$Utils.finishFunc('getRewardData');
				};
				self.$apis.flowLogGet(postData, callback);
			},
			
			

		},
	};
</script>

<style>
	@import "../../assets/style/common.css";
	.teamTop{width: 100%; height: 360rpx; background: #537DEB; color: #FFF;text-align: center;}
	.teamTop .money{font-size: 80rpx; font-weight: bold; padding: 60rpx 4% 10rpx 4%; line-height: 100rpx;}
	.teamTop .yuan{ font-size:28rpx; margin-bottom: 40rpx;}
	.teamTop .txBtn{ width: 120rpx; height: 44rpx;line-height: 44rpx; background: #fff; color: #333; border-radius: 6rpx; margin: 0 auto;font-size: 26rpx;}
	
	.orderNav .tt{ width: 50%;}
	.teamBox1 .item{ padding: 30rpx 4%;border-bottom: 2rpx solid #e7e7e7;}
	.teamBox1 .item .photo{width: 100rpx; height: 100rpx;}
	.teamBox1 .item .photo image{ width: 100%; height: 100%; border-radius: 50%;}
	.teamBox1 .item .cont{
		margin-left: 20rpx;
		width: 350rpx;
		font-size: 28rpx;
	}
	.teamBox1 .item .cont .data{
		font-size: 26rpx;
		color: #666;
		margin-top: 20rpx;
	}
	.teamBox1 .item .right{
		font-size: 30rpx;
	}
	
</style>
