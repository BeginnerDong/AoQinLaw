<template>
	<view>
		<view class="infmtDatail flexRowBetween" >
			<view class="topTit" >
				<view class="title avoidOverflow2">{{mainData.title}}</view>
				<view class="lable flexRowBetween">
					<view>{{mainData.description}}</view>
					<view class="time">{{mainData.create_time}}</view>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="seviseDetaCont2 pdlr4" style="padding: 20px 4%;">
			<view class="content ql-editor" style="padding: 0;" v-html="mainData.content">
			</view>
		</view>
		

	</view>
</template>

<script>
	export default {
		data() {
			return {
				
				mainData:{}
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.searchItem = {
					id:self.id
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
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
	@import "../../assets/style/service.css";
	page{padding-bottom: 140rpx;}
	.infmtDatail{}
	.infmtDatail .topTit{padding:20rpx 3%;}
	.title{ font-size: 28rpx; line-height: 44rpx;}
	.lable{ padding-top: 20rpx; font-size: 24rpx; color: #537deb;}
	.lable .time{ color: #999;}
</style>
