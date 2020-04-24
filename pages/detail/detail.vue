<template>
	<view>
		<div id="poster" style="z-index:-999;position: absolute;left: 0;top: 0;right: 0;">
			<div class="img">
				<img class="img-one" crossOrigin="anonymous" :src="mainData.posterImg&&mainData.posterImg[0]?mainData.posterImg[0].url+'?'+new Date().getTime():''"
				 style="width:100%;height:100%" />
				<!-- <img class="img-one" src="../../static/images/达人/img2.png" /> -->
			</div>
			<div class="ilblock imgb">
				<img :src="QrData&&QrData.url?QrData.url+'?'+new Date().getTime():''" crossOrigin="anonymous" />
				<!-- <img src="../../static/images/达人/img8.png" /> -->
			</div>
		</div>
		
		<div style="width:100%;height:100%;position:fixed;top:0;background:black;opacity:0.6;z-index:666" v-if="showPoster"></div>
		<div style="z-index:999;width:80%;height:80%;position: fixed;top: 10%;left:10%" v-if="showPoster">
			<img :src="url" style="width:100%;height:100%" />
			
		</div>
		<view class="userTit fs13 whiteBj boxShaow">
			<view class="flex">
				<view class="userPhoto mgr10"><image :src="userData.headImgUrl?userData.headImgUrl:''" mode=""></image></view>
				<view>{{userData.nickname}}</view>
			</view>
		</view>
		<view class="userTitH"></view>
		<view class="banner-box pr">
			<view class="shareBtn" @click="url==''?getQrData():ifShowPoster()" v-if="userData.primary_scope>10">会员分享</view>
			<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#86e2b1">
				<block v-for="(item,index) in mainData.bannerImg" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item.url" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="pdlr4 pdb10" style="background-color: #fff;">
			<view class="fs15 pdtb10" style="line-height: 44rpx;">{{mainData.title}}</view>
			<view class="flexRowBetween">
				<view class="flex">
					<view class="price fs16 ftw mgr5">{{mainData.sku&&mainData.sku[specsCurr]?mainData.sku[specsCurr].price:''}}</view>
					<view class="yuanJia fs10 color9">门市价￥{{mainData.sku&&mainData.sku[specsCurr]?mainData.sku[specsCurr].o_price:''}}</view>
				</view>
				<view class="flex fyBtn fs10 mgl5" v-if="userData.primary_scope>10">
					<view class="btn mgr10 flex">
						<view class="tt">返佣</view>
						<view class="fyNmy">￥{{mainData.class_one}}</view>
					</view>
					<view class="btn red flex">
						<view class="tt">返佣</view>
						<view class="fyNmy">￥{{mainData.class_two}}</view>
					</view>
				</view>
			</view>
			<view class="flexRowBetween pdt10 color9 fs12">
				<view>销售量：{{mainData.sale_count}}</view>
				<view>库存量：{{mainData.stock}}</view>
			</view>
		</view>
		
		<view class="f5H10"></view>
		<view class="pdlr4 pdt15 pdb15" style="background-color: #fff;">
			<view class="fs15">规格</view>
			<view class="flex" style="flex-wrap: wrap;">
				<view class="specsBtn" :class="specsCurr==index?'on':''" 
				@click="specsChange(index)" v-for="(item,index) in mainData.sku" :key="index">{{item.title}}</view>
			</view>
			
		</view>
		<view class="f5H10"></view>
		<view class="pdlr4 pdt15 pdb15 fs13" style="background-color: #fff;">
			<view class="fs15">商家地址</view>
			<view class="mgt10 flexRowBetween" @click="phoneCall">
				<view class="flex">
					<view>电话：</view>
					<view class="msgText">{{mainData.shop&&mainData.shop[0]?mainData.shop[0].phone:''}}</view>
				</view>
				<view class="adrsIcon"><image src="../../static/images/detailsl-icon1.png" mode=""></image></view>
			</view>
			<view class="mgt10 flexRowBetween" @click="openMap()">
				<view class="flex">
					<view>地址：</view>
					<view class="msgText">{{mainData.shop&&mainData.shop[0]?mainData.shop[0].address:''}}</view>
				</view>
				<view class="adrsIcon" ><image src="../../static/images/detailsl-icon2.png" mode=""></image></view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="pdlr4 pdt15 xqInfor" style="background-color: #fff;">
			<view class="fs15 pdb15">图文详情</view>
			<view class="cont fs13" style="min-height: 100px;">
				<view class="content ql-editor" style="padding:0;"
				v-html="mainData.content">
				</view>
			</view>
		</view>
		
		<view class="xqbotomBar center color6" style="height: 100rpx;">
			<view class="threeItem" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">商城首页</view>
			<view class="threeItem" @click="kefuShow">客服微信</view>
			<view class="threeItem buy" v-show="mainData.is_Buying&&!mainData.is_noStock" 
			@click="goBuy()">立即购买</view>
			<view class="threeItem buy" v-show="mainData.is_notBuying&&!mainData.is_noStock">预售中</view>
			<view class="threeItem buy" style="background: #d0d0d0;" v-show="mainData.is_noStock">已售罄</view>
		</view>
		
		<!-- 微信客户弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="kefuShow center whiteBj radius10" v-show="is_kefuShow">
			<view class="closeBtn" @click="kefuShow">×</view>
			<view class="fs15">客服微信</view>
			<view class="pdtb15 fs13">{{kefuData.description}} (手机号同微信)</view>
			<view class="flexCenter">
				<view class="ewm"><image :src="kefuData.mainImg&&kefuData.mainImg[0]?kefuData.mainImg[0].url:''" mode=""></image></view>
			</view>
			
		</view>
		
	</view>
