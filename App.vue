<script>
	export default {
		onLaunch: function() {
			uni.getStorage({//获得保存在本地的用户信息
			                key: 'loginMessage',  
			                success:(res) => {  
			                    console.log(res.data)  
			                    uni.request({// 再次校验并刷新token的有效时间  
			                        url: "http://192.168.10.61:8080/ver",  
			                        method: "POST",
			                        data: {
			                        	access_token:res.data.access_token,
										username:res.data.username
			                        }, 
			                        method: "POST",  
			                        success: (e) => {  
										console.log(e) 
			                            if (e.statusCode === 200 && e.data.code === 200) { 
											uni.redirectTo({
												url: "../index/main_show"
											});
											console.log("success")
			                            }  
			                        }  
			                    })  
			                }  
			            });
			console.log('App Launch')
		},
		onShow: function() {
			// 配置1秒后自动关闭启动页
			    setTimeout(() => {
			        // #ifdef APP-PLUS
			        plus.navigator.closeSplashscreen();
			        // #endif
			    }, 2000);
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		},
		
	}
</script>

<style>
	/*每个页面公共css */
</style>
