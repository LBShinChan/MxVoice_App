
<template>
	<view class="bg">
		<view class="login">
			<image style="margin-left: 5%;width: 252px;height: 72px;" src="../../assets/img/logo.png"></image>
			<view><input type="username" placeholder="用户名" v-model="username" required="required" /></view>
			<view><input type="email" placeholder="邮箱(重要 这是用户名的唯一标识)" v-model="email" required="required" /></view>
			<view><input type="password" placeholder="密码" v-model="password" required="required"/></view>
			<view><button type="primary" @tap="register">注册</button></view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				username: "",
				email:"",
				password: "",
			};
		},
		methods: {
			register() {
				
			// if(this.username === ""){
			// 	uni.showToast({
			// 		title: '请输入用户名',
			// 		icon:"loading" ,
			// 		duration: 200,
			// 		mask:true
			// 	});
			// 	return false;
			// }
			
			// if(this.email === ""){
			// 	uni.showToast({
			// 		title: '请输入邮箱',
			// 		icon:"loading" ,
			// 		duration: 200,
			// 		mask:true
			// 	});
			// 	return false;
			// }
			
			// if(this.password === ""){
			// 	uni.showToast({
			// 		title: '请输入密码',
			// 		icon:"loading" ,
			// 		duration: 200,
			// 		mask:true
			// 	});
			// 	return false;
			// }
			
				
			uni.request({ 
					url: "http://192.168.10.61:8080/register",
					method: "POST",
					data: {
						username: this.username,
						email:this.email,
						password: this.password,
						clientid:uni.getStorageSync("clientid")
					},
					// data:this.loginForm,
					success: (res) => {
						console.log("登录成功！！！！！！")
						console.log(res)  //还需要保存token信息
						console.log(res.data.msg)
						
						
						if (res.data.code === 200) {
							uni.showToast({
								title: '注册成功，我们已向您的邮件发送了一封激活邮件，请尽快激活！',
								icon: "none",
								duration: 2000
							});
							uni.setStorageSync("username", this.username)
							uni.setStorageSync("access_token",res.data.msg)
							
							console.log("输出保存的access_token:" + uni.getStorageSync("access_token"))
							uni.redirectTo({
								url: "../index/login"
							});
						} else {
							uni.showToast({
								title: '用户名或密码错误',
								icon: "loading",
								duration: 2000
							});
						}
					},
				})
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
		

		.login {
			width: 75vw;
			transform: translateY(-120rpx);
			padding: 20rpx;

			input {
				border: 1rpx solid #C0C0C0;
				border-radius: 10rpx;
				padding: 16rpx;
			}

			view {
				margin: 20rpx 0;
			}

			view:last-of-type {
				margin-top: 40rpx;
			}

			image {
				width: 170rpx;
				height: 170rpx;
			}
		}
	}
</style>

