<template>
	<view class="myCace pdlr4">
		<view class="item">
			<view  @click="Router.navigateTo({route:{path:'/pages/myCaseDetail/myCaseDetail'}})">
				<view>标题标题标题标题标题标题标题标题</view>
				<view class="tag ok">已解决</view>
				<view class="lable flexRowBetween">
					<view class="left">刑事类</view>
					<view class="right">2019年8月27日</view>
				</view>
				<view class="info">内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
			</view>
			<view style="display: flex; justify-content: flex-end;padding-top: 20rpx;">
				<view class="plBtn" @click="Router.navigateTo({route:{path:'/pages/myCasePjDetail/myCasePjDetail'}})">评论服务</view>
			</view>
		</view>
		<view class="item">
			<view @click="Router.navigateTo({route:{path:'/pages/myCaseDetail/myCaseDetail'}})">
				<view>标题标题标题标题标题标题标题标题</view>
				<view class="tag ing">解决中</view>
				<view class="lable flexRowBetween">
					<view class="left">刑事类</view>
					<view class="right">2019年8月27日</view>
				</view>
				<view class="info">内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
			</view>
		</view>
		<view class="item">
			<view @click="Router.navigateTo({route:{path:'/pages/myCaseDetail/myCaseDetail'}})">
				<view>标题标题标题标题标题标题标题标题</view>
				<view class="tag no">未解决</view>
				<view class="lable flexRowBetween">
					<view class="left">刑事类</view>
					<view class="right">2019年8月27日</view>
				</view>
				<view class="info">内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
			</view>
		</view>
		

	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
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
	page{ background: #F5F5F5;}
	
</style> 
