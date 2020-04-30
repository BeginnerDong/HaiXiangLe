<template>
	<view>
		<view class="topNavH"></view>
		<view class="topNavFix f5bj">
			<view class="orderNav whiteBj boxShaow color6 mgb15 flexRowBetween">
				<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">全部订单</view>
				<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">已支付</view>
				<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">已预约</view>
				<view class="tt" :class="curr==4?'on':''" @click="changeCurr('4')">已完成</view>
			</view>
		</view>
		
		
		<view>
			<view class="mglr4">
				<view class="proRow">
					
					<view class="item" v-for="(item,index) in mainData" :key="index">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color6">交易时间：{{item.create_time}}</view>
							<view class="red" v-if="item.type==2">已付款</view>
							<view class="red" v-if="item.type==1">已预约</view>
							<view class="red" v-if="item.transport_status==2">已完成</view>
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
						<view class="borderB1 mgt15" v-if="item.type==1"></view>
						<view class="pdtb15 borderB1 fs12 color6" v-if="item.type==1">
							<view class="iconText flex">
								<view class="sIcon"><image src="../../static/images/confirml-icon1.png" mode=""></image></view>
								<view class="sText">商家地址：{{item.shopInfo&&item.shopInfo[0]?item.shopInfo[0].address:''}}</view>
							</view>
							<view class="iconText flex">
								<view class="sIcon"><image src="../../static/images/confirml-icon2.png" mode=""></image></view>
								<view class="sText">商家电话：{{item.shopInfo&&item.shopInfo[0]?item.shopInfo[0].phone:''}}</view>
							</view>
							<view class="iconText flex">
								<view class="sIcon"><image src="../../static/images/confirml-icon2.1.png" mode=""></image></view>
								<view class="sText">预约时间：{{Utils.timeto(item.invalid_time*1000,'ymd-hms')}}</view>
							</view>
						</view>
						<view class="mgt15 flexRowBetween" v-if="item.type==1">
							<view :style="item.transport_status==2?'text-decoration: line-through':''">核销码：{{item.order_no}}</view>
							<view class="hxEwm"  @click="hxEwmShow(index)"><image :src="item.qrcode?item.qrcode:''" mode=""></image></view>
						</view>
					</view>
					
					
				</view>
			</view>
		</view>
		
		
		<!-- 无数据 -->
		<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
		
		<!-- 核销码放大弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="hxEwmShow" v-show="is_hxEwmShow">
			<view><image class="ewm" :src="mainData[previewIndex]?mainData[previewIndex].qrcode:''" mode=""></image></view>
			<view class="closeBtn" @click="hxEwmShow">×</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				is_show:false,
				curr:1,
				is_hxEwmShow:false,
				mainData:[],
				searchItem:{
					thirdapp_id:2,
					pay_status:1,
					
				},
				previewIndex:-1
			}
		},
		
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			if(options.num){
				self.changeCurr(options.num)
			}else{
				self.$Utils.loadAll(['getMainData'], self);
			}
			
			
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			changeCurr(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr
					if(self.curr==1){
						delete self.searchItem.type;
						delete self.searchItem.transport_status;
					}else if(self.curr==2){
						self.searchItem.type=2
						self.searchItem.transport_status=['in',[0,1]]
					}else if(self.curr==3){
						self.searchItem.type=1
						self.searchItem.transport_status=['in',[0,1]]
					}else{
						delete self.searchItem.type;
						self.searchItem.transport_status=2
					}
					self.getMainData(true)
				}
			},
			
			hxEwmShow(index){
				const self = this;
				console.log(index)
				if(index||index==0){
					self.previewIndex = index
				};
				self.is_show = !self.is_show
				self.is_hxEwmShow = !self.is_hxEwmShow
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
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
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
	
	.topNavFix{width: 100%;height: 80rpx;position: fixed;top: 0rpx;right: 0;left: 0;box-sizing: border-box;z-index: 22;}
	.topNavH{height: 80rpx;}
	
	
	
</style>
