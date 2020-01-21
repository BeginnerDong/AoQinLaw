<template>
	<view>
		<view class="newslisbox">
			<view class="newslis" v-for="(item,index) in mainData" :key="index"  :data-id="item.id">
				<view class="twoCt flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/informationDetails/informationDetails?id='+$event.currentTarget.dataset.id}})">
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

	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:[],
				paginate:{
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 10
				}
			}
		},
		
		onLoad() {
			const self = this;
			//self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
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
							title: ['=', ['热点资讯']],
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
		padding-bottom: 140rpx;
	}
	

</style>
