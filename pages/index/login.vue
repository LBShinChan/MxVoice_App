
<template>
	<view class="bg">
		<view class="login">
			<image style="margin-left: 5%;width: 252px;height: 72px;" src="../../assets/img/logo.png"></image>
			<view><input type="text" placeholder="用户名" v-model="username" /></view>
			<view><input type="password" placeholder="密码" v-model="password" /></view>
			<view><button type="primary" @tap="login">登录</button></view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				username: "",
				password: "",
				loginMessage:{}
			};
		},
		methods: {
			login() {
				
			uni.request({ 
					url: "http://192.168.10.61:8080/test",
					method: "POST",
					data: {
						username: this.username,
						password: this.password
					},
					// data:this.loginForm,
					success: (res) => {
						console.log("登录成功！！！！！！")
						console.log(res)  //还需要保存token信息
						console.log(res.data.msg)
						
						
						if (res.data.code === 200) {
							uni.showToast({
								title: '登录成功',
								duration: 2000
							});
							uni.setStorageSync("username", this.username)
							uni.setStorageSync("access_token",res.data.msg)
							
							console.log("输出保存的access_token:" + uni.getStorageSync("access_token"))
							this.loginMessage.username = this.username
							this.loginMessage.access_token = res.data.msg
							uni.setStorageSync("loginMessage", this.loginMessage)
							uni.redirectTo({
								url: "../index/main_show"
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

