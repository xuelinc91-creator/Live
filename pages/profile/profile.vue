<template>
	<view class="profile-container">
		<!-- 页面头部 -->
		<view class="page-header">
			<text class="page-title">👤 个人中心</text>
			<text class="page-subtitle">管理你的账户和设置</text>
		</view>

		<!-- 用户信息卡片 -->
		<view class="user-card">
			<view class="user-avatar">
				<text class="avatar-emoji">👤</text>
			</view>
			<view class="user-info">
				<text class="user-name">辩论达人</text>
				<text class="user-level">Lv.5 活跃用户</text>
				<view class="user-stats">
					<view class="stat-item">
						<text class="stat-number">156</text>
						<text class="stat-label">参与辩论</text>
					</view>
					<view class="stat-item">
						<text class="stat-number">23</text>
						<text class="stat-label">获胜次数</text>
					</view>
					<view class="stat-item">
						<text class="stat-number">89</text>
						<text class="stat-label">获得点赞</text>
					</view>
				</view>
			</view>
		</view>

		<!-- 功能菜单 -->
		<view class="menu-section">
			<view class="menu-item" @click="handleMenuClick('achievements')">
				<view class="menu-icon">
					<text class="icon">🏆</text>
				</view>
				<text class="menu-text">我的成就</text>
				<text class="menu-arrow">></text>
			</view>
			
			<view class="menu-item" @click="handleMenuClick('history')">
				<view class="menu-icon">
					<text class="icon">📚</text>
				</view>
				<text class="menu-text">辩论历史</text>
				<text class="menu-arrow">></text>
			</view>
			
			<view class="menu-item" @click="handleMenuClick('favorites')">
				<view class="menu-icon">
					<text class="icon">❤️</text>
				</view>
				<text class="menu-text">我的收藏</text>
				<text class="menu-arrow">></text>
			</view>
			
			<view class="menu-item" @click="handleMenuClick('settings')">
				<view class="menu-icon">
					<text class="icon">⚙️</text>
				</view>
				<text class="menu-text">设置</text>
				<text class="menu-arrow">></text>
			</view>
		</view>

		<!-- 成就展示 -->
		<view class="achievement-section">
			<text class="section-title">🏆 最新成就</text>
			<view class="achievement-list">
				<view class="achievement-item" v-for="(achievement, index) in achievements" :key="index">
					<view class="achievement-icon">
						<text class="icon">{{ achievement.icon }}</text>
					</view>
					<view class="achievement-info">
						<text class="achievement-name">{{ achievement.name }}</text>
						<text class="achievement-desc">{{ achievement.description }}</text>
					</view>
					<view class="achievement-status" :class="achievement.status">
						<text class="status-text">{{ achievement.statusText }}</text>
					</view>
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
				currentTab: 'profile',
				achievements: [
					{
						name: '辩论新手',
						description: '完成第一次辩论',
						icon: '🎯',
						status: 'completed',
						statusText: '已获得'
					},
					{
						name: '观点达人',
						description: '获得100个点赞',
						icon: '👍',
						status: 'completed',
						statusText: '已获得'
					},
					{
						name: '连胜王者',
						description: '连续获胜5次',
						icon: '👑',
						status: 'in-progress',
						statusText: '进行中'
					},
					{
						name: '活跃用户',
						description: '连续登录30天',
						icon: '📅',
						status: 'locked',
						statusText: '未解锁'
					}
				]
			}
		},
		methods: {
			handleMenuClick(menu) {
				switch(menu) {
					case 'achievements':
						uni.showToast({
							title: '成就系统开发中',
							icon: 'none'
						});
						break;
					case 'history':
						uni.showToast({
							title: '历史记录功能开发中',
							icon: 'none'
						});
						break;
					case 'favorites':
						uni.showToast({
							title: '收藏功能开发中',
							icon: 'none'
						});
						break;
					case 'settings':
						uni.showToast({
							title: '设置功能开发中',
							icon: 'none'
						});
						break;
				}
			},
			switchTab(tab) {
				this.currentTab = tab;
				if (tab === 'home') {
					uni.redirectTo({
						url: '/pages/home/home'
					});
				} else if (tab === 'activity') {
					uni.redirectTo({
						url: '/pages/activity/activity'
					});
				} else if (tab === 'message') {
					uni.redirectTo({
						url: '/pages/message/message'
					});
				}
			}
		}
	}
</script>

