<template>
	<view>
		
		<view class="logo pr center pubColor" style="font-size: 100rpx;line-height: 100rpx;padding: 180rpx 4% 140rpx 4%;">Welcome</view>
		
		<view class="loginCont">
			<view class="item flex mgb20">
				<input type="text" value="" placeholder="账号" placeholder-class="placeholder">
			</view>
			<view class="item flex mgb20">
				<input type="password" value="" placeholder="密码" placeholder-class="placeholder">
			</view>
			
			<view class="item submitbtn" style="margin-top: 60rpx; padding: 0;border: 0;" @click="Router.navigateTo({route:{path:'/pages/business-order/business-order'}})">
				<button class="Wbtn" type="submint">立即登录</button>
			</view>
		</view>
		
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: false,
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
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
	
	page{padding-bottom: 40rpx;background: #f5f5f5;}	
	.loginBj{width: 444rpx;height: 444rpx;margin: 0 auto;}
	.loginBj image{width: 100%;height: 100%;}
	
	.loginCont{width:90%;margin: 0 auto;z-index: 2;padding:0 60rpx;box-sizing: border-box;}
	.loginCont .item{border-radius: 45rpx;height: 80rpx;box-sizing: border-box;padding: 0 30rpx;border: 1px solid #c3c3c3;overflow: hidden;}
	.loginCont .item input{font-size: 26rpx;height: 80rpx;width: 100%;box-sizing: border-box;}
</style>
