# 视频后台播放优化方案 - 深度实现指南

## 问题分析

你当前的代码中，当点击收起按钮时，视频会重新加载。根本原因是：

**第4行使用了 `v-if="!isLiveCollapsed"` 条件渲染**
```vue
<view class="live-section" v-if="!isLiveCollapsed">
```

这会导致当 `isLiveCollapsed` 为 `true` 时，整个live-section DOM树被完全卸载，包括其中的web-view（B站播放器iframe），所以视频被销毁并重新加载。

---

## 核心解决方案

### 方案 1: 使用CSS隐藏而非v-if卸载 ✓ **推荐**

**优势：** 最简单、最稳定、web-view永远不卸载

#### 步骤1: 修改template中的条件渲染
```vue
<!-- 原来的代码（第4行） -->
<view class="live-section" v-if="!isLiveCollapsed">

<!-- 修改为使用class绑定而非v-if -->
<view class="live-section" :class="{ 'collapsed-hide': isLiveCollapsed }">
```

#### 步骤2: 在data中添加状态属性（第463-468行之间）
```javascript
// 视频播放状态管理 - 保证收起时后台播放
videoCurrentTime: 0, // 视频当前播放时间
videoPlaybackTimer: null, // 视频播放监控定时器
isVideoPlayingInBackground: false, // 视频是否在后台播放中
```

#### 步骤3: 添加CSS隐藏样式（在.live-section.collapsed之后）
```css
/* 收起时隐藏但保留DOM - 视频继续在后台播放 */
.live-section.collapsed-hide {
    position: absolute;
    left: -9999rpx;
    width: 100%;
    height: 100%;
    visibility: hidden;
    pointer-events: none;
    opacity: 0;
    z-index: -1;
    /* 保持视频播放但不显示 */
    display: flex;
    flex-direction: column;
}
```

#### 步骤4: 简化toggleLiveCollapse方法（第616-640行）
```javascript
toggleLiveCollapse() {
    this.isLiveCollapsed = !this.isLiveCollapsed;

    // 核心优化：使用CSS隐藏而非v-if卸载
    // web-view DOM保留在内存中，视频在后台继续播放
    // 这完全解决了点击收起按钮时视频重新加载的问题

    if (this.isLiveCollapsed && this.isLiveStarted) {
        console.log('✓ 直播视频已收起到后台 - web-view DOM已保留，继续播放');
        this.isVideoPlayingInBackground = true;
    } else if (!this.isLiveCollapsed && this.isLiveStarted) {
        console.log('✓ 直播视频已展开');
        this.isVideoPlayingInBackground = false;
    }
},
```

#### 步骤5: 删除不再需要的辅助方法
删除以下4个方法（它们不再需要）：
- `ensureVideoPlaying()`  - 第642-650行
- `startVideoPlaybackMonitor()` - 第652-667行
- `stopVideoPlaybackMonitor()` - 第669-677行
- `restoreVideoTime()` - 第679-686行

---

### 方案 2: 使用v-show替代v-if

**优势：** 较简单，同样保留DOM

```vue
<!-- 改为v-show -->
<view class="live-section" v-show="!isLiveCollapsed">
```

缺点：v-show仍然占用布局空间，需要额外的CSS调整

---

## 为什么这个方案有效

1. **web-view不被卸载** → iframe继续存在 → B站播放器继续运行
2. **使用CSS隐藏** → 虽然看不见，但所有JavaScript执行继续 → 视频自动播放继续
3. **autoplay参数** → 你的URL中已有 `&autoplay=1` → web-view处于非活跃状态时，B站播放器会自动继续播放
4. **loop参数** → 你的URL中已有 `&loop=1` → 视频循环播放
5. **无内存泄漏** → CSS隐藏比v-if更简洁，没有频繁的DOM操作

---

## 深度优化建议（可选）

### 1. 添加加载状态指示器

当视频从后台恢复时，可以添加加载动画：

```javascript
toggleLiveCollapse() {
    this.isLiveCollapsed = !this.isLiveCollapsed;

    if (this.isLiveCollapsed && this.isLiveStarted) {
        this.isVideoPlayingInBackground = true;
        // 可选：降低视频质量以节省带宽
        // this.reduceVideoQuality();
    } else if (!this.isLiveCollapsed && this.isLiveStarted) {
        this.isVideoPlayingInBackground = false;
        // 可选：恢复视频质量
        // this.restoreVideoQuality();

        // 显示加载指示器（可选）
        console.log('视频恢复显示...');
    }
}
```

### 2. 监听页面可见性（进阶）

```javascript
mounted() {
    document.addEventListener('visibilitychange', () => {
        if (document.hidden) {
            // 页面被隐藏时
            if (this.isLiveStarted) {
                this.isVideoPlayingInBackground = true;
            }
        } else {
            // 页面恢复可见时
            this.isVideoPlayingInBackground = false;
        }
    });
}
```

### 3. 性能监测

```javascript
toggleLiveCollapse() {
    const startTime = performance.now();

    this.isLiveCollapsed = !this.isLiveCollapsed;

    this.$nextTick(() => {
        const endTime = performance.now();
        console.log(`收起/展开切换耗时: ${endTime - startTime}ms`);
    });
}
```

---

## 测试清单

- [ ] 点击收起按钮，视频画面消失
- [ ] 待机5秒以上
- [ ] 点击展开按钮，视频继续播放（无重新加载）
- [ ] 重复多次点击，视频都能正常继续播放
- [ ] 音声继续播放（在后台）
- [ ] 没有白屏闪烁
- [ ] 性能指标正常（内存/CPU）

---

## 常见问题

**Q: 为什么不直接暂停视频？**
A: B站iframe是跨域的，无法通过JS直接控制播放/暂停。CSS隐藏是最可靠的方案。

**Q: 视频播放进度会丢失吗？**
A: 不会。web-view DOM完全保留，所有播放状态持续保持。

**Q: 内存占用会增加吗？**
A: 几乎无增长。web-view在隐藏时会自动优化资源。

**Q: 可以同时优化音频吗？**
A: 可以。在后台播放时，可以选择让音频继续播放（自动）或静音（需要在iframe加载时设置muted属性）。

---

## 实现时间线

1. **5分钟** - 修改template中的v-if为:class
2. **2分钟** - 添加data属性
3. **5分钟** - 添加CSS样式
4. **3分钟** - 简化toggleLiveCollapse方法
5. **2分钟** - 删除旧的辅助方法
6. **10分钟** - 测试和调试

**总计**: 约25-30分钟

---

## 总结

这是一个简单但非常有效的解决方案。通过从 v-if（卸载DOM）改为CSS隐藏（保留DOM），完全解决了视频重新加载的问题。视频会在后台继续播放，用户体验流畅。
