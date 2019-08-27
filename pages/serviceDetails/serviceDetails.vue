<template>
	<view>

		<view  class="seviseDetalTop">
			<img class="topImg" style="width:100%; height: 360rpx;" src="../../static/images/details-img.png" alt="">
			<view class="freeVip" style="right: 0; left: auto; border-radius: 30rpx 0 0 30rpx;">会员免费</view>
			<view class="pdlr4">
				<view class="tit">法律咨询</view>
				<view class="text2">专业律师代写各种诉讼和非诉讼法律文书，起诉讼、答辩状、申请书、合同、欠条、遗嘱、章程、员工手册等等。</view>
			</view>
		</view>
		<view class="f5H10"></view>
		<view class="seviseDetalNav flexRowBetween pdlr4">
			<view :class="num==1? 'on':''" @click="change('1')">服务项目</view>
			<view :class="num==2? 'on':''" @click="change('2')">服务详情</view>
		</view>
		
		<view class="seviseDetaCont1 pdlr4" v-if="num==1">
			
			<view class="item">
				<view class="tit">不涉及财产关系100元/小时；(不足1小时按1小时计算)</view>
				<view class="price">56.00</view>
				<img class="icon" src="../../static/images/details-icon1.png" alt="">
			</view>
			<view class="item">
				<view class="tit">不涉及财产关系100元/小时；(不足1小时按1小时计算)</view>
				<view class="price">56.00</view>
				<img class="icon" src="../../static/images/details-icon2.png" alt="">
			</view>
			<view class="Detai_BfixBox">
				总金额：
				<span class="price" style="font-size: 30rpx;">56.00</span> 
				<view class="Rbtn" @click="webSelf.$Router.navigateTo({route:{path:'/pages/purchase/purchase'}})">立即下单</view>
			</view>
		</view>
		
		<view class="seviseDetaCont2 pdlr4" v-if="num==2">
			<view class="tit">律师提醒：</view>
			<view>一份经过专业律师起草审核的文书可以最大限度的避免各种隐藏法律风险，极大维护您的合法权益。</view>
			<view>一份经过专业律师起草审核的文书可以最大限度的避免各种隐藏法律风险，极大维护您的合法权益。</view>
			<view class="tit">律师提醒：</view>
			<view>一份经过专业律师起草审核的文书可以最大限度的避免各种隐藏法律风险，极大维护您的合法权益。</view>
			<view>一份经过专业律师起草审核的文书可以最大限度的避免各种隐藏法律风险，极大维护您的合法权益。</view>
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
				num:1
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
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

</style>
