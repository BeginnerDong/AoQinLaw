<template>
	<view class="consult pdlr4">
		<view class="item" v-for="(item,index) in consult" :key="index">
			<view class="info">{{item.infor}}</view>
			<view class="lable flexRowBetween">
				<view class="left">{{item.lable}}</view>
				<view class="right">{{item.time}}</view>
			</view>
		</view>
		

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
				consult:[
					{
						infor:"内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。"	,
						lable:"刑事类",
						time:"2019年8月26号"
					},
					{
						infor:"222内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。"	,
						lable:"刑事类",
						time:"2019年8月27号"
					},
					{
						infor:"222内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。"	,
						lable:"刑事类",
						time:"2019年8月27号"
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
	page{ background: #F5F5F5;}
	.consult .item{ background: #fff; border-radius: 10rpx;padding: 30rpx; box-shadow: 0 0 4rpx rgba(0,0,0,0.1); margin-top: 30rpx;}
	.consult .item .info{ font-size: 26rpx; color: #333; line-height: 44rpx;}
	.consult .item .lable{padding-top: 20rpx; color: #999; font-size: 24rpx;}
	.consult .item .lable .left{ color: #fdc125; font-size: 26rpx;}
</style> 
