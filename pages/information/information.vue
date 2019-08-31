<template>
	<view>
		<view class="newslisbox">
			<view class="newslis" v-for="(item,index) in articleData" :key="index"  >
				<view class="twoCt flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/informationDetails/informationDetails'}})">
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

	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				labelData: {},
				productData:[],
				articleData:[],
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

	page {
		padding-bottom: 140rpx;
	}
	

</style>
