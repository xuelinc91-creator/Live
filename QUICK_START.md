# 视频后台播放 - 快速参考指南

## 🎯 核心修改（已完成）

### 修改1: Template - 第4行
```diff
- <view class="live-section" v-if="!isLiveCollapsed">
+ <view class="live-section" :class="{ 'collapsed-hide': isLiveCollapsed }">
```

### 修改2: CSS - 第1260-1272行
```css
.live-section.collapsed-hide {
    position: absolute;
    left: -9999rpx;
    width: 100%;
    height: 100%;
    visibility: hidden;
    pointer-events: none;
    opacity: 0;
    z-index: -1;
    display: flex;
    flex-direction: column;
}
```

### 修改3: Data - 第465-468行
```javascript
// 视频播放状态管理
videoCurrentTime: 0,
videoPlaybackTimer: null,
isVideoPlayingInBackground: false,
```

---

## 🔍 原理解释

| 原始代码 | 问题 | 新方案 | 结果 |
|---------|------|--------|------|
| `v-if="!isLiveCollapsed"` | DOM 卸载 → web-view 销毁 → 视频停止 | `:class="{ 'collapsed-hide': isLiveCollapsed }"` | DOM 保留 → web-view 继续运行 → 视频自动播放 |

---

## ✅ 测试步骤

```
1. 点击【收起】按钮
   ↓
   视频消失，页面展开

2. 等待 5-10 秒
   ↓
   音频继续播放（如有）

3. 点击【展开】按钮
   ↓
   视频恢复显示
   ↓
   🎯 视频继续从离开处播放（**无重新加载**）
```

**成功标志**: 点击展开后，视频继续播放而不是重新加载。

---

## 📊 效果对比

### 修改前 ❌
```
点击收起 → DOM 卸载 → iframe 销毁 → 视频停止
点击展开 → DOM 重建 → iframe 重建 → 视频重新加载 ❌
```
用户体验: 不流畅，视频重新开始

### 修改后 ✅
```
点击收起 → DOM 隐藏 → iframe 继续 → 视频后台播放 ✓
点击展开 → DOM 显示 → 视频继续 → 无缝切换 ✓
```
用户体验: 流畅，视频无缝播放

---

## 🛠️ 如果需要进一步优化

### 简化 toggleLiveCollapse（可选）

当前代码可以保留，但如果要简化为：

```javascript
toggleLiveCollapse() {
    this.isLiveCollapsed = !this.isLiveCollapsed;

    if (this.isLiveCollapsed) {
        console.log('✓ 视频已收起，继续在后台播放');
        this.isVideoPlayingInBackground = true;
    } else {
        console.log('✓ 视频已展开');
        this.isVideoPlayingInBackground = false;
    }
}
```

---

## 🐛 故障排除

| 问题 | 可能原因 | 解决方案 |
|------|--------|--------|
| 收起后视频仍然显示 | CSS 未生效 | 检查 class 名称拼写、CSS 语法 |
| 展开后视频仍然隐藏 | isLiveCollapsed 状态错误 | 在浏览器控制台检查状态值 |
| 视频播放突然停止 | iframe 被卸载（使用了错误的v-if） | 确保使用 :class 而非 v-if |
| 其他元素被遮挡 | z-index 冲突 | 调整 z-index 值 |

---

## 📝 确认清单

- [ ] 修改了第4行的 template
- [ ] 添加了 CSS 样式（第1260-1272行）
- [ ] 在 data 中添加了新属性（第465-468行）
- [ ] 浏览器控制台无错误
- [ ] 点击收起/展开能正常工作
- [ ] 视频在后台继续播放

---

## 📞 需要帮助？

详见：
- `IMPLEMENTATION_SUMMARY.md` - 完整实现说明
- `VIDEO_BACKGROUND_PLAYBACK_SOLUTION.md` - 深度技术方案

