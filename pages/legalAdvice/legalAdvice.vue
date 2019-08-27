<template>
	<view>

		<view class="fvzixun palr4">
			<view class="zixunLis" v-for="(item,index) in zixunLis" :key="index">
				<view class="one">
					<view class="photo">
						<image :src="item.questioner_photo" ></image>
					</view>
					<view class="msg">
						<view class="name">{{item.question_name}}</view>
						<view class="phone">{{item.question_phone}}</view>
					</view>
				</view>
				<view class="two">
					<image class="icon" src="../../static/images/home-icon9.png" mode=""></image>
					{{item.question}}
				</view>
				<view class="three">
					<img class="L-icon" src="../../static/images/home-icon10.png" alt="">
					<view class="r-answer">
						<view class="line1 flexRowBetween">
							<view class="ll">
								<image :src="item.answer_photo" mode=""></image>
								<view>{{item.answer_name}}</view>
							</view>
							<view class="rr flexRowBetween starClass">
								<view class="starBox">
									<image src="../../static/images/home-icon12.png" mode=""></image>
									<image src="../../static/images/home-icon12.png" mode=""></image>
									<image src="../../static/images/home-icon12.png" mode=""></image>
									<image src="../../static/images/home-icon13.png" mode=""></image>
									<image src="../../static/images/home-icon11.png" mode=""></image>
								</view>
								<view>9.5分</view>
							</view>
						</view>
						<view class="line2">{{item.answer}}</view>
					</view>
				</view>
				<view class="four flexRowBetween ">
					<view class="tit">{{item.answer_label}}</view>
					<view class="time">{{item.answer_time}}</view>
				</view>
			</view>
		</view>
		<!-- 法律咨询 end -->

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
				zixunLis:[
					{
						questioner_photo:'../../static/images/home-img2.png',
						question_name:"张胜兰",
						question_phone:"15623020235",
						question:"问题问题问题问题问题问题问题问题问题问题问题问题问题问题",
						answer_photo:"../../static/images/tean-img.png",
						answer_name:"王律师",
						answer:"答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案",
						answer_label:"婚姻问题",
						answer_time:"2019年8月24日"
						
					},
					{
						questioner_photo:'../../static/images/tean-img.png',
						question_name:"李云海",
						question_phone:"15623028888",
						question:"2问题问题问题问题问题问题问题问题问题问题问题问题问题问题",
						answer_photo:"../../static/images/home-img2.png",
						answer_name:"刘律师",
						answer:"答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案",
						answer_label:"婚姻问题",
						answer_time:"2019年8月22日"
					},
					{
						questioner_photo:'../../static/images/home-img2.png',
						question_name:"张胜兰",
						question_phone:"15623020235",
						question:"问题问题问题问题问题问题问题问题问题问题问题问题问题问题",
						answer_photo:"../../static/images/tean-img.png",
						answer_name:"王律师",
						answer:"答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案答案",
						answer_label:"婚姻问题",
						answer_time:"2019年8月24日"
						
					}
				],
				
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
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
	@import "../../assets/style/index.css";

	page {
		padding-bottom: 60rpx;
	}
	

</style>
