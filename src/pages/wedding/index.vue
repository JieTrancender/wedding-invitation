<!--
 * @Author: wedding-invitation
 * @Description: 滚动式婚礼邀请页面
-->
<template>
  <div class="wedding-page">
    <!-- 背景音乐控制 -->
    <div v-if="musicUrl" class="music-control" @tap="toggleMusic">
      <image :src="isPlaying ? '/static/images/music_play.png' : '/static/images/music_icon.png'" class="music-icon" :class="{'rotate': isPlaying}" />
    </div>

    <!-- 封面 -->
    <div class="section section-cover">
      <div class="cover-content">
        <image :src="coverImage" class="cover-photo" mode="aspectFill" />
        <h1 class="couple-names">{{ info.name || '新郎 & 新娘' }}</h1>
        <p class="title-text">我们结婚啦！</p>
        <div class="scroll-hint">
          <text class="scroll-text">向下滚动</text>
        </div>
      </div>
    </div>

    <!-- 爱情故事 -->
    <div v-if="storyList && storyList.length > 0" class="section section-story">
      <div class="section-header">
        <h2 class="section-title">爱情故事</h2>
        <div class="section-line"></div>
      </div>
      <div class="story-timeline">
        <div v-for="(item, index) in storyList" :key="index" class="story-item" :class="index % 2 === 0 ? 'left' : 'right'">
          <div class="story-time">{{ item.date }}</div>
          <div class="story-content">
            <image :src="item.url" class="story-photo" mode="aspectFill" />
            <p class="story-desc">{{ item.desc }}</p>
          </div>
        </div>
      </div>
    </div>

    <!-- 婚礼信息 -->
    <div class="section section-info">
      <div class="section-header">
        <h2 class="section-title">婚礼信息</h2>
        <div class="section-line"></div>
      </div>
      <div class="info-card">
        <div class="info-item">
          <div class="info-icon">📅</div>
          <div class="info-text">
            <div class="info-label">日期</div>
            <div class="info-value">{{ formatDate(info.date) || '2026年1月25日' }}</div>
          </div>
        </div>
        <div class="info-item">
          <div class="info-icon">🕐</div>
          <div class="info-text">
            <div class="info-label">时间</div>
            <div class="info-value">{{ formatTime(info.time) || '12:00' }}</div>
          </div>
        </div>
        <div class="info-item">
          <div class="info-icon">📍</div>
          <div class="info-text">
            <div class="info-label">地点</div>
            <div class="info-value">{{ formatHotel(info.hotel) || '某某酒店' }}</div>
          </div>
        </div>
      </div>
    </div>

    <!-- 宾客互动区 -->
    <div class="section section-rsvp">
      <div class="section-header">
        <h2 class="section-title">宾客回执</h2>
        <div class="section-line"></div>
      </div>
      <div class="rsvp-form">
        <div class="form-item">
          <label class="form-label">姓名 <span class="required">*</span></label>
          <input v-model="form.name" class="form-input" placeholder="请输入您的姓名" />
        </div>
        <div class="form-item">
          <label class="form-label">是否出席 <span class="required">*</span></label>
          <radio-group class="radio-group" @change="onAttendChange">
            <label class="radio-item">
              <radio value="1" :checked="form.attend" color="#ff4c91" />
              <span>是</span>
            </label>
            <label class="radio-item">
              <radio value="0" :checked="!form.attend" color="#ff4c91" />
              <span>否</span>
            </label>
          </radio-group>
        </div>

        <div v-if="form.attend" class="form-item">
          <label class="form-label">出席人数 <span class="required">*</span></label>
          <input v-model.number="form.peopleCount" type="number" class="form-input" placeholder="请输入出席人数" />
        </div>
        <div class="form-item">
          <label class="form-label">祝福留言</label>
          <textarea v-model="form.message" class="form-textarea" placeholder="请留下您的祝福..." maxlength="200" />
        </div>
        <button class="submit-btn" @tap="submitForm">提交回执</button>
      </div>

      <!-- 统计信息（仅管理员可见） -->
      <div v-if="showStats" class="stats-card">
        <div class="stat-item">
          <div class="stat-number">{{ stats.total || 0 }}</div>
          <div class="stat-label">总回执</div>
        </div>
        <div class="stat-item">
          <div class="stat-number">{{ stats.attend || 0 }}</div>
          <div class="stat-label">出席</div>
        </div>
        <div class="stat-item">
          <div class="stat-number">{{ stats.people || 0 }}</div>
          <div class="stat-label">总人数</div>
        </div>
      </div>
    </div>

    <!-- 尾页 & 分享引导 -->
    <div class="section section-footer">
      <div class="footer-content">
        <h3 class="footer-title">期待与您共度这重要时刻</h3>
        <p class="footer-subtitle">感谢您的祝福</p>
        <button class="share-btn" open-type="share">
          <span class="btn-icon">📤</span>
          <span>转发给其他亲友</span>
        </button>
      </div>
    </div>


  </div>
