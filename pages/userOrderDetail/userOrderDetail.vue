<template>
	<view>
		
		<view class="mglr4">
			<view class=" whiteBj boxShaow oh mgt15 mgb15 radius8">
				<view><image class="w" style="height: 8rpx;" src="../../static/images/the-ordert-icon5.png" mode="widthFix"></image></view>
				<view class="pdlr4 pdt15 pdb15 fs14">
					<view class="flex mt-1">
						<view class="mr-3 color2">{{mainData.snap_address&&mainData.snap_address.name?mainData.snap_address.name:''}}</view>
						<view class="color6">{{mainData.snap_address&&mainData.snap_address.phone?mainData.snap_address.phone:''}}</view>
					</view>
					<view class="mgt10">{{mainData.snap_address.city+mainData.snap_address.detail}}</view>
				</view>
			</view>
			<view class="proRow">
				<view class="item mgb15 flexRowBetween" @click="Router.redirectTo({route:{path:'/pages/detail/detail?id='+mainData.orderItem[0].snap_product.product.id}})">
					<view class="pic"><image :src="mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product&&mainData.orderItem[0].snap_product.product&&
							mainData.orderItem[0].snap_product.product.mainImg&&mainData.orderItem[0].snap_product.product.mainImg[0]?mainData.orderItem[0].snap_product.product.mainImg[0].url:''" mode=""></image></view>
					<view class="infor">
						<view class="tit avoidOverflow">{{mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product&&
						mainData.orderItem[0].snap_product.product?mainData.orderItem[0].snap_product.product.title:''}}</view>
						<view class="fs12 color6 ">{{mainData.title}}</view>
						<view class="B-price">
							<view class="flexRowBetween">
								<view class="price">{{mainData.price}}</view>
								<view>×{{mainData.count}}</view>
							</view>
							
						</view>
					</view>
				</view>
			</view>
			
			<view class=" pdlr4 pdt15 pdb15 whiteBj radius8 fs13">
				<view class="flex borderB1">
					<view class=" pdtb10" style="width: 40rpx;height: 40rpx;"><image src="../../static/images/confirml-icon5.png" mode=""></image></view>
					<view class="mgl10">订单信息</view>
				</view>
				<view class="pdt15 flex">
					<view class="color6 fs12 mgr15">购买数量</view>
					<view>{{mainData.count}}</view>
				</view>
				<view class="pdt15 flex">
					<view class="color6 fs12 mgr15">订单金额</view>
					<view class="price">{{mainData.price}}</view>
				</view>
				<view class="pdt15 flex">
					<view class="color6 fs12 mgr15">交易时间</view>
					<view class="">{{mainData.create_time}}</view>
				</view>
				<view class="pdt15 flex">
					<view class="color6 fs12 mgr15">快递单号</view>
					<view class="">{{mainData.express_info!=''?mainData.express_info:'暂无'}}</view>
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
	@import "../../assets/style/detail.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;padding-bottom: 140rpx;box-sizing: border-box;}
	
	/* .tabBtn .tt.on{color2: #222;font-weight: bold;} */
	.tabBtn .tt.on::before{content: "";width: 0;height: 0;border-right:14rpx solid transparent;border-left:14rpx solid transparent;border-top:14rpx solid #ffb2c2;position: absolute;left: 50%;bottom: 0;transform: translateX(-50%);}
	
	.proRow .item .pic{width: 180rpx;height: 180rpx;}
	.proRow .item .infor{width: 70%;height: 180rpx;}
	
	.editMsg{width: 48%;height: 60rpx;padding: 0 10rpx;box-sizing: border-box;border: 1px solid #eee;box-sizing: border-box;text-align: center;}
	.editMsg input{width: 100%;height: 60rpx;line-height: 60rpx;box-sizing: border-box;text-align: center;font-size: 26rpx;}
	
</style>
