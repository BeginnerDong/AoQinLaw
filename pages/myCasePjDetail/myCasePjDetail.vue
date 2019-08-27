<template>
<view>
		<view class="myCace">
			<view class="item" style="margin-top: 0; border-radius: 0;">
				<view>标题标题标题标题标题标题标题标题</view>
				<view class="lable flexRowBetween">
					<view class="left">刑事类</view>
					<view class="right">2019年8月27日</view>
				</view>
				<view class="info">内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="lawyerList">
			<view class="info pr"  style="margin-top: 0;">
				<view class="info-left">
					<image src="../../static/images/lvshi-img1.png"></image>
				</view>
				<view class="info-right" style="width:78%">
					<view class="name">张胜男<view class="lable">诉讼律师</view></view>
					<view class="two">
						<image class="icon" src="../../static/images/lvshi-icon2.png" ></image>
						执业2年
					</view>
					<view class="three">
						<block>
							<view class="info-item">刑事诉讼</view>
							<view class="info-item">债权债务</view>
						</block>
					</view>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="CasePjia pdlr4">
			<view class="child flexRowBetween">
				<view class="tit">星级评价：</view>
				<view class="rr flexRowBetween">
					<view class="starBigBox">
						<image src="../../static/images/home-icon11.png" mode=""></image>
						<image src="../../static/images/home-icon11.png" mode=""></image>
						<image src="../../static/images/home-icon11.png" mode=""></image>
						<image src="../../static/images/home-icon11.png" mode=""></image>
						<image src="../../static/images/home-icon11.png" mode=""></image>
					</view>
				</view>
			</view>
			<view class="child flexRowBetween" style="align-items: flex-start;">
				<view class="tit">评价：</view>
				<view class="rr">
					<textarea value="" placeholder="请输入您想要评价的内容" />
				</view>
			</view>
			<view class="submitbtn" style="margin-top: 120rpx;">
				<button type="submit">提交</button>
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
				wx_info:{}
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
	@import "../../assets/style/lawyer.css";
	.CasePjia{ padding: 30rpx 4%; }
	.CasePjia .child{padding: 20rpx 0; font-size: 26rpx; justify-content: flex-start;}
	.CasePjia .child .tit{ width: 22%;}
	.CasePjia .child .rr{ width: 75%;}
	.CasePjia .child .rr textarea{ width: 100%; display: block; padding: 20rpx; font-size: 24rpx; line-height: 40rpx; background: #F2F2F2;box-sizing: border-box;}

</style> 
