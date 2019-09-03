<template>

		<!-- 律师详情 -->
	<view class="lawyerDatail">
		
		<view class="lawyerList ">
			<view class="info pr"  style="margin-top: 0;">
				<view class="info-left">
					<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''"></image>
				</view>
				<view class="info-right">
					<view class="name">{{mainData.title}}<view class="lable">{{mainData.passage1}}</view></view>
					<view class="two" style="margin-top: 40rpx;padding-right: 0;">
						<image class="icon" src="../../static/images/lvshi-icon2.png" ></image>
						执业{{mainData.small_title}}年
						<view class="flexRowBetween starClass" style="margin-left: 10rpx;">
							<view class="starBox">
								<!-- <image src="../../static/images/home-icon12.png" mode=""></image>
								<image src="../../static/images/home-icon12.png" mode=""></image>
								<image src="../../static/images/home-icon12.png" mode=""></image>
								<image src="../../static/images/home-icon13.png" mode=""></image>
								<image src="../../static/images/home-icon11.png" mode=""></image> -->
								<image v-for="item in stars" :src="averageScore > item ?(averageScore-item == 0.5?halfSrc:selectedSrc) : normalSrc" mode="">

								</image>
							</view>
							<view v-if="messageData.length>0">{{averageScore*2?averageScore*2:'0'}}分</view>
							<view v-if="messageData.length==0">暂无评分</view>
						</view>
					</view>
					<view class="three" style="margin-top: 40rpx;">
						<block v-for="child in mainData.keywords">
							<view class="info-item">{{child}}</view>
						</block>
					</view>
				</view>
				<view class="rr_car" @click="Router.navigateTo({route:{path:'/pages/lawyerCard/lawyerCard?id='+mainData.id}})">
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
			<view class="content ql-editor" v-html="mainData.content">
			</view>
		</view>
		
		<!-- 口碑评价内容 -->
		<view class="pingjiabox pdlr4" v-if="num==2" >
			<view class="list" v-for="item in messageData">
				<view class="left-pic">
					<image :src="item.user&&item.user.headImgUrl?item.user.headImgUrl:''"></image>
				</view>
				<view class="flexRowBetween">
					<view class="name overflow1">{{item.user&&item.user.headImgUrl?item.user.nickname:''}}</view>
					<view class="font9 color3">{{item.create_time}}</view>
				</view>
				<view class="cont avoidOverflow3">
					{{item.content}}
				</view>
			</view>
		</view>
		
	</view>


</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				num:1,		
				messageData:[],
				averageScore:0,
				stars: [0, 1, 2, 3, 4],
				normalSrc: '../../static/images/home-icon11.png',
				selectedSrc: '../../static/images/home-icon12.png',
				halfSrc: '../../static/images/home-icon13.png',
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getMessageData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMessageData()
			};
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
				postData.searchItem = {
					id:self.id
				}
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
						self.mainData.keywords = self.mainData.keywords.split(',')
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			getMessageData(isNew) {
				const self = this;
				if (isNew) {
					self.messageData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 5
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					relation_id:self.id,					
				};
				postData.compute = {
				  totalCount:[
					'sum',
					'score',
					{relation_id:self.id}
				  ],  
				};
				postData.getAfter = {
					user: {
						tableName: 'User',
						searchItem: {
							status:1,
						},
						middleKey: 'user_no',
						key: 'user_no',
						condition: 'in',
						info:['headImgUrl','nickname']
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.messageData.push.apply(self.messageData, res.info.data);					
					}
					self.averageScore = res.info.compute.totalCount/self.messageData.length
					self.$Utils.finishFunc('getMessageData');
				};
				self.$apis.messageGet(postData, callback);
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
