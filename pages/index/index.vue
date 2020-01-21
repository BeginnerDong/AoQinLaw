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
		<view class="f5H10"></view>
		<swiper class="swiper-box1" autoplay="true" interval="5000" duration="1000" vertical="true">
			<block v-for="(item,index) in newsData" :key="index">
				<swiper-item class="swiper-item avoidOverflow" style="line-height: 90rpx;font-size:14px;width: 94%;padding: 0 10px;">
					{{item.description}}
				</swiper-item>
			</block>
		</swiper>
		<view class="f5H10"></view>	
		<!-- 八个导航 -->
		<view class="indHome" style="border-bottom: none;">
			<view class="circel-nav ilblock" @click="Router.redirectTo({route:{path:'/pages/caseSubmit/caseSubmit'}})" >
				<image src="../../static/images/home-icon2.png"></image>
				<view class="color2 font13">法律咨询</view>
			</view>
			<button class="circel-nav ilblock" v-if="userInfoData.order&&userInfoData.order.length>0" open-type="contact" style="outline: none;line-height: 1.3;overflow: initial;background: #fff;border: none;">
				<image src="../../static/images/home-icon1.png"></image>
				<view class="color2 font13">会员专线</view>
			</button>
			<view class="circel-nav ilblock" @click="showToast" v-if="userInfoData.order&&userInfoData.order.length==0">
				<image src="../../static/images/home-icon1.png"></image>
				<view class="color2 font13">会员专线</view>
			</view>
			<view class="circel-nav ilblock"  @click="Router.navigateTo({route:{path:'/pages/serviceList/serviceList'}})">
				<image src="../../static/images/home-icon3.png"></image>
				<view class="color2 font13">法律服务</view>
			</view>
			<view class="circel-nav ilblock" @click="Router.navigateTo({route:{path:'/pages/VIPservice/VIPservice'}})">
				<image src="../../static/images/home-icon4.png"></image>
				<view class="color2 font13">开通会员</view>
			</view>
			<view class="circel-nav ilblock" @click="Router.navigateTo({route:{path:'/pages/companyProfile/companyProfile'}})">
				<image src="../../static/images/home-icon5.png"></image>
				<view class="color2 font13">合同范本</view>
			</view>
			<view class="circel-nav ilblock" @click="Router.navigateTo({route:{path:'/pages/generalAgent/generalAgent'}})">
				<image src="../../static/images/home-icon6.png"></image>
				<view class="color2 font13">法律顾问</view>
			</view>
			<view class="circel-nav ilblock" @click="Router.navigateTo({route:{path:'/pages/lawyerList/lawyerList'}})">
				<image src="../../static/images/home-icon7.png"></image>
				<view class="color2 font13">合作律师</view>
			</view>
			<view class="circel-nav ilblock" @click="Router.navigateTo({route:{path:'/pages/contactUs/contactUs'}})">
				<image src="../../static/images/home-icon8.png"></image>
				<view class="color2 font13">联系我们</view>
			</view>
		</view>
		
		<view class="f5H10"></view>
		<!-- 特色服务 -->
		<view class="infor-title flexRowBetween">
			<view class="tt">法律服务</view>
			<view class="more" @click="Router.navigateTo({route:{path:'/pages/serviceList/serviceList'}})">更多&gt;</view>
		</view>
		<view class="proLis flexRowBetween">
			<view class="item-lis" v-for="(item,index) in productData" :key="index" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/serviceDetails/serviceDetails?id='+$event.currentTarget.dataset.id}})">
				<image style="height: 275rpx;" class="img" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" alt="" />
				<view class="tit avoidOverflow">{{item.title}}</view>
				<!-- <view class="price">{{item.price}}</view> -->
				<!-- <view class="freeVip">会员免费</view> -->
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
			<view class="zixunLis" v-for="(item,index) in consultData" :key="index" v-if="index<3">
				<view class="one">
					<view class="photo">
						<image  :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
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
									<image v-for="c_item in stars" :src="item.score/2 > c_item ?(item.score/2-c_item == 0.5?halfSrc:selectedSrc) : normalSrc" mode="">
									
									</image>
								</view>
								<view>{{item.score}}分</view>
							</view>
						</view>
						<view class="line2">{{item.passage2}}</view>
					</view>
				</view>
				<view class="four flexRowBetween ">
					<view class="tit">{{item.label&&item.label[0]?item.label[0].title:''}}</view>
					<!-- <view class="time">{{item.create_time}}</view> -->
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
			<view class="newslis" v-for="(item,index) in articleData" :key="index"  :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/informationDetails/informationDetails?id='+$event.currentTarget.dataset.id}})" v-if="index<4">
				<view class="twoCt flexRowBetween">
					<view class="leftbox">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
					</view>
					<view class="cont">
						<view class="title avoidOverflow2">{{item.title}}</view>
						<view class="lable flexRowBetween">
							<view>{{item.description}}</view>
							<!-- <view class="time">{{item.create_time}}</view> -->
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 热点资讯 end-->

		<!--底部tab键-->
		<view class="navbar" v-if="showNav">
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
				<view class="text">免费咨询</view>
			</view>
			<button class="navbar_item" v-if="userInfoData.order&&userInfoData.order.length>0" open-type="contact" style="outline: none;line-height: 1.4;overflow: initial;background: #fff;border: none;">
				<view class="nav_img">
					<image src="../../static/images/nabar4.png" />
				</view>
				<view class="text">会员专线</view>
			</button>
			<button class="navbar_item" style="outline: none;line-height: 1.4;overflow: initial;background: #fff;border: none;"  @click="showToast" v-if="userInfoData.order&&userInfoData.order.length==0">
				<view class="nav_img">
					<image src="../../static/images/nabar4.png" />
				</view>
				<view class="text">会员专线</view>
			</button>
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
				interval: 4000,
				duration: 1000,
				labelData: [],
				productData:[],
				articleData:[],
				userInfoData:{},
				consultData:[],
				stars: [0, 1, 2, 3, 4],
				normalSrc: '../../static/images/home-icon11.png',
				selectedSrc: '../../static/images/home-icon12.png',
				halfSrc: '../../static/images/home-icon13.png',
				newsData:[],
				showNav:false
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getNewsData','getUserInfoData','getLabelData','getProductData','getArticleData','getConsultData'], self);
		},
		
		onShareAppMessage(res){
		  const self = this;
		    return {
		      title: '小法宝',
		      path: 'pages/index/index',
		      success: function (res){
		        console.log(res);
		        console.log(parentNo)
		        if(res.errMsg == 'shareAppMessage:ok'){
		          console.log('分享成功')
		          if (self.data.shareBtn){
		            if(res.hasOwnProperty('shareTickets')){
		            console.log(res.shareTickets[0]);
		              self.data.isshare = 1;
		            }else{
		              self.data.isshare = 0;
		            }
		          }
		        }else{
		          wx.showToast({
		            title: '分享失败',
		          })
		          self.data.isshare = 0;
		        }
		      },
		      fail: function(res) {
		        console.log(res)
		      }
		    }
		},
		
		methods: {
			
			showToast(){
				const self = this;
				self.$Utils.showToast('您还不是会员', 'none')
			},
			
			getUserInfoData() {
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
						self.userInfoData = res.info.data[0];
						self.showNav = true
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getUserInfoData');		
				};
				self.$apis.userInfoGet(postData, callback);
			},

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
						self.labelData = res.info.data[0].mainImg;
						
					}
					console.log('self.labelData', self.labelData)
					self.$Utils.finishFunc('getLabelData');
			
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getNewsData() {
				const self = this;
				const postData = {
					searchItem: {
						thirdapp_id: 2,
						parentid:49
					},
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.newsData.push.apply(self.newsData,res.info.data)
					}
					self.$Utils.finishFunc('getNewsData');
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
				postData.paginate = {
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 4
				};
				postData.order = {
					listorder:'desc'
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.productData.push.apply(self.productData,res.info.data)
					}
					console.log('2132',self.productData)
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
				postData.paginate = {
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 4
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
				postData.paginate = {
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 4
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
	button::after{
		border:none
	}
	.swiper-box1{
		height:90rpx
	}
</style>
