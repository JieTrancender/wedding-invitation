# 新婚礼邀请页面所需图片资源

以下为新页面 `pages/wedding/index.vue` 需要的图片资源说明：

## 已有资源（可直接使用）
- `/static/images/music_play.png` - 音乐播放图标（已存在）
- `/static/images/music_icon.png` - 音乐图标（已存在）

## 需要补充的资源

### 1. 背景音乐控制图标
- `music_stop.png` - 音乐停止图标
  - 建议：可以使用 `music_icon.png` 作为静止状态

### 2. 滚动提示图标
- `scroll-down.png` - 向下滚动提示图标
  - 建议：可以使用简单的向下箭头图标

### 3. 信息卡片图标
- `calendar.png` - 日历图标（婚礼日期）
- `clock.png` - 时钟图标（婚礼时间）
- `location.png` - 位置图标（婚礼地点）

### 4. 底部功能图标
- `share.png` - 分享图标
- `qr.png` - 二维码图标

## 临时解决方案

在图片资源准备好之前，可以使用以下临时方案：

1. **音乐图标**：使用现有的 `music_icon.png`
2. **滚动提示**：暂时隐藏或使用文字替代
3. **信息图标**：暂时使用 emoji 或文字标签
4. **分享/二维码**：使用文字按钮代替

## 推荐的图标尺寸
- 音乐图标：40rpx × 40rpx
- 信息图标：60rpx × 60rpx
- 功能图标：40rpx × 40rpx
- 滚动提示：40rpx × 40rpx
