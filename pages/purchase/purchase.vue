<template>
	<view>
		<view class="purchs_head flexRowBetween">
			<view class="xian"></view>
			<view class="item flexRowBetween">
				<image src="../../static/images/xiadan-icon1.png" alt=""></image>
				<view class="tit">提交订单</view>
			</view>
			<view class="item flexRowBetween">
				<image class="twoIcon" src="../../static/images/xiadan-icon2.png" alt=""></image>
				<view class="tit">支付金额</view>
			</view>
			<view class="item flexRowBetween">
				<image src="../../static/images/xiadan-icon3.png" alt=""></image>
				<view class="tit">律师回电服务</view>
			</view>
		</view>
		<view class="f5H10"></view>

		<view class="purch_indList">
			<view class="purchTitle flexRowBetween pdlr4">
				<view class="tit">服务项目</view>
			</view>
			<view class="fwxm mglr4" v-for="item in mainData">
				<view class="text">{{item.title}}</view>
				<view class="price">{{item.price}}</view>
			</view>
			<view class="f5H10"></view>
			<view class="purchTitle flexRowBetween pdlr4" style="border-bottom:2rpx solid #E7E7E7 ;">
				<view class="tit">个人姓名</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入姓名" placeholder-style="color:#999" v-model="submitData.name"/>
				</view>
			</view>
			<view class="purchTitle flexRowBetween pdlr4" style="border-bottom:2rpx solid #E7E7E7 ;">
				<view class="tit">电话</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入电话" placeholder-style="color:#999" v-model="submitData.phone"/>
				</view>
			</view>
			<view class="purchTitle flexRowBetween pdlr4">
				<view class="tit">案件经过</view>
			</view>
			<view class="pdlr4">
				<textarea value="" placeholder="请在此输入...." placeholder-style="color:#999;"  v-model="submitData.description"/>
				</view>
			<view class="Detai_BfixBox">
				总金额：
				<span class="price" style="font-size: 30rpx;">{{price}}</span> 
				<button class="Rbtn" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)" style="margin: 0;font-size:14px">立即下单</button>
			</view>
			
			
		</view>
		
		

	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				price:0,
				mainData:[],
				submitData:{
					name:'',
					description:'',
					phone:''
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.idArray = options.id;
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
		},
		
		methods: {
			
		
			
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
					thirdapp_id: 2,
					id: ['in', self.idArray]
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					self.$Utils.finishFunc('getMainData');
					self.countPrice()
				};
				self.$apis.skuGet(postData, callback)
			},
			
			countPrice() {
				const self = this;
				for (var i = 0; i < self.mainData.length; i++) {
					self.price += parseFloat(self.mainData[i].price)
				}
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.userInfoData.name==''){
					uni.showModal({
					    title: '提示',
					    content: '您还未注册，立即注册吗？',
					    success: function (res) {
					        if (res.confirm) {
					            self.Router.navigateTo({route:{path:'/pages/myMessage/myMessage'}})
					        } else if (res.cancel) {
					            console.log('用户点击取消');
					        }
					    }
					});
					return
				};
				const pass = self.$Utils.checkComplete(self.submitData);
				const callback = (user, res) => {
					if(pass){
						self.addOrder();
					}else{
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请补充信息','none')
					}	
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			
			addOrder() {
				const self = this;
				const orderList = [{
					sku: [],
					type: 1
				}];
				for (var i = 0; i < self.mainData.length; i++) {
					orderList[0].sku.push({
						id: self.mainData[i].id,
						count: 1
					}, );
				};
				if (!self.order_id) {
					const postData = {
						tokenFuncName: 'getProjectToken',
						orderList: orderList,
						data: {
							passage1: 'user',
							name: self.submitData.name,
							phone:self.submitData.phone,
							description: self.submitData.description
						}
					};
					const callback = (res) => {
						if (res && res.solely_code == 100000) {
							self.order_id = res.info.id
							self.pay(self.order_id);
						};
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
						price: self.price
					}
				};
				postData.payAfter = [];
				var ratio = wx.getStorageSync('user_info').thirdApp.ratio;
				if (self.userInfoData&&self.userInfoData.behavior==0&&self.userInfoData.parent.length>0) {
					if (self.price > 0 && ratio>0) {
						postData.payAfter.push(
							{
								tableName: 'FlowLog',
								FuncName: 'add',
								data: {
									relation_user: wx.getStorageSync('user_info').user_no,
									count: self.price*(ratio/100),
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
									self.$Utils.showToast('操作成功', 'none');
									setTimeout(function() {
										self.Router.reLaunch({route:{path:'/pages/user/user'}})
									}, 800)
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						}
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.pay(postData, callback);
			},

		},
	};
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/service.css";
	page{padding-bottom: 140rpx;}
	.purchs_head{padding: 40rpx 4%; position: relative;}
	.purchs_head .xian{ border-top: 2rpx dashed #537DEB; width: 370rpx; position: absolute; top: 64rpx; left: 50%; transform: translateX(-50%);}
	.purchs_head .item{ width: 33.3%; flex-direction: column; font-size: 26rpx;}
	.purchs_head .item image{ width: 48rpx; height: 48rpx; margin: 0 auto; display: block; margin-bottom: 30rpx;}
	.purchs_head .item .twoIcon{ background: #fff; padding: 0 20rpx;}
	
	.purchTitle{ height: 100rpx; font-size:30rpx;}
	.purchTitle .tit{ width: 30%; font-weight: bold;}
	.purchTitle .rr{ width: 50%; color: #333; text-align: right; display: block;}
	.purchTitle .rr input{ width: 100%; display: block; text-align: right; font-size: 24rpx;}
	
	.purch_indList .fwxm{ padding-bottom:40rpx; border: 2rpx solid #e7e7e7; background: #F9F9F9;padding: 30rpx; font-size: 24rpx; color: #666; margin-bottom: 40rpx;}
	.purch_indList .fwxm .price{ font-size: 26rpx; margin-top: 20rpx;}
	.purch_indList textarea{ width: 100%; box-sizing: border-box; padding: 20rpx;border:2rpx solid #e7e7e7; background: #f2f2f2; font-size: 26rpx; line-height: 40rpx;}
	
</style>
