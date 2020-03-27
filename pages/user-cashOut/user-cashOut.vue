<template>
	<view>
		<view style="padding: 70rpx 10%;">
			<view class="flexColumn center mgb30">
				<view class="icon" style="width:140rpx; height: 140rpx;"><image src="../../static/images/withdrawall-icon1.png" mode=""></image></view>
				<view class="fs14 pdt15 color6 ">佣金余额</view>
				<view class="money ftw fs24 ftw pdt20">￥562</view>
			</view>
			<view class="pdt20">
				<input class="inputEdit" type="text" value="" placeholder="请输入提现金额" />
			</view>
			<view class="submitbtn mgt15">
				<view class="Wbtn" @click="txAlertShow">申请提现</view>
			</view>
			<view class="fs12">
				<view class="flexCenter mgt20">
					<view style="width: 30rpx;height: 30rpx;"><image src="../../static/images/withdrawall-icon.png" mode=""></image></view>
					<view class="color6 mgl5">满足50元即可提现，一个工作日内到账！</view>
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
					<view class="color6 mgl5">满足50元即可提现，一个工作日内到账！</view>
				</view>
				<view class="red mgt10 center">目前为人工提现，每次提现时，请向联联的财务客服人员(微信号：dfnv5235) 发送：“小联你好，我要提现”的指令，即可很快到账。</view>
			</view>
			<view class="flexRowBetween">
				<view style="width: 50%; height: 100rpx;line-height: 100rpx;box-sizing: border-box;border-right: 1px solid #eee;" @click="txAlertShow">取消</view>
				<view style="width: 50%;height: 100rpx;line-height: 100rpx;" class="pubColor">确认提现</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{},
				is_txAlertShow:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			txAlertShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_txAlertShow = !self.is_txAlertShow
			}
		}
	};
</script>

<style>
	.inputEdit{width: 100%;height: 80rpx;box-sizing: border-box;border-radius: 10rpx;border: 1px solid #E1E1E1;text-align: center;font-size: 26rpx;}
	.txAlertShow{width:80%;min-height: 400rpx;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 50;box-sizing: border-box;}
</style>
