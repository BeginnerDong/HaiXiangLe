<template>
	<view>
		
		<view class="myteam flexRowBetween fs13 pdt15 mgt10">
			<view class="item flex" v-for="(item,index) in mainData" :key="index">
				<view class="photo"><image :src="item.info&&item.info[0]?item.info[0].headImgUrl:''" mode=""></image></view>
				<view class="name">{{item.info&&item.info[0]?item.info[0].nickname:''}}</view>
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
				teamData:[{},{},{},{},{},{}],
				mainData:[]
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					parent_no:uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					info:{
						tableName:'User',
						middleKey:'child_no',
						key:'user_no',
						searchItem:{
							status:1,
							
						},
						condition:'='
					}
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					};
					
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.distriGet(postData, callback);
			},

		},
	};
</script>

<style>
	.myteam{flex-wrap: wrap;padding: 30rpx 4%;}
	.myteam .item{width: 45%;margin-bottom: 40rpx;}
	.myteam .photo{width: 80rpx;height: 80rpx;border-radius: 50%;overflow: hidden;margin-right: 20rpx;}
	.myteam .name{width:60%;}
</style>
