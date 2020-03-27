<template>
	<view>
		
		<view class="userHead pr pdt30">
			<view class="headbj"><image src="../../static/images/about-img.png" mode=""></image></view>
			<view class="infor radius10 boxShaow whiteBj">
				<view class="flexCenter">
					<image class="photo" src="../../static/images/about-img1.png" mode=""></image>
				</view>
				
				<view class="center mgt5">快乐的猫</view>
				<view class="userBox2 flexRowBetween color6 fs12 mgt20 center">
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<view class="icon">
							<image src="../../static/images/about-icon.png"></image>
						</view>
						<view>全部订单</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<view class="icon">
							<image src="../../static/images/about-icon1.png"></image>
						</view>
						<view>已支付</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<view class="icon">
							<image src="../../static/images/about-icon2.png"></image>
						</view>
						<view>已预约</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<view class="icon">
							<image src="../../static/images/about-icon3.png"></image>
						</view>
						<view>已完成</view>
					</view>
				</view>
				
			</view>
		</view>
		
		<view class="myRowBetween fs13 mglr4 whiteBj radius10 mgt15">
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-myMoney/user-myMoney'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon4.png" mode=""></image>
					<view>我的金库</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon10.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-myInviteCode/user-myInviteCode'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon5.png" mode=""></image>
					<view>我的邀请码</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon10.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-myTeam/user-myTeam'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon6.png" mode=""></image>
					<view>我的团队</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon10.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon7.png" mode=""></image>
					<view>地址管理</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon10.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-ContactUs/user-ContactUs'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon8.png" mode=""></image>
					<view>联系我们</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon10.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/business-login/business-login'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon9.png" mode=""></image>
					<view>商家登录</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon10.png" mode=""></image></view>
			</view>
		</view>
			
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/hotProduct/hotProduct'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">爆品</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
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


		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/navbar.css";
	page{padding-bottom: 140rpx;background: #F5F5F5;}
	
	.headbj{position: absolute;left: 0;right: 0;top: 0;}
	.headbj image{width: 100%;height: 258rpx;}
	.userHead .infor{height: 360rpx;margin: 0 4%;box-sizing: border-box;position:relative ;margin-top: 30rpx; z-index: 1;padding: 40rpx 20rpx 30rpx 20rpx;}
	
	.userHead .photo{width: 110rpx;height: 110rpx;border-radius: 50%;box-sizing: border-box;}
	
	.userBox2{width: 100%;height: 92rpx;box-sizing: border-box;}
	.userBox2 .item{width: 25%;}
	.userBox2 .item .icon{width: 42rpx;height:42rpx;display: block;margin: 0 auto;margin-bottom: 10rpx;position: relative;}
	.userBox2 .item .icon image{width: 100%;height: 100%;}
	
	.myRowBetween .item{padding: 30rpx 4%;}
	.myRowBetween .ll .icon{width: 30rpx;height: 32rpx;margin-right: 20rpx;}
	
	
</style>
