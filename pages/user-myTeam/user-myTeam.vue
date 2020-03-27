<template>
	<view>
		
		<view class="myteam flexRowBetween fs13 pdt15 mgt10">
			<view class="item flex" v-for="(item,index) in teamData" :key="index">
				<view class="photo"><image src="../../static/images/teaml-img.png" mode=""></image></view>
				<view class="name">快乐的猫</view>
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
				wx_info:{},
				teamData:[{},{},{},{},{},{}]
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
				self.$apis.orderGet(postData, callback);
			}

		},
	};
</script>

<style>
	.myteam{flex-wrap: wrap;padding: 30rpx 4%;}
	.myteam .item{width: 45%;margin-bottom: 40rpx;}
	.myteam .photo{width: 80rpx;height: 80rpx;border-radius: 50%;overflow: hidden;margin-right: 20rpx;}
	.myteam .name{width:60%;}
</style>
