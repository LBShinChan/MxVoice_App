<template>
	<view class="bg">
	      <view class="content" style="text-align: center;  display: inline-block"> 
			欢迎来到mxvoice的后台扫码测试模块
			<br><br><br>
		    <button type="primary" @tap="scanQRCode">扫码登录</button>
			<br><br><br>
			<h2>二维码包含的UUID ：</h2> {{UUID}}
			<br><br><br>
			<h2>已获取access_token: </h2> {{access_token}}
			<br><br><br><br>
			<button type="success" @tap="login">回到登录页面</button>
			<br>
			<button type="logout" @tap="logout">退出登录</button>
			
		 </view>
    </view>
</template>

<script>
	export default {
		data() {
			return {
                 url:'',
				 tokenId:'',
				 access_token:'',
			}
		},
		onLoad() {
            this.UUID = uni.getStorageSync("UUID")
			this.access_token = uni.getStorageSync("access_token")
		},
		methods: {
			 login(){
				 
				 uni.redirectTo({
				 	url: "../index/login"
				 });
			 },
			 
             scanQRCode(){
				 
				 uni.scanCode({
				     success: function (res) {
						 
				        console.log('条码类型：' + res.scanType);
				        console.log('条码内容：' + res.result);  //其中蕴含网址链接
						
					    console.log("执行访问条码上的链接.....")
						
						var paras = res.result.split("=");
						console.log(paras);
						uni.setStorageSync("UUID",paras[1])
						this.UUID = uni.getStorageSync("UUID");
						 // http://xxxxxxxxxxxxx/tokenId
						uni.request({
							url: res.result,
							method: "GET",
							header: {
									access_token: uni.getStorageSync("access_token")
									},
							success: (res) => {
								console.log(res.data)
								console.log("执行完条码链接又得到了，once_token")
								uni.setStorageSync("once_token",res.data.once_token)
								this.once_token = uni.getStorageSync("once_token")
								console.log("print once_token:")
								console.log(this.once_token)
								
								
								uni.redirectTo({
									url: "../index/sure_login"
								});
							},
						
						})
						 
						 // void plus.runtime.openWeb(res.result,function(){
							//  console.log("跳转操作")
						 // });
						 
				     }
				 });
			 },
			 
			 logout(){
				 uni.removeStorageSync("loginMessage");
				 uni.removeStorageSync("access_token");
				 uni.removeStorageSync("username");
				 uni.removeStorageSync("password");
				 uni.removeStorageSync("once_token");
				 
				 uni.redirectTo({
				 	url: "../index/login"
				 });
			 }
			 
		}
	}
</script>

<style lang="scss">
	.bg {
		background: linear-gradient(to bottom, skyblue, #fff 50%);
		height: 93vh;
		display: flex;
		align-items: center;
		justify-content: center;
		}
		
	.content{
	}

</style>
