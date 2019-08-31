<template>
	<view>

		<!-- 律师列表 -->
		<view class="lawyerList mglr4">
			<view class="info"  v-for="(item,index) in mainData" :key="index" @click="Router.navigateTo({route:{path:'/pages/lawyerDetail/lawyerDetail?id='+item.id}})">
				<view class="info-left">
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
				</view>
				<view class="info-right">
					<view class="name">{{item.title}}<view class="lable">{{item.passage1}}</view></view>
					<view class="two">
						<image class="icon" src="../../static/images/lvshi-icon2.png" ></image>
						执业{{item.small_title}}年
						<view class="flexRowBetween starClass" style="margin-left: 10rpx;">
							<view class="starBox">
								<image src="../../static/images/home-icon12.png" mode=""></image>
								<image src="../../static/images/home-icon12.png" mode=""></image>
								<image src="../../static/images/home-icon12.png" mode=""></image>
								<image src="../../static/images/home-icon13.png" mode=""></image>
								<image src="../../static/images/home-icon11.png" mode=""></image>
							</view>
							<view>8.5分</view>
						</view>
						<img class="arrow" src="../../static/images/arrow-icon1.png" alt="">
					</view>
					<view class="three">
						<block v-for="child in item.keywords" :key="index">
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
				Router:this.$Router,
				
				mainData:[],
				
			}
		},
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		
		methods: {

			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 5
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id:2,
					type:2,
					show :1
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['律师']],
						},
						middleKey: 'menu_id',
						key: 'id',
						condition: 'in',
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].keywords = self.mainData[i].keywords.split(',')
						}
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},

		},
	};
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/lawyer.css";
	page{ background: #F5F5F5;padding-bottom: 60rpx;}




</style>
