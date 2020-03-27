<template>
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
				</view>
			</view>
			
			<view class="pdlr4 whiteBj radius10 mgt15">
				<view class="pdtb15 fs13">
					<view class="iconText flex">
						<view class="color6 mgr15">姓名</view>
						<view>张三</view>
					</view>
					<view class="iconText flex">
						<view class="color6 mgr15">电话</view>
						<view>15623568955</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 160rpx;">
			<view class="btn">确认核销</view>
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
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
		}
	};
</script>

<style>
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	
</style>
