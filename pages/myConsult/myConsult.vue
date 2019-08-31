<template>
	<view class="consult pdlr4">
		<view class="item" v-for="(item,index) in mainData" :key="index">
			<view class="info">{{item.content}}</view>
			<view class="lable flexRowBetween">
				<view class="left">{{item.description}}</view>
				<view class="right">{{item.create_time}}</view>
			</view>
		</view>
		

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
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id:2,
					type:2,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
			

		},
	};
</script>

<style>
	@import "../../assets/style/common.css";
	page{ background: #F5F5F5;}
	.consult .item{ background: #fff; border-radius: 10rpx;padding: 30rpx; box-shadow: 0 0 4rpx rgba(0,0,0,0.1); margin-top: 30rpx;}
	.consult .item .info{ font-size: 26rpx; color: #333; line-height: 44rpx;}
	.consult .item .lable{padding-top: 20rpx; color: #999; font-size: 24rpx;}
	.consult .item .lable .left{ color: #fdc125; font-size: 26rpx;}
</style> 
