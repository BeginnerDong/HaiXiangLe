<template>
	<view v-if="showAll">
		
		<view class="logo pr center pubColor" style="font-size: 100rpx;line-height: 100rpx;padding: 180rpx 4% 140rpx 4%;">Welcome</view>
		
		<view class="loginCont">
			<view class="item flex mgb20">
				<input type="text" v-model="submitData.login_name" placeholder="账号" placeholder-class="placeholder">
			</view>
			<view class="item flex mgb20">
				<input type="password" v-model="submitData.password" placeholder="密码" placeholder-class="placeholder">
			</view>
			
			<view class="item submitbtn" style="margin-top: 60rpx; padding: 0;border: 0;"  @click="submit">
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
				submitData:{
					login_name:'',
					password:''
				},
				showAll:false
			}
		},
		
		onLoad(options) {
			const self = this;
			if (uni.getStorageSync('staffToken')) {
				uni.redirectTo({
					url: '/pages/business-order/business-order'
				})
			}else{
				self.showAll = true
			}
		},
		
		methods: {
			
			submit() {
				const self = this;
			
				const postData = {
					login_name: self.submitData.login_name,
					password:self.submitData.password
				};
				if (self.$Utils.checkComplete(self.submitData)) {
					
					const callback = (res) => {
						if (res.solely_code == 100000) {
							console.log(res);
							uni.setStorageSync('staffToken', res.token);
							uni.setStorageSync('staffInfo', res.info);
							uni.redirectTo({
								url: '/pages/business-order/business-order'
							}) 
						} else {
							self.$Utils.showToast(res.msg,'none')
						}
					}
					self.$apis.shopLogin(postData, callback);
				} else {
					self.$Utils.showToast('请补全登录信息', 'none')
				};
			},
			
		},
	};
</script>

<style>
	
	page{padding-bottom: 40rpx;background: #f5f5f5;}	
	.loginBj{width: 444rpx;height: 444rpx;margin: 0 auto;}
	.loginBj image{width: 100%;height: 100%;}
	
	.loginCont{width:90%;margin: 0 auto;z-index: 2;padding:0 60rpx;box-sizing: border-box;}
	.loginCont .item{border-radius: 45rpx;height: 80rpx;box-sizing: border-box;padding: 0 30rpx;border: 1px solid #c3c3c3;overflow: hidden;}
	.loginCont .item input{font-size: 26rpx;height: 80rpx;line-height: 44rpx;padding: 20rpx 0 16rpx 0;width: 100%;box-sizing: border-box;}
</style>
