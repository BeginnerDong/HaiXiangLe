<template>
	<view>
		
		<view class="mglr4">
			<view class=" whiteBj boxShaow oh mgt15 mgb15 radius8">
				<view><image class="w" style="height: 8rpx;" src="../../static/images/the-ordert-icon5.png" mode="widthFix"></image></view>
				<view class="pdlr4 pdt15 pdb15 fs14">
					<view class="flex mt-1">
						<view class="mr-3 color2">张思德</view>
						<view class="color6">15689562352</view>
					</view>
					<view class="mgt10">陕西省西安市雁塔区高新大都荟</view>
				</view>
			</view>
			<view class="proRow">
				<view class="item mgb15 flexRowBetween" >
					<view class="pic"><image src="../../static/images/the-ordert-img.png" mode=""></image></view>
					<view class="infor">
						<view class="tit avoidOverflow">墨西哥牛油果8枚单果200g左右</view>
						<view class="fs12 color6 ">200g</view>
						<view class="B-price">
							<view class="flexRowBetween">
								<view class="price">138</view>
								<view>×1</view>
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
					<view>1</view>
				</view>
				<view class="pdt15 flex">
					<view class="color6 fs12 mgr15">订单金额</view>
					<view class="price">156</view>
				</view>
				<view class="pdt15 flex">
					<view class="color6 fs12 mgr15">交易时间</view>
					<view class="">2020年3月10日</view>
				</view>
				<view class="pdt15 flex">
					<view class="color6 fs12 mgr15">快递单号</view>
					<view class="">15634645131463546465</view>
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
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				curr:1,
				count:1,
				is_show:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			currChange(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			},
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.count++;
				} else {
					if (self.count > 1) {
						self.count--;
					}
				};			
				self.countTotalPrice();
			}
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
