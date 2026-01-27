<!--
 * @Author: wedding-invitation
 * @Description: 滚动式婚礼邀请页面
-->
<template>
  <div class="wedding-page" :class="{ 'snap-mode': snapEnabled }">
    <!-- 背景音乐控制 -->

    <div v-if="musicUrl" class="music-control" @tap="toggleMusic">
      <image :src="isPlaying ? '/static/images/music_play.png' : '/static/images/music_icon.png'" class="music-icon" :class="{'rotate': isPlaying}" />
    </div>

    <!-- 封面 -->
    <div class="section section-cover snap-section">

      <image :src="coverImage" class="cover-photo" mode="aspectFill" />
      <div class="cover-overlay"></div>
      <div class="cover-content">
        <h1 class="couple-names">{{ info.name || '新郎 & 新娘' }}</h1>
        <div class="cover-divider"></div>
        <p class="title-text">我们结婚啦！</p>
        <p class="cover-subtitle">诚邀您赴约，共见爱的仪式感</p>

        <div class="scroll-hint">
          <span class="arrow">↓</span>
          <span class="arrow arrow-secondary">↓</span>
        </div>
      </div>



    </div>

    <view id="content-start"></view>

    <!-- 开场大图 + 文案 + 喜宴信息 -->
    <div class="section section-opening snap-section">
      <div class="opening-layout">
        <div class="opening-top-text" :class="{ 'in-view': openingInView }">
          <p>那年初见，心跳如鼓，惊起一池春水</p>
          <p>而今携手，步履从容，共赴一世山河</p>
          <p>敬备喜筵，恭候光临，共襄此生之约</p>


        </div>
        <image class="opening-photo" :src="openingImage" mode="aspectFill" lazy-load="true" />

        <div class="opening-bottom-info" :class="{ 'in-view': openingInView }">
          <div class="overlay-info overlay-parents">
            <span class="label">父亲</span> <strong class="highlight">莫尚态</strong>
            <span class="separator">/</span>
            <span class="label">母亲</span> <strong class="highlight">李桂华</strong>
          </div>
          <div class="overlay-info"><span class="label">喜宴时间</span> <strong class="highlight">2026年02月24日 - 2026年02月25日</strong></div>
          <div class="overlay-info">
            <span class="label">喜宴地址</span>
            <strong>{{ locationInfo.address || '四川省南充市仪陇县观紫镇大兴村四社' }}</strong>
          </div>
          <div class="action-buttons-container">
            <button class="action-button phone-button" @tap="showPhoneActions">
              <span class="action-icon">📱</span>
              <span class="action-text">联系电话</span>
            </button>
            <button class="action-button nav-button" @tap="openMapNavigation">
              <span class="action-icon">📍</span>
              <span class="action-text">导航到现场</span>
            </button>
          </div>
        </div>








      </div>
    </div>

    <!-- 开场大图 + 文案 + 喜宴信息（副本第三屏） -->
    <div class="section section-opening section-opening-2 snap-section">
      <div class="opening-layout opening-layout-2">
        <div class="opening-top-text" :class="{ 'in-view': openingInView2 }">
          <p class="title-line">始于2011，定于2017</p>
          <p>2011年9月1日，初识于高中教室</p>
          <p>2017年1月1日，相守于新年晨光</p>
          <p>六年时光，从同窗到恋人</p>
          <p>心动未改，终成笃定</p>
        </div>

        <image class="opening-photo opening-photo-2" :src="openingImage2" mode="aspectFill" lazy-load="true" />

      </div>
    </div>

    <!-- 开场大图 + 文案（第四屏，对称布局） -->
    <div class="section section-opening section-opening-3 snap-section">
      <div class="opening-layout opening-layout-2 opening-layout-3">
        <image class="opening-photo opening-photo-2" :src="openingImage3" mode="aspectFill" lazy-load="true" />
        <div class="opening-top-text opening-top-text-3" :class="{ 'in-view': openingInView3 }">
          <p class="title-line">2025年12月31日</p>
          <p>我们在岁末签下彼此的名字</p>
          <p>从此，新年第一天</p>
          <p>就是“我们”的第一天</p>
          <p>—— 法律上的我们，始于2025年末</p>

        </div>
      </div>
    </div>

    <!-- 开场大图 + 文案（第五屏：采用第二屏排版，图片在上文字在下） -->
    <div class="section section-opening section-opening-4 snap-section">
      <div class="opening-layout opening-layout-2 opening-layout-4">
        <image class="opening-photo opening-photo-2 opening-photo-4" :src="openingImage4" mode="aspectFill" lazy-load="true" />
        <div class="opening-top-text opening-top-text-4" :class="{ 'in-view': openingInView4 }">
          <p class="title-line">九年相守，一日成婚，一生白首</p>
          <p>从2017年元旦的第一句“我们”</p>
          <p>到2026年初春的这场郑重相约</p>
          <p>时光未改初心，岁月愈见深情</p>
          <p>愿与你晨昏共度，青丝成雪</p>
          <p>不负这九年等待，更不负余生每一日</p>
          <p>—— 相恋九年，余生皆你</p>
        </div>
      </div>
    </div>







    <!-- 第六屏：上图、中间文案、下方宾客回执 -->
    <div class="section section-opening section-opening-5 snap-section">
      <div class="opening-layout opening-layout-5-copy">
        <image class="opening-photo opening-photo-5-copy" :src="openingImage5" mode="aspectFill" lazy-load="true" />

        <div class="rsvp-inline">
          <div class="opening-top-text opening-top-text-5-copy" :class="{ 'in-view': openingInView5 }">
            <p>诚邀您参加我们的婚礼</p>
            <p>期待您的到来，与我们共同庆祝</p>
            <p>您的祝福是我们最珍贵的礼物</p>
          </div>

          <div class="rsvp-form">

            <div class="form-item form-inline">
              <label class="form-label">姓名 <span class="required">*</span></label>
              <input v-model="form.name" class="form-input" placeholder="请输入您的姓名" />
            </div>
            <div class="form-item form-inline">
              <label class="form-label">出席 <span class="required">*</span></label>
              <radio-group class="radio-group inline" @change="onAttendChange">
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



            <div class="form-item">
              <textarea
                v-model="form.message"
                class="form-textarea form-textarea-double"
                placeholder="请留下您的祝福..."
                maxlength="200"
                rows="2"
              />
            </div>





            <button class="submit-btn" @tap="submitForm">提交回执</button>
            <button class="share-btn-inline" open-type="share">
              <span class="btn-icon">📤</span>
              <span>转发给其他亲友</span>
            </button>
          </div>

          <div v-if="showStats" class="stats-card stats-inline">
            <span class="stat-inline-text">总回执：{{ stats.total || 0 }}</span>
            <span class="stat-inline-text">出席：{{ stats.attend || 0 }}</span>
          </div>


        </div>
      </div>
    </div>






  </div>

  <!-- 电话选择弹窗 -->
  <div class="phone-action-sheet" v-if="showPhoneModal" @tap="showPhoneModal = false">
    <div class="phone-action-content" @tap.stop>
      <div class="phone-action-header">选择拨打</div>

      <!-- 新郎家人 -->
      <div v-if="contactInfo.heNumber || contactInfo.heFatherNumber || contactInfo.heMotherNumber || contactInfo.heSisterNumber" class="phone-action-group">
        <div class="phone-action-group-title">新郎家人</div>
        <div class="phone-action-item" v-if="contactInfo.heNumber" @tap="callPhone(contactInfo.heNumber)">
          <span class="phone-action-label">新郎</span>
          <span class="phone-action-number">{{ contactInfo.heNumber }}</span>
        </div>
        <div class="phone-action-item" v-if="contactInfo.heFatherNumber" @tap="callPhone(contactInfo.heFatherNumber)">
          <span class="phone-action-label">父亲</span>
          <span class="phone-action-number">{{ contactInfo.heFatherNumber }}</span>
        </div>
        <div class="phone-action-item" v-if="contactInfo.heMotherNumber" @tap="callPhone(contactInfo.heMotherNumber)">
          <span class="phone-action-label">母亲</span>
          <span class="phone-action-number">{{ contactInfo.heMotherNumber }}</span>
        </div>
        <div class="phone-action-item" v-if="contactInfo.heSisterNumber" @tap="callPhone(contactInfo.heSisterNumber)">
          <span class="phone-action-label">姐姐</span>
          <span class="phone-action-number">{{ contactInfo.heSisterNumber }}</span>
        </div>
      </div>

      <!-- 新娘家人 -->
      <div v-if="contactInfo.sheNumber || contactInfo.sheFatherNumber || contactInfo.sheMotherNumber" class="phone-action-group">
        <div class="phone-action-group-title">新娘家人</div>
        <div class="phone-action-item" v-if="contactInfo.sheNumber" @tap="callPhone(contactInfo.sheNumber)">
          <span class="phone-action-label">新娘</span>
          <span class="phone-action-number">{{ contactInfo.sheNumber }}</span>
        </div>
        <div class="phone-action-item" v-if="contactInfo.sheFatherNumber" @tap="callPhone(contactInfo.sheFatherNumber)">
          <span class="phone-action-label">父亲</span>
          <span class="phone-action-number">{{ contactInfo.sheFatherNumber }}</span>
        </div>
        <div class="phone-action-item" v-if="contactInfo.sheMotherNumber" @tap="callPhone(contactInfo.sheMotherNumber)">
          <span class="phone-action-label">母亲</span>
          <span class="phone-action-number">{{ contactInfo.sheMotherNumber }}</span>
        </div>
      </div>

      <div class="phone-action-cancel" @tap="showPhoneModal = false">取消</div>
    </div>
  </div>