<style>
	.profile-container {
		min-height: 100vh;
		background: linear-gradient(180deg, #FF69B4 0%, #FF8C00 25%, #FFD700 50%, #32CD32 100%);
		padding: 120rpx 20rpx 140rpx 20rpx;
	}

	.page-header {
		text-align: center;
		margin-bottom: 30rpx;
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

	.user-card {
		background-color: #FFFFFF;
		border: 6rpx solid #000;
		border-radius: 30rpx;
		padding: 30rpx;
		margin-bottom: 30rpx;
		display: flex;
		align-items: center;
		gap: 25rpx;
		box-shadow: 0 8rpx 20rpx rgba(0, 0, 0, 0.1);
	}

	.user-avatar {
		width: 120rpx;
		height: 120rpx;
		background: linear-gradient(135deg, #FF1493, #FF69B4);
		border: 6rpx solid #000;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-shrink: 0;
	}

	.avatar-emoji {
		font-size: 60rpx;
	}

	.user-info {
		flex: 1;
		display: flex;
		flex-direction: column;
		gap: 10rpx;
	}

	.user-name {
		font-size: 36rpx;
		font-weight: bold;
		color: #000;
	}

	.user-level {
		font-size: 26rpx;
		color: #666;
	}

	.user-stats {
		display: flex;
		gap: 30rpx;
		margin-top: 10rpx;
	}

	.stat-item {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 5rpx;
	}

	.stat-number {
		font-size: 32rpx;
		font-weight: bold;
		color: #FF1493;
	}

	.stat-label {
		font-size: 22rpx;
		color: #666;
	}

	.menu-section {
		background-color: #FFFFFF;
		border: 6rpx solid #000;
		border-radius: 30rpx;
		margin-bottom: 30rpx;
		overflow: hidden;
		box-shadow: 0 8rpx 20rpx rgba(0, 0, 0, 0.1);
	}

	.menu-item {
		display: flex;
		align-items: center;
		padding: 25rpx 30rpx;
		border-bottom: 2rpx solid #f0f0f0;
		transition: all 0.3s ease;
	}

	.menu-item:last-child {
		border-bottom: none;
	}

	.menu-item:active {
		background-color: #f8f9fa;
		transform: scale(0.98);
	}

	.menu-icon {
		width: 60rpx;
		height: 60rpx;
		background: linear-gradient(135deg, #00FFFF, #00BFFF);
		border: 3rpx solid #000;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-right: 20rpx;
	}

	.icon {
		font-size: 28rpx;
	}

	.menu-text {
		flex: 1;
		font-size: 32rpx;
		font-weight: bold;
		color: #000;
	}

	.menu-arrow {
		font-size: 32rpx;
		color: #999;
	}

	.achievement-section {
		background-color: #FFFFFF;
		border: 6rpx solid #000;
		border-radius: 30rpx;
		padding: 30rpx;
		box-shadow: 0 8rpx 20rpx rgba(0, 0, 0, 0.1);
	}

	.section-title {
		font-size: 32rpx;
		font-weight: bold;
		color: #000;
		display: block;
		margin-bottom: 20rpx;
	}

	.achievement-list {
		display: flex;
		flex-direction: column;
		gap: 15rpx;
	}

	.achievement-item {
		display: flex;
		align-items: center;
		gap: 20rpx;
		padding: 20rpx;
		background-color: #f8f9fa;
		border: 3rpx solid #000;
		border-radius: 20rpx;
	}

	.achievement-icon {
		width: 60rpx;
		height: 60rpx;
		background: linear-gradient(135deg, #FFD700, #FFA500);
		border: 3rpx solid #000;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-shrink: 0;
	}

	.achievement-info {
		flex: 1;
		display: flex;
		flex-direction: column;
		gap: 5rpx;
	}

	.achievement-name {
		font-size: 28rpx;
		font-weight: bold;
		color: #000;
	}

	.achievement-desc {
		font-size: 24rpx;
		color: #666;
	}

	.achievement-status {
		padding: 8rpx 16rpx;
		border-radius: 15rpx;
		border: 2rpx solid #000;
		flex-shrink: 0;
	}

	.achievement-status.completed {
		background-color: #4CAF50;
	}

	.achievement-status.in-progress {
		background-color: #FF9800;
	}

	.achievement-status.locked {
		background-color: #ccc;
	}

	.status-text {
		font-size: 22rpx;
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