</template>

<script>
	import html2canvas from '@/common/html2canvas.js'
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
				is_buyBtn:false,
				userData:{},
				mainData:{},
				kefuData:{},
				showPoster: false,
				url: '',
				QrData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			console.log(options);
			var options = self.$Utils.getHashParameters();
			console.log(options);
			self.id = options[0].id;
			if(options[0].shareUser){
				self.shareUser = options[0].shareUser
				const callback = (res) => {
					self.$Utils.loadAll(['getUserData','getMainData','getKefuData','wxJsSdk'], self);
				};
				self.$Token.getProjectToken(callback, {
					refreshToken: true,parent_no:options.shareUser
				})
			}else{
				self.$Utils.loadAll(['getUserData','getMainData','getKefuData','wxJsSdk'], self);
			}
			console.log('self.id',self.id)
			console.log('self.shareUser',self.shareUser)
		},
		
		onReady() {
			const self = this;
			self.$Utils.loadAll(['wxJsSdk'], self);
		},
		
		onShow() {
			const self = this;
			self.orderList = [];
			uni.removeStorageSync('payPro');
		},
		
		methods: {
			
			wxJsSdk() {
				const self = this;
				const postData = {
					thirdapp_id: 2,
					url: window.location.href
				};
				const callback = (res) => {
					self.$jweixin.config({
						debug: false, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
						appId: res.appId, // 必填，公众号的唯一标识
						timestamp: res.timestamp, // 必填，生成签名的时间戳
						nonceStr: res.nonceStr, // 必填，生成签名的随机串
						signature: res.signature, // 必填，签名
						jsApiList: ['openLocation'] // 必填，需要使用的JS接口列表
					});
					self.$jweixin.ready(function() { //需在用户可能点击分享按钮前就先调用
					});
					self.$jweixin.error(function(res) {
						console.log('error', res)
					});
					self.$Utils.finishFunc('wxJsSdk');
				};
				self.$apis.WxJssdk(postData, callback);
			},
			
			ifShowPoster() {
				const self = this;
				self.showPoster = !self.showPoster
			},
			
			getQrData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.param = 'http://test.solelyfinance.com/wx/?shareUser=' + uni.getStorageSync(
						'user_no') + '&id=' + self.mainData.id + '#/pages/detail/detail',
					postData.ext = 'png';
				const callback = (res) => {
					console.log(res);
					self.QrData = res.info;
					console.log(9990, self.QrData)
					html2canvas(document.getElementById("poster"), {
						width: 375,
						height: 665,
						useCORS: true,
						allowTaint: false,
						taintTest: true,
					}).then(function(canvas) {
						var imgUrl = canvas.toDataURL();
						self.url = imgUrl;
						console.log('self.url', self.url)
			
			
					});
					self.showPoster = true
			
				};
				self.$apis.getQrCommonCode(postData, callback);
			},
			
			goBuy(){
				const self = this;
				uni.setStorageSync('canClick',false);
				self.orderList.push(
					{sku_id:self.mainData.sku[self.specsCurr].id,count:1,
					type:self.mainData.type,product:self.mainData,skuIndex:self.specsCurr},
				);
				uni.setStorageSync('payPro', self.orderList);
				if(self.mainData.type==1){
					if(self.shareUser){
						self.Router.navigateTo({route:{path:'/pages/orderConfim-yuyue/orderConfim-yuyue?shareUser='+self.shareUser}})
					}else{
						self.Router.navigateTo({route:{path:'/pages/orderConfim-yuyue/orderConfim-yuyue'}})
					}
					
				}else{
					if(self.shareUser){
						self.Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim?shareUser='+self.shareUser}})
					}else{
						self.Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})
					}
				}
				
				uni.setStorageSync('canClick',true);
			},
			
			openMap(){
				const self = this;
				
				self.$jweixin.openLocation({
				    latitude: parseFloat(self.mainData.shop[0].latitude),
				    longitude: parseFloat(self.mainData.shop[0].longitude),
					name:'',
					address:self.mainData.shop[0].address,
				    success: function () {
				        console.log('success');
				    }
				});
			},
			
			phoneCall(){
				const self = this;
				uni.makePhoneCall({
				    phoneNumber: self.kefuData.description //仅为示例
				});
			},
			
			getKefuData() {
				const self = this;
				const postData = {
					searchItem: {
						title:'客服'
					}
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.kefuData = res.info.data[0];
					}
					console.log('res', res)
					self.$Utils.finishFunc('getKefuData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			tokenGet() {
				const self = this;
				const postData = {
					searchItem: {
						user_no: 'U411908080398720'
					}
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.userData = res.info;
						uni.setStorageSync('user_token', res.token);
						uni.setStorageSync('user_no', res.info.user_no);
						uni.setStorageSync('user_info', res.info);
					}
					console.log('res', res)
					self.$Utils.finishFunc('tokenGet');
				};
				self.$apis.tokenGet(postData, callback);
			},
			
			getUserData() {
				const self = this;
				console.log(2323434)
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
			
			specsChange(index){
				const self = this;
				self.specsCurr = index;
				
			},
			
			kefuShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_kefuShow = !self.is_kefuShow
			},
			
			getMainData() {
				const self = this;
				var now = new Date().getTime();
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id:self.id
				};
				postData.getAfter = {
					sku: {
						tableName: 'Sku',
						middleKey: 'product_no',
						key: 'product_no',
						condition: '=',
						searchItem: {
							status: 1
						}
					},
					shop: {
						token:uni.getStorageSync('user_token'),
						tableName: 'UserInfo',
						middleKey: 'shop_no',
						key: 'user_no',
						condition: '=',
						searchItem: {
							status: 1
						}
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						if(self.mainData.stock==0||self.mainData.sellout==1){
							self.mainData.is_noStock = true
						};
						if(parseInt(self.mainData.end_time)>now){
							self.mainData.is_notBuying = true
						}else{
							self.mainData.is_Buying = true
						}
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					self.$Utils.finishFunc('getMainData');
						
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proList.css";
	@import "../../assets/style/detail.css";
	
	page{padding-bottom:140rpx;}
	view{box-sizing: border-box;}
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
	.img{
		width: 100%;
		height: 1330rpx;
	}
	.img-one{
		width: 100%;
		height: 100%;
		/* margin-left: 15px; */
	}
	.imgb{
		width: 60px;
		height: 60px;
		margin: 10px;
		position: absolute;
		bottom: 0;
	}
	.imgb img{
		width: 60px;
		height: 60px;
	}
	.ilblock{
		display: inline-block;
	}
</style>