</template>

<script setup lang="ts">


import { ref, onMounted, onUnmounted, getCurrentInstance, nextTick } from 'vue'
import { onShow, onShareAppMessage, onShareTimeline } from '@dcloudio/uni-app'


import { showToast } from '@src/utils'
import { getCommonConfig, getResouces, submitRSVP, getRsvpStats } from '@src/api/wedding-invitation'



// 获取全局实例（必须在最前面定义）
const instance = getCurrentInstance()

const info = ref<any>({})
const coverImage = ref('')
const shareImage = ref('')
const musicUrl = ref('')
const contactInfo = ref({
  heNumber: '',
  sheNumber: '',
  heFatherNumber: '',
  heMotherNumber: '',
  heSisterNumber: '',
  sheFatherNumber: '',
  sheMotherNumber: ''
})
const locationInfo = ref({
  lon: 0,
  lat: 0,
  address: ''
})
const storyList = ref<any[]>([])
const storyLoaded = ref<boolean[]>([])
const storyHidden = ref<boolean[]>([])
const touchStartX = ref<number[]>([])
const isPlaying = ref(false)
const snapEnabled = ref(true)
const showPhoneModal = ref(false)
const openingImage = 'https://klife.keyboard-man.com/wedding_invitation/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20260124150433_234_980.jpg?imageslim/zlevel/3'
const openingImage2 = 'https://klife.keyboard-man.com/wedding_invitation/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20260123172039_160_980.jpg?imageslim/zlevel/3'
const openingImage3 = 'https://klife.keyboard-man.com/wedding_invitation/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20260123172427_164_980.jpg?imageslim/zlevel/3'
const openingImage4 = 'https://klife.keyboard-man.com/wedding_invitation/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20260123172430_167_980.jpg?imageslim/zlevel/3'
const openingImage5 = 'https://klife.keyboard-man.com/wedding_invitation/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20260124150340_233_980.jpg?imageslim/zlevel/3'

// 预下载图片，减少首屏等待（微信小程序使用原生能力，其它端退回 uni.getImageInfo）
const prefetchImages = (urls: string[]) => {
  const valid = (urls || []).filter(Boolean)
  if (!valid.length) return
  valid.forEach(url => {
    // #ifdef MP-WEIXIN
    wx.getImageInfo({ src: url, success: () => {}, fail: () => {} })
    // #endif

    // #ifndef MP-WEIXIN
    uni.getImageInfo({ src: url, success: () => {}, fail: () => {} })
    // #endif
  })
}

const prefetchStaticImages = () => {
  prefetchImages([openingImage, openingImage2, openingImage3, openingImage4, openingImage5])
}



const openingInView = ref(false)


const openingInView2 = ref(false)
const openingInView3 = ref(false)
const openingInView4 = ref(false)
const openingInView5 = ref(false)

let openingObserver: UniApp.IntersectionObserver | null = null
let openingObserver2: UniApp.IntersectionObserver | null = null
let openingObserver3: UniApp.IntersectionObserver | null = null
let openingObserver4: UniApp.IntersectionObserver | null = null
let openingObserver5: UniApp.IntersectionObserver | null = null







const handleOpeningInView = () => {
  if (openingInView.value) return
  openingInView.value = true
  if (openingObserver) {
    openingObserver.disconnect()
    openingObserver = null
  }
}

const handleOpeningInView2 = () => {
  if (openingInView2.value) return
  openingInView2.value = true
  if (openingObserver2) {
    openingObserver2.disconnect()
    openingObserver2 = null
  }
}

const handleOpeningInView3 = () => {
  if (openingInView3.value) return
  openingInView3.value = true
  if (openingObserver3) {
    openingObserver3.disconnect()
    openingObserver3 = null
  }
}

const handleOpeningInView4 = () => {
  if (openingInView4.value) return
  openingInView4.value = true
  if (openingObserver4) {
    openingObserver4.disconnect()
    openingObserver4 = null
  }
}

