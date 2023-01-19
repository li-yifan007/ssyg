<template>
	<view class="login-container">
		<uni-icons type="contact-filled" size="100" color="#AFAFAF"></uni-icons>
		<button type="primary" class="btn-login" @tap="getUserInfo">一键登录</button>
		<view class="tips-text">登录后尽享更多权益</view>
	</view>
</template>

<script>
	import {
		mapMutations
	} from 'vuex'

	export default {
		name: "my-login",
		data() {
			return {

			};
		},
		methods: {
			...mapMutations('m_user', ['updateUserInfo', 'updateToken']),

			getUserInfo(e) {
				uni.getUserProfile({
					desc: 'Wexin', // 这个参数是必须的
					success: res => {
						console.log("用户信息", res)
						this.updateUserInfo(res.userInfo)
						this.getToken(res)
					}
				})
			},

			async getToken(info) {
				const [err, res] = await uni.login().catch(err => err)

				if (err || res.errMsg !== 'login:ok') return uni.$showMsg('登录失败！')

				const query = {
					code: res.code,
					encryptedData: info.encryptedData,
					iv: info.iv,
					rawData: info.rawData,
					signature: info.signature
				}

				const token = "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOjIzLCJpYXQiOjE1NjQ3MzAwNzksImV4cCI6MTAwMTU2NDczMDA3OH0.YPt-XeLnjV-_1ITaXGY2FhxmCe4NvXuRnRB8OMCfnPo"
				this.updateToken(token)
			}
		}
	}

	// import {
	// 	mapMutations,
	// 	mapState
	// } from 'vuex'

	// export default {
	// 	data() {
	// 		return {

	// 		};
	// 	},
	// 	computed: {
	// 		...mapState('m_user', ['redirectInfo'])
	// 	},
	// 	methods: {
	// 		...mapMutations('m_user', ['updateUserInfo', 'updateToken', 'updateRedirectInfo']),
	// 		// 用户授权之后，获取用户的基本信息
	// 		getUserInfo(e) {
	// 			uni.getUserProfile({
	// 				desc: 'Wexin', // 这个参数是必须的
	// 				success: res => {
	// 					console.log("用户信息", res)
	// 					this.updateUserInfo(res.userInfo)
	// 					this.getToken(res)
	// 				}
	// 			})
	// 		},

	// 		async getToken(info) {
	// 			// 获取 code 对应的值
	// 			const [err, res] = await uni.login().catch(err => err)

	// 			if (err || res.errMsg !== 'login:ok') return uni.$showMsg('登录失败！')

	// 			// 准备参数
	// 			const query = {
	// 				code: res.code,
	// 				encryptedData: info.encryptedData,
	// 				iv: info.iv,
	// 				rawData: info.rawData,
	// 				signature: info.signature
	// 			}

	// 			const token = "0"
	// 			this.updateToken(token)
	// 			this.navigateBack()
	// 		},
	// 		navigateBack() {
	// 			if (this.redirectInfo && this.redirectInfo.openType === 'switchTab') {
	// 				uni.switchTab({
	// 					url: this.redirectInfo.from,
	// 					complete: () => {
	// 						this.updateRedirectInfo(null)
	// 					}
	// 				})
	// 			}
	// 		}
	// 	}
	// }
</script>

<style lang="scss">
	.login-container {
		height: 750rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		background-color: #f8f8f8;
		position: relative;
		overflow: hidden;

		&::after {
			content: ' ';
			display: block;
			position: absolute;
			width: 100%;
			height: 40px;
			left: 0;
			bottom: 0;
			background-color: white;
			border-radius: 100%;
			transform: translateY(50%);
		}

		.btn-login {
			width: 90%;
			border-radius: 100px;
			margin: 15px 0;
			background-color: #c00000;
		}

		.tips-text {
			font-size: 12px;
			color: gray;
		}
	}
</style>
