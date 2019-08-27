<template>
	<view>

		<view class="seviseDetaCont2 pdlr4">
			<view class="dataiBigtit">11大标题大标题大标题大标题大标题大标题</view>
			<view class="tit">一、标题标题标题标题标题标题标题标</view>
			<view>1、一份经过专业律师起草审核的文书可以最大限度的避免各种隐藏法律风险，极大维护您的合法权益。</view>
			<view>2、一份经过专业律师起草审核的文书可以最大限度的避免各种隐藏法律风险，极大维护您的合法权益。</view>
			<view class="tit">二、律师提醒</view>
			<view>1、一份经过专业律师起草审核的文书可以最大限度的避免各种隐藏法律风险，极大维护您的合法权益。</view>
			<view>2、一份经过专业律师起草审核的文书可以最大限度的避免各种隐藏法律风险，极大维护您的合法权益。</view>
			<view>
				<image src="../../static/images/home-img1.png" alt="" />
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
				score: '',
				wx_info: {},
				num: 1
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

	page {
		padding-bottom: 60rpx;
	}

	
</style>
