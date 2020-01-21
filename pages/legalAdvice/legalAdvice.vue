<template>
	<view>

		<view class="fvzixun palr4">
			<view class="zixunLis" v-for="(item,index) in mainData" :key="index">
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

	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:[],
				stars: [0, 1, 2, 3, 4],
				normalSrc: '../../static/images/home-icon11.png',
				selectedSrc: '../../static/images/home-icon12.png',
				halfSrc: '../../static/images/home-icon13.png',
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

			getMainData() {
				const self = this;
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
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
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
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
