<template>
	<view>
		<view class="backBtn" @click="pre()"><image src="../../static/images/back-icon.png" mode=""></image></view>
		<view class="mapBox">
			<map style="width: 100%; height: 100%;" :latitude="latitude" :longitude="longitude" :markers="covers">
			</map>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				wx_info:{},
				is_show:false,
				id:0, // 使用 marker点击事件 需要填写id
				title: 'map',
				latitude: 39.909,
				longitude: 116.39742,
				covers: [{
					latitude: 39.909,
					longitude: 116.39742,
					iconPath: '../../../static/location.png'
				}, {
					latitude: 39.90,
					longitude: 116.39,
					iconPath: '../../../static/location.png'
				}]
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
	.mapBox{position: fixed;top: 0;right: 0;bottom: 0;left: 0;width: 100%;height: 100%;}
</style>