const handleOpeningInView5 = () => {
  if (openingInView5.value) return
  openingInView5.value = true
  if (openingObserver5) {
    openingObserver5.disconnect()
    openingObserver5 = null
  }
}

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

  setupOpeningObserver()
  setupOpeningObserver2()
  setupOpeningObserver3()
  setupOpeningObserver4()
  setupOpeningObserver5()
  prefetchStaticImages()
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
  setupOpeningObserver()
  setupOpeningObserver2()
  setupOpeningObserver3()
  setupOpeningObserver4()
  setupOpeningObserver5()
})








onUnmounted(() => {
  if (innerAudioContext) {
    innerAudioContext.stop()
    innerAudioContext = null
  }
  if (openingObserver) {
    openingObserver.disconnect()
    openingObserver = null
  }
  if (openingObserver2) {
    openingObserver2.disconnect()
    openingObserver2 = null
  }
  if (openingObserver3) {
    openingObserver3.disconnect()
    openingObserver3 = null
  }
  if (openingObserver4) {
    openingObserver4.disconnect()
    openingObserver4 = null
  }
  if (openingObserver5) {
    openingObserver5.disconnect()
    openingObserver5 = null
  }
})








const setupOpeningObserver = () => {
  if (openingObserver) return
  nextTick(() => {
    openingObserver = uni.createIntersectionObserver(instance?.proxy, { thresholds: [0, 0.1, 0.2, 0.5], observeAll: true })
    openingObserver
      .relativeToViewport({ top: 0, bottom: 0 })
      .observe('.section-opening .opening-layout', (res) => {
        if (res.intersectionRatio > 0.1) {
          handleOpeningInView()
        }
      })
  })
}

const setupOpeningObserver2 = () => {
  if (openingObserver2) return
  nextTick(() => {
    openingObserver2 = uni.createIntersectionObserver(instance?.proxy, { thresholds: [0, 0.1, 0.2, 0.5], observeAll: true })
    openingObserver2
      .relativeToViewport({ top: 0, bottom: 0 })
      .observe('.section-opening-2 .opening-layout', (res) => {
        if (res.intersectionRatio > 0.1) {
          handleOpeningInView2()
        }
      })
  })
}

const setupOpeningObserver3 = () => {
  if (openingObserver3) return
  nextTick(() => {
    openingObserver3 = uni.createIntersectionObserver(instance?.proxy, { thresholds: [0, 0.1, 0.2, 0.5], observeAll: true })
    openingObserver3
      .relativeToViewport({ top: 0, bottom: 0 })
      .observe('.section-opening-3 .opening-layout', (res) => {
        if (res.intersectionRatio > 0.1) {
          handleOpeningInView3()
        }
      })
  })
}

const setupOpeningObserver4 = () => {
  if (openingObserver4) return
  nextTick(() => {
    openingObserver4 = uni.createIntersectionObserver(instance?.proxy, { thresholds: [0, 0.1, 0.2, 0.5], observeAll: true })
    openingObserver4
      .relativeToViewport({ top: 0, bottom: 0 })
      .observe('.section-opening-4 .opening-layout', (res) => {
        if (res.intersectionRatio > 0.1) {
          handleOpeningInView4()
        }
      })
  })
}

const setupOpeningObserver5 = () => {
  if (openingObserver5) return
  nextTick(() => {
    openingObserver5 = uni.createIntersectionObserver(instance?.proxy, { thresholds: [0, 0.1, 0.2, 0.5], observeAll: true })
    openingObserver5
      .relativeToViewport({ top: 0, bottom: 0 })
      .observe('.section-opening-5 .opening-layout', (res) => {
        if (res.intersectionRatio > 0.1) {
          handleOpeningInView5()
        }
      })
  })
}








