<template>
	<view>

		<view  class="seviseDetalTop">
			<img class="topImg" style="width:100%; height: 360rpx;" :src="mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''" alt="">
			<!-- <view class="freeVip" style="right: 0; left: auto; border-radius: 30rpx 0 0 30rpx;">会员免费</view> -->
			<view class="pdlr4">
				<view class="tit">{{mainData.title}}</view>
				<view class="text2">{{mainData.description}}</view>
			</view>
		</view>
		<view class="f5H10"></view>
		<view class="seviseDetalNav flexRowBetween pdlr4">
			<view :class="num==1? 'on':''" @click="change('1')">服务项目</view>
			<view :class="num==2? 'on':''" @click="change('2')">服务详情</view>
		</view>
		
		<view class="seviseDetaCont1 pdlr4" v-if="num==1">
			
			<view class="item" v-for="(item,index) in mainData.sku">
				<view class="tit">{{item.title}}</view>
				<view class="price">{{item.price}}</view>
				<img @click="selectProduct(index)" class="icon" :src="item.select?'../../static/images/details-icon1.png':'../../static/images/details-icon2.png'" alt="">
			</view>
			
			<view class="Detai_BfixBox" style="background: #fff;">
				总金额：
				<span class="price" style="font-size: 30rpx;">{{price}}</span> 
				<view class="Rbtn" @click="goBuy">立即下单</view>
			</view>
		</view>
		
		<view class="seviseDetaCont2 pdlr4" v-if="num==2">
			<view class="content ql-editor" v-html="mainData.content">
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
				price:0
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
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
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2,
					id:self.id
				};
				postData.getAfter = {
					sku: {
						tableName: 'Sku',
						searchItem: {
							status:1,
						},
						middleKey: 'product_no',
						key: 'product_no',
						condition: 'in',
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
						for (var i = 0; i < self.mainData.sku.length; i++) {
							self.mainData.sku[i].select = false
						}
					} else {
						self.$Utils.showToast(res.msg, 'none');
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			selectProduct(index){
				const self = this;
				self.mainData.sku[index].select=!self.mainData.sku[index].select
				self.countPrice()
			},
			
			countPrice(){
				const self = this;
				self.price = 0;
				for (var i = 0; i < self.mainData.sku.length; i++) {
					if(self.mainData.sku[i].select){
						self.price += parseFloat(self.mainData.sku[i].price)
					}
				}
				console.log('self.price',self.price)
			},
			
			goBuy(){
				const self = this;
				var idArry= [];
				for (var i = 0; i < self.mainData.sku.length; i++) {
					if(self.mainData.sku[i].select){
						idArry.push(self.mainData.sku[i].id);			
					}
				}
				if(idArry.length==0){
					self.$Utils.showToast('未选择服务','none')
					return
				};
				self.Router.navigateTo({route:{path:'/pages/purchase/purchase?id='+idArry}})
			},

		},
	};
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/service.css";
	page{padding-bottom: 140rpx;}

</style>
