<template>
	<view>
		<!-- banner -->
		<view class="banner-box">
			<view class="banner">
				<swiper class="swiper-box" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration"
				 indicator-active-color="#537deb">
					<block v-for="(item,index) in labelData" :key="index">
						<swiper-item class="swiper-item">
							<image :src="item.url" class="slide-image" />
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
			<view class="circel-nav ilblock" @click="Router.navigateTo({route:{path:'/pages/lawyerList/lawyerList'}})">
				<image src="../../static/images/home-icon2.png"></image>
				<view class="color2 font13">律师</view>
			</view>
			<view class="circel-nav ilblock"  @click="Router.navigateTo({route:{path:'/pages/serviceList/serviceList'}})">
				<image src="../../static/images/home-icon3.png"></image>
				<view class="color2 font13">特色服务</view>
			</view>
			<view class="circel-nav ilblock" @click="Router.navigateTo({route:{path:'/pages/VIPservice/VIPservice'}})">
				<image src="../../static/images/home-icon4.png"></image>
				<view class="color2 font13">会员服务</view>
			</view>
			<view class="circel-nav ilblock" @click="Router.navigateTo({route:{path:'/pages/legalAdvice/legalAdvice'}})">
				<image src="../../static/images/home-icon5.png"></image>
				<view class="color2 font13">法律咨询</view>
			</view>
			<view class="circel-nav ilblock" @click="Router.navigateTo({route:{path:'/pages/generalAgent/generalAgent'}})">
				<image src="../../static/images/home-icon6.png"></image>
				<view class="color2 font13">普通代理</view>
			</view>
			<view class="circel-nav ilblock" @click="Router.navigateTo({route:{path:'/pages/companyProfile/companyProfile'}})">
				<image src="../../static/images/home-icon7.png"></image>
				<view class="color2 font13">公司简介</view>
			</view>
			<view class="circel-nav ilblock" @click="Router.navigateTo({route:{path:'/pages/contactUs/contactUs'}})">
				<image src="../../static/images/home-icon8.png"></image>
				<view class="color2 font13">联系我们</view>
			</view>
		</view>

		<!-- 特色服务 -->
		<view class="infor-title flexRowBetween">
			<view class="tt">特色服务</view>
			<view class="more" @click="Router.navigateTo({route:{path:'/pages/serviceList/serviceList'}})">更多&gt;</view>
		</view>
		<view class="proLis flexRowBetween">
			<view class="item-lis" v-for="(item,index) in productData" :key="index" 
			@click="Router.navigateTo({route:{path:'/pages/serviceDetails/serviceDetails?id='+item.id}})">
				<image class="img" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" alt="" />
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
			<view class="more" @click="Router.navigateTo({route:{path:'/pages/legalAdvice/legalAdvice'}})">更多&gt;</view>
		</view>
		<view class="fvzixun palr4">
			<view class="zixunLis" v-for="(item,index) in consultData" :key="index">
				<view class="one">
					<view class="photo">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
					</view>
					<view class="msg">
						<view class="name">{{item.title}}</view>
						<view class="phone">{{item.contactPhone}}</view>
					</view>
				</view>
				<view class="two">
					<image class="icon" src="../../static/images/home-icon9.png" mode=""></image>
					{{item.passage1}}
				</view>
				<view class="three">
					<img class="L-icon" src="../../static/images/home-icon10.png" alt="">
					<view class="r-answer">
						<view class="line1 flexRowBetween">
							<view class="ll">
								<image :src="item.bannerImg&&item.bannerImg[0]?item.bannerImg[0].url:''" mode=""></image>
								<view>{{item.small_title}}</view>
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
						<view class="line2">{{item.passage2}}</view>
					</view>
				</view>
				<view class="four flexRowBetween ">
					<view class="tit">{{item.label&&item.label[0]?item.label[0].title:''}}</view>
					<view class="time">{{item.create_time}}</view>
				</view>
			</view>
		</view>
		<!-- 法律咨询 end -->
		
		<view class="f5H10"></view>

		<!-- 热点资讯 -->
		<view class="infor-title flexRowBetween">
			<view class="tt">热点资讯</view>
			<view class="more" @click="Router.navigateTo({route:{path:'/pages/information/information'}})">更多&gt;</view>
		</view>
		
		<view class="newslisbox">
			<view class="newslis" v-for="(item,index) in articleData" :key="index"  
			@click="Router.navigateTo({route:{path:'/pages/informationDetails/informationDetails'}})">
				<view class="twoCt flexRowBetween">
					<view class="leftbox">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
					</view>
					<view class="cont">
						<view class="title avoidOverflow2">{{item.title}}</view>
						<view class="lable flexRowBetween">
							<view>{{item.description}}</view>
							<view class="time">{{item.create_time}}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 热点资讯 end-->

		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">小法宝</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/caseSubmit/caseSubmit'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">案件提交</view>
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
				indicatorDots: true,
				autoplay: true,
				interval: 2000,
				duration: 500,
				labelData: {},
				productData:[],
				articleData:[],
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
				consultData:[]
				
				
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getLabelData','getProductData','getArticleData','getConsultData'], self);
		},
		methods: {

			getLabelData() {
				const self = this;
				const postData = {
					searchItem: {
						thirdapp_id: 2,
						title:'首页轮播图'
					},
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData = res.info.data[0].mainImg
					}
					console.log('res', res)
					self.$Utils.finishFunc('getLabelData');
			
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getProductData() {
				const self = this;
				const postData = {
					searchItem: {
						thirdapp_id: 2,
						type:1
					},
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.productData.push.apply(self.productData,res.info.data)
					}
					console.log('res', res)
					self.$Utils.finishFunc('getProductData');
			
				};
				self.$apis.productGet(postData, callback);
			},
			
					
			getConsultData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['法律咨询']],
						},
						middleKey: 'menu_id',
						key: 'id',
						condition: 'in',
					},
				};
				postData.getAfter = {
					label: {
						tableName: 'Label',
						searchItem: {
							status:1
						},
						middleKey: 'menu_id',
						key: 'id',
						condition: 'in',
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.consultData.push.apply(self.consultData, res.info.data);
					}
					self.$Utils.finishFunc('getConsultData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			getArticleData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['热点资讯']],
						},
						middleKey: 'menu_id',
						key: 'id',
						condition: 'in',
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.articleData.push.apply(self.articleData, res.info.data);
					}
					self.$Utils.finishFunc('getArticleData');
				};
				self.$apis.articleGet(postData, callback);
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
