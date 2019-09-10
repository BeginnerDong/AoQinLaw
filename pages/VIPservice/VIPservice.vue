<template>
	<view>
		<!-- banner -->
		<view class="banner-box">
			<view class="banner">
				<swiper class="swiper-box" @change="changeIndex" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" indicator-active-color="#537deb" indicator-color="#dbdbdb">
					<block v-for="(item,index) in mainData" :key="index">
						<swiper-item class="swiper-item">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="slide-image" />
						</swiper-item>
					</block>
				</swiper>
			</view>
		</view>
		
		<view class="msg pdlr4">
			<view class="tit">服务说明</view>
			<view class="content ql-editor" v-html="mainData[currentIndex]?mainData[currentIndex].content:''">
			</view>
		</view>
		
		<view class="submitbtn" style="margin: 100rpx auto 0 auto">
			<button type="submit" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">立即购买</button>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				mainData:[],
				indicatorDots: true,
				autoplay: false,
				interval: 2000,
				duration: 500,
				currentIndex:0
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			changeIndex(e){
				const self = this;
				self.currentIndex = e.detail.current;
				console.log(e)
			},
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.getAfter = {
					parent:{
						tableName:'Distribution',
						middleKey:'user_no',
						key:'child_no',
						searchItem:{
							status:1,
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getUserInfoData');		
				};
				self.$apis.userInfoGet(postData, callback);
			},

			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2,
					type:2,
				};	
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							const regex = new RegExp('<img', 'gi');
							self.mainData[i].content = self.mainData[i].content.replace(regex, `<img style="max-width: 100%;"`);
						}
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const callback = (user, res) => {
					self.addOrder();
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			
			addOrder() {
				const self = this;
				const orderList = [{
					product: [],
					type: 2
				}];
			
				orderList[0].product.push({
					id: self.mainData[self.currentIndex].id,
					count: 1
				}, );
				if (!self.order_id) {
					const postData = {
						tokenFuncName: 'getProjectToken',
						orderList: orderList,
					};
					console.log('addOrder', self.addressData)
			
					const callback = (res) => {
						if (res && res.solely_code == 100000) {
							self.order_id = res.info.id
							self.pay(self.order_id);
						}else{
							uni.setStorageSync('canClick', true);
							self.$Utils.showToast(res.msg,'none')
						}
					};
					self.$apis.addOrder(postData, callback);
				} else {
					self.pay(self.order_id)
				}
			},
			
			
			
			
			pay() {
				const self = this;
				var order_id = self.order_id;
				const postData = {
					tokenFuncName: 'getProjectToken',
					searchItem: {
						id: order_id,
					},
					wxPay: {
						price: self.mainData[self.currentIndex].price
					}
				};
				postData.payAfter = [];
				var ratio = wx.getStorageSync('user_info').thirdApp.ratio;
				if (self.userInfoData&&self.userInfoData.behavior==0&&self.userInfoData.parent.length>0) {
					if (self.mainData[self.currentIndex].price > 0 && ratio>0) {
						postData.payAfter.push({
							tableName: 'FlowLog',
							FuncName: 'add',
							data: {
								relation_user: wx.getStorageSync('user_info').user_no,
								count: self.mainData[self.currentIndex].price*(ratio/100),
								trade_info: '下级消费返佣',
								user_no: self.userInfoData.parent[0].parent_no,
								type: 2,
								thirdapp_id: 2,
							}
						},
						{
							tableName: 'UserInfo',
							FuncName: 'update',
							data: {
								behavior:1
							},
							searchItem:{
								user_no:wx.getStorageSync('user_info').user_no
							}
						}
						);
					};
				}
				
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								if (payData == 1) {
									self.$Utils.showToast('购买成功', 'none');
									setTimeout(function() {
										self.Router.reLaunch({route:{path:'/pages/user/user'}})
									}, 800)
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						}
					} else {
						self.$Utils.showToast('支付失败', 'none')
					}
				};
				self.$apis.pay(postData, callback);
			},
			
			

		},
	};
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/index.css";

	.swiper-box{ height: 400rpx; width: 100%; margin: 50rpx auto; border-radius: 10rpx; }
	.swiper-box .swiper-item image{ width: 503rpx;height: 324rpx;  display: block;margin: 0 auto;}
	.msg view{ padding-bottom: 30rpx; font-size: 26rpx; color: #666; line-height: 40rpx;}
	view.tit{ font-size: 30rpx; font-weight: bold; color: #333; line-height: 60rpx;padding-bottom: 0;}
	.ql-editor{padding: 30rpx 0;}
	
	.seviceNum{padding: 20rpx 4%;}
	.seviceNum .lis{ height: 80rpx;border-bottom: 2rpx solid #e7e7e7; font-size: 26rpx; color: #666;}
	.seviceNum .nmb{ color: #537DEB;}

</style>
