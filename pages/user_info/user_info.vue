<template>
	<view class="">
		<view class="myRowBetween">
			<view class="item flexRowBetween pdtb15 borderB1">
				<view class="ll">姓名</view>
				<view class="rr fs13">
					<input type="text"  v-model="submitData.name" placeholder="请输入姓名" placeholder-class="placeholder">
				</view>
			</view>	
			<view class="item flexRowBetween pdtb15 borderB1">
				<view class="ll">手机号</view>
				<view class="rr fs13" >
					<input type="number" maxlength="11" v-model="submitData.phone" placeholder="请输入手机号" placeholder-class="placeholder">
				</view>
			</view>	
			<view class="item flexRowBetween pdtb15 borderB1">
				<view class="ll">所在地区</view>
				<view class="rr fs13 color9" @click="showMulLinkageThreePicker">
					<input type="text" placeholder="选择您的位置" disabled="true"   v-model="submitData.address">
					<image class="arrowR" style="width: 19px;" src="../../static/images/detailsl-icon2.png" mode=""></image>
				</view>
			</view>	

			
			<view class="submitbtn pdtb15"  style="margin-top: 200rpx;">
				<button class="btn" type="button"   @click="Utils.stopMultiClick(submit)">确定</button>
			</view>
			
		</view>
		<mpvue-city-picker :themeColor="themeColor" ref="mpvueCityPicker" :pickerValueDefault="cityPickerValueDefault"
		 @onCancel="onCancel" @onConfirm="onConfirm"></mpvue-city-picker>
		
	</view>
</template>

<script>
	import mpvuePicker from '../../components/mpvue-picker/mpvuePicker.vue';
	// https://github.com/zhetengbiji/mpvue-citypicker
	import mpvueCityPicker from '../../components/mpvue-citypicker/mpvueCityPicker.vue'
	import cityData from '../../common/city.data.js';

	export default {
		components: {
			mpvuePicker,
			mpvueCityPicker
		},
		
		
		data() {
			return {
				submitData: {
					name: '',
					phone:'',
					address:''
				},
				mulLinkageTwoPicker: cityData,
				cityPickerValueDefault: [0, 0, 0],
				themeColor: '#F98A48',
				Utils:this.$Utils,
				mode: '',
				deepLength: 1,
				pickerValueDefault: [0],
				pickerValueArray:[],
				
			}
		},
		onLoad() {
			const self = this;
			self.getMainData()
		},
		
		methods: {
			
			showMulLinkageThreePicker() {
				this.$refs.mpvueCityPicker.show()
			},
			
			onConfirm(e) {
				this.submitData.address = e.label;
				console.log('e',e)
			},
			
			onCancel(e){
				console.log('e',e)
			},
			
			
	

			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					console.log(res);
					self.submitData.name = res.info.data[0].name;
					self.submitData.phone = res.info.data[0].phone;
					self.submitData.address = res.info.data[0].address;
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},



			userInfoUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('完善成功','success');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'error')
					};
					
				};
				self.$apis.userInfoUpdate(postData, callback);
			},

			submit() {
				const self = this;
				
				var newObject = self.$Utils.cloneForm(self.submitData);
				const pass = self.$Utils.checkComplete(newObject);
				console.log('self.data.sForm', self.submitData)
				console.log('pass', pass)
				if (pass) {
					self.userInfoUpdate()
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息','success');
				};
			},

		}
	}
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.myRowBetween .item{padding:30rpx 4%}
</style>
