<template>
	<view>
		
		<view class="topNavFix f5bj">
			<view class="numHead pdlr4 pdt30 pdb20 flexRowBetween white pubBj pr">
				<view class="quitBtn fs13" @click="loginOff">退出登录</view>
				<view class="item flexColumn on pr">
					<view class="fs18 ftn">56</view>
					<view class="fs13 pdt5">订单数量总数(个)</view>
				</view>
				<view class="item flexColumn">
					<view class="red fs18 ftn">5623</view>
					<view class="fs13 pdt5">订单总金额(元)</view>
				</view>
			</view>
			<view class="tooling_indNav color6 mgt15 mgb15 mglr4">
				<view class="list flexRowBetween text-center whiteBj">
					<view class="tt" :class="currNav==1?'on':''" @click="currNavChange('1')">快递包邮</view>
					<view class="tt" :class="currNav==2?'on':''" @click="currNavChange('2')">到店核销</view>
				</view>
			</view>
			<view class="orderNav whiteBj boxShaow color6 mgb15 flexRowBetween" v-show="currNav==1" >
				<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">全部订单</view>
				<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">待发货</view>
				<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">已发货</view>
				<view class="tt" :class="curr==4?'on':''" @click="changeCurr('3')">已收货</view>
			</view>
			<view class="orderNav whiteBj boxShaow color6 mgb15 flexRowBetween" v-show="currNav==2" >
				<view class="tt" :class="statusTwo==1?'on':''" @click="changeStatusTwo('1')">全部订单</view>
				<view class="tt" :class="statusTwo==2?'on':''" @click="changeStatusTwo('2')">未核销</view>
				<view class="tt" :class="statusTwo==3?'on':''" @click="changeStatusTwo('3')">已核销</view>
			</view>
		</view>
		<view class="topNavH"></view>
		
		<view>
			<view class="mglr4">
				<view class="proRow" v-show="currNav==1">
					<view class="item" v-for="(item,index) in mainData" :key="index">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color6">交易时间：{{item.create_time}}</view>
							<view class="red">{{item.transport_status==0?'待核销':'已核销'}}</view>
						</view>
						<view class="flexRowBetween">
							<view class="pic">
								<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&item.orderItem[0].snap_product.product&&
							item.orderItem[0].snap_product.product.mainImg&&item.orderItem[0].snap_product.product.mainImg[0]?item.orderItem[0].snap_product.product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs14">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&
						item.orderItem[0].snap_product.product?item.orderItem[0].snap_product.product.title:''}}</view>
								<view class=" fs12 color6">{{item.title}}</view>
								<view class="B-price flexRowBetween">
									<view class="price fs15 ftw">{{item.price}}</view>
									<view class="fs13">×{{item.count}}</view>
								</view>
							</view>
						</view>
						<view class="borderB1 mgt15"></view>
						<view class="pdt15 fs12 color6">
							<view class="iconText">姓名：{{item.name}}</view>
							<view class="iconText flex">电话：{{item.phone}}</view>
						</view>
						<view class="borderB1 mgt15"></view>
						<view class="underBtn flexEnd pdlr4 mgt15">
							<view class="Bbtn" style="width: 190rpx;" @click="orderNumberShow">填写快递单号</view>
						</view>
						<view class="pdt15 fs12 color6">快递单号：54654643513213456</view>
					</view>
				</view>
				<view class="proRow" v-show="currNav==2">
					<view class="item" v-for="(item,index) in mainData" :key="index">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color6">交易时间：{{item.create_time}}</view>
							<view class="red">{{item.transport_status==0?'待核销':'已核销'}}</view>
						</view>
						<view class="flexRowBetween">
							<view class="pic">
								<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&item.orderItem[0].snap_product.product&&
							item.orderItem[0].snap_product.product.mainImg&&item.orderItem[0].snap_product.product.mainImg[0]?item.orderItem[0].snap_product.product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs14">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&
						item.orderItem[0].snap_product.product?item.orderItem[0].snap_product.product.title:''}}</view>
								<view class=" fs12 color6">{{item.title}}</view>
								<view class="B-price flexRowBetween">
									<view class="price fs15 ftw">{{item.price}}</view>
									<view class="fs13">×{{item.count}}</view>
								</view>
							</view>
						</view>
						<view class="borderB1 mgt15"></view>
						<view class="flexRowBetween pdt15 fs12">
							<view>预约时间：2020.02.03 16:00</view>
							<view>{{item.name}}<span class="mgl5">{{item.phone}}</span></view>
						</view>
						<view class="f5bj pdlr4 pdt10 pdb10 fs12 color6 radius8 mgt10">
							<view>备注信息：附近即可落地生根两会上公开更好发动机是客户高考分数高刚恢复健康的说法</view>
						</view>
						<!-- <view class="pdt15 fs12 color6">
							<view class="iconText">姓名：{{item.name}}</view>
							<view class="iconText flex">电话：{{item.phone}}</view>
						</view> -->
					</view>
				</view>
			</view>
		</view>
		
		<!-- 快递单号弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="orderNumber whiteBj radius10" v-show="is_orderNumber">
			<view class="closebtn" @click="orderNumberShow">×</view>
			<view class="submitbtn mgt20">
				<input class="Wbtn" type="number" style="background-color: #F5F5F5;line-height: 40rpx;padding: 20rpx 0;box-sizing: border-box;color: #222;" placeholder="请输入单号" placeholder-class="placeholder">
			</view>
			<view class="submitbtn mgt25">
				<view class="Wbtn">确定</view>
			</view>
		</view>
		
		<!-- 右侧固定按钮 -->
		<view class="R-fixIcon"  @click="scan()">
			<image src="../../static/images/merchantsl-icon.png" mode=""></image>
		</view>
		
		<!-- 无数据 -->
		<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
		<!-- <view class="submitbtn">
			<view class="btn"  style="position: fixed;margin-left: 10%;bottom: 10%;">退出登录</view>
		</view> -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show:false,
				curr:1,
				currNav:1,
				mainData:[],
				searchItem:{
					pay_status:1,
					type:1,
					
					user_type:0
				},
				statusTwo:1,
				is_orderNumber:false
			}
		},
		
		onReady() {
			const self = this;
			//self.$Utils.loadAll(['wxJsSdk'], self);
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['wxJsSdk'], self);
		},
		
		onShow() {
			const self = this;
			self.getMainData(true)
		},
		
		methods: {
			currNavChange(currNav){
				const self = this;
				if(currNav!=self.currNav){
					self.currNav = 	currNav
				}
			},
			orderNumberShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_orderNumber = !self.is_orderNumber;
			},
			loginOff(){
				const self = this;
				uni.removeStorageSync('staffInfo');
				uni.removeStorageSync('staffToken');
				self.Router.redirectTo({route:{path:'/pages/user/user'}})
			},
			
			scan() {
				const self = this;
				self.$jweixin.scanQRCode({
					needResult: 1, // 默认为0，扫描结果由微信处理，1则直接返回扫描结果，
					scanType: ["qrCode", "barCode"], // 可以指定扫二维码还是一维码，默认二者都有
					success: function(res) {
						var result = res.resultStr; // 当needResult 为 1 时，扫码返回的结果
						console.log('result', result)
						self.Router.navigateTo({route:{path:'/pages/business-orderHX/business-orderHX?id='+result}})
					}
				});
			},
			
			wxJsSdk() {
				const self = this;
				const postData = {
					thirdapp_id: 2,
					url: location.href.split('#')[0]
				};
				const callback = (res) => {
					self.$jweixin.config({
						debug: false, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
						appId: res.appId, // 必填，公众号的唯一标识
						timestamp: res.timestamp, // 必填，生成签名的时间戳
						nonceStr: res.nonceStr, // 必填，生成签名的随机串
						signature: res.signature, // 必填，签名
						jsApiList: ['scanQRCode'] // 必填，需要使用的JS接口列表
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
			
			changeCurr(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr
					if(self.curr==1){
						delete self.searchItem.transport_status
					}else if(self.curr==2){
						self.searchItem.transport_status = 0
					}else if(self.curr==3){
						self.searchItem.transport_status = 2
					}
					self.getMainData(true)
				}
			},
			changeStatusTwo(statusTwo){
				const self = this;
				if(statusTwo!= self.statusTwo){
					self.statusTwo = statusTwo
					if(self.statusTwo==1){
						delete self.searchItem.transport_status
					}else if(self.statusTwo==2){
						self.searchItem.transport_status = 0
					}else if(self.statusTwo==3){
						self.searchItem.transport_status = 2
					}
					self.getMainData(true)
				}
			},
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getStaffToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.shop_no = uni.getStorageSync('staffInfo').user_no;
				postData.getAfter = {
					orderItem:{
						tableName:'OrderItem',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1,
							user_type:0
						},
						condition:'='
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proList.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	.topNavFix{width: 100%;height: 400rpx;position: fixed;top: 0rpx;right: 0;left: 0;box-sizing: border-box;z-index: 22;}
	.topNavH{height: 400rpx;}
	
	.numHead .item{width: 50%;position: relative;}
	.numHead .item.on::before{width: 2px;height: 60rpx;background-color: #fff;position: absolute;top:50%;right: 0;transform: translateY(-50%);}
	.quitBtn{width: 130rpx;height: 50rpx;line-height: 50rpx;background-color: #40c77f;position: absolute;right: 0;top: 16rpx;text-align: center;border-radius: 30rpx 0 0 30rpx;}
	
	.orderNumber{width: 90%;min-height: 420rpx;box-sizing: border-box;position: fixed;top: 45%;left: 50%;transform: translate(-50%,-50%);z-index: 50;padding: 80rpx 40rpx 100rpx 40rpx;}
</style>
