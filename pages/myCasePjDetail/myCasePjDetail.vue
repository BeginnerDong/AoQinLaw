<template>
	<view>
		<view class="myCace">
			<view class="item" style="margin-top: 0; border-radius: 0;">
				<view class="avoidOverflow">{{mainData.products[0].snap_product.product.title}}</view>
				<view class="lable flexRowBetween">
					<!-- <view class="left">刑事类</view> -->
					<view class="right">{{mainData.create_time}}</view>
				</view>
				<view class="info">{{mainData.description}}</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="lawyerList">
			<view class="info pr"  style="margin-top: 0;">
				<view class="info-left">
					<image :src="mainData.lawyer[0].mainImg[0].url"></image>
				</view>
				<view class="info-right" style="width:78%">
					<view class="name">{{mainData.lawyer[0].title}}<view class="lable">{{mainData.lawyer[0].passage1}}</view></view>
					<view class="two">
						<image class="icon" src="../../static/images/lvshi-icon2.png"></image>
						执业{{mainData.lawyer[0].small_title}}年
					</view>
					<view class="three">
						<view class="info-item" v-for="item in mainData.lawyer[0].keywords">{{item}}</view>		
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
						<view style="margin-right: 10px;width: 40rpx;height: 40rpx;" v-for="item in stars">
							<image class="star-image" :src="submitData.score > item ?(submitData.score-item == 0.5?halfSrc:selectedSrc) : normalSrc" mode="">
								<view class="startItem" style="left:0rpx"  @click="selectLeft(item+0.5)"></view>
								 <view class="startItem" style="left:20rpx"  @click="selectRight(item+1)"></view>
							</image>
						</view>
						
						<!-- <image src="../../static/images/home-icon11.png" mode=""></image>
						<image src="../../static/images/home-icon11.png" mode=""></image>
						<image src="../../static/images/home-icon11.png" mode=""></image>
						<image src="../../static/images/home-icon11.png" mode=""></image> -->
					</view>
				</view>
			</view>
			<view class="child flexRowBetween" style="align-items: flex-start;">
				<view class="tit">评价：</view>
				<view class="rr">
					<textarea value="" placeholder="请输入您想要评价的内容" v-model="submitData.content"/>
				</view>
			</view>
			<view class="submitbtn" style="margin-top: 120rpx;">
				<button open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">提交</button>
			</view>
			
		</view>
</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				mainData:{},
				submitData: {
					order_no: '',
					relation_id: '',
					score: 0,
					content: '',
					type:1
				},
				stars: [0, 1, 2, 3, 4],
				normalSrc: '../../static/images/home-icon11.png',
				selectedSrc: '../../static/images/home-icon12.png',
				halfSrc: '../../static/images/home-icon13.png',
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			selectLeft(key) {
			    const self = this;
				console.log('self.submitData.score',self.submitData.score);
				console.log('key',key);
			    if (self.submitData.score == 0.5 && key == 0.5) {
			      self.submitData.score = 0;
			    }else{
					self.submitData.score = key
				}				
			    console.log("得" + self.submitData.score + "分")	 
				console.log(self.submitData)	 
			  },
			  
			
			  selectRight(key) {
				const self = this;
				self.submitData.score = key
			    console.log("得" + key + "分")
			  },

		
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.id
				};
				postData.getAfter = {
					lawyer:{
						tableName:'Article',
						middleKey:'lawyer',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'='
					},
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
						self.submitData.order_no = self.mainData.order_no;
						if(self.mainData.lawyer.length>0){
							self.submitData.relation_id = self.mainData.lawyer[0].id;
							self.mainData.lawyer[0].keywords = self.mainData.lawyer[0].keywords.split(',')
						};
						console.log(self.mainData.lawyer[0].keywords)
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');
		
				};
				self.$apis.orderGet(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData', self.submitData)
				if (pass) {
						const callback = (user, res) => {
							self.messageAdd();
						};
						self.$Utils.getAuthSetting(callback);
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.saveAfter = [{
				  tableName:'OrderItem',
				  FuncName:'update',
				  searchItem:{
				    order_no:self.mainData.order_no
				  },
				  data:{
				    isremark:1,
				  }
				}];
				const callback = (data) => {
			
					if (data.solely_code == 100000) {
			
						self.$Utils.showToast('评价成功', 'none')
						setTimeout(function() {
							uni.navigateBack({
								delta: 1
							})
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}
			
				};
				self.$apis.messageAdd(postData, callback);
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
	.star-image {
	  position: absolute;
	  width: 40rpx;
	  height: 40rpx;
	  src: "../../images/home-icon11.png";
	}
	 
	.startItem {
	  position: absolute;
		top:0;
	  width: 20rpx;
	  height: 40rpx;
	}
</style> 
