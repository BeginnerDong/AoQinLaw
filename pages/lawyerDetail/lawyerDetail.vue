<template>

		<!-- 律师详情 -->
	<view class="lawyerDatail">
		
		<view class="lawyerList ">
			<view class="info pr"  v-for="(item,index) in lawyerList" :key="index" style="margin-top: 0;">
				<view class="info-left">
					<image :src="item.photoImg"></image>
				</view>
				<view class="info-right">
					<view class="name">{{item.name}}<view class="lable">诉讼律师</view></view>
					<view class="two" style="margin-top: 40rpx;padding-right: 0;">
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
					</view>
					<view class="three" style="margin-top: 40rpx;">
						<block v-for="child in item.c_item">
							<view class="info-item">{{child}}</view>
						</block>
					</view>
				</view>
				<view class="rr_car" @click="webSelf.$Router.navigateTo({route:{path:'/pages/lawyerCard/lawyerCard'}})">
					<image src="../../static/images/lawyer%20details-icon1.png" mode=""></image>
					<view>名片</view>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		<view class="seviseDetalNav flexRowBetween pdlr4">
			<view :class="num==1? 'on':''" @click="change('1')">简介</view>
			<view :class="num==2? 'on':''" @click="change('2')">口碑评价</view>
		</view>
		
		<view class="seviseDetaCont2 pdlr4" v-if="num==1" style="padding-top: 30rpx;">
			<view>在经办的每个案件中，都力求到竭尽全力，用最专业的角度和负责任的态度去完成每一个案件中，能为当事人争取到最好的结果为当事人维护合法权益。</view>
			<view>在经办的每个案件中，都力求到竭尽全力，用最专业的角度和负责任的态度去完成每一个案件中，能为当事人争取到最好的结果为当事人维护合法权益。</view>
			<view>在经办的每个案件中，都力求到竭尽全力，用最专业的角度和负责任的态度去完成每一个案件中，能为当事人争取到最好的结果为当事人维护合法权益。</view>
			<view>
				<image src="../../static/images/home-img1.png" alt=""/>
			</view>
		</view>
		
		<!-- 口碑评价内容 -->
		<view class="pingjiabox pdlr4" v-if="num==2" >
			<view class="list">
				<view class="left-pic">
					<image src="../../static/images/home-img2.png"></image>
				</view>
				<view class="flexRowBetween">
					<view class="name overflow1">张胜男</view>
					<view class="font9 color3">2019.08.26</view>
				</view>
				<view class="cont avoidOverflow3">内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。</view>
			</view>
			<view class="list">
				<view class="left-pic">
					<image src="../../static/images/home-img2.png"></image>
				</view>
				<view class="flexRowBetween">
					<view class="name overflow1">张胜男</view>
					<view class="font9 color3">2019.08.26</view>
				</view>
				<view class="cont avoidOverflow3">内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。</view>
			</view>
			<view class="list">
				<view class="left-pic">
					<image src="../../static/images/home-img2.png"></image>
				</view>
				<view class="flexRowBetween">
					<view class="name overflow1">张胜男</view>
					<view class="font9 color3">2019.08.26</view>
				</view>
				<view class="cont avoidOverflow3">内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。</view>
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
				num:1,
				wx_info:{},
				lawyerList:[
					{
						photoImg:'../../static/images/lvshi-img1.png',
						name:"张胜男",
						lable:"诉讼律师",
						workTime:"5年",
						score:"8.5",
						c_item:["刑事诉讼","债权债务"]
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
				if(num!=self.num){
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
	@import "../../assets/style/lawyer.css";
	@import "../../assets/style/service.css";
	page{padding-bottom: 60rpx;}

	
</style>
