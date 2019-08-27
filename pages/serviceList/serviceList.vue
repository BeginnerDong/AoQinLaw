<template>
	<view>

		<!-- 特色服务 -->
		<view class="proLis flexRowBetween">
			<view class="item-lis" v-for="(item,index) in produtList" :key="index" @click="webSelf.$Router.navigateTo({route:{path:'/pages/serviceDetails/serviceDetails'}})">
				<image class="img" :src="item.picUl" alt="" />
				<view class="tit avoidOverflow">{{item.title}}</view>
				<view class="price">{{item.price}}</view>
				<view class="freeVip">会员免费</view>
			</view>
		</view>
		<!-- 特色服务 end -->


	</view>
</template>

<script>
	export default {
		data() {
			return {
				webSelf: this,
				showView: false,
				score:'',
				wx_info:{},
				
				produtList: [
					{
						picUl: "../../static/images/home-img1.png",
						title: "1法律咨询",
						price: "56.00"
					},
					{
						picUl: "../../static/images/home-banner.png",
						title: "2法律咨询",
						price: "66.00"
					},
					{
						picUl: "../../static/images/profile-img.png",
						title: "法律咨询",
						price: "56.00"
					},
					{
						picUl: "../../static/images/home-img1.png",
						title: "法律咨询",
						price: "56.00"
					},
					{
						picUl: "../../static/images/details-img2.png",
						title: "产品标题1",
						price: "56.00"
					}
				]
				
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {

			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';

				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data;
					} else {
						self.$Utils.showToast(res.msg, 'none')
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
	@import "../../assets/style/service.css";

</style>