</template>

<script setup lang="ts">

import { ref, onMounted, onUnmounted, getCurrentInstance } from 'vue'
import { onShow } from '@dcloudio/uni-app'
import { showToast } from '@src/utils'
import { getCommonConfig, getResouces, submitRSVP, getRsvpStats } from '@src/api/wedding-invitation'



// 获取全局实例（必须在最前面定义）
const instance = getCurrentInstance()

const info = ref<any>({})
const coverImage = ref('')
const musicUrl = ref('')
const storyList = ref<any[]>([])
const isPlaying = ref(false)

// 表单数据
const form = ref({
  name: '',
  peopleCount: 1,
  attend: true,
  message: ''
})

// 统计数据
const stats = ref({
  total: 0,
  attend: 0,
  people: 0
})
// 是否显示统计卡片（仅管理员可见）
const showStats = ref(false)

// 背景音频
let innerAudioContext: any = null


onMounted(() => {

  // 检查用户数据
  const mpUserData = instance?.appContext.config.globalProperties.$MpUserData
  console.log('页面加载时 $MpUserData 状态:', mpUserData)

  if (!mpUserData?.openid) {
    console.warn('页面加载时未找到 openid，登录可能未完成')
  }

  loadData()
  initAudio()
})

// 每次页面显示时检查用户数据
const checkUserData = () => {
  const mpUserData = instance?.appContext.config.globalProperties.$MpUserData
  console.log('页面显示时 $MpUserData 状态:', mpUserData)
  return mpUserData?.openid || ''
}

onShow(() => {
  checkUserData()
})

onUnmounted(() => {
  if (innerAudioContext) {
    innerAudioContext.stop()
    innerAudioContext = null
  }
})

const loadData = () => {
  // 获取婚礼信息
  if (import.meta.env.VITE_VUE_WECHAT_TCB === 'true') {
    const db = wx.cloud.database()
    const common = db.collection('common')
    common.get().then((res: any) => {
      info.value = typeof res.data[0].info === 'string' ? JSON.parse(res.data[0].info) : res.data[0].info
      musicUrl.value = res.data[0].videoUrl
    })
  } else {
    getCommonConfig().then(res => {
      info.value = typeof res.data.info === 'string' ? JSON.parse(res.data.info) : res.data.info
      musicUrl.value = res.data.videoUrl
    })
  }

  // 获取封面图片
  getResouces('wedding-cover').then(res => {
    if (res.data && res.data.length > 0) {
      coverImage.value = res.data[0].url
    }
  })

  // 获取爱情故事
  getResouces('love-story').then(res => {
    storyList.value = res.data || []
  })

  // 获取统计数据（需要提供openid）
  const openId = instance?.appContext.config.globalProperties.$MpUserData?.openid
  getRsvpStats(openId).then(res => {
    if (res.data) {
      stats.value = res.data
      showStats.value = true
      console.log('当前用户是管理员，显示统计数据')
    } else {
      // 非管理员，不显示统计数据
      stats.value = { total: 0, attend: 0, people: 0 }
      showStats.value = false
      console.log('当前用户不是管理员，不显示统计数据')
    }
  }).catch(err => {
    console.error('获取统计数据失败:', err)
    showStats.value = false
  })

}



