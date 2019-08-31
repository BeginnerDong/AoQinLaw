<template>
	<view>

		<view class="imgbox">
			<image src="../../static/images/vip-img2.png"  mode=""></image>
		</view>
		<view class="submitbtn"  @click="Router.navigateTo({route:{path:'/pages/VIPservice/VIPservice'}})">
			<button type="button">立即购买</button>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				
				
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
	.imgbox{ width: 314rpx; height: 309rpx; display: block;margin: 200rpx auto}
	.imgbox image{ width: 100%; height: 100%;}
</style> 
