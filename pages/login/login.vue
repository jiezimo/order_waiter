<template>
	<view>
		<!-- 顶部logo -->
		<view class="imgs">
			<image src="../../static/logo.png"></image>
		</view>
		<!-- 登录表单 -->
		<view>
			<input type="number" v-model="username" value="" placeholder="账号" />
			<input type="number" v-model="password" value="" placeholder="密码" password="" />
			<button type="default" @click="login()">立即登录</button>
		</view>
		<view class="footer">
			<view class="warn">
				<text space="emsp">本软件仅限餐厅工作人员使用 严禁泄露账号密码</text> <br>
				<text class="show">版权所有@2019-2029-Sandy/Coder保留一切权利</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				username: '123',
				password: '123456',
			}
		},
		onShow() {
			var h = document.body.scrollHeight;
			window.onresize = function() {
				if (document.body.scrollHeight < h) {
					document.getElementsByClassName("footer")[0].style.display = "none";
				} else {
					document.getElementsByClassName("footer")[0].style.display = "block";
				}
			};
		},
		methods: {
			login() {
				if (this.username.length == "") {
					uni.showToast({
						title: '用户名不能为空',
						icon: 'none'
					})
					return
				}
				if (this.password.length < 5) {
					uni.showToast({
						title: '用户密码错误',
						icon: 'none'
					})
					return
				}
				uni.showLoading({
					title: '登录中'
				})
				setTimeout(function() {
					//登录接口
					console.log(this.username),
					console.log(this.password),
					uni.request({
						url: this.$apiPath + "?m=admin&c=login&a=login",
						method: 'POST',
						dataType: 'json',
						header: {
							"content-type": "application/x-www-form-urlencoded"
						},
						data: {
							email: this.username,
							password: this.password,
						},
						success: (res) => {
							if (res.data.error == 0) {
								console.log(res.data.data.token)
								uni.setStorage({
									key: 'token',
									data: res.data.data.token,
									success(res) {
										console.log('存入成功了')
									},
								});
								uni.switchTab({
									url: '../order/order'
								});
							} else {
								uni.showToast({
									title: res.data.msg
								})
							}
						}
					})
				}.bind(this), 1000)
			}
		}
	}
</script>

<style>
	page {
		background: #39355a;
		overflow: hidden;
		/* display: inline-block; */
		/* border-top:1px solid; */
		/* padding-top: 150rpx; */
	}

	.imgs {
		width: 160rpx;
		height: 160rpx;
		box-shadow: 0rpx 0rpx 60rpx 0rpx rgba(0, 0, 0, .3);
		border-radius: 50%;
		margin: 150rpx auto;
	}

	.imgs image {
		width: 100%;
		height: 100%;
		border-radius: 50%;
	}

	input {
		margin: 0 65rpx;
		background: #eff1ff;
		margin-top: 35rpx;
		border-radius: 100rpx;
		padding: 30rpx 40rpx;
		font-size: 30rpx;
		color: #94afce;
		box-shadow: 0rpx 0rpx 60rpx 0rpx rgba(0, 0, 0, .3);
	}

	button {
		margin: 100rpx 66rpx;
		border-radius: 100rpx;
		background: #6c5381;
		color: #cccccc;
		box-shadow: 0rpx 0rpx 60rpx 0rpx rgba(0, 0, 0, .3);
	}

	button.button-hover {
		transform: translate(2rpx, 2rpx);
		background: #6c5381;
		color: #cccccc;
	}

	.footer {
		position: fixed;
		top: 92%;
		width: 100%;
		height: 80rpx;

	}

	.warn {
		color: #005BBA;
		font-size: 22rpx;
		text-align: center;
	}

	.show {
		color: rgba(26, 26, 26, 0.5);
		font-size: 21rpx;
		vertical-align: middle;
	}
</style>
