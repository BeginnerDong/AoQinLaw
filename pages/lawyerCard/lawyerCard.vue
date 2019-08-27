<template>

		<!-- 律师详情 -->
	<view class="lawyerDatail">
		<view class="mglr4" ><image src="../../static/images/card-icon1.png" style="width:35rpx; height: 30rpx; display: block; margin-top: 10rpx;padding: 20rpx; "  @click="webSelf.$Router.navigateTo({route:{path:'/pages/index/index'}})"></image></view>
		<view class="lawyerList ">
			<view class="info pr"  v-for="(item,index) in lawyerList" :key="index" style="margin-top: 0; padding-top: 0rpx;">
				<view class="info-left" style="width: 125rpx; height: 174rpx;">
					<image :src="item.photoImg"></image>
				</view>
				<view class="info-right">
					<view class="name">{{item.name}}<view class="lable">诉讼律师</view></view>
					<view class="two" style="margin-top: 30rpx;padding-right: 0;">
						<image class="icon" src="../../static/images/lvshi-icon2.png" ></image>
						执业{{item.workTime}}
						<view class="flexRowBetween starClass" style="margin-left: 10rpx;">
							<view class="starBox">
								<image src="../../static/images/home-icon12.png" mode=""></image>
								<image src="../../static/images/home-icon12.png" mode=""></image>
								<image src="../../static/images/home-icon12.png" mode=""></image>
								<image src="../../static/images/home-icon13.png" mode=""></image>
								<image src="../../static/images/home-icon11.png" mode=""></image>
							</view>
							<view>{{item.score}}分</view>
						</view>
					</view>
					<view class="three" style="margin-top: 26rpx;">
						<block v-for="child in item.c_item">
							<view class="info-item">{{child}}</view>
						</block>
					</view>
				</view>
				<view class="rr_btn">
					<view>同步到通讯录</view>
					<view class="bj">分享名片</view>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="pdlr4">
			<view class="iconTitle">
				<image class="icon" src="../../static/images/card-icon2.png" mode=""></image>
				个人信息
			</view>
			<view class="msglist">
				<view class="tit">手机</view>
				<view class="text">***********</view>
				<view class="see">查看</view>
			</view>
			<view class="msglist">
				<view class="tit">微信</view>
				<view class="text">***********</view>
				<view class="see">查看</view>
			</view>
			<view class="msglist">
				<view class="tit">邮箱</view>
				<view class="text">568946562@qq.com</view>
			</view>
			<view class="msglist">
				<view class="tit">公司</view>
				<view class="text">西安纯粹科技</view>
			</view>
			<view class="msglist">
				<view class="tit">地址</view>
				<view class="text">西安市雁塔区高新大都荟A座2007</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="pdlr4">
			<view class="iconTitle">
				<image class="icon" src="../../static/images/card-icon3.png" mode=""></image>
				个性签名
			</view>
			<view class="font14" style="padding-bottom: 30rpx;">同一个环境中，强者和弱者的分解就在于谁能改变它</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="pdlr4">
			<view class="iconTitle">
				<image class="icon" src="../../static/images/card-icon4.png" mode=""></image>
				照片展示
			</view>
			<view class="photoShow">
				<view class="item">
					<image src="../../static/images/card-img1.png" mode=""></image>
				</view>
				<view class="item">
					<image src="../../static/images/card-img1.png" mode=""></image>
				</view>
				<view class="item">
					<image src="../../static/images/card-img1.png" mode=""></image>
				</view>
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
				num:1,
				wx_info:{},
				lawyerList:[
					{
						photoImg:'../../static/images/lvshi-img1.png',
						name:"张胜男",
						lable:"诉讼律师",
						workTime:"5年",
						score:"8.5",
						c_item:["刑事诉讼","债权债务"]
					}
				]
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
	@import "../../assets/style/lawyer.css";
	page{padding-bottom: 60rpx;}
	.rr_btn{ position: absolute; top: 50%;right:3%; transform:translateY(-50%);}
	.rr_btn view{ width: 170rpx; height: 50rpx; line-height: 50rpx;text-align: center; border:2rpx solid #537DEB; font-size: 24rpx; color: #333;border-radius: 10rpx; }
	.rr_btn view.bj{background: #537DEB; color: #fff; margin-top: 50rpx;}
	
	.iconTitle{ display: flex;justify-content: flex-start;align-items: center; height: 100rpx;}
	.iconTitle .icon{ width: 30rpx; height: 30rpx; margin-right: 20rpx;}
	.msglist{ height: 100rpx; display: flex;justify-content: flex-start; position: relative; font-size: 26rpx; align-items: center; border-bottom: 2rpx solid #e7e7e7;}
	.msglist .tit{ width: 60rpx; color: #999; margin-right: 30rpx;}
	.msglist .text{ width: 80%; font-size: 28rpx;}
	.msglist .see{text-align: center;  line-height: 40rpx; height: 40rpx;padding: 0 30rpx; background: #537DEB; color: #fff; font-size: 24rpx; border-radius: 20rpx; position: absolute; right: 0; top: 50%;transform: translateY(-50%);}
	.photoShow{ display: flex;flex-wrap: wrap;}
	.photoShow .item{ width: 210rpx; height: 260rpx; margin-bottom:30rpx; margin-right: 20rpx;}
	.photoShow .item image{ width: 100%; height: 100%;}
</style>
