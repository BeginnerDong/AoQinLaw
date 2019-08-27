<template>
	<view>

		<!-- 律师列表 -->
		<view class="lawyerList mglr4">
			<view class="info"  v-for="(item,index) in lawyerList" :key="index" @click="webSelf.$Router.navigateTo({route:{path:'/pages/lawyerDetail/lawyerDetail'}})">
				<view class="info-left">
					<image :src="item.photoImg"></image>
				</view>
				<view class="info-right">
					<view class="name">{{item.name}}<view class="lable">诉讼律师</view></view>
					<view class="two">
						<image class="icon" src="../../static/images/lvshi-icon2.png" ></image>
						执业{{item.workTime}}
						<view class="flexRowBetween starClass" style="margin-left: 10rpx;">
							<view class="starBox">
								<image src="../../static/images/home-icon12.png" mode=""></image>
								<image src="../../static/images/home-icon12.png" mode=""></image>
								<image src="../../static/images/home-icon12.png" mode=""></image>
								<image src="../../static/images/home-icon13.png" mode=""></image>
								<image src="../../static/images/home-icon11.png" mode=""></image>
							</view>
							<view>{{item.score}}分</view>
						</view>
						<img class="arrow" src="../../static/images/arrow-icon1.png" alt="">
					</view>
					<view class="three">
						<block v-for="child in item.c_item" :key="index">
							<view class="info-item">{{child}}</view>
						</block>
					</view>
				</view>
			</view>
		</view>
		<!-- 律师列表 end -->


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
				lawyerList:[
					{
						photoImg:'../../static/images/lvshi-img1.png',
						name:"张胜男",
						lable:"诉讼律师",
						workTime:"5年",
						score:"8.5",
						c_item:["刑事诉讼","债权债务"]
					},
					{
						photoImg:'../../static/images/lvshi-img1.png',
						name:"李海海",
						lable:"诉讼律师",
						workTime:"5年",
						score:"9.5",
						c_item:["人身损害","债权债务","财产纠纷","家庭纠纷","财产纠纷"]
					},
					{
						photoImg:'../../static/images/lvshi-img1.png',
						name:"刘某某",
						lable:"诉讼律师",
						workTime:"5年",
						score:"8.5",
						c_item:["刑事诉讼","债权债务"]
					},
					{
						photoImg:'../../static/images/lvshi-img1.png',
						name:"李海海",
						lable:"诉讼律师",
						workTime:"5年",
						score:"9.5",
						c_item:["人身损害","债权债务","财产纠纷","家庭纠纷","财产纠纷"]
					}
				]
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
	@import "../../assets/style/lawyer.css";
	page{ background: #F5F5F5;padding-bottom: 60rpx;}




</style>
