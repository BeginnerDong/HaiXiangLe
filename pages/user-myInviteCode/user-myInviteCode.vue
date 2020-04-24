<template>
	<view>
		<view class="codeTop flexCenter pubBj white">
			<view class="flexColumn">
				<view class="photo">
					<image :src="userData.headImgUrl?userData.headImgUrl:''" mode=""></image>
				</view>
				<view class="pdt15">{{userData.nickname}}</view>
			</view>
		</view>
		<view class="pdlr4 pdt20 center">
			<view class="mgb10 fs15">我的邀请码</view>
			<view class="codeEwm">
				<image :src="userData.mainImg&&userData.mainImg[0]?userData.mainImg[0].url:''" mode=""></image>
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
				userData:{}
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getUserData'], self);
		},
		methods: {
			
			getUserData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.solely_code == 100000&&res.info.data[0]) {
						self.userData = res.info.data[0]
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getUserData');
					
				};
				self.$apis.userGet(postData, callback);
			},

			
		},
	};
</script>

<style>
	.codeTop{height: 257rpx;}
	.photo{width: 100rpx;height: 100rpx;border-radius: 50%;overflow: hidden;}
	.codeEwm{width: 400rpx;height: 400rpx;margin: 0 auto;}
</style>
