<template>
	<view>
		
		<view>
			<view class="mglr4">
				<view class="proRow">
					<view class="item">
						<view class="flex titIcon">
							<view class="icon"><image src="../../static/images/confirml-icon.png" mode=""></image></view>
							<view>商家信息</view>
						</view>
						
						<view class="fs14 pdtb15">{{mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product&&
						mainData.orderItem[0].snap_product.product?mainData.orderItem[0].snap_product.product.title:''}}</view>
						<view class="borderB1"></view>
						<view class="pdt15 fs12 color6">
							<view class="iconText flex">
								<view class="sIcon"><image src="../../static/images/confirml-icon1.png" mode=""></image></view>
								<view class="msgText">商家地址：{{mainData.shopInfo&&mainData.shopInfo[0]?mainData.shopInfo[0].address:''}}</view>
							</view>
							<view class="iconText flex">
								<view class="sIcon"><image src="../../static/images/confirml-icon2.png" mode=""></image></view>
								<view class="msgText">商家电话：{{mainData.shopInfo&&mainData.shopInfo[0]?mainData.shopInfo[0].phone:''}}</view>
							</view>
						</view>
					</view>
					
					<view class="item">
						<view class="flex titIcon">
							<view class="icon"><image :src="mainData.qrcode?mainData.qrcode:''" mode=""></image></view>
							<view>二维码</view>
						</view>
						<view class="pdt15 flexCenter">
							<view style="width: 300rpx;height: 300rpx;"><image src="../../static/images/confirml-img.png" mode=""></image></view>
						</view>
						<view class="fs13 color9 mgt10 center">出示二维码即可消费多个订单</view>
					</view>
					
					<view class="item">
						<view class="flex titIcon">
							<view class="icon"><image src="../../static/images/confirml-icon4.png" mode=""></image></view>
							<view>预约备注</view>
						</view>
						<view class="mgt15">{{mainData.passage1!=''?mainData.passage1:'无备注信息'}}</view>
					</view>
					<view class="item">
						<view class="flexRowBetween dashedB1 pdb15">
							<view class="red ftw fs15">电子码：{{mainData.order_no}}</view>
							<view class="dzEWM"><image :src="mainData.qrcode?mainData.qrcode:''" mode=""></image></view>
						</view>
						<view class="pdt15 fs13">未核销</view>
					</view>
					<view class="item">
						<view class="flex titIcon">
							<view class="icon"><image src="../../static/images/confirml-icon5.png" mode=""></image></view>
							<view>订单信息</view>
						</view>
						<view class="mgt15 fs12">
							<view class="msgLine flex">
								<view class="color9 msgtit">用户姓名</view>
								<view class="msgTex">{{mainData.name!=''?mainData.name:''}}</view>
							</view>
							<view class="msgLine flex">
								<view class="color9 msgtit">用户电话</view>
								<view class="msgTex">{{mainData.phone!=''?mainData.phone:''}}</view>
							</view>
							<view class="msgLine flex">
								<view class="color9 msgtit">购买数量</view>
								<view class="msgTex">{{mainData.count!=''?mainData.count:''}}</view>
							</view>
							<!-- <view class="msgLine flex">
								<view class="color9 msgtit">开始核销时间</view>
								<view class="msgTex">2020年3月27日</view>
							</view>
							<view class="msgLine flex">
								<view class="color9 msgtit">有效日期</view>
								<view class="msgTex">2020年3月27日-2020年6月27日</view>
							</view> -->
						</view>
					</view>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id  = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				console.log(2323434)
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.id
				};
				postData.getAfter = {
					orderItem:{
						tableName:'OrderItem',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1
						},
						condition:'='
					},
					shopInfo:{
						tableName:'UserInfo',
						middleKey:'shop_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'='
					},
				};
				const callback = (res) => {
					if (res.solely_code == 100000&&res.info.data[0]) {
						self.mainData = res.info.data[0]
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');
					
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	/* @import "../../assets/style/orderNav.css"; */
	/* @import "../../assets/style/proList.css"; */
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;padding-bottom: 60rpx;}
	
	.titIcon {padding:10rpx 0 20rpx 0;font-size: 30rpx;border-bottom: 1px solid #eee;}
	.titIcon .icon{width: 34rpx;height: 34rpx;margin-right: 16rpx;}
	
	.msgLine{margin-bottom: 30rpx;}
	.msgLine:last-child{margin-bottom: 0;}
	.msgtit{font-size: 24rpx;color: #999;margin-right: 30rpx;}
	.msgTex{font-size: 26rpx;}
	.dzEWM{width: 60rpx;height: 60rpx;}
</style>
