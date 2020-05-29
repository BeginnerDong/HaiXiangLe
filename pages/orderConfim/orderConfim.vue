<template>
	<view>
		<view class="mglr4 mgt15">
			<view class="">
				<view class="whiteBj radius10 mgb15" v-if="addressData.name">
					<view><image class="w" style="height: 8rpx;" src="../../static/images/the-ordert-icon5.png" mode="widthFix"></image></view>
					<view class="pdlr4 pdt10 pdb15">
						<view class="flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
							<view class="fs13 color6">
								<view class="flex mgt5">
									<view class="mgr15 color2">{{addressData.name}}</view>
									<view>{{addressData.phone}}</view>
								</view>
								<view class="mgt5">{{addressData.city+addressData.detail}}</view>
							</view>
							<view class="flexEnd" style="width: 20%;">
								<image class="arrowR" src="../../static/images/the-ordert-icon6.png" mode=""></image>
							</view>
						</view>
					</view>
				</view>
				<view class="whiteBj radius10 mgb15" v-else>
					<view><image class="w" style="height: 8rpx;" src="../../static/images/the-ordert-icon5.png" mode="widthFix"></image></view>
					<view class="pdlr4 pdt10 pdb15 flexCenter fs14 color6" style="height: 140rpx;box-sizing: border-box;" 
					@click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
						<image style="width: 30rpx;height: 30rpx;" src="../../static/images/addressl-icon4.png" mode=""></image>
						<view class="mgl10">添加地址</view>
					</view>
				</view>
				
			</view>
			
			<view class="orderLis fs13 mgt15 pdlr4 radius10 whiteBj oh pdt15 pdb15" v-for="(item,index) in mainData" :key="index">
				<view class="flex pdb15 borderB1">
					<view class="pic oh"><image :src="item.product&&item.product.mainImg&&item.product.mainImg[0]?item.product.mainImg[0].url:''" mode=""></image></view>
					<view class="infor mgl10 avoidOverflow3">{{item.product&&item.product.title?item.product.title:''}}</view>
				</view>
				<view class="flexRowBetween borderB1 pdtb15">
					<view class="flex">规格：{{item.product&&item.product.sku&&item.product.sku[item.skuIndex]?item.product.sku[item.skuIndex].title:''}}</view>
					<view class="flexEnd price">{{item.product&&item.product.sku&&item.product.sku[item.skuIndex]?item.product.sku[item.skuIndex].price:''}}</view>
				</view>
				<view class="flexRowBetween  pdt15">
					<view class="flex">购买数量：</view>
					<view class="flexEnd numBox">
						<view class="btn" @click="counter('-')">-</view>
						<view class="num">{{item.count}}</view>
						<view class="btn add" @click="counter('+')">+</view>
					</view>
				</view>
			</view>
			
			<view class="pdlr4 pdt15 pdb15 mgt15 radius10 whiteBj oh flex">
				<view class="mgr10">备注</view>
				<view class="beizhu"><input type="text" v-model="submitData.passage1" placeholder="请输入备注" placeholder-class="placeholder" /></view>
			</view>
			
			<view class="mgt15 flex color6 fs13">
				<view class="seltIcon mgr5">
					<image @click="seltAgree" v-show="!isAgree" src="../../static/images/addressl-icon1.png" mode=""></image>
					<image @click="seltAgree" v-show="isAgree" src="../../static/images/addressl-icon.png" mode=""></image>
				</view>
				<view class="">我同意</view>
				<view class="pubColor" @click="xieyiShow">《平台用户服务协议》</view>
			</view>
		</view>
		
		<view class="xqbotomBar">
			<view class="flex mgl15 fs12">总计：<view class="price fs14 ftw">{{totalPrice}}</view></view>
			<view class="payBtn" @click="submit">微信支付</view>
		</view>
		
		<!-- 服务协议 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="xieyiShow whiteBj radius10" v-show="is_xieyiShow">
			<view class="pdlr4">
				<view class="pdtb15 fs15 ftw center">服务协议</view>
				<view class="xqInfor fs13">
					<scroll-view  class="cont" scroll-y="true" >
						<view>
							<view class="content ql-editor" style="padding:0;"
							v-html="artData.content">
							</view>
						</view>
					</scroll-view>
				</view>
			</view>
			<view class="submitbtn">
				<view class="Wbtn" @click="xieyiShow">确认</view>
			</view>
		</view>
		
		<!-- 下单弹框 -->
		<view class="xieyiShow payShow whiteBj radius10" v-show="is_payShow">
			<view class="center pdtb15 color6">订单确认</view>
			<!-- 未填信息 -->
			<!-- <view class="">
				<view class="borderB1"></view>
				<view class="borderB1 flexCenter pdtb25">
					请填写下单信息
				</view>
				<view class="center pdtb15 pubColor" @click="payShowShow">确认</view>
			</view> -->
			<!-- 显示下单信息 -->
			<view class="">
				<view class="borderB1"></view>
				<view class="borderB1 pdlr4 pdt15 pdb15 fs13">
					<view class="mgb10">下单姓名：{{addressData.name}}</view>
					<view class="mgb10">下单电话：{{addressData.phone}}</view>
					<view class="">下单地址：{{addressData.city+addressData.detail}}</view>
				</view>
				<view class="center flexRowBetween">
					<view style="width: 50%; height: 100rpx;line-height: 100rpx;color:#ff6262 ;box-sizing: border-box;border-right: 1px solid #eee;" @click="payShowShow">重新输入</view>
					<view style="width: 50%;height: 100rpx;line-height: 100rpx;" class="pubColor" @click="addOrder">确认</view>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	import hTimePicker from "@/components/h-timePicker/h-timePicker.vue";
	export default {
		components: { hTimePicker },
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				count:1,
				is_xieyiShow:false,
				isAgree:false,
				is_payShow:false,
				mainData:[],
				submitData:{
					passage1:'',
				},
				totalPrice:0,
				pay:{},
				time:'',
				addressData:{},
				userData:{},
				distriData:[],
				artData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.mainData = uni.getStorageSync('payPro');
			if(options.shareUser){
				self.shareUser = options.shareUser
			};
			self.$Utils.loadAll(['getUserData','getDistriData','getArtData'], self);
			self.countTotalPrice()
		},
		
		onShow() {
			const self = this;
			if(uni.getStorageSync('choosedAddressData')){
				self.addressData = uni.getStorageSync('choosedAddressData')
			}else{
				self.getAddressData()
			};
		},
		
		methods: {
			
			getArtData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.getBefore = {
					article:{
						tableName:'Label',
						middleKey:'menu_id',
						key:'id',
						searchItem:{
							title: ['in', ['服务协议']],
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.artData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						//const type = new RegExp('<img');
						
						self.artData.content = self.artData.content.replace(regex, `<img style="max-width: 100%;"`);
						//self.artData.content = self.artData.content.replace(type, `<image`);
						
					};
					console.log(self.artData.content)
					self.$Utils.finishFunc('getArtData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			getDistriData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					child_no:uni.getStorageSync('user_info').user_no
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.distriData.push.apply(self.distriData,res.info.data)
					};
					
					self.$Utils.finishFunc('getDistriData');
				};
				self.$apis.distriGet(postData, callback);
			},
			
			getUserData() {
				const self = this;
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
			
			getAddressData() {
				const self = this;		
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					isdefault:1
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.addressData = res.info.data[0];
					}
				};
				self.$apis.addressGet(postData, callback);
			},
			
			
			
			counter(type) {
				const self = this;
				if (type == '+') {
					self.mainData[0].count++;
				} else {
					if (self.mainData[0].count > 1) {
						self.mainData[0].count--;
					}
				};
				self.countTotalPrice();
			},
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				for (var i = 0; i < self.mainData.length; i++) {
					self.totalPrice += self.mainData[i].product.sku[self.mainData[i].skuIndex].price * self.mainData[i].count
				};
				self.totalPrice = parseFloat(self.totalPrice).toFixed(2);
				var wxPay = self.totalPrice;
				//console.log('wxPay',wxPay)
				if (wxPay > 0) {
					self.pay.wxPay = {
						price: wxPay,
					};
				} else {
					delete self.pay.wxPay;
				};
				console.log(self.pay)
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				
				
				
				if(JSON.stringify(self.addressData) == '{}'){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请选择收货地址','none')
					return
				};
				
				if (!self.isAgree) {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请同意平台用户服务协议', 'none')
					return
				};
				var data = {
					snap_address: self.addressData,
					shop_no:self.mainData[0].product.shop_no,
					passage1:self.submitData.passage1
				}
				var orderList = [{
					sku_id: self.mainData[0].sku_id,
					count: self.mainData[0].count,
					type: self.mainData[0].type,
					data: data,
					snap_address: self.addressData
				}];
				self.payShowShow(orderList)
				//self.addOrder(orderList)
				
			},
			
			addOrder(orderList) {
				const self = this;	
				/* if(self.orderId){
					self.goPay()
					return
				}; */
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(self.orderList);
				postData.data = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.payShowShow()
						self.goPay()
					} else {		
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			goPay(order_id) {
				const self = this;	
				uni.setStorageSync('canClick',false);
				
				const postData = self.$Utils.cloneForm(self.pay)	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};	
				postData.payAfter = [];
				if(self.distriData.length>0){
					for (var i = 0; i < self.distriData.length; i++) {
						if(self.distriData[i].level==1){
							console.log('self.distriData[i]',self.distriData[i])
							postData.payAfter.push(
								{
									tableName: 'FlowLog',
									FuncName: 'add',
									data: {
										count:parseFloat(self.mainData[0].product.class_one),
										thirdapp_id:2,
										status:1,
										trade_info:'团队返佣',
										type:2,
										account:1,
										behavior:2,
										user_no:self.distriData[i].parent_no,
										relation_user:uni.getStorageSync('user_info').user_no
									},
								}
							)
						}else if(self.distriData[i].level==2){
							console.log('self.distriData[i]',self.distriData[i])
							postData.payAfter.push(
								{
									tableName: 'FlowLog',
									FuncName: 'add',
									data: {
										count:parseFloat(self.mainData[0].product.class_two),
										thirdapp_id:2,
										status:1,
										trade_info:'团队返佣',
										type:2,
										account:1,
										behavior:2,
										user_no:self.distriData[i].parent_no,
										relation_user:uni.getStorageSync('user_info').user_no
									},
								}
							)
						}
					}
				};
				if(self.distriData.length==0&&self.shareUser){
					postData.payAfter.push(
						{
							tableName: 'FlowLog',
							FuncName: 'add',
							data: {
								count:parseFloat(self.mainData[0].product.class_one),
								thirdapp_id:2,
								status:1,
								trade_info:'分享商品',
								type:2,
								account:1,
								behavior:1,
								user_no:self.shareUser,
								relation_user:uni.getStorageSync('user_info').user_no
							},
						}
					)
				};
				if(self.userData.primary_scope>10){
					postData.payAfter.push(
						{
							tableName: 'FlowLog',
							FuncName: 'add',
							data: {
								count:parseFloat(self.mainData[0].product.class_one),
								thirdapp_id:2,
								status:1,
								trade_info:'购买商品',
								type:2,
								account:1,
								behavior:1,
								user_no:uni.getStorageSync('user_info').user_no,
								relation_user:uni.getStorageSync('user_info').user_no
							},
						}
					)
				}
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										
										self.$Router.redirectTo({route:{path:'/pages/userOrder/userOrder'}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								
								self.$Router.redirectTo({route:{path:'/pages/userOrder/userOrder'}})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			seltAgree(){
				const self = this;
				self.isAgree = !self.isAgree 
			},
			xieyiShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_xieyiShow = !self.is_xieyiShow
			},
			
			payShowShow(orderList){
				const self = this;
				if(orderList){
					self.orderList = orderList
				};
				self.is_show = !self.is_show
				self.is_payShow = !self.is_payShow
			}
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	@import "../../assets/style/proList.css";
	page{background: #F5F5F5;padding-bottom: 140rpx;}
	
	
	.orderLis .pic{width: 110rpx;height: 110rpx;border-radius: 10rpx;overflow: hidden;}
	.orderLis .infor{width: 75%; height: 110rpx;line-height: 38rpx;}
	
	
	.beizhu{width: 85%;}
	.beizhu input{width: 100%;height: 40rpx;line-height: 40rpx;font-size: 26rpx;color: #666;display: block;margin: 0;padding: 0;}
	
	.seltIcon{width: 30rpx;height: 30rpx;}
	
	
	.xieyiShow{width: 90%;min-height: 300rpx;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 50;box-sizing: border-box;}
	.xieyiShow .xqInfor .cont{height: 400rpx;padding-bottom: 80rpx;}
	
	
</style>