const loadData = () => {


      // 获取婚礼信息
  if (import.meta.env.VITE_VUE_WECHAT_TCB === 'true') {
    const db = wx.cloud.database()
    const common = db.collection('common')
    common.get().then((res: any) => {
      info.value = typeof res.data[0].info === 'string' ? JSON.parse(res.data[0].info) : res.data[0].info
      musicUrl.value = res.data[0].videoUrl
      initAudio()
    })
  } else {
    getCommonConfig().then(res => {
      info.value = typeof res.data.info === 'string' ? JSON.parse(res.data.info) : res.data.info
      musicUrl.value = res.data.videoUrl

      // 获取联系电话
      contactInfo.value.heNumber = res.data.heNumber || ''
      contactInfo.value.sheNumber = res.data.sheNumber || ''
      console.log('[联系电话] 新郎电话:', contactInfo.value.heNumber)
      console.log('[联系电话] 新娘电话:', contactInfo.value.sheNumber)

      // 从 detail 字段中获取家人的电话信息
      if (res.data.detail) {
        try {
          const detailObj = typeof res.data.detail === 'string' ? JSON.parse(res.data.detail) : res.data.detail

          // 获取新郎家人的电话
          contactInfo.value.heFatherNumber = detailObj.heFatherNumber || ''
          contactInfo.value.heMotherNumber = detailObj.heMotherNumber || ''
          contactInfo.value.heSisterNumber = detailObj.heSisterNumber || ''

          // 获取新娘家人的电话
          contactInfo.value.sheFatherNumber = detailObj.sheFatherNumber || ''
          contactInfo.value.sheMotherNumber = detailObj.sheMotherNumber || ''

          console.log('[联系电话] 新郎父亲电话:', contactInfo.value.heFatherNumber)
          console.log('[联系电话] 新郎母亲电话:', contactInfo.value.heMotherNumber)
          console.log('[联系电话] 新郎姐姐电话:', contactInfo.value.heSisterNumber)
          console.log('[联系电话] 新娘父亲电话:', contactInfo.value.sheFatherNumber)
          console.log('[联系电话] 新娘母亲电话:', contactInfo.value.sheMotherNumber)
        } catch (err) {
          console.error('[联系电话] 解析 detail 字段失败:', err)
        }
      }

      // 获取位置信息
      if (res.data.locationLon && res.data.locationLat) {
        locationInfo.value.lon = res.data.locationLon
        locationInfo.value.lat = res.data.locationLat
      }

      // 从 hotel 字段中提取 address（用于地址显示和地图导航）
      if (res.data.hotel) {
        try {
          const hotelObj = typeof res.data.hotel === 'string' ? JSON.parse(res.data.hotel) : res.data.hotel
            locationInfo.value.address = hotelObj.address || res.data.hotel
            console.log('[酒店信息] 解析 hotel 成功:', hotelObj)
          } catch {
            locationInfo.value.address = res.data.hotel
          }
      }

      initAudio()
    })
  }


  // 获取封面图片
  getResouces('wedding-cover').then(res => {
    console.log('[封面图片] 请求 wedding-cover 类型资源，返回结果:', res)
    if (res.data && res.data.length > 0) {
      coverImage.value = res.data[0].url
      console.log('[封面图片] 成功获取封面图片 URL:', coverImage.value)
      prefetchImages([coverImage.value])
    } else {
      console.warn('[封面图片] 未找到 wedding-cover 类型的资源')
    }
  }).catch(err => {
    console.error('[封面图片] 获取 wedding-cover 类型资源失败:', err)
  })

  // 获取分享图片
  getResouces('invitation').then(res => {
    console.log('[分享图片] 请求 invitation 类型资源，返回结果:', res)
    if (res.data && res.data.length > 0) {
      shareImage.value = res.data[0].url
      console.log('[分享图片] 成功获取分享图片 URL:', shareImage.value)
      prefetchImages([shareImage.value])
    } else {
      console.warn('[分享图片] 未找到 invitation 类型的资源，将使用封面图片作为备用')
    }
  }).catch(err => {
    console.error('[分享图片] 获取 invitation 类型资源失败:', err)
  })


  // 获取爱情故事
  getResouces('love-story').then(res => {
    storyList.value = res.data || []
    storyLoaded.value = storyList.value.map(() => false)
    storyHidden.value = storyList.value.map(() => false)
    touchStartX.value = storyList.value.map(() => 0)
    // 轻微延迟触发动画，避免图片缓存瞬间加载看不到过渡
    setTimeout(() => {
      storyLoaded.value = storyList.value.map(() => true)
    }, 200)
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
  innerAudioContext.autoplay = true

  innerAudioContext.onPlay(() => {
    isPlaying.value = true
  })

  innerAudioContext.onPause(() => {
    isPlaying.value = false
  })

  innerAudioContext.onEnded(() => {
    innerAudioContext.play()
  })

  // 尝试自动播放（受微信策略影响，可能需用户手势）
  innerAudioContext.play()
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

// 打开地图导航
const openMapNavigation = () => {
  console.log('[地图导航] 打开地图导航', locationInfo.value)

  const { lon, lat, address } = locationInfo.value

  if (!lon || !lat) {
    showToast('暂无位置信息，无法导航')
    return
  }

  // 使用从 hotel 字段提取的 address；如果为空则使用默认地址
  const mapAddress = address || '四川省南充市仪陇县观紫镇大兴村四社'

  // 构建地图名称：新郎&新娘 婚礼喜宴地点
  const mapName = info.value?.name ? `${info.value.name} 婚礼喜宴地点` : '婚礼喜宴地点'

  console.log('[地图导航] 地图名称:', mapName)
  console.log('[地图导航] 地图地址:', mapAddress)
  console.log('[地图导航] 使用 hotel address:', !!address)

  // 微信小程序使用 wx.openLocation
  uni.openLocation({
    latitude: lat,
    longitude: lon,
    name: mapName,
    address: mapAddress,
    scale: 15,
    success: () => {
      console.log('[地图导航] 成功打开地图')
    },
    fail: (err: any) => {
      console.error('[地图导航] 打开地图失败:', err)
      showToast('打开地图失败，请重试')
    }
  })
}

// 显示电话选择弹窗
const showPhoneActions = () => {
  console.log('[电话选择] 打开电话选择弹窗')
  // 收集所有可用的电话号码
  const { heNumber, sheNumber, heFatherNumber, heMotherNumber, heSisterNumber, sheFatherNumber, sheMotherNumber } = contactInfo.value

  const phoneList = []
  if (heNumber) phoneList.push({ label: '新郎', number: heNumber })
  if (heFatherNumber) phoneList.push({ label: '新郎父亲', number: heFatherNumber })
  if (heMotherNumber) phoneList.push({ label: '新郎母亲', number: heMotherNumber })
  if (heSisterNumber) phoneList.push({ label: '新郎姐姐', number: heSisterNumber })
  if (sheNumber) phoneList.push({ label: '新娘', number: sheNumber })
  if (sheFatherNumber) phoneList.push({ label: '新娘父亲', number: sheFatherNumber })
  if (sheMotherNumber) phoneList.push({ label: '新娘母亲', number: sheMotherNumber })

  console.log('[电话选择] 可用电话列表:', phoneList)

  // 如果只有一个电话，直接拨打
  if (phoneList.length === 0) {
    showToast('暂无联系电话')
  } else if (phoneList.length === 1) {
    callPhone(phoneList[0].number)
  } else {
    // 有多个电话，显示选择弹窗
    showPhoneModal.value = true
  }
}

// 拨打电话
const callPhone = (phoneNumber: string) => {
  console.log('[拨打电话] 电话号码:', phoneNumber)
  showPhoneModal.value = false

  if (!phoneNumber) {
    showToast('电话号码为空')
    return
  }

  // 微信小程序使用 wx.makePhoneCall
  uni.makePhoneCall({
    phoneNumber,
    success: () => {
      console.log('[拨打电话] 成功拨打电话')
    },
    fail: (err: any) => {
      console.error('[拨打电话] 拨打电话失败:', err)
      showToast('拨打电话失败，请重试')
    }
  })
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

const markStoryLoaded = (index: number) => {
  storyLoaded.value[index] = true
}

const onStoryTouchStart = (index: number, e: any) => {
  const x = e.touches?.[0]?.clientX || 0
  touchStartX.value[index] = x
}

const onStoryTouchEnd = (index: number, e: any) => {
  const x = e.changedTouches?.[0]?.clientX || 0
  const delta = x - touchStartX.value[index]
  // 右滑隐藏，左滑显示
  if (delta > 40) {
    storyHidden.value[index] = true
  } else if (delta < -40) {
    storyHidden.value[index] = false
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

// 分享到聊天
onShareAppMessage(() => {
  const title = info.value?.name ? `${info.value.name}的婚礼邀请` : '婚礼邀请函'
  const imageUrl = shareImage.value || coverImage.value || ''
  console.log('[分享到聊天] 标题:', title)
  console.log('[分享到聊天] 使用图片类型:', shareImage.value ? 'invitation' : 'wedding-cover')
  console.log('[分享到聊天] 图片URL:', imageUrl)
  console.log('[分享到聊天] shareImage.value:', shareImage.value)
  console.log('[分享到聊天] coverImage.value:', coverImage.value)
  return {
    title,
    path: '/pages/index/index',
    imageUrl
  }
})

// 分享到朋友圈
onShareTimeline(() => {
  const title = info.value?.name ? `${info.value.name}的婚礼邀请` : '婚礼邀请函'
  const imageUrl = shareImage.value || coverImage.value || ''
  console.log('[分享到朋友圈] 标题:', title)
  console.log('[分享到朋友圈] 使用图片类型:', shareImage.value ? 'invitation' : 'wedding-cover')
  console.log('[分享到朋友圈] 图片URL:', imageUrl)
  console.log('[分享到朋友圈] shareImage.value:', shareImage.value)
  console.log('[分享到朋友圈] coverImage.value:', coverImage.value)
  return {
    title,
    query: '',
    imageUrl
  }
})
</script>


<style lang="scss" scoped>
.wedding-page {
  min-height: 100vh;
  background: linear-gradient(180deg, #fff5f5 0%, #ffe8e8 100%);
  font-family: 'Noto Serif SC', 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'Helvetica Neue', Arial, sans-serif;
  color: #1f1f1f;
  letter-spacing: 0.2rpx;
  -webkit-font-smoothing: antialiased;
}

.snap-mode {
  height: 100vh;
  overflow-y: auto;
  scroll-snap-type: y mandatory;
}

.snap-mode .snap-section {
  scroll-snap-align: start;
  min-height: 100vh;
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
  padding: 68rpx 46rpx;
  position: relative;
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

.section-opening {
  position: relative;
  min-height: 100vh;
  padding: 32rpx 20rpx 36rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #fff5e1;
}

.section-opening-2 {
  background: radial-gradient(120% 120% at 50% 20%, rgba(255, 235, 214, 0.9) 0%, rgba(246, 222, 214, 0.9) 50%, rgba(240, 210, 200, 0.92) 100%), #f5e9df;
  padding: 40rpx 24rpx 44rpx;
}

.section-opening-4 {
  background: radial-gradient(140% 140% at 50% 10%, rgba(253, 248, 241, 0.95) 0%, rgba(245, 236, 228, 0.94) 45%, rgba(236, 224, 215, 0.95) 100%), #f9f4ed;
  padding: 0;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: stretch !important;
  justify-content: flex-start !important;
  gap: 0;
}

.section-opening-5 {
  background: radial-gradient(120% 120% at 50% 20%, rgba(255, 235, 214, 0.9) 0%, rgba(246, 222, 214, 0.9) 50%, rgba(240, 210, 200, 0.92) 100%), #f5e9df;
  padding: 40rpx 24rpx 44rpx;
}

















.section-opening::before {
  display: none;
}



.opening-layout {
  width: 100%;
  max-width: 960rpx;
  display: grid;
  gap: 20rpx;
  align-items: center;
  align-content: center;
  justify-items: center;
  text-align: center;
  background: transparent;
  border-radius: 0;
  padding: 0;
  box-shadow: none;
}

.opening-layout-2 {
  max-width: 1040rpx;
  gap: 24rpx;
  align-items: start;
  justify-items: stretch;
  text-align: left;
  background: rgba(255, 255, 255, 0.78);
  border-radius: 28rpx;
  padding: 32rpx 26rpx;
  box-shadow: 0 18rpx 38rpx rgba(133, 83, 60, 0.16);
}






.opening-top-text {
  background: transparent;
  border-radius: 0;
  padding: 0 12rpx 0;
  margin-bottom: 20rpx;
  box-shadow: none;


  position: relative;
  color: rgba(46, 25, 19, 0.9);
  font-size: 33rpx;
  line-height: 1.68;
  letter-spacing: 0.9rpx;
  font-weight: 520;
  font-family: 'Noto Serif SC', 'Source Han Serif SC', 'Songti SC', 'SF Pro Display', 'PingFang SC', 'Helvetica Neue', 'system-ui', Arial, serif;
  text-shadow: 0 1rpx 3rpx rgba(0, 0, 0, 0.08);
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;

  opacity: 0;
  transform: translateY(14rpx);
  &.in-view {
    animation: fadeSoft 1s ease both;
    opacity: 1;
    transform: translateY(0);
  p {
    margin: 10rpx 0;
    white-space: nowrap;
    animation: fadeLine 1s ease-out both;
  }
  .title-line { font-weight: 650; letter-spacing: 1.2rpx; }
  p:nth-child(1) { animation-delay: 0s; }
  p:nth-child(2) { animation-delay: 0.18s; }
  p:nth-child(3) { animation-delay: 0.36s; }
  p:nth-child(4) { animation-delay: 0.54s; }
  p:nth-child(5) { animation-delay: 0.72s; }

  }
}



.opening-top-text::before {

  content: '';
  position: absolute;
  inset: -6rpx -12rpx -6rpx -12rpx;
  background: linear-gradient(180deg, rgba(255, 245, 225, 0.65), rgba(255, 245, 225, 0));
  border-radius: 18rpx;
  z-index: -1;
  pointer-events: none;
}

/* 第三屏特化样式 */
.opening-layout-2 {
  .opening-top-text {
    padding: 0;
    margin-bottom: 14rpx;
    text-align: left;
    border-left: 8rpx solid #b43b3b;
    padding-left: 18rpx;
    opacity: 0;
    transform: translateY(14rpx);
    &.in-view {
      animation: fadeSoft 1s ease both;
      opacity: 1;
      transform: translateY(0);
      p { white-space: normal; }
      p:nth-child(1) { animation-delay: 0s; }
      p:nth-child(2) { animation-delay: 0.2s; }
      p:nth-child(3) { animation-delay: 0.4s; }
      p:nth-child(4) { animation-delay: 0.6s; }
      p:nth-child(5) { animation-delay: 0.8s; }
    }
  }
  .opening-top-text::before { display: none; }
  .opening-top-text .title-line {
    font-size: 36rpx;
    letter-spacing: 1.6rpx;
    color: #361510;
  }
  .opening-top-text p { margin: 10rpx 0 0; }

  .opening-photo-2 {
    width: 100%;
    height: 54vh;
    object-fit: cover;
    border-radius: 24rpx;
    border: 6rpx solid rgba(255, 255, 255, 0.7);
    box-shadow: 0 18rpx 36rpx rgba(106, 54, 35, 0.22);
  }
}

/* 第四屏对称排版：图上文下、右对齐 */
.opening-layout-3 {
  gap: 22rpx;
  align-items: start;
  justify-items: end;
  text-align: right;

  .opening-photo-2 {
    height: 50vh;
    border-radius: 26rpx;
    border: 5rpx solid rgba(255, 255, 255, 0.65);
    box-shadow: 0 16rpx 32rpx rgba(84, 48, 32, 0.18);
  }

  .opening-top-text-3 {
    padding: 0;
    margin: 0;
    border-left: none;
    border-right: 8rpx solid #8a2c2c;
    padding-right: 18rpx;
    text-align: right;
    align-self: stretch;
    opacity: 0;
    transform: translateY(14rpx);
    &.in-view {
      animation: fadeSoft 1s ease both;
      opacity: 1;
      transform: translateY(0);
      p { white-space: normal; }
      p:nth-child(1) { animation-delay: 0s; }
      p:nth-child(2) { animation-delay: 0.2s; }
      p:nth-child(3) { animation-delay: 0.4s; }
      p:nth-child(4) { animation-delay: 0.6s; }
      p:nth-child(5) { animation-delay: 0.8s; }
      p:nth-child(6) { animation-delay: 1.0s; }
      p:nth-child(7) { animation-delay: 1.2s; }
      p:nth-child(8) { animation-delay: 1.4s; }
      p:nth-child(9) { animation-delay: 1.6s; }
    }
    .title-line {
      font-size: 36rpx;
      letter-spacing: 1.4rpx;
      color: #2f120e;
      font-weight: 660;
    }
    p { margin: 10rpx 0 0; }
  }
}

/* 第五屏：统一卡片容器，图片上文字下，居中对齐 */
.opening-layout-4 {
  width: 90vw;
  max-width: 90vw;
  margin: 40rpx auto;
  background: rgba(255, 255, 255, 0.85);
  border-radius: 28rpx;
  box-shadow: 0 16rpx 40rpx rgba(106, 54, 35, 0.15);
  overflow: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  text-align: center;
  padding: 0;
  backdrop-filter: blur(10rpx);
}




.opening-photo-4 {
  width: 100%;
  max-width: 100%;
  height: 48vh;
  object-fit: cover;
  border-radius: 0;
  border: none;
  box-shadow: none;
  margin: 0;
  padding: 0;
  display: block;
}

/* 第五屏统一卡片样式 */
.section-opening-4 .opening-photo,
.section-opening-4 .opening-photo-2,
.section-opening-4 .opening-photo-4 {
  width: 100% !important;
  max-width: 100% !important;
  height: 48vh;
  object-fit: cover;
  border-radius: 0 !important;
  border: 0 !important;
  box-shadow: none !important;
  margin: 0 !important;
  padding: 0 !important;
  display: block;
}







.opening-layout-4 .opening-top-text,
.opening-layout-4 .opening-top-text-4 {
  padding: 40rpx 48rpx;
  margin: 0;
  max-width: 100%;
  text-align: center;
  background: transparent;
  border-radius: 0;
  box-shadow: none;
  opacity: 0;
  transform: translateY(14rpx);
  line-height: 1.88;
  letter-spacing: 0.9rpx;

  &.in-view {
    animation: fadeSoft 1s ease both;
    opacity: 1;
    transform: translateY(0);
    p { white-space: normal; }
    p:nth-child(1) { animation-delay: 0s; }
    p:nth-child(2) { animation-delay: 0.18s; }
    p:nth-child(3) { animation-delay: 0.36s; }
    p:nth-child(4) { animation-delay: 0.54s; }
    p:nth-child(5) { animation-delay: 0.72s; }
    p:nth-child(6) { animation-delay: 0.90s; }
    p:nth-child(7) { animation-delay: 1.08s; }
  }
  .title-line {
    font-size: 38rpx;
    letter-spacing: 1.6rpx;
    color: #2f120e;
    font-weight: 700;
    margin-bottom: 16rpx;
  }
  p { margin: 12rpx 0 0; }
  p:last-child {
    text-align: center;
    font-weight: 650;
    margin-top: 20rpx;
    color: #b43b3b;
  }
}


/* 第六屏：沿用第二屏风格，上文下图排版 */
.section-opening-5 {
  @extend .section-opening;
  padding: 8rpx 20rpx 22rpx;
}

.opening-layout-5-copy {
  width: 90vw;
  max-width: 90vw;
  margin: 40rpx auto;
  background: rgba(255, 255, 255, 0.88);
  border-radius: 28rpx;
  box-shadow: 0 16rpx 40rpx rgba(106, 54, 35, 0.15);
  overflow: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  text-align: center;
  padding: 0;
  backdrop-filter: blur(10rpx);
}

.opening-photo-5-copy {
  width: 100%;
  max-width: 100%;
  height: 30vh;
  max-height: 300rpx;
  object-fit: cover;
  border-radius: 0;
  border: none;
  box-shadow: none;
  margin: 0;
  padding: 0;
  display: block;
}



.opening-top-text-5-copy {
  max-width: 100%;
  margin: 0;
  padding: 24rpx 40rpx 20rpx;
  text-align: center;
  line-height: 1.68;
  background: transparent;
  border-radius: 0;
  box-shadow: none;
}

.opening-top-text-5-copy p {
  font-weight: 520;
  font-size: 30rpx;
  margin: 8rpx 0;
}

.rsvp-inline {
  background: transparent;
  border-radius: 0;
  padding: 0 40rpx 32rpx;
  box-shadow: none;
  display: flex;
  flex-direction: column;
  gap: 10rpx;
  width: 100%;
}


.rsvp-inline .section-header {
  margin-bottom: 12rpx;
}

.rsvp-inline .rsvp-form {
  margin-bottom: 6rpx;
}

.rsvp-inline .stats-card {
  margin-top: 4rpx;
}

.form-item.form-inline {
  display: flex;
  align-items: center;
  gap: 16rpx;
}

.form-item.form-inline .form-label {
  margin-bottom: 0;
  min-width: 160rpx;
}

.form-item.form-inline .form-input,
.form-item.form-inline .form-textarea-single,
.form-item.form-inline .form-textarea-double {
  flex: 1;
  width: auto;
}

.radio-group.inline {


  display: flex;
  gap: 28rpx;
  align-items: center;
}





























.opening-photo {

  width: 100%;
  max-width: 900rpx;
  height: 46vh;
  object-fit: cover;
  border-radius: 30rpx;
  box-shadow: 0 10rpx 22rpx rgba(0, 0, 0, 0.12);
  margin: 12rpx 0 14rpx;
  transition: transform 0.35s ease, box-shadow 0.35s ease;
}

.opening-photo:active {
  transform: scale(0.993);
  box-shadow: 0 8rpx 18rpx rgba(0, 0, 0, 0.10);
}



.opening-bottom-info {
  background: transparent;
  border-radius: 0;
  padding: 0 12rpx 0;
  margin-top: 24rpx;
  box-shadow: none;




  color: #b22222;
  font-size: 30rpx;
  line-height: 1.58;
  letter-spacing: 0.6rpx;
  word-spacing: 2rpx;
  font-weight: 700;



  opacity: 0;
  transform: translateY(14rpx);
  display: grid;
  gap: 14rpx;
  &.in-view {
    animation: fadeSoft 1s ease 0.05s both;
    opacity: 1;
    transform: translateY(0);
    .overlay-info {
      animation: fadeLine 1s ease-out both;
    }
    .overlay-info:nth-child(1) { animation-delay: 0.25s; }
    .overlay-info:nth-child(2) { animation-delay: 0.50s; }
  }





  .label {
    color: #3a1f1a;
    font-size: 26rpx;
    font-weight: 500;
  }
  .highlight {
    color: #b22222;
    font-weight: 800;
  }
  .overlay-parents {
    display: inline-flex;
    align-items: baseline;
    justify-content: center;
    gap: 10rpx;
    white-space: nowrap;
  }
  .overlay-parents .separator {
    color: #b22222;
    font-weight: 700;
    opacity: 0.8;
    padding: 0 4rpx;
  }
}

/* 操作按钮容器 */
.action-buttons-container {
  margin-top: 32rpx;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20rpx;
  padding: 0 20rpx;
  opacity: 0;
  transform: translateY(14rpx);
  animation: fadeSoft 1s ease 0.5s both;
}

/* 操作按钮 */
.action-button {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8rpx;
  width: 100%;
  height: 88rpx;
  color: #fff;
  border: none;
  border-radius: 44rpx;
  font-size: 28rpx;
  font-weight: 700;
  letter-spacing: 0.5rpx;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

/* 电话按钮 */
.phone-button {
  background: linear-gradient(135deg, #ff6b9d, #ff8fb3);
  box-shadow: 0 8rpx 24rpx rgba(255, 107, 157, 0.35);
}

.phone-button:active {
  transform: scale(0.97);
  box-shadow: 0 4rpx 12rpx rgba(255, 107, 157, 0.25);
}

/* 导航按钮 */
.nav-button {
  background: linear-gradient(135deg, #ff4c91, #ff6b9d);
  box-shadow: 0 8rpx 24rpx rgba(255, 76, 145, 0.35);
}

.nav-button:active {
  transform: scale(0.97);
  box-shadow: 0 4rpx 12rpx rgba(255, 76, 145, 0.25);
}

/* 按钮图标 */
.action-icon {
  font-size: 32rpx;
}

/* 按钮文字 */
.action-text {
  font-size: 28rpx;
  font-weight: 700;
}

/* 电话选择弹窗 */
.phone-action-sheet {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: flex-end;
  justify-content: center;
  z-index: 1000;
  animation: fadeIn 0.3s ease;
}

.phone-action-content {
  width: 100%;
  background: #fff;
  border-radius: 24rpx 24rpx 0 0;
  overflow: hidden;
  animation: slideUp 0.3s ease;
}

.phone-action-header {
  padding: 32rpx;
  text-align: center;
  font-size: 32rpx;
  font-weight: 700;
  color: #3a1f1a;
  border-bottom: 1rpx solid #f0f0f0;
}

.phone-action-group-title {
  padding: 24rpx 32rpx 16rpx;
  font-size: 26rpx;
  font-weight: 600;
  color: #999;
  background: #fafafa;
}

.phone-action-group:first-child .phone-action-group-title {
  padding-top: 32rpx;
}

.phone-action-group:last-child {
  padding-bottom: 16rpx;
}

.phone-action-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 32rpx;
  border-bottom: 1rpx solid #f5f5f5;
  transition: background 0.2s ease;
}

.phone-action-item:active {
  background: #f9f9f9;
}

.phone-action-label {
  font-size: 30rpx;
  color: #3a1f1a;
  font-weight: 500;
}

.phone-action-number {
  font-size: 32rpx;
  color: #ff4c91;
  font-weight: 700;
}

.phone-action-cancel {
  padding: 32rpx;
  text-align: center;
  font-size: 30rpx;
  color: #666;
  font-weight: 500;
  border-top: 1rpx solid #f5f5f5;
  margin-top: 8rpx;
}

.phone-action-cancel:active {
  background: #f9f9f9;
}












.opening {
  position: relative;
  background: rgba(255, 255, 255, 0.9);
  border-radius: 24rpx;
  padding: 38rpx 22rpx 36rpx;
  box-shadow: 0 16rpx 38rpx rgba(255, 76, 145, 0.16);
  color: #202020;
  max-width: 920rpx;
  width: 100%;
  display: grid;
  gap: 32rpx;
  justify-items: center;
  backdrop-filter: blur(8rpx);
}





.opening-timeline {
  position: relative;
  display: grid;
  gap: 20rpx;
  margin: 0 auto 18rpx;
  text-align: left;
  width: fit-content;
  max-width: 920rpx;
  padding: 30rpx 38rpx 30rpx 64rpx;
  border-radius: 24rpx;
  border-left: 6rpx solid #ff7ab3;
  background: rgba(255, 255, 255, 0.78);
  box-shadow: 0 14rpx 34rpx rgba(255, 76, 145, 0.12);
  backdrop-filter: blur(6rpx);
  overflow: hidden;
  animation: fadeList 0.9s ease both;
}
.opening-timeline::before {
  content: '';
  position: absolute;
  left: 28rpx;
  top: 20rpx;
  bottom: 20rpx;
  width: 4rpx;
  background: linear-gradient(180deg, rgba(255, 124, 167, 0.6), rgba(255, 76, 145, 0.22));
  border-radius: 999rpx;
}




.opening-row {
  position: relative;
  display: flex;
  justify-content: flex-start;
  gap: 20rpx;
  align-items: baseline;
  font-size: 34rpx;
  line-height: 1.75;
  padding-left: 10rpx;
  animation: slideRow 0.9s ease both;
}
.opening-row::before {
  content: '';
  position: absolute;
  left: -34rpx;
  top: 16rpx;
  width: 14rpx;
  height: 14rpx;
  border-radius: 50%;
  border: 3rpx solid #ff7ab3;
  background: #fff;
  box-shadow: 0 6rpx 18rpx rgba(255, 76, 145, 0.25);
  animation: nodePulse 2.8s ease-in-out infinite;
}
.opening-row:nth-child(1) { animation-delay: 0.05s; }
.opening-row:nth-child(2) { animation-delay: 0.15s; }
.opening-row:nth-child(3) { animation-delay: 0.25s; }
.opening-row:nth-child(4) { animation-delay: 0.35s; }




.opening-time {
  min-width: 240rpx;
  text-align: left;
  font-weight: 800;
  color: #ff4c91;
  font-variant-numeric: tabular-nums;
  letter-spacing: 1rpx;
  padding: 6rpx 14rpx;
  border-radius: 16rpx;
  background: rgba(255, 124, 167, 0.08);
  border: 1rpx solid rgba(255, 124, 167, 0.18);
}



.opening-desc {
  text-align: left;
  color: #1f1f1f;
  font-weight: 700;
  letter-spacing: 0.6rpx;
}

















/* 封面 */
.section-cover {
  position: relative;
  height: 100vh;
  overflow: hidden;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  background: #000;
  .cover-photo {
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  .cover-overlay {
    display: none;
  }

  .cover-content {
    position: relative;
    z-index: 1;
    text-align: center;
    padding: 0 60rpx 120rpx;
    width: 100%;
    animation: fadeInUp 1s ease-out;
    .couple-names {
      font-size: 72rpx;
      color: #fff;
      font-weight: 800;
      margin-bottom: 14rpx;
      letter-spacing: 3rpx;
      text-shadow: 0 10rpx 26rpx rgba(0, 0, 0, 0.52), 0 0 12rpx rgba(0, 0, 0, 0.36);
      animation: softFloat 4s ease-in-out infinite alternate;
    }

    .cover-divider {
      width: 140rpx;
      height: 6rpx;
      margin: 10rpx auto 18rpx;
      border-radius: 999rpx;
      background: linear-gradient(90deg, rgba(255, 162, 203, 0.2), rgba(255, 255, 255, 0.9), rgba(255, 162, 203, 0.2));
      box-shadow: 0 8rpx 18rpx rgba(0, 0, 0, 0.18);
      backdrop-filter: blur(6rpx);
      animation: dividerShine 3.6s ease-in-out infinite;
    }

    .title-text {
      font-size: 36rpx;
      color: #fff;
      font-weight: 700;
      letter-spacing: 1.6rpx;
      margin-bottom: 18rpx;
      padding: 14rpx 32rpx;
      display: inline-block;
      border-radius: 999rpx;
      background: rgba(255, 255, 255, 0.16);
      border: 1rpx solid rgba(255, 255, 255, 0.45);
      box-shadow: 0 8rpx 22rpx rgba(0, 0, 0, 0.35);
      text-shadow: 0 5rpx 12rpx rgba(0, 0, 0, 0.35);
      backdrop-filter: blur(8rpx);
      animation: glowPulse 3.8s ease-in-out infinite;
    }

    .cover-subtitle {
      font-size: 30rpx;
      color: rgba(255, 255, 255, 0.92);
      letter-spacing: 1rpx;
      margin-bottom: 32rpx;
      text-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.35);
      animation: softFade 2.1s ease 0.2s both;
    }


    .scroll-hint {
      margin-top: 8rpx;
      display: flex;
      flex-direction: column;

      align-items: center;
      gap: 8rpx;
      .arrow {
        font-size: 40rpx;
        color: #fff;
        text-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.35);
        animation: bounce 1.6s infinite;
      }
      .arrow-secondary {
        opacity: 0.7;
        font-size: 34rpx;
        animation-delay: 0.2s;
      }
      &::after {
        content: '继续下滑';
        font-size: 22rpx;
        letter-spacing: 1rpx;
        color: #fff;
        opacity: 0.9;
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

@keyframes glowPulse {
  0% { box-shadow: 0 8rpx 22rpx rgba(0, 0, 0, 0.28); transform: translateY(0); }
  50% { box-shadow: 0 12rpx 32rpx rgba(255, 255, 255, 0.2); transform: translateY(-4rpx); }
  100% { box-shadow: 0 8rpx 22rpx rgba(0, 0, 0, 0.28); transform: translateY(0); }
}

@keyframes dividerShine {
  0% { opacity: 0.85; filter: brightness(1); }
  50% { opacity: 1; filter: brightness(1.25); }
  100% { opacity: 0.85; filter: brightness(1); }
}

@keyframes softFade {
  from { opacity: 0; transform: translateY(10rpx); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes softFloat {
  0% { transform: translateY(0); }
  100% { transform: translateY(-10rpx); }
}

@keyframes fadeList {
  from { opacity: 0; transform: translateY(16rpx); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes nodePulse {
  0% { transform: scale(1); box-shadow: 0 6rpx 18rpx rgba(255, 76, 145, 0.2); }
  50% { transform: scale(1.08); box-shadow: 0 10rpx 26rpx rgba(255, 76, 145, 0.32); }
  100% { transform: scale(1); box-shadow: 0 6rpx 18rpx rgba(255, 76, 145, 0.2); }
}

@keyframes fadeSoft {
  from { opacity: 0; transform: translateY(16rpx); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes fadeLine {
  from { opacity: 0; transform: translateY(10rpx); }
  to { opacity: 1; transform: translateY(0); }
}


@keyframes slideRow {

  from { opacity: 0; transform: translateY(14rpx); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideUp {
  from { transform: translateY(100%); }
  to { transform: translateY(0); }
}

/* 爱情故事 */



.story-timeline {
  display: grid;
  gap: 56rpx;
  padding: 0 28rpx;
}

.story-item {

  margin: 0;
  text-align: center;
  &.left,
  &.right {
    padding: 0;
    text-align: center;
  }
  .story-time {
    display: none;
  }

  .story-content {
    width: 100%;
    padding: 0;
    background: transparent;
    box-shadow: none;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 18rpx;
  }
  .story-card {
    position: relative;
    width: 100%;
    height: 70vh;
    overflow: hidden;
    border-radius: 24rpx;
    box-shadow: 0 20rpx 52rpx rgba(0, 0, 0, 0.24);
  }

  .story-photo {
    width: 100%;
    height: 100%;
    max-width: none;
    border-radius: 0;
    object-fit: cover;
    display: block;
  }
  .story-desc-overlay {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    padding: 46rpx 38rpx 78rpx;
    font-size: 40rpx;
    font-weight: 700;
    line-height: 1.9;
    color: #fff;
    letter-spacing: 1.2rpx;
    background: linear-gradient(180deg, rgba(0, 0, 0, 0.05) 5%, rgba(0, 0, 0, 0.65) 40%, rgba(0, 0, 0, 0.9));
    opacity: 0;
    transform: translateY(22rpx);
    transition: opacity 0.6s ease, transform 0.6s ease;
    text-shadow: 0 6rpx 16rpx rgba(0, 0, 0, 0.55);
    backdrop-filter: blur(4rpx);
  }

  .story-desc-show {
    opacity: 1;
    transform: translateY(0);
  }
  .story-arrow {
    position: absolute;
    right: 32rpx;
    bottom: 26rpx;
    font-size: 32rpx;
    opacity: 0.92;
    display: inline-flex;
    align-items: center;
    gap: 6rpx;
    padding: 8rpx 14rpx;
    border-radius: 999rpx;
    background: rgba(0, 0, 0, 0.35);
    color: #fff;
  }





  .story-desc {
    display: none;
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
  .form-textarea-single {
    min-height: 72rpx;
    height: 72rpx;
    padding: 14rpx 20rpx;
    line-height: 1.5;
  }
  .form-textarea-double {
    min-height: 120rpx;
    max-height: 160rpx;
    height: 120rpx;
    padding: 18rpx 20rpx;
    line-height: 1.6;
    resize: none;
    box-sizing: border-box;
    overflow: auto;
    flex-shrink: 0;
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

.share-btn-inline {
  margin-top: 26rpx;
  width: 100%;
  height: 88rpx;
  background: #fff;
  color: #ff4c91;
  border: 2rpx solid #ff4c91;
  border-radius: 44rpx;
  font-size: 30rpx;
  font-weight: 700;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 12rpx;
  box-shadow: 0 8rpx 18rpx rgba(255, 76, 145, 0.16);
}


.stats-card {

  display: flex;
  justify-content: center;
  align-items: center;
  gap: 24rpx;
  background: #fff;
  border-radius: 16rpx;
  padding: 16rpx 20rpx;
  box-shadow: 0 8rpx 22rpx rgba(255, 76, 145, 0.12);
}

.stats-inline .stat-inline-text {
  font-size: 28rpx;
  color: #ff4c91;
  font-weight: 700;
}

.stats-inline .stat-inline-text + .stat-inline-text {
  margin-left: 10rpx;
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
