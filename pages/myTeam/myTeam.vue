<template>
	<view>
		<view class="teamTop">
			<view class="money">2635.00</view>
			<view class="yuan">总金额(元)</view>
			<view class="txBtn" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myCashOut/myCashOut'}})">提现</view>
			<!-- myCashOut -->
		</view>
		
		<view class="orderNav pdlr4" style="background: #f5f5f5;">
			<view class="tt" :class="num==1?'on':''" @click="change('1')">奖励记录</view>
			<view class="tt" :class="num==2?'on':''" @click="change('2')">团队成员</view>
		</view>
		
		<view class="teamBox1" v-if="num==1">
			<view class="item flexRowBetween" v-for="(item,index) in rewardData" :key="index">
				<view class="left flexRowBetween">
					<view class="photo">
						<image :src="item.photoUrl"></image>
					</view>
					<view class="cont">
						<view>{{item.name}}</view>
						<view class="data red">{{item.time}}</view>
					</view>
				</view>
				<view class="right red">{{item.price}}</view>
			</view>
		</view>
		<view class="teamBox1" v-if="num==2">
			<view class="item flexRowBetween" v-for="(item,index) in memberDta" :key="index">
				<view class="left flexRowBetween">
					<view class="photo">
						<image :src="item.photoUrl"></image>
					</view>
					<view class="cont">
						<view>{{item.name}}</view>
					</view>
				</view>
				<view class="right" style="color: #666; font-size: 26rpx;">{{item.time}}</view>
			</view>
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
				num:1,
				rewardData:[
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
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			change(num){
				const self = this;
				if(num!= self.num){
					self.num = num
				}
			},
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
	.teamTop{width: 100%; height: 360rpx; background: #537DEB; color: #FFF;text-align: center;}
	.teamTop .money{font-size: 80rpx; font-weight: bold; padding: 60rpx 4% 10rpx 4%; line-height: 100rpx;}
	.teamTop .yuan{ font-size:28rpx; margin-bottom: 40rpx;}
	.teamTop .txBtn{ width: 120rpx; height: 44rpx;line-height: 44rpx; background: #fff; color: #333; border-radius: 6rpx; margin: 0 auto;font-size: 26rpx;}
	
	.orderNav .tt{ width: 50%;}
	.teamBox1 .item{ padding: 30rpx 4%;border-bottom: 2rpx solid #e7e7e7;}
	.teamBox1 .item .photo{width: 100rpx; height: 100rpx; border-radius: 50%; overflow: hidden;}
	.teamBox1 .item .photo image{ width: 100%; height: 100%;}
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
