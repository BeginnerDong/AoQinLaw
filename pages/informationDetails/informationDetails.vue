<template>
	<view>
		<view class="infmtDatail flexRowBetween" >
			<view class="topTit" >
				<view class="title avoidOverflow2">夫妻因婚外离婚，无过错放可以向对方索要多少精神损害赔偿？</view>
				<view class="lable flexRowBetween">
					<view>婚姻家庭</view>
					<view class="time">2019.08.02</view>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="seviseDetaCont2 pdlr4">
			<view class="tit">一、婚外情精神损害排场考虑因素：</view>
			<view>一份经过专业律师起草审核的文书可以最大限度的避免各种隐藏法律风险，极大维护您的合法权益一份经过专业律师起草审核的文书可以最大限度的避免各种隐藏法律风险，极大维护您的合法权益。</view>
			<view>1.份经过专业律师起草审核的文书可以最大限度的避免各种隐藏法律风险，极大维护您的合法权益。</view>
			<view>2.份经过专业律师起草审核的文书可以最大限度的避免各种隐藏法律风险，极大维护您的合法权益。</view>
			<view class="tit">二、婚外情精神损害排场考虑因素：</view>
			<view>一份经过专业律师起草审核的文书可以最大限度的避免各种隐藏法律风险，极大维护您的合法权益份经过专业律师起草审核的文书可以最大限度的避免各种隐藏法律风险，极大维护您的合法权益。</view>
			<view>1.份经过专业律师起草审核的文书可以最大限度的避免各种隐藏法律风险，极大维护您的合法权益。</view>
			<view>1.份经过专业律师起草审核的文书可以最大限度的避免各种隐藏法律风险，极大维护您的合法权益。</view>
			<view>
				<image src="../../static/images/home-img1.png" alt=""/>
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
	page{padding-bottom: 140rpx;}
	.infmtDatail{}
	.infmtDatail .topTit{padding:20rpx 3%;}
	.title{ font-size: 28rpx; line-height: 44rpx;}
	.lable{ padding-top: 20rpx; font-size: 24rpx; color: #537deb;}
	.lable .time{ color: #999;}
</style>
