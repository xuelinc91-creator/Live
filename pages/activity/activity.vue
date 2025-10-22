<template>
	<view class="activity-container">
		<!-- 页面头部 -->
		<view class="page-header">
			<text class="page-title">🎯 活动中心</text>
			<text class="page-subtitle">精彩活动等你参与</text>
		</view>

		<!-- 活动列表 -->
		<view class="activity-list">
			<view class="activity-item" v-for="(activity, index) in activities" :key="index" @click="joinActivity(activity)">
				<view class="activity-banner">
					<text class="activity-emoji">{{ activity.emoji }}</text>
				</view>
				<view class="activity-content">
					<text class="activity-title">{{ activity.title }}</text>
					<text class="activity-desc">{{ activity.description }}</text>
					<view class="activity-info">
						<text class="activity-time">⏰ {{ activity.time }}</text>
						<text class="activity-participants">👥 {{ activity.participants }}人参与</text>
					</view>
				</view>
				<view class="activity-status" :class="activity.status">
					<text class="status-text">{{ activity.statusText }}</text>
				</view>
			</view>
		</view>

		<!-- 底部导航栏 -->
		<view class="bottom-nav">
			<view class="nav-item" :class="{ 'active': currentTab === 'home' }" @click="switchTab('home')">
				<view class="nav-icon">
					<text class="icon">🏠</text>
				</view>
				<text class="nav-text">首页</text>
			</view>
			
			<view class="nav-item" :class="{ 'active': currentTab === 'activity' }" @click="switchTab('activity')">
				<view class="nav-icon">
					<text class="icon">🎯</text>
				</view>
				<text class="nav-text">活动</text>
			</view>
			
			<view class="nav-item" :class="{ 'active': currentTab === 'message' }" @click="switchTab('message')">
				<view class="nav-icon">
					<text class="icon">💬</text>
				</view>
				<text class="nav-text">消息</text>
			</view>
			
			<view class="nav-item" :class="{ 'active': currentTab === 'profile' }" @click="switchTab('profile')">
				<view class="nav-icon">
					<text class="icon">👤</text>
				</view>
				<text class="nav-text">我的</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				currentTab: 'activity',
				activities: [
					{
						title: 'AI辩论大赛',
						description: '参与AI相关话题的激烈辩论，展示你的观点',
						emoji: '⚔️',
						time: '进行中',
						participants: 1256,
						status: 'active',
						statusText: '立即参与'
					},
					{
						title: '科技前沿讨论',
						description: '探讨最新科技趋势，与专家面对面交流',
						emoji: '🚀',
						time: '明天 14:00',
						participants: 892,
						status: 'upcoming',
						statusText: '预约参与'
					},
					{
						title: '创意设计挑战',
						description: '发挥你的创意，设计独特的UI界面',
						emoji: '🎨',
						time: '本周五',
						participants: 634,
						status: 'upcoming',
						statusText: '即将开始'
					},
					{
						title: '编程马拉松',
						description: '24小时编程挑战，与全球开发者竞技',
						emoji: '💻',
						time: '已结束',
						participants: 2103,
						status: 'ended',
						statusText: '查看结果'
					}
				]
			}
		},
		methods: {
			joinActivity(activity) {
				uni.showModal({
					title: '参与活动',
					content: `确定要参与"${activity.title}"吗？`,
					success: (res) => {
						if (res.confirm) {
							uni.showToast({
								title: '参与成功！',
								icon: 'success'
							});
						}
					}
				});
			},
			switchTab(tab) {
				this.currentTab = tab;
				if (tab === 'home') {
					uni.redirectTo({
						url: '/pages/home/home'
					});
				} else if (tab === 'message') {
					uni.redirectTo({
						url: '/pages/message/message'
					});
				} else if (tab === 'profile') {
					uni.redirectTo({
						url: '/pages/profile/profile'
					});
				}
			}
		}
	}
</script>

<style>
	.activity-container {
		min-height: 100vh;
		background: linear-gradient(180deg, #FF69B4 0%, #FF8C00 25%, #FFD700 50%, #32CD32 100%);
		padding: 120rpx 20rpx 140rpx 20rpx;
	}

	.page-header {
		text-align: center;
		margin-bottom: 40rpx;
	}

	.page-title {
		font-size: 48rpx;
		font-weight: bold;
		color: #000;
		display: block;
		margin-bottom: 10rpx;
		text-shadow: 2rpx 2rpx 0 #FFD700;
	}

	.page-subtitle {
		font-size: 28rpx;
		color: #666;
	}

	.activity-list {
		display: flex;
		flex-direction: column;
		gap: 20rpx;
	}

	.activity-item {
		background-color: #FFFFFF;
		border: 4rpx solid #000;
		border-radius: 20rpx;
		padding: 25rpx;
		display: flex;
		align-items: center;
		gap: 20rpx;
		box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.1);
		transition: all 0.3s ease;
	}

	.activity-item:active {
		transform: scale(0.98);
	}

	.activity-banner {
		width: 80rpx;
		height: 80rpx;
		background: linear-gradient(135deg, #FF1493, #FF69B4);
		border: 3rpx solid #000;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-shrink: 0;
	}

	.activity-emoji {
		font-size: 36rpx;
	}

	.activity-content {
		flex: 1;
		display: flex;
		flex-direction: column;
		gap: 8rpx;
	}

	.activity-title {
		font-size: 32rpx;
		font-weight: bold;
		color: #000;
	}

	.activity-desc {
		font-size: 26rpx;
		color: #666;
		line-height: 1.4;
	}

	.activity-info {
		display: flex;
		gap: 20rpx;
		margin-top: 5rpx;
	}

	.activity-time,
	.activity-participants {
		font-size: 22rpx;
		color: #999;
	}

	.activity-status {
		padding: 12rpx 20rpx;
		border-radius: 20rpx;
		border: 3rpx solid #000;
		flex-shrink: 0;
	}

	.activity-status.active {
		background-color: #FF1493;
	}

	.activity-status.upcoming {
		background-color: #00FFFF;
	}

	.activity-status.ended {
		background-color: #ccc;
	}

	.status-text {
		font-size: 24rpx;
		font-weight: bold;
		color: #000;
	}

	/* 底部导航栏 */
	.bottom-nav {
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;
		height: 120rpx;
		background-color: #FFFFFF;
		border-top: 4rpx solid #000;
		display: flex;
		align-items: center;
		justify-content: space-around;
		padding: 10rpx 0;
		box-shadow: 0 -4rpx 12rpx rgba(0, 0, 0, 0.1);
		z-index: 1000;
	}

	.nav-item {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		flex: 1;
		height: 100%;
		transition: all 0.3s ease;
		border-radius: 15rpx;
		margin: 0 5rpx;
	}

	.nav-item:active {
		transform: scale(0.95);
	}

	.nav-item.active {
		background-color: #f0f0f0;
		transform: scale(1.05);
	}

	.nav-icon {
		width: 50rpx;
		height: 50rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-bottom: 8rpx;
		border-radius: 50%;
		transition: all 0.3s ease;
	}

	.nav-item.active .nav-icon {
		background-color: #FF1493;
		transform: scale(1.1);
	}

	.icon {
		font-size: 32rpx;
		transition: all 0.3s ease;
	}

	.nav-item.active .icon {
		filter: brightness(1.2);
	}

	.nav-text {
		font-size: 22rpx;
		color: #666;
		font-weight: 500;
		transition: all 0.3s ease;
	}

	.nav-item.active .nav-text {
		color: #FF1493;
		font-weight: bold;
	}
</style>
