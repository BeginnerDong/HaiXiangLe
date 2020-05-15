<template>
	<view>
		<view style="padding: 70rpx 10%;">
			<view class="flexColumn center mgb30">
				<view class="icon" style="width:140rpx; height: 140rpx;"><image src="../../static/images/withdrawall-icon1.png" mode=""></image></view>
				<view class="fs14 pdt15 color6 ">佣金余额</view>
				<view class="money ftw fs24 ftw pdt20">￥{{userInfoData.balance}}</view>
			</view>
			<view class="pdt20">
				<input class="inputEdit" type="text" v-model="submitData.count" placeholder="请输入提现金额" />
			</view>
			<view class="submitbtn mgt15">
				<view class="Wbtn" @click="submit">申请提现</view>
			</view>
			<view class="fs12">
				<view class="flexCenter mgt20">
					<view style="width: 30rpx;height: 30rpx;"><image src="../../static/images/withdrawall-icon.png" mode=""></image></view>
					<view class="color6 mgl5">满足{{limit}}元即可提现，一个工作日内到账！</view>
				</view>
				<view class="red mgt10 center">目前为人工提现，每次提现时，请向联联的财务客服人员(微信号：dfnv5235) 发送：“小联你好，我要提现”的指令，即可很快到账。</view>
			</view>
		</view>
		
		<!-- 提现弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="txAlertShow center whiteBj radius10" v-show="is_txAlertShow">
			<view class="pdtb15 borderB1">温馨提示</view>
			<view class="fs12 pdlr4 pdt15 pdb15 borderB1">
				<view class="flexCenter">
					<view style="width: 30rpx;height: 30rpx;"><image src="../../static/images/withdrawall-icon.png" mode=""></image></view>
					<view class="color6 mgl5">满足{{limit}}元即可提现，一个工作日内到账！</view>
				</view>
				<view class="red mgt10 center">目前为人工提现，每次提现时，请向联联的财务客服人员(微信号：dfnv5235) 发送：“小联你好，我要提现”的指令，即可很快到账。</view>
			</view>
			<view class="flexRowBetween">
				<view style="width: 50%; height: 100rpx;line-height: 100rpx;box-sizing: border-box;border-right: 1px solid #eee;" @click="txAlertShow">取消</view>
				<view style="width: 50%;height: 100rpx;line-height: 100rpx;" class="pubColor" @click="flowLogAdd">确认提现</view>
			</view>
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
				userInfoData:{},
				submitData:{
					count:''
				},
				is_txAlertShow:false,
				limit:0,
				is_show:false
			}
		},
		
		onLoad() {
			const self = this;
			const callback = (res) => {
				self.$Utils.loadAll(['getUserInfoData'], self);
				self.limit = uni.getStorageSync('user_info').thirdApp.withdraw
			};
			self.$Token.getProjectToken(callback, {
				refreshToken: true
			})
			
		},
		
		methods: {
			txAlertShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_txAlertShow = !self.is_txAlertShow
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('self.submitData',self.submitData)
				if (pass) {
					
					if(parseFloat(self.submitData.count)>parseFloat(self.userInfoData.balance)){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('余额不足', 'none');
						return
					};
					if(parseFloat(self.submitData.count)<parseFloat(self.limit)){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('最少提现'+parseFloat(self.limit)+'元', 'none');
						return
					};
					if(parseFloat(self.submitData.count)<=0){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请输入正确的金额', 'none');
						return
					};
					
					self.txAlertShow()
					
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入提现金额', 'none')
				};
			},
			
			flowLogAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				/* if(!wx.getStorageSync('user_info')||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				}; */
				postData.data = {
					count:-self.submitData.count,
					thirdapp_id:2,
					status:1,
					trade_info:'提现',
					type:2,
					account:1,
					withdraw:1,
					withdraw_status:0
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('提交成功', 'none');
						setTimeout(function() {
							self.Router.redirectTo({route:{path:'/pages/user/user'}})
						}, 800)
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.flowLogAdd(postData, callback);
			},
			
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{}
				};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0]
						
					} else {
						self.$Utils.showToast(res.msg, 'none');
					};
					self.$Utils.finishFunc('getUserInfoData');
			
				};
				self.$apis.userInfoGet(postData, callback);
			},
		}
	};
</script>

<style>
	.inputEdit{width: 100%;height: 80rpx;box-sizing: border-box;border-radius: 10rpx;border: 1px solid #E1E1E1;text-align: center;font-size: 26rpx;}
	.txAlertShow{width:80%;min-height: 400rpx;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 50;box-sizing: border-box;}
</style>
