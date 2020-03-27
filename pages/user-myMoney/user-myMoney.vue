<template>
	<view>
		<view class="myExtendTop flexColumn center pubBj white pr">
			<view class="money ftw">437.1</view>
			<view class="fs13 pdt10">总收益(元)</view>
			
		</view>
		<view class="userBox2 flexRowBetween fs12">
			<view class="child flexColumn">
				<view class="fs18 ftw red mgb5">234</view>
				<view class="color6">可提现</view>
			</view>
			<view class="child flexColumn">
				<view class="fs18 ftw mgb5">203.1</view>
				<view class="color6">已提现</view>
			</view>
			<view class="child flexCenter">
				<view class="txBtn white pubBj"  @click="Router.navigateTo({route:{path:'/pages/user-cashOut/user-cashOut'}})">提现</view>
			</view>
		</view>
		
		<view class="orderNav flexRowBetween fs15">
			<view class="tt" :class="num==1?'on':''" @click="change('1')">直卖收益</view>
			<view class="tt" :class="num==2?'on':''" @click="change('2')">团队收益</view>
		</view>
		
		<view class="">
			<view class="myRowBetween mglr4" v-show="num==1">
				<view class="item flexRowBetween" v-for="(item,index) in spendData" :key="index">
					<view class="ll">
						<view class="">购买商品</view>
						<view class="fs12 color9">2020.01.17</view>
					</view>
					<view class="rr red">+56</view>
				</view>
			</view>
			<view class="myRowBetween mglr4" v-show="num==2">
				<view class="item flexRowBetween" v-for="(item,index) in spendData" :key="index">
					<view class="ll">
						<view class="">团队返佣</view>
						<view class="fs12 color9">2020.01.18</view>
					</view>
					<view class="rr red">+20</view>
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
				num:1,
				spendData:[{},{},{}],
				is_show:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			change(num){
				const self = this;
				if(num!= self.num){
					self.num = num
				}
			}
			
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
