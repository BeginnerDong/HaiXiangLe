<template>
	<view>
		<view class="userTit fs13 whiteBj">
			<view class="flex">
				<view class="userPhoto mgr10"><image src="../../static/images/home-img.png" mode=""></image></view>
				<view>快乐的猫</view>
			</view>
		</view>
		<view class="userTitH"></view>
		<view class="banner-box pr">
			<view class="shareBtn" @click="Router.navigateTo({route:{path:'/pages/detailShare/detailShare'}})">会员分享</view>
			<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#86e2b1">
				<block v-for="(item,index) in labelData" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="pdlr4 pdb10">
			<view class="fs15 pdtb10" style="line-height: 44rpx;">【西安】29层云顶餐厅吃五星自助~仅178-2大2小抢悦豪自助餐~海鲜烧烤的號惬盛宴~使用7月底！菜品丰富！吃到扶墙走！</view>
			<view class="flexRowBetween">
				<view class="flex">
					<view class="price fs16 ftw mgr5">165.99</view>
					<view class="yuanJia fs10 color9">门市价￥230</view>
				</view>
				<view class="flex fyBtn fs10 mgl5">
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
			<view class="flexRowBetween pdt10 color9 fs12">
				<view>销售量：662</view>
				<view>库存量：662</view>
			</view>
		</view>
		
		<view class="f5H10"></view>
		<view class="pdlr4 pdt15 pdb15">
			<view class="fs15">规格</view>
			<view class="flex" style="flex-wrap: wrap;">
				<view class="specsBtn" :class="specsCurr==index?'on':''" @click="specsChange(index)" v-for="(item,index) in specsData" :key="index">{{item}}</view>
			</view>
			
		</view>
		<view class="f5H10"></view>
		<view class="pdlr4 pdt15 pdb15 fs13">
			<view class="fs15">商家地址</view>
			<view class="mgt10 flexRowBetween">
				<view class="flex">
					<view>电话：</view>
					<view class="msgText">15623562389</view>
				</view>
				<view class="adrsIcon"><image src="../../static/images/detailsl-icon1.png" mode=""></image></view>
			</view>
			<view class="mgt10 flexRowBetween">
				<view class="flex">
					<view>地址：</view>
					<view class="msgText">陕西省西安市雁塔区高新大都荟4号楼</view>
				</view>
				<view class="adrsIcon" @click="Router.navigateTo({route:{path:'/pages/detailMap/detailMap'}})"><image src="../../static/images/detailsl-icon2.png" mode=""></image></view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="mglr4 pdt15 xqInfor">
			<view class="fs15 pdb15">图文详情</view>
			<view class="cont fs13">
				<view class="center">放假开个会过分的话看</view>
				<view class="center">局分类山沟供货方度高还是个费刚发的</view>
				<view><image class="w" src="../../static/images/detailsl-img1.png" mode="widthFix"></image></view>
				<view><image class="w" src="../../static/images/detailsl-img2.png" mode="widthFix"></image></view>
				<view class="center">过几天个人果然考科四噢</view>
				<view class="center">刚发的给家人说公开课看个就看了</view>
				<view><image class="w" src="../../static/images/detailsl-img1.png" mode="widthFix"></image></view>
			</view>
		</view>
		
		<view class="xqbotomBar center color6" style="height: 100rpx;">
			<view class="threeItem" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">商城首页</view>
			<view class="threeItem" @click="kefuShow">客服微信</view>
			<view class="threeItem buy" v-show="!is_buyBtn" @click="Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})">立即购买</view>
			<view class="threeItem buy" v-show="is_buyBtn">预售中</view>
			<view class="threeItem buy" style="background: #d0d0d0;" v-show="is_buyBtn">已售罄</view>
		</view>
		
		<!-- 微信客户弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="kefuShow center whiteBj radius10" v-show="is_kefuShow">
			<view class="closeBtn" @click="kefuShow">×</view>
			<view class="fs15">客服微信</view>
			<view class="pdtb15 fs13">15623562356 (手机号同微信)</view>
			<view class="flexCenter">
				<view class="ewm"><image src="../../static/images/invite-codel-img1.png" mode=""></image></view>
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
				wx_info:{},
				is_show:false,
				labelData: [
					"../../static/images/detailsl-img.png",
					"../../static/images/detailsl-img.png",
					"../../static/images/detailsl-img.png"
				],
				specsCurr:0,
				specsData:['2盒装','3盒装'],
				is_kefuShow:false,
				is_buyBtn:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			specsChange(index){
				const self = this;
				self.specsCurr = index
			},
			kefuShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_kefuShow = !self.is_kefuShow
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
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proList.css";
	@import "../../assets/style/detail.css";
	
	page{padding-bottom:140rpx;}
	.shareBtn{width: 160rpx;height: 60rpx;text-align: center;color: #fff;line-height: 60rpx;border-radius: 30rpx 0 0 30rpx;position: absolute;top: 78rpx;right: 0;z-index: 1;background: #86E2B1;}
	.swiper-box {height: 400rpx;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item{width: 100%;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item image{width: 100%;height: 100%;box-sizing: border-box;}
	
	.specsBtn{min-width: 140rpx;height: 60rpx;line-height: 60rpx;text-align: center;box-sizing: border-box;padding: 0 20rpx;border: 1px solid #c4c4c4; color: #666;border-radius: 10rpx;margin: 20rpx 30rpx 0 0;font-size: 26rpx;}
	.specsBtn.on{border-color:#86e2b1;color: #86E2B1;background: #edfff7;}
	
	.msgText{width: 500rpx;}
	.adrsIcon{width: 40rpx;height: 40rpx;}
	
	.kefuShow{width: 80%;min-height: 300rpx;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 50;box-sizing: border-box;padding: 80rpx 30rpx;}
	.kefuShow .ewm{width: 200rpx;height: 200rpx;}
	 
</style>
