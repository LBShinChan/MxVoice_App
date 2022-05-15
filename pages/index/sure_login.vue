<template>

   <view class="bg">
      

    <button type="success" @tap="sure_login">确认登录</button>
    <button type="success" @tap="backMain">回到主页</button>
   </view>
	
</template>

<script>
	    
	export default {
		data() {
			return {
	             url:'',
				 UUID:'',
				 once_token:'',
			}
		},
		onLoad() {
	        this.UUID = uni.getStorageSync("UUID")
			this.once_token = uni.getStorageSync("once_token")
		},
		methods: {
			 sure_login(){
				 console.log("SSS")
				 uni.request({
				 	url: "http://192.168.10.61:8080/login/confirm?uuid="+uni.getStorageSync("UUID"),
				 	method: "GET",
					header: {
							once_token: uni.getStorageSync("once_token")
							},
				 	success: (res) => {
				 		
						console.log(res.data)
						
						if (res.data.code === 200) {
							uni.showToast({
								title: '登录成功',
								duration: 2000
							});
							uni.redirectTo({
								url: "../index/main_show"
							});
						}else{
							uni.showToast({
								title: '登录失败',
								duration: 2000
							});
							uni.redirectTo({
								url: "../index/login"
							});
						}
				 		
				 		
				 	},
				 })
			   },
			  //下一个方法
			  backMain(){
				  console.log("回到主页")
				  uni.redirectTo({
				  	url: "../index/main_show"
				  });
			  },
		},	 
}
	
</script>

<style>
	
	.bg {
		background: linear-gradient(to bottom, skyblue, #fff 50%);
		height: 93vh;
		display: flex;
		align-items: center;
		justify-content: center;
		}
		
		
</style>