const initAudio = () => {
  if (!musicUrl.value) return

  innerAudioContext = uni.createInnerAudioContext()
  innerAudioContext.src = musicUrl.value
  innerAudioContext.loop = true
  innerAudioContext.autoplay = false

  innerAudioContext.onPlay(() => {
    isPlaying.value = true
  })

  innerAudioContext.onPause(() => {
    isPlaying.value = false
  })

  innerAudioContext.onEnded(() => {
    innerAudioContext.play()
  })
}

const toggleMusic = () => {
  if (!innerAudioContext) return

  if (innerAudioContext.paused) {
    innerAudioContext.play()
    showToast('背景音乐已开启')
  } else {
    innerAudioContext.pause()
    showToast('背景音乐已暂停')
  }
}

const onAttendChange = (e: any) => {
  const value = Number(e?.detail?.value)
  const attend = value === 1
  form.value.attend = attend
  // 如果选择不出席，将人数重置为0
  if (!attend) {
    form.value.peopleCount = 0
  } else if (form.value.peopleCount === 0) {
    // 如果选择出席且人数为0，设置默认值为1
    form.value.peopleCount = 1
  }
}


const submitForm = async () => {
  if (!form.value.name) {
    showToast('请输入您的姓名')
    return
  }
  // 只有出席时才需要验证人数
  if (form.value.attend) {
    if (!form.value.peopleCount || form.value.peopleCount < 1) {
      showToast('请输入有效的出席人数')
      return
    }
  }

  // 获取 openid
  const mpUserData = instance?.appContext.config.globalProperties.$MpUserData
  const openId = mpUserData?.openid

  console.log('获取到的 openid:', openId)

  if (!openId) {
    console.error('无法获取 openid，用户数据:', mpUserData)
    showToast('用户信息获取失败，请关闭小程序重新进入')
    return
  }

  const attendValue = form.value.attend ? 1 : 0

  const submitData = {
    ...form.value,
    attend: attendValue,
    openid: openId,
    peopleCount: form.value.attend ? form.value.peopleCount : 0
  }

  console.log('提交的数据:', submitData)
  console.log('form.attend:', form.value.attend)
  console.log('submitData.attend:', submitData.attend)

  try {
    await submitRSVP(submitData)

    showToast('感谢您的回执！')
    // 重置表单
    form.value = {
      name: '',
      peopleCount: 1,
      attend: true,
      message: ''
    }
    // 刷新统计数据
    const res = await getRsvpStats(openId)
    if (res.data) {
      stats.value = res.data
      showStats.value = true
    } else {
      stats.value = { total: 0, attend: 0, people: 0 }
      showStats.value = false
    }
  } catch (err) {
    console.error('提交回执失败:', err)
    showToast('提交失败，请重试')
  }
}

const formatDateTime = (dateTime: any) => {
  if (!dateTime) return ''
  const date = new Date(dateTime)
  if (Number.isNaN(date.getTime())) return ''
  const y = date.getFullYear()
  const m = `${date.getMonth() + 1}`.padStart(2, '0')
  const d = `${date.getDate()}`.padStart(2, '0')
  const hh = `${date.getHours()}`.padStart(2, '0')
  const mm = `${date.getMinutes()}`.padStart(2, '0')
  return `${y}-${m}-${d} ${hh}:${mm}`
}

// 格式化日期
const formatDate = (date: any) => {

  if (!date) return ''
  if (typeof date === 'string') {
    try {
      const dateObj = JSON.parse(date)
      return `${dateObj.year}年${dateObj.month}月${dateObj.day}日`
    } catch {
      return date
    }
  }
  return `${date.year}年${date.month}月${date.day}日`
}

// 格式化时间
const formatTime = (time: any) => {
  if (!time) return ''
  if (typeof time === 'string') {
    try {
      const timeObj = JSON.parse(time)
      return `${timeObj.ceremony} - ${timeObj.banquet}`
    } catch {
      return time
    }
  }
  return `${time.ceremony} - ${time.banquet}`
}

