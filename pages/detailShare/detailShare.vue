<template>
	<view>
		<view class="backBtn" @click="pre()"><image src="../../static/images/back-icon.png" mode=""></image></view>
		<view class="sharePic">
			<image src="../../static/images/posterst-img.png" mode=""></image>
		</view>
		
		<view class="fixEwm"><image src="../../static/images/posterst-img1.png" mode=""></image></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				wx_info:{},
				is_show:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			pre(){
				this.$router.go(-1);
			},
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	/* @import "../../assets/style/detail.css"; */
	page{padding-bottom:140rpx;}
	.backBtn{width:20rpx;height: 35rpx;position: absolute;top: 50rpx;left: 20rpx; z-index: 1;padding: 0 20rpx;}
	.sharePic{position: fixed;top: 0;right: 0;bottom: 0;left: 0;width: 100%;height: 100%;}
	.fixEwm{width: 200rpx;height: 200rpx;position: fixed;right:60rpx;bottom: 60rpx;z-index: 10;}
</style>
