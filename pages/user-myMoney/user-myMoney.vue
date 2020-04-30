<template>
	<view>
		<view class="myExtendTop flexColumn center pubBj white pr">
			<view class="money ftw">{{userInfoData.all&&userInfoData.all.count!=0?userInfoData.all.count:'0.00'}}</view>
			<view class="fs13 pdt10">总收益(元)</view>
			
		</view>
		<view class="userBox2 flexRowBetween fs12">
			<view class="child flexColumn">
				<view class="fs18 ftw red mgb5">{{userInfoData.balance}}</view>
				<view class="color6">可提现</view>
			</view>
			<view class="child flexColumn">
				<view class="fs18 ftw mgb5">{{userInfoData.hasCashOut&&userInfoData.hasCashOut.count!=0?userInfoData.hasCashOut.count:'0.00'}}</view>
				<view class="color6">已提现</view>
			</view>
			<view class="child flexCenter">
				<view class="txBtn white pubBj"  @click="Router.redirectTo({route:{path:'/pages/user-cashOut/user-cashOut'}})">提现</view>
			</view>
		</view>
		
		<view class="orderNav flexRowBetween fs15">
			<view class="tt" :class="num==1?'on':''" @click="change('1')">直卖收益</view>
			<view class="tt" :class="num==2?'on':''" @click="change('2')">团队收益</view>
		</view>
		
		<view class="">
			<view class="myRowBetween mglr4">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
					<view class="ll">
						<view class="">{{item.trade_info}}</view>
						<view class="fs12 color9">{{item.create_time}}</view>
					</view>
					<view class="rr red">{{item.count}}</view>
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
				num:1,
				spendData:[{},{},{}],
				userInfoData:{},
				searchItem:{
					thirdapp_id:2,
					type:2,
					behavior:1
				},
				mainData:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getUserInfoData','getMainData'], self);
		},
		
		methods: {
			
			getMainData(isNew) {
				const self = this;
				var now = new Date().getTime();
				if(isNew){
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}else{
						
					};
					self.$Utils.finishFunc('getMainData');
						
				};
				self.$apis.flowLogGet(postData, callback);
			},
			
			change(num){
				const self = this;
				if(num!= self.num){
					self.num = num;
					self.searchItem.behavior = self.num;
					self.getMainData(true)
				}
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.getAfter = {
					all: {
						tableName: 'FlowLog',
						searchItem: {
							status:1,
							type:2,
						},
						middleKey: 'user_no',
						key: 'user_no',
						condition: 'in',
						compute:{
						  count:[
						    'sum',
						    'count',
						    {
						      status:1,type:2,count:['>',0]
						    }
						  ]
						},
					},
					hasCashOut: {
						tableName: 'FlowLog',
						searchItem: {
							status:1,
							type:2,
							withdraw:1
						},
						middleKey: 'user_no',
						key: 'user_no',
						condition: 'in',
						compute:{
						  count:[
						    'sum',
						    'count',
						    {
						      status:1,type:2,withdraw:1
						    }
						  ]
						},
					},
				}
				const callback = (res) => {
					if (res.solely_code == 100000&&res.info.data[0]) {
						self.userInfoData = res.info.data[0]
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getUserInfoData');
					
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/orderNav.css";
	.myExtendTop{padding:100rpx 4% 120rpx 4% ;height: 340rpx;box-sizing: border-box;}
	.myExtendTop .money{font-size: 60rpx; line-height:60rpx;}
	.txBtn{width: 140rpx;line-height: 60rpx;border-radius: 40rpx;text-align: center;box-shadow: 0 4rpx 0rpx #dae5df;}
	.ewmBtn{padding:10rpx 20rpx;background: #fff;border-radius: 40rpx 0 0 40rpx; color: #222;position: absolute;top: 40rpx;right: 0;}
	.ewmBtn .icon{width: 30rpx;height: 30rpx;margin-right: 10rpx;}
	
	.userBox2{height: 80px;box-sizing: border-box;margin: -80rpx 4% 20rpx 4% ;position: relative; z-index: 1; box-shadow: 0 4rpx 10rpx rgba(0,0,0,0.1);padding: 30rpx 10rpx;background: #fff;border-radius: 10rpx; }
	.userBox2 .child{width: 33.33%;position: relative;line-height: 44rpx;}
	.userBox2 .child::after{content: '';width: 1px;height: 60rpx;background: #e1e1e1;position: absolute;right: 0;top: 50%;transform: translateY(-50%);}
	.userBox2 .child:last-child::after{display: none;}
	
	/* .myRowBetween .ll .photo{width: 80rpx;height: 80rpx;border-radius: 50%;overflow: hidden;}
	.myRowBetween .ll .photo image{width: 100%;height: 100%;display: block;}
	.myRowBetweenL70 .item .ll{width: 70%;}
	.myRowBetweenL70 .item .rr{width: 30%;} */
	
	/* .ewmAlert{position: fixed;top: 40%;left: 50%;transform: translateX(-50%);z-index: 45;}
	.ewmAlert .ewm{width: 360rpx;height: 360rpx;display: block;} */
	
</style>
