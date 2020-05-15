<template>
	<view>
		<view class="pdlr4">
			<view class="proList">
				<view class="item boxShaow" v-for="(item,index) in mainData" :key="index" 
				:data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/detail/detail?id='+$event.currentTarget.dataset.id}})">
					<view class="xsNum">销售量：{{item.sale_count}}</view>
					<view class="pic">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="tit borderB1">{{item.title}}</view>
					<view class="infor">
						<view class="flexRowBetween">
							<view class="ll">
								<view class="flex">
									<view class="price fs16 ftw mgr5">{{item.price}}</view>
									<view class="yuanJia fs10 color9">门市价￥{{item.o_price}}</view>
								</view>
								<view class="flex fyBtn fs10 mgt5" v-if="userData.primary_scope>10">
									<view class="btn mgr10 flex">
										<view class="tt">返佣</view>
										<view class="fyNmy">￥{{item.class_one}}</view>
									</view>
									<view class="btn red flex">
										<view class="tt">返佣</view>
										<view class="fyNmy">￥{{item.class_two}}</view>
									</view>
								</view>
							</view>
							<view class="rr">
								<view class="buyBtn green"  v-show="item.is_Buying&&!item.is_noStock">抢购中</view>
								<view class="buyBtn red" v-show="item.is_notBuying&&!item.is_noStock">预售</view>
								<view class="buyBtn gray" v-show="item.is_noStock">已售罄</view>
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
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/hotProduct/hotProduct'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2-a.png" />
				</view>
				<view class="text this-text">爆品</view>
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
				proData:[{},{},{}],
				mainData:[],
				userData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getUserData'], self);
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					start_time: ['<', now],
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
					}
				};
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							if(self.mainData[i].stock==0||self.mainData[i].sellout==1){
								self.mainData[i].is_noStock = true
							};
							if(parseInt(self.mainData[i].end_time)>now){
								self.mainData[i].is_notBuying = true
							}else{
								self.mainData[i].is_Buying = true
							}
						}
					}else{
						
					};
					self.$Utils.finishFunc('getMainData');
						
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/proList.css";
	
	page{padding-bottom: 140rpx;background: #F5F5F5;}	
	
	
</style>
