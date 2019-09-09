<template>

		<!-- 律师详情 -->
	<view class="lawyerDatail">
		<view class="mglr4" >
			<image src="../../static/images/card-icon1.png" style="width:35rpx; height: 30rpx; display: block; margin-top: 10rpx;padding: 20rpx; "  
			@click="Router.reLaunch({route:{path:'/pages/index/index'}})"></image></view>
		<view class="lawyerList ">
			<view class="info pr"   style="margin-top: 0; padding-top: 0rpx;">
				<view class="info-left" style="width: 125rpx; height: 174rpx;">
					<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''"></image>
				</view>
				<view class="info-right">
					<view class="name">{{mainData.title}}<view class="lable">{{mainData.passage1}}</view></view>
					<view class="two" style="margin-top: 30rpx;padding-right: 0;">
						<image class="icon" src="../../static/images/lvshi-icon2.png" ></image>
						执业{{mainData.small_title}}年
						<view class="flexRowBetween starClass" style="margin-left: 10rpx;">
							<view class="starBox">
								<image v-for="c_item in stars" :src="mainData.averageScore > c_item ?(mainData.averageScore-c_item == 0.5?halfSrc:selectedSrc) : normalSrc" mode="">
								
								</image>
							</view>
							<view v-if="mainData.averageScore>0">{{mainData.averageScore*2}}分</view>
							<view v-if="mainData.averageScore==0">暂无评分</view>
						</view>
					</view>
					<view class="three" style="margin-top: 26rpx;">
						<block v-for="child in mainData.keywords">
							<view class="info-item">{{child}}</view>
						</block>
					</view>
				</view>
				<view class="rr_btn">
					<view @click="addContact">同步到通讯录</view>
					<button class="share" open-type="share">分享名片</button>
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
				<view class="text">{{showPhone?mainData.contactPhone:'********'}}</view>
				<view class="see" @click="showPhoneOrWechat('phone')">查看</view>
				<view class="see" v-if="showPhone" @click="callPhone()">打电话</view>
			</view>
			<view class="msglist">
				<view class="tit">微信</view>
				<view class="text">{{showWechat?mainData.wechat:'********'}}</view>
				<view class="see" @click="showPhoneOrWechat('wechat')">查看</view>
				<view class="see" v-if="showWechat" @click="copyBtn()">加微信</view>
			</view>
			<view class="msglist">
				<view class="tit">邮箱</view>
				<view class="text">{{mainData.email}}</view>
			</view>
			<view class="msglist">
				<view class="tit">公司</view>
				<view class="text">{{mainData.company}}</view>
			</view>
			<view class="msglist">
				<view class="tit">地址</view>
				<view class="text">{{mainData.address}}</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="pdlr4">
			<view class="iconTitle">
				<image class="icon" src="../../static/images/card-icon3.png" mode=""></image>
				个性签名
			</view>
			<view class="font14" style="padding-bottom: 30rpx;">{{mainData.description}}</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="pdlr4">
			<view class="iconTitle">
				<image class="icon" src="../../static/images/card-icon4.png" mode=""></image>
				照片展示
			</view>
			<view class="photoShow">
				<view class="item" v-for="item in mainData.bannerImg">
					<image :src="item.url" mode=""></image>
				</view>
				
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
				userInfoData:{},
				showPhone:false,
				showWechat:false,
				averageScore:0,
				stars: [0, 1, 2, 3, 4],
				normalSrc: '../../static/images/home-icon11.png',
				selectedSrc: '../../static/images/home-icon12.png',
				halfSrc: '../../static/images/home-icon13.png',
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
		},
		
		methods: {
			
			addContact(){
			  const self = this;
			    wx.addPhoneContact({
			      firstName: self.mainData.title,
			      mobilePhoneNumber: self.mainData.contactPhone,
			      success:function(){
			          console.log('添加成功')
			      }
			  })
			},
			
			showPhoneOrWechat(type){
				const self = this;
				if(self.userInfoData.order.length>0){
					if(type=='phone'){
						self.showPhone = true
					}else if(type=='wechat'){
						self.showWechat = true
					}
				}else{
					self.$Utils.showToast('您还不是会员', 'none')
				}				
			},
			
			callPhone(){
			  const self = this;
			  wx.makePhoneCall({
			    phoneNumber: self.mainData.contactPhone,
			  })
			},
			
			copyBtn(e) {
			  const self = this;
			  wx.setClipboardData({
			    data: self.mainData.wechat,
			    success: function (res) {
			      wx.showToast({
			        title: '复制成功',
			      });
			    }
			  });
			},
			
			onShareAppMessage(res){
			  const self = this;
			    
			    return {
			      title: '澳秦法律',
			      path: 'pages/lawyerCard/lawyerCard',
			      success: function (res){
			        console.log(res);
			        console.log(parentNo)
			        if(res.errMsg == 'shareAppMessage:ok'){
			          console.log('分享成功')
			          if (self.data.shareBtn){
			            if(res.hasOwnProperty('shareTickets')){
			            console.log(res.shareTickets[0]);
			              self.data.isshare = 1;
			            }else{
			              self.data.isshare = 0;
			            }
			          }
			        }else{
			          wx.showToast({
			            title: '分享失败',
			          })
			          self.data.isshare = 0;
			        }
			      },
			      fail: function(res) {
			        console.log(res)
			      }
			    }
			},
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				var now = Date.parse(new Date())/1000;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.getAfter = {
					order:{
						tableName:'Order',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1,
							invalid_time:['>',now],
							pay_status: 1,
							type:2
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
				console.log('852369')
				const postData = {};
				postData.searchItem = {
					id:self.id
				}
				postData.getAfter = {
					message: {
						tableName: 'Message',
						searchItem: {
							status:1
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
						compute:{
						  score:[
						    'sum',
						    'score',
						    {
						      status:1,
						    }
						  ],
						  count:[
						    'count',
						    'count',
						    {
						      status:1,
						    }
						  ]
						},
					},
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
						self.mainData.keywords = self.mainData.keywords.split(',');
						self.mainData.averageScore = self.mainData.message.score/self.mainData.message.count
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
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
	.share{background: #537DEB;color: #fff;margin-top: 50rpx;width: 170rpx;height: 50rpx;line-height: 50rpx;text-align: center;border: 2rpx solid #537DEB;font-size: 24rpx;border-radius: 10rpx;}
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
