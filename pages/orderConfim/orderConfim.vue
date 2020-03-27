<template>
	<view>
		<view class="mglr4 mgt15">
			<view class="">
				<view class="whiteBj radius10 mgb15" >
					<view><image class="w" style="height: 8rpx;" src="../../static/images/the-ordert-icon5.png" mode="widthFix"></image></view>
					<view class="pdlr4 pdt10 pdb15 flexCenter fs14 color6" style="height: 140rpx;box-sizing: border-box;" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
						<image style="width: 30rpx;height: 30rpx;" src="../../static/images/addressl-icon4.png" mode=""></image>
						<view class="mgl10">添加地址</view>
					</view>
				</view>
				<view class="whiteBj radius10 mgb15" >
					<view><image class="w" style="height: 8rpx;" src="../../static/images/the-ordert-icon5.png" mode="widthFix"></image></view>
					<view class="pdlr4 pdt10 pdb15">
						<view class="flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
							<view class="fs13 color6">
								<view class="flex mgt5">
									<view class="mgr15 color2">唐久</view>
									<view>15689562352</view>
								</view>
								<view class="mgt5">西安市莲湖区糜家桥村72号 (搬到27号)</view>
							</view>
							<view class="flexEnd" style="width: 20%;">
								<image class="arrowR" src="../../static/images/the-ordert-icon6.png" mode=""></image>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="orderLis fs13 mgt15 pdlr4 radius10 whiteBj oh pdt15 pdb15">
				<view class="flex pdb15 borderB1">
					<view class="pic oh"><image src="../../static/images/the-ordert-img.png" mode=""></image></view>
					<view class="infor mgl10 avoidOverflow3">【西安】29层云顶餐厅吃五星自助~仅178-2大2小抢悦豪自助餐~海鲜烧烤的號惬盛宴~使用7月底！菜品丰富！吃到扶墙走！</view>
				</view>
				<view class="flexRowBetween borderB1 pdtb15">
					<view class="flex">规格：2盒装</view>
					<view class="flexEnd price">56.2</view>
				</view>
				<view class="flexRowBetween  pdt15">
					<view class="flex">购买数量：</view>
					<view class="flexEnd numBox">
						<view class="btn" @click="counter('-')">-</view>
						<view class="num">{{count}}</view>
						<view class="btn add" @click="counter('+')">+</view>
					</view>
				</view>
			</view>
			
			<view class="pdlr4 pdt15 pdb15 mgt15 radius10 whiteBj oh flex">
				<view class="mgr10">备注</view>
				<view class="beizhu"><input type="text" value="" placeholder="请输入备注" placeholder-class="placeholder" /></view>
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
			<view class="flex mgl15 fs12">总计：<view class="price fs14 ftw">56.2</view></view>
			<view class="payBtn" @click="payShowShow">微信支付</view>
		</view>
		
		<!-- 服务协议 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="xieyiShow whiteBj radius10" v-show="is_xieyiShow">
			<view class="pdlr4">
				<view class="pdtb15 fs15 ftw center">服务协议</view>
				<view class="xqInfor fs13">
					<scroll-view  class="cont" scroll-y="true" >
						<view>
							<view>1.解放路第三方个人管理供货方就是大概回房间号购房款规划局分雕刻时光</view>
							<view>1.解放路第三方个人管理供货方就是大概回房间号购房款规划局分雕刻时光</view>
							<view>1.解放路第三方个人管理供货方就是大概回房间号购房款规划局分雕刻时光</view>
							<view>1.解放路第三方个人管理供货方就是大概回房间号购房款规划局分雕刻时光</view>
							<view>1.解放路第三方个人管理供货方就是大概回房间号购房款规划局分雕刻时光</view>
							<view>1.解放路第三方个人管理供货方就是大概回房间号购房款规划局分雕刻时光</view>
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
			<view class="">
				<view class="borderB1"></view>
				<view class="borderB1 flexCenter pdtb25">
					请填写下单信息
				</view>
				<view class="center pdtb15 pubColor" @click="payShowShow">确认</view>
			</view>
			<!-- 显示下单信息 -->
			<view class="">
				<view class="borderB1"></view>
				<view class="borderB1 pdlr4 pdt15 pdb15 fs13">
					<view class="mgb10">下单姓名：唐久</view>
					<view class="mgb10">下单电话：15623562356</view>
					<view class="">下单地址：陕西省西安市雁塔区高新大都荟</view>
				</view>
				<view class="center flexRowBetween">
					<view style="width: 50%; height: 100rpx;line-height: 100rpx;color:#ff6262 ;box-sizing: border-box;border-right: 1px solid #eee;" @click="payShowShow">重新输入</view>
					<view style="width: 50%;height: 100rpx;line-height: 100rpx;" class="pubColor">确认</view>
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
				count:1,
				is_xieyiShow:false,
				isAgree:false,
				is_payShow:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
			payShowShow(){
				const self = this;
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
	.beizhu input{width: 100%;height: 40rpx;line-height: 40px;font-size: 26rpx;color: #666;}
	
	.seltIcon{width: 30rpx;height: 30rpx;}
	
	
	.xieyiShow{width: 90%;min-height: 300rpx;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 50;box-sizing: border-box;}
	.xieyiShow .xqInfor .cont{height: 400rpx;padding-bottom: 80rpx;}
	
	
</style>
