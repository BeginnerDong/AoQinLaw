<template>
	<view>
		<!-- banner -->
		<view class="banner-box">
			<view class="banner">
				<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="interval" duration="duration"
				 indicator-active-color="#537deb">
					<block v-for="(item,index) in labelData" :key="index">
						<swiper-item class="swiper-item">
							<image :src="item" class="slide-image" />
						</swiper-item>
					</block>
				</swiper>
			</view>
		</view>

		<!-- 八个导航 -->
		<view class="indHome">
			<view class="circel-nav ilblock">
				<image src="../../static/images/home-icon1.png"></image>
				<view class="color2 font13">在线咨询</view>
			</view>
			<view class="circel-nav ilblock" @click="webSelf.$Router.navigateTo({route:{path:'/pages/lawyerList/lawyerList'}})">
				<image src="../../static/images/home-icon2.png"></image>
				<view class="color2 font13">律师</view>
			</view>
			<view class="circel-nav ilblock"  @click="webSelf.$Router.navigateTo({route:{path:'/pages/serviceList/serviceList'}})">
				<image src="../../static/images/home-icon3.png"></image>
				<view class="color2 font13">特色服务</view>
			</view>
			<view class="circel-nav ilblock" @click="webSelf.$Router.navigateTo({route:{path:'/pages/VIPservice/VIPservice'}})">
				<image src="../../static/images/home-icon4.png"></image>
				<view class="color2 font13">会员服务</view>
			</view>
			<view class="circel-nav ilblock" @click="webSelf.$Router.navigateTo({route:{path:'/pages/legalAdvice/legalAdvice'}})">
				<image src="../../static/images/home-icon5.png"></image>
				<view class="color2 font13">法律咨询</view>
			</view>
			<view class="circel-nav ilblock" @click="webSelf.$Router.navigateTo({route:{path:'/pages/generalAgent/generalAgent'}})">
				<image src="../../static/images/home-icon6.png"></image>
				<view class="color2 font13">普通代理</view>
			</view>
			<view class="circel-nav ilblock" @click="webSelf.$Router.navigateTo({route:{path:'/pages/companyProfile/companyProfile'}})">
				<image src="../../static/images/home-icon7.png"></image>
				<view class="color2 font13">公司简介</view>
			</view>
			<view class="circel-nav ilblock" @click="webSelf.$Router.navigateTo({route:{path:'/pages/contactUs/contactUs'}})">
				<image src="../../static/images/home-icon8.png"></image>
				<view class="color2 font13">联系我们</view>
			</view>
		</view>

		<!-- 特色服务 -->
		<view class="infor-title flexRowBetween">
			<view class="tt">特色服务</view>
			<view class="more" @click="webSelf.$Router.navigateTo({route:{path:'/pages/serviceList/serviceList'}})">更多&gt;</view>
		</view>
		<view class="proLis flexRowBetween">
			<view class="item-lis" v-for="(item,index) in produtList" :key="index" @click="webSelf.$Router.navigateTo({route:{path:'/pages/serviceDetails/serviceDetails'}})">
				<image class="img" :src="item.picUl" alt="" />
				<view class="tit avoidOverflow">{{item.title}}</view>
				<view class="price">{{item.price}}</view>
				<view class="freeVip">会员免费</view>
			</view>
		</view>
		<!-- 特色服务 end -->
		<view class="f5H10"></view>

		<!-- 法律咨询 -->
		<view class="infor-title flexRowBetween">
			<view class="tt">法律咨询</view>
			<view class="more" @click="webSelf.$Router.navigateTo({route:{path:'/pages/legalAdvice/legalAdvice'}})">更多&gt;</view>
		</view>
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
		
		<view class="f5H10"></view>

		<!-- 热点资讯 -->
		<view class="infor-title flexRowBetween">
			<view class="tt">热点资讯</view>
			<view class="more">更多&gt;</view>
		</view>
		
		<view class="newslisbox">
			<view class="newslis" v-for="(item,index) in newslis" :key="index">
				<view class="twoCt flexRowBetween">
					<view class="leftbox">
						<image :src="item.imgUrl"></image>
					</view>
					<view class="cont">
						<view class="title avoidOverflow2">{{item.title}}</view>
						<view class="lable flexRowBetween">
							<view>{{item.lable}}</view>
							<view class="time">{{item.time}}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 热点资讯 end-->

		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">小法宝</view>
			</view>
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/caseSubmit/caseSubmit'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">案件提交</view>
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
				
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png",
				],
				produtList: [
					{
						picUl: "../../static/images/home-img1.png",
						title: "1法律咨询",
						price: "56.00"
					},
					{
						picUl: "../../static/images/home-banner.png",
						title: "2法律咨询",
						price: "66.00"
					},
					{
						picUl: "../../static/images/profile-img.png",
						title: "法律咨询",
						price: "56.00"
					},
					{
						picUl: "../../static/images/home-img1.png",
						title: "法律咨询",
						price: "56.00"
					},
					{
						picUl: "../../static/images/details-img2.png",
						title: "产品标题1",
						price: "56.00"
					}
				],
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
				newslis:[
					{
						imgUrl:"../../static/images/home-img3.png",
						title:"标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题",
						lable:"婚姻问题",
						time:"2019年8月24日"
					},
					{
						imgUrl:"../../static/images/home-img1.png",
						title:"2标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题",
						lable:"婚姻问题",
						time:"2019年8月24日"
					},
					{
						imgUrl:"../../static/images/home-img3.png",
						title:"标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题",
						lable:"婚姻问题",
						time:"2019年8月24日"
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
	@import "../../assets/style/service.css";

	page {
		padding-bottom: 140rpx;
	}
	

</style>
