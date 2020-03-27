<template>
	<view>
		
		<view class="userTit fs13 flexRowBetween whiteBj">
			<view class="flex">
				<view class="userPhoto mgr10"><image src="../../static/images/home-img.png" mode=""></image></view>
				<view>快乐的猫</view>
			</view>
			<view class="flexEnd">西安站</view>
		</view>
		<view class="userTitH"></view>
		
		<view class="pdlr4">
			<view class="proList">
				<view class="item boxShaow" v-for="(item,index) in proData" :key="index" @click="Router.navigateTo({route:{path:'/pages/detail/detail'}})">
					<view class="xsNum">销售量：662</view>
					<view class="pic">
						<image src="../../static/images/home-img1.png" mode=""></image>
					</view>
					<view class="tit borderB1">美食接发大水考虑价格就过来就是大概工人更进反馈聊到几个大概就分类三都感觉刚放假东立国际割发代首卡啦啦啦</view>
					<view class="infor">
						<view class="flexRowBetween">
							<view class="ll">
								<view class="flex">
									<view class="price fs16 ftw mgr5">165.99</view>
									<view class="yuanJia fs10 color9">门市价￥230</view>
								</view>
								<view class="flex fyBtn fs10 mgt5">
									<view class="btn mgr10 flex">
										<view class="tt">返佣</view>
										<view class="fyNmy">￥23</view>
									</view>
									<view class="btn red flex">
										<view class="tt">返佣</view>
										<view class="fyNmy">￥13</view>
									</view>
								</view>
							</view>
							<view class="rr">
								<view class="buyBtn green"  v-show="item.is_buyGreen">抢购中</view>
								<view class="buyBtn red" v-show="item.is_buyRed">预售</view>
								<view class="buyBtn gray" v-show="item.is_buyGray">已售罄</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/hotProduct/hotProduct'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">爆品</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">我的</view>
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
				is_show: false,
				wx_info:{},
				proData:[
					{is_buyGreen:true},
					{is_buyRed:true},
					{is_buyGray:true}
				]
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
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/proList.css";
	
	page{padding-bottom: 140rpx;background: #F5F5F5;}	
	
	
</style>
