<template>
	<view class="message-container">
		<!-- 页面头部 -->
		<view class="page-header">
			<text class="page-title">💬 消息中心</text>
			<text class="page-subtitle">及时了解最新动态</text>
		</view>

		<!-- 消息分类 -->
		<view class="message-tabs">
			<view class="tab-item" :class="{ 'active': currentTab === 'all' }" @click="switchMessageTab('all')">
				<text class="tab-text">全部</text>
				<view class="tab-badge" v-if="unreadCount.all > 0">{{ unreadCount.all }}</view>
			</view>
			<view class="tab-item" :class="{ 'active': currentTab === 'system' }" @click="switchMessageTab('system')">
				<text class="tab-text">系统</text>
				<view class="tab-badge" v-if="unreadCount.system > 0">{{ unreadCount.system }}</view>
			</view>
			<view class="tab-item" :class="{ 'active': currentTab === 'activity' }" @click="switchMessageTab('activity')">
				<text class="tab-text">活动</text>
				<view class="tab-badge" v-if="unreadCount.activity > 0">{{ unreadCount.activity }}</view>
			</view>
		</view>

		<!-- 消息列表 -->
		<scroll-view class="message-list" scroll-y="true">
			<view class="message-item" v-for="(message, index) in filteredMessages" :key="index" 
				  :class="{ 'unread': !message.read }" @click="readMessage(message)">
				<view class="message-avatar">
					<text class="avatar-icon">{{ message.icon }}</text>
				</view>
				<view class="message-content">
					<view class="message-header">
						<text class="message-title">{{ message.title }}</text>
						<text class="message-time">{{ message.time }}</text>
					</view>
					<text class="message-text">{{ message.content }}</text>
					<view class="message-tags" v-if="message.tags">
						<text class="tag" v-for="tag in message.tags" :key="tag">{{ tag }}</text>
					</view>
				</view>
				<view class="message-status" v-if="!message.read">
					<view class="unread-dot"></view>
				</view>
			</view>
		</scroll-view>

		<!-- 底部导航栏 -->
		<view class="bottom-nav">
			<view class="nav-item" :class="{ 'active': currentNavTab === 'home' }" @click="switchTab('home')">
				<view class="nav-icon">
					<text class="icon">🏠</text>
				</view>
				<text class="nav-text">首页</text>
			</view>
			
			<view class="nav-item" :class="{ 'active': currentNavTab === 'activity' }" @click="switchTab('activity')">
				<view class="nav-icon">
					<text class="icon">🎯</text>
				</view>
				<text class="nav-text">活动</text>
			</view>
			
			<view class="nav-item" :class="{ 'active': currentNavTab === 'message' }" @click="switchTab('message')">
				<view class="nav-icon">
					<text class="icon">💬</text>
				</view>
				<text class="nav-text">消息</text>
			</view>
			
			<view class="nav-item" :class="{ 'active': currentNavTab === 'profile' }" @click="switchTab('profile')">
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
				currentNavTab: 'message',
				currentTab: 'all',
				unreadCount: {
					all: 5,
					system: 2,
					activity: 3
				},
				messages: [
					{
						id: 1,
						type: 'system',
						title: '系统通知',
						content: '欢迎使用辩论LIVE！您的账户已成功激活，现在可以参与各种精彩辩论活动了。',
						icon: '🔔',
						time: '2分钟前',
						read: false,
						tags: ['欢迎', '激活']
					},
					{
						id: 2,
						type: 'activity',
						title: '活动邀请',
						content: 'AI辩论大赛即将开始！快来参与这场关于人工智能的激烈辩论吧。',
						icon: '🎯',
						time: '1小时前',
						read: false,
						tags: ['辩论', 'AI']
					},
					{
						id: 3,
						type: 'system',
						title: '功能更新',
						content: '新版本已发布！新增了AI语音识别功能，让您的辩论体验更加精彩。',
						icon: '🚀',
						time: '3小时前',
						read: true,
						tags: ['更新', '新功能']
					},
					{
						id: 4,
						type: 'activity',
						title: '活动提醒',
						content: '您关注的"科技前沿讨论"活动将在明天14:00开始，记得准时参加哦！',
						icon: '⏰',
						time: '5小时前',
						read: false,
						tags: ['提醒', '科技']
					},
					{
						id: 5,
						type: 'activity',
						title: '辩论结果',
						content: '恭喜！您在"AI是否会取代人类工作"辩论中获得了最佳观点奖！',
						icon: '🏆',
						time: '1天前',
						read: true,
						tags: ['获奖', '辩论']
					},
					{
						id: 6,
						type: 'system',
						title: '安全提醒',
						content: '为了您的账户安全，请定期更新密码，不要在公共场所登录账户。',
						icon: '🔒',
						time: '2天前',
						read: true,
						tags: ['安全', '提醒']
					}
				]
			}
		},
		computed: {
			filteredMessages() {
				if (this.currentTab === 'all') {
					return this.messages;
				}
				return this.messages.filter(msg => msg.type === this.currentTab);
			}
		},
		methods: {
			switchMessageTab(tab) {
				this.currentTab = tab;
			},
			readMessage(message) {
				if (!message.read) {
					message.read = true;
					this.updateUnreadCount();
					uni.showToast({
						title: '消息已读',
						icon: 'success'
					});
				}
			},
			updateUnreadCount() {
				this.unreadCount.all = this.messages.filter(msg => !msg.read).length;
				this.unreadCount.system = this.messages.filter(msg => msg.type === 'system' && !msg.read).length;
				this.unreadCount.activity = this.messages.filter(msg => msg.type === 'activity' && !msg.read).length;
			},
			switchTab(tab) {
				this.currentNavTab = tab;
				if (tab === 'home') {
					uni.redirectTo({
						url: '/pages/home/home'
					});
				} else if (tab === 'activity') {
					uni.redirectTo({
						url: '/pages/activity/activity'
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
	.message-container {
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

	.message-tabs {
		display: flex;
		background-color: #FFFFFF;
		border: 4rpx solid #000;
		border-radius: 25rpx;
		margin-bottom: 20rpx;
		overflow: hidden;
	}

	.tab-item {
		flex: 1;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 20rpx;
		position: relative;
		transition: all 0.3s ease;
	}

	.tab-item.active {
		background-color: #FF1493;
	}

	.tab-text {
		font-size: 28rpx;
		font-weight: bold;
		color: #000;
	}

	.tab-item.active .tab-text {
		color: #FFFFFF;
	}

	.tab-badge {
		position: absolute;
		top: 10rpx;
		right: 10rpx;
		background-color: #FF0000;
		color: #FFFFFF;
		font-size: 20rpx;
		padding: 4rpx 8rpx;
		border-radius: 10rpx;
		min-width: 20rpx;
		text-align: center;
	}

	.message-list {
		flex: 1;
		max-height: 800rpx;
	}

	.message-item {
		background-color: #FFFFFF;
		border: 4rpx solid #000;
		border-radius: 20rpx;
		padding: 25rpx;
		margin-bottom: 15rpx;
		display: flex;
		align-items: flex-start;
		gap: 20rpx;
		box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.1);
		transition: all 0.3s ease;
		position: relative;
	}

	.message-item:active {
		transform: scale(0.98);
	}

	.message-item.unread {
		background-color: #f8f9fa;
		border-color: #FF1493;
	}

	.message-avatar {
		width: 80rpx;
		height: 80rpx;
		background: linear-gradient(135deg, #00FFFF, #00BFFF);
		border: 3rpx solid #000;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-shrink: 0;
	}

	.avatar-icon {
		font-size: 36rpx;
	}

	.message-content {
		flex: 1;
		display: flex;
		flex-direction: column;
		gap: 8rpx;
	}

	.message-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.message-title {
		font-size: 32rpx;
		font-weight: bold;
		color: #000;
	}

	.message-time {
		font-size: 22rpx;
		color: #999;
	}

	.message-text {
		font-size: 26rpx;
		color: #333;
		line-height: 1.4;
	}

	.message-tags {
		display: flex;
		gap: 10rpx;
		margin-top: 8rpx;
	}

	.tag {
		background-color: #e3f2fd;
		color: #1976d2;
		font-size: 20rpx;
		padding: 4rpx 8rpx;
		border-radius: 10rpx;
		border: 1rpx solid #1976d2;
	}

	.message-status {
		position: absolute;
		top: 20rpx;
		right: 20rpx;
	}

	.unread-dot {
		width: 16rpx;
		height: 16rpx;
		background-color: #FF0000;
		border-radius: 50%;
		border: 2rpx solid #FFFFFF;
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
