<template>
	<view class="myCace pdlr4">
		<view class="item" v-for="item in mainData">
			<view   :data-id = "item.id"
			@click="Router.navigateTo({route:{path:'/pages/myCaseDetail/myCaseDetail?id='+$event.currentTarget.dataset.id}})">
				<view class="avoidOverflow">{{item.products[0].snap_product.product.title}}</view>
				<view class="tag ok" v-if="item.transport_status==2">已解决</view>
				<view class="tag ing" v-if="item.transport_status==1">解决中</view>
				<view class="tag no" v-if="item.transport_status==0">未解决</view>
				<view class="lable flexRowBetween">
					<!-- <view class="left">刑事类</view> -->
					<view class="right">{{item.create_time}}</view>
				</view>
				<view class="info">{{item.description}}</view>
			</view>
			<view style="display: flex; justify-content: flex-end;padding-top: 20rpx;" v-if="item.products[0].isremark==0&&item.transport_status==2">
				<view class="plBtn" :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/myCasePjDetail/myCasePjDetail?id='+$event.currentTarget.dataset.id}})">评论服务</view>
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
				searchItem:{
					pay_status:1
				}
			}
		},
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			uni.setStorageSync('canClick', false);
		},
		
		onShow(){
			const self = this;
			self.getMainData(true)
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
					self.$Utils.clearPageIndex(self);
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.thirdapp_id = 2;
				postData.searchItem.type = 1;
				postData.order = {
					create_time: 'desc'
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info.data.length > 0) {
							self.mainData.push.apply(self.mainData, res.info.data);
						} else {
							self.$Utils.showToast('没有更多了', 'none');
						};
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('网络故障', 'none')
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
	page{ background: #F5F5F5;}
	
</style> 
