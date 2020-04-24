
<script>
	import token from "./common/token.js"
	export default {
		onLaunch: function() {
			
			if(location.search){
				var searchArr = location.search.substr(1).split('&');
			};
			if(location.hash&&location.hash .split('?')[1]){
				var hashArr = location.hash .split('?')[1].split('&');
				var hash = location.hash .split('?')[0]
			};
			var arr = [];
			if(searchArr){
				arr = arr.concat(searchArr)
			};
			if(hashArr){
				arr = arr.concat(hashArr)
			};
			var params = {};
			for (var i = 0; i < arr.length; i++) {
				var data = arr[i].split('=')
				if (data.length === 2) {
				  params[data[0]] = data[1]
				};
			};
			console.log('22',params)
			if(params.user_no){
				console.log(333)
				const callback = (res) => {
					console.log(222)
				};
				token.getProjectToken(callback, {
					refreshToken: true,daren_no:params.user_no
				})
			};
			//token.getProjectToken();
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	@import "/assets/style/style.css";
	@import "/assets/style/quill.css";
	
	uni-swiper .uni-swiper-dots-horizontal{line-height: 16rpx;bottom: 5px;}
	
	uni-tabbar .uni-tabbar__icon{width: 44rpx;height: 44rpx;}
	uni-page-head .uni-page-head__title{font-weight: normal;}
	/* uni-swiper .uni-swiper-dot{width: 22rpx;height: 12rpx;border-radius: 6rpx;background-color: #c5c5c5;} */
	.uni-page-head{border-bottom: 1px solid #eee;}
	.ql-editor img{max-width: 100%;margin: 0 auto;}
	
</style>
