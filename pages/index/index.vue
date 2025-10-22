<template>
	<view class="container">
		<!-- 顶部装饰圆圈 -->
		<view class="top-decoration">
			<view class="circle circle-left"></view>
			<view class="circle circle-middle">
				<text class="lightning-icon">⚡</text>
			</view>
			<view class="circle circle-right">
				<text class="sparkle-icon">✨</text>
			</view>
		</view>

		<!-- 主标题 -->
		<view class="title-section">
			<text class="main-title">辩论LIVE</text>
		</view>

		<!-- 描述栏 -->
		<view class="description-bar">
			<text class="emoji">😤</text>
			<text class="description-text">神仙打架·全民围观</text>
			<text class="emoji">🍿</text>
		</view>

		<!-- 主要功能框 -->
		<view class="feature-box">
			<view class="feature-button feature-button-top" @click="handleVoting">
				<view class="button-icon">
					<view class="icon-circle">
						<text class="lightning-icon">⚡</text>
					</view>
				</view>
				<text class="button-text">实时投票·手速battle</text>
			</view>
			
			<view class="feature-button feature-button-bottom" @click="handleDebate">
				<view class="button-icon">
					<view class="icon-circle">
						<text class="star-icon">⭐</text>
					</view>
				</view>
				<text class="button-text">观点碰撞·脑洞大开</text>
			</view>
		</view>

		<!-- 登录按钮 -->
		<view class="login-button" @click="handleLogin">
			<text class="login-icon">⚡️</text>
			<text class="login-text">一键闪电登录</text>
			<text class="login-icon">⚡️</text>
			<view class="floating-icon">
				<text class="thumb-icon">👍</text>
			</view>
		</view>


		<!-- 免责声明 -->
		<view class="disclaimer">
			<text class="disclaimer-icon">💡</text>
			<text class="disclaimer-text">登录=同意被辩论洗脑</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		onLoad() {

		},
		methods: {
			handleVoting() {
				uni.showToast({
					title: '实时投票功能',
					icon: 'none'
				});
			},
			handleDebate() {
				uni.showToast({
					title: '观点碰撞功能',
					icon: 'none'
				});
			},
			handleLogin() {
				// 显示登录提示
				uni.showToast({
					title: '闪电登录中...',
					icon: 'loading',
					duration: 1500
				});
				
				// 延迟跳转到首页
				setTimeout(() => {
					uni.redirectTo({
						url: '/pages/home/home'
					});
				}, 1500);
			},
		}
	}
</script>

<style>
	.container {
		min-height: 100vh;
		background: linear-gradient(180deg, #FF69B4 0%, #FF8C00 25%, #FFD700 50%, #32CD32 100%);
		padding: 120rpx 30rpx 40rpx 30rpx; /* 增加顶部间距，避免被摄像头遮挡 */
		position: relative;
		display: flex;
		flex-direction: column;
		align-items: center;
		box-sizing: border-box;
	}

	/* 顶部装饰圆圈 */
	.top-decoration {
		width: 100%;
		height: 120rpx;
		position: relative;
		margin-bottom: 40rpx;
	}

	.circle {
		position: absolute;
		border-radius: 50%;
		border: 6rpx solid #000;
	}

	.circle-left {
		width: 100rpx;
		height: 100rpx;
		background-color: #FF1493;
		left: 0;
		top: 0;
	}

	.circle-middle {
		width: 80rpx;
		height: 80rpx;
		background-color: #FF1493;
		left: 50%;
		top: 20rpx;
		transform: translateX(-50%);
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.circle-right {
		width: 60rpx;
		height: 60rpx;
		background-color: #FFD700;
		right: 0;
		top: 30rpx;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.lightning-icon, .sparkle-icon {
		font-size: 32rpx;
		color: #FFD700;
	}

	/* 主标题 */
	.title-section {
		margin-bottom: 40rpx;
	}

	.main-title {
		font-size: 72rpx;
		font-weight: bold;
		color: #000;
		text-shadow: 
			4rpx 4rpx 0 #00FFFF,
			-2rpx -2rpx 0 #FF1493,
			2rpx -2rpx 0 #FFD700;
		letter-spacing: 4rpx;
	}

	/* 描述栏 */
	.description-bar {
		background-color: #FFD700;
		border: 4rpx solid #000;
		border-radius: 30rpx;
		padding: 20rpx 30rpx;
		margin-bottom: 40rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 20rpx;
	}

	.description-text {
		font-size: 32rpx;
		font-weight: bold;
		color: #000;
	}

	.emoji {
		font-size: 36rpx;
	}

	/* 主要功能框 */
	.feature-box {
		background-color: #FFFFFF;
		border: 6rpx solid #000;
		border-radius: 30rpx;
		padding: 30rpx;
		margin-bottom: 40rpx;
		width: 100%;
		max-width: 600rpx;
	}

	.feature-button {
		display: flex;
		align-items: center;
		padding: 25rpx;
		border-radius: 20rpx;
		margin-bottom: 20rpx;
		border: 4rpx solid #000;
	}

	.feature-button:last-child {
		margin-bottom: 0;
	}

	.feature-button-top {
		background-color: #FF1493;
	}

	.feature-button-bottom {
		background-color: #00FFFF;
	}

	.button-icon {
		margin-right: 20rpx;
	}

	.icon-circle {
		width: 60rpx;
		height: 60rpx;
		background-color: #FFD700;
		border: 4rpx solid #000;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.star-icon {
		font-size: 28rpx;
		color: #FFFFFF;
	}

	.button-text {
		font-size: 32rpx;
		font-weight: bold;
		color: #000;
	}

	/* 登录按钮 */
	.login-button {
		background-color: #FF1493;
		border: 6rpx solid #000;
		border-radius: 30rpx;
		padding: 25rpx 40rpx;
		margin-bottom: 40rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 15rpx;
		position: relative;
		width: 100%;
		max-width: 500rpx;
	}

	.login-text {
		font-size: 36rpx;
		font-weight: bold;
		color: #000;
	}

	.login-icon {
		font-size: 32rpx;
		color: #FFD700;
	}

	.floating-icon {
		position: absolute;
		top: -15rpx;
		right: -15rpx;
		width: 50rpx;
		height: 50rpx;
		background-color: #00FFFF;
		border: 4rpx solid #000;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.thumb-icon {
		font-size: 24rpx;
		color: #FFD700;
	}

	/* 免责声明 */
	.disclaimer {
		background-color: #000000;
		border: 4rpx solid #FFD700;
		border-radius: 20rpx;
		padding: 20rpx 30rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 15rpx;
		width: 100%;
		max-width: 500rpx;
	}

	.disclaimer-text {
		font-size: 28rpx;
		color: #FFFFFF;
	}

	.disclaimer-icon {
		font-size: 32rpx;
	}

</style>