// 格式化酒店信息
const formatHotel = (hotel: any) => {
  if (!hotel) return ''
  if (typeof hotel === 'string') {
    try {
      const hotelObj = JSON.parse(hotel)
      return hotelObj.name
    } catch {
      return hotel
    }
  }
  return hotel.name
}
</script>

<style lang="scss" scoped>
.wedding-page {
  min-height: 100vh;
  background: linear-gradient(180deg, #fff5f5 0%, #ffe8e8 100%);
}

/* 背景音乐控制 */
.music-control {
  position: fixed;
  right: 30rpx;
  top: 50rpx;
  width: 80rpx;
  height: 80rpx;
  z-index: 999;
  .music-icon {
    width: 100%;
    height: 100%;
    &.rotate {
      animation: rotate 3s linear infinite;
    }
  }
}

@keyframes rotate {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

/* 通用section样式 */
.section {
  padding: 60rpx 40rpx;
  position: relative;
  &:not(:last-child)::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 2rpx;
    height: 80rpx;
    background: linear-gradient(180deg, #ff4c91, transparent);
  }
}

.section-header {
  text-align: center;
  margin-bottom: 60rpx;
  .section-title {
    font-size: 48rpx;
    color: #333;
    font-weight: bold;
    margin-bottom: 20rpx;
  }
  .section-line {
    width: 80rpx;
    height: 4rpx;
    background: #ff4c91;
    margin: 0 auto;
  }
}

/* 封面 */
.section-cover {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(180deg, #ffe8e8 0%, #fff5f5 100%);
  .cover-content {
    text-align: center;
    animation: fadeInUp 1s ease-out;
    .cover-photo {
      width: 500rpx;
      height: 500rpx;
      border-radius: 50%;
      margin-bottom: 40rpx;
      box-shadow: 0 20rpx 60rpx rgba(255, 76, 145, 0.3);
    }
    .couple-names {
      font-size: 56rpx;
      color: #333;
      font-weight: bold;
      margin-bottom: 20rpx;
    }
    .title-text {
      font-size: 36rpx;
      color: #ff4c91;
      margin-bottom: 60rpx;
    }
  .scroll-hint {
    margin-top: 60rpx;
    .scroll-text {
      font-size: 24rpx;
      color: #ff4c91;
      animation: bounce 2s infinite;
    }
  }
  }
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(60rpx);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes bounce {
  0%, 20%, 50%, 80%, 100% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(-20rpx);
  }
  60% {
    transform: translateY(-10rpx);
  }
}

/* 爱情故事 */
.story-timeline {
  position: relative;
  padding: 40rpx 0;
  &::before {
    content: '';
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    width: 4rpx;
    height: 100%;
    background: linear-gradient(180deg, #ff4c91, #ffb6c1);
  }
}

.story-item {
  position: relative;
  margin-bottom: 60rpx;
  &.left {
    text-align: right;
    padding-right: 50%;
  }
  &.right {
    text-align: left;
    padding-left: 50%;
  }
  .story-time {
    font-size: 28rpx;
    color: #ff4c91;
    font-weight: bold;
    margin-bottom: 20rpx;
  }
  .story-content {
    background: #fff;
    padding: 30rpx;
    border-radius: 20rpx;
    box-shadow: 0 10rpx 30rpx rgba(255, 76, 145, 0.1);
    position: relative;
    &::before {
      content: '';
      position: absolute;
      top: 40rpx;
      width: 20rpx;
      height: 20rpx;
      background: #ff4c91;
      border-radius: 50%;
    }
  }
  .left .story-content::before {
    right: -50%;
  }
  .right .story-content::before {
    left: -50%;
  }
  .story-photo {
    width: 100%;
    height: 300rpx;
    border-radius: 10rpx;
    margin-bottom: 20rpx;
  }
  .story-desc {
    font-size: 28rpx;
    color: #666;
    line-height: 1.6;
  }
}

/* 婚礼信息 */
.info-card {
  background: #fff;
  border-radius: 20rpx;
  padding: 40rpx;
  box-shadow: 0 10rpx 30rpx rgba(255, 76, 145, 0.1);
}

  .info-item {
    display: flex;
    align-items: center;
    margin-bottom: 30rpx;
    &:last-child {
      margin-bottom: 0;
    }
    .info-icon {
      width: 60rpx;
      height: 60rpx;
      margin-right: 20rpx;
      font-size: 40rpx;
      line-height: 60rpx;
      text-align: center;
    }
    .info-text {
      flex: 1;
      .info-label {
        font-size: 24rpx;
        color: #999;
        margin-bottom: 8rpx;
      }
      .info-value {
        font-size: 32rpx;
        color: #333;
        font-weight: 500;
      }
    }
  }

/* 宾客互动区 */
.rsvp-form {
  background: #fff;
  border-radius: 20rpx;
  padding: 40rpx;
  margin-bottom: 40rpx;
  box-shadow: 0 10rpx 30rpx rgba(255, 76, 145, 0.1);
}

.form-item {
  margin-bottom: 30rpx;
  &:last-of-type {
    margin-bottom: 40rpx;
  }
  .form-label {
    display: block;
    font-size: 28rpx;
    color: #333;
    margin-bottom: 15rpx;
    .required {
      color: #ff4c91;
      margin-left: 4rpx;
    }
  }
  .form-input {
    width: 100%;
    height: 80rpx;
    border: 2rpx solid #eee;
    border-radius: 10rpx;
    padding: 0 20rpx;
    font-size: 28rpx;
    color: #333;
  }
  .form-textarea {
    width: 100%;
    min-height: 150rpx;
    border: 2rpx solid #eee;
    border-radius: 10rpx;
    padding: 20rpx;
    font-size: 28rpx;
    color: #333;
    line-height: 1.6;
  }
  .radio-group {
    display: flex;
    gap: 40rpx;
    .radio-item {
      display: flex;
      align-items: center;
      font-size: 28rpx;
      color: #333;
      radio {
        margin-right: 10rpx;
      }
    }
  }
}

.submit-btn {
  width: 100%;
  height: 90rpx;
  background: linear-gradient(135deg, #ff4c91, #ff6b9d);
  color: #fff;
  border: none;
  border-radius: 45rpx;
  font-size: 32rpx;
  font-weight: bold;
  box-shadow: 0 10rpx 30rpx rgba(255, 76, 145, 0.4);
}

.stats-card {
  display: flex;
  justify-content: space-around;
  background: #fff;
  border-radius: 20rpx;
  padding: 40rpx;
  box-shadow: 0 10rpx 30rpx rgba(255, 76, 145, 0.1);
  .stat-item {
    text-align: center;
    .stat-number {
      font-size: 48rpx;
      color: #ff4c91;
      font-weight: bold;
      margin-bottom: 10rpx;
    }
    .stat-label {
      font-size: 24rpx;
      color: #999;
    }
  }
}

/* 尾页 */
.section-footer {

  text-align: center;
  padding: 100rpx 40rpx;
  background: linear-gradient(180deg, #ffe8e8 0%, #ff4c91 100%);
  .footer-content {
    .footer-title {
      font-size: 48rpx;
      color: #fff;
      margin-bottom: 20rpx;
    }
    .footer-subtitle {
      font-size: 32rpx;
      color: rgba(255, 255, 255, 0.9);
      margin-bottom: 60rpx;
    }
    .share-btn {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      width: 80%;
      height: 90rpx;
      background: #fff;
      color: #ff4c91;
      border: none;
      border-radius: 45rpx;
      font-size: 32rpx;
      font-weight: bold;
      margin: 20rpx 0;
      box-shadow: 0 10rpx 30rpx rgba(0, 0, 0, 0.2);
      .btn-icon {
        font-size: 40rpx;
        margin-right: 15rpx;
      }
    }
  }
}

</style>
