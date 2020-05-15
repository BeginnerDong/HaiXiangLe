<template>
	<view>
		
		<view class="mglr4">
			<view class="proRow">
				<view class="item">
					<view class="fs12 flexRowBetween mgb10">
						<view class="color6">交易时间：{{mainData.create_time}}</view>
						<view class="red">{{mainData.transport_status==0?'待核销':'已核销'}}</view>
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image :src="mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product&&mainData.orderItem[0].snap_product.product&&
							mainData.orderItem[0].snap_product.product.mainImg&&mainData.orderItem[0].snap_product.product.mainImg[0]?mainData.orderItem[0].snap_product.product.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow2 fs14">{{mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product&&
						mainData.orderItem[0].snap_product.product?mainData.orderItem[0].snap_product.product.title:''}}</view>
							<view class=" fs12 color6">{{mainData.title}}</view>
							<view class="B-price flexRowBetween">
								<view class="price fs15 ftw">{{mainData.price}}</view>
								<view class="fs13">×{{mainData.count}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="pdlr4 whiteBj radius10 mgt15">
				<view class="pdtb15 fs13">
					<view class="iconText flex">
						<view class="color6 mgr15">姓名</view>
						<view>{{mainData.name}}</view>
					</view>
					<view class="iconText flex">
						<view class="color6 mgr15">电话</view>
						<view>{{mainData.phone}}</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 160rpx;">
			<button class="btn" type="button" v-if="mainData.transport_status==0" @click="$Utils.stopMultiClick(orderUpdate)">确认核销</button>
			<button class="btn" type="button" v-if="mainData.transport_status==2">已核销</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				mainData:{},
				id:''
			}
		},
		
		onLoad(options) {
			const self = this;
			var options = self.$Utils.getHashParameters();
			self.id = options[0].id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			orderUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getStaffToken';
				postData.data = {
					transport_status:2,
				};
				postData.searchItem = {
					id:self.id,
					user_type:0
				};
				
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			getMainData() {
				const self = this;
				console.log('852369',self.id)
				const postData = {
					searchItem:{
						id:self.id,
						user_type:0
					}
				};
				postData.searchItem.shop_no = uni.getStorageSync('staffInfo').user_no;
				postData.tokenFuncName = 'getStaffToken'
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
				console.log('postData',postData)
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none');
						
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					};
					console.log('self.mainData',self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	
</style>
