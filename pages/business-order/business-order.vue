<template>
	<view>
		
		<view class="topNavFix f5bj">
			<view class="orderNav whiteBj boxShaow color6 mgb15 flexRowBetween">
				<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">全部订单</view>
				<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">未核销</view>
				<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">已核销</view>
			</view>
		</view>
		<view class="topNavH"></view>
		
		<view>
			<view class="mglr4">
				<view class="proRow">
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color6">交易时间：2020-03-27</view>
							<view class="red">未核销</view>
						</view>
						<view class="flexRowBetween">
							<view class="pic">
								<image src="../../static/images/the-ordert-img.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs14">美味的鸡蛋饼</view>
								<view class=" fs12 color6">200g</view>
								<view class="B-price flexRowBetween">
									<view class="price fs15 ftw">138</view>
									<view class="fs13">×1</view>
								</view>
							</view>
						</view>
						<view class="borderB1 mgt15"></view>
						<view class="pdt15 fs12 color6">
							<view class="iconText">姓名：张三</view>
							<view class="iconText flex">电话：15623568955</view>
						</view>
					</view>
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color6">交易时间：2020-03-27</view>
							<view class="red">已核销</view>
						</view>
						<view class="flexRowBetween">
							<view class="pic">
								<image src="../../static/images/the-ordert-img.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs14">美味的鸡蛋饼</view>
								<view class=" fs12 color6">200g</view>
								<view class="B-price flexRowBetween">
									<view class="price fs15 ftw">138</view>
									<view class="fs13">×1</view>
								</view>
							</view>
						</view>
						<view class="borderB1 mgt15"></view>
						<view class="pdt15 fs12 color6">
							<view class="iconText">姓名：张三</view>
							<view class="iconText flex">电话：15623568955</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 右侧固定按钮 -->
		<view class="R-fixIcon" @click="Router.navigateTo({route:{path:'/pages/business-orderHX/business-orderHX'}})">
			<image src="../../static/images/merchantsl-icon.png" mode=""></image>
		</view>
		
		<!-- 无数据 -->
		<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
		
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
				is_show:false,
				curr:1,
				is_hxEwmShow:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr
				}
			},
			hxEwmShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_hxEwmShow = !self.is_hxEwmShow
			}
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
