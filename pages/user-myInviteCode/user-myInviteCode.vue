<template>
	<view>
		<view class="codeTop flexCenter pubBj white">
			<view class="flexColumn">
				<view class="photo">
					<image src="../../static/images/invite-codel-img.png" mode=""></image>
				</view>
				<view class="pdt15">快乐的猫</view>
			</view>
		</view>
		<view class="pdlr4 pdt20 center">
			<view class="mgb10 fs15">我的邀请码</view>
			<view class="codeEwm">
				<image src="../../static/images/invite-codel-img1.png" mode=""></image>
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
			
		}
	};
</script>

<style>
	.codeTop{height: 257rpx;}
	.photo{width: 100rpx;height: 100rpx;border-radius: 50%;overflow: hidden;}
	.codeEwm{width: 400rpx;height: 400rpx;margin: 0 auto;}
</style>
