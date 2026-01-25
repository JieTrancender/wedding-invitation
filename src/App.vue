<!--
 * @Author: zouyaoji@https://github.com/zouyaoji
 * @Date: 2022-04-12 21:49:06
 * @LastEditTime: 2023-08-20 00:48:17
 * @LastEditors: zouyaoji 370681295@qq.com
 * @Description:
 * @FilePath: \wedding-invitation\src\App.vue
-->
<script setup lang="ts">
import { onLaunch, onShow, onHide } from '@dcloudio/uni-app'
import { getCurrentInstance } from 'vue'
import { code2Session } from './api/wedding-invitation'

const instance = getCurrentInstance()

onLaunch(() => {
  console.log('App Launch')

  // 先检查本地存储中是否已有openid
  try {
    const cachedOpenId = uni.getStorageSync('cached_openid')
    if (cachedOpenId) {
      instance.appContext.config.globalProperties.$MpUserData = {
        openid: cachedOpenId
      }
      console.log('从本地缓存恢复 openid:', cachedOpenId)
    }
  } catch (err) {
    console.log('读取缓存失败:', err)
  }

  // 在 onLaunch 中执行登录，避免热重载导致重复调用
  uni.login({
    provider: 'weixin',
    success: async (res) => {
      console.log('uni.login 成功，code:', res.code)

      // 尝试获取用户信息（如果用户之前已经授权过）
      let userInfo = null
      try {
        const userInfoRes = await new Promise((resolve, reject) => {
          uni.getUserInfo({
            success: resolve,
            fail: reject
          })
        }) as any
        userInfo = {
          nickName: userInfoRes.userInfo.nickName,
          avatarUrl: userInfoRes.userInfo.avatarUrl
        }
        console.log('获取到用户信息:', userInfo)
      } catch (userInfoErr) {
        console.log('获取用户信息失败（需要用户授权）:', userInfoErr)
        userInfo = null
      }

      // 调用code2Session，如果获取到用户信息则一并传递
      try {
        const sessionRes = (await code2Session(res.code, userInfo)) as any
        console.log('code2Session 响应:', sessionRes)

        // 从响应的 data 中获取 openid 和 user
        const openid = sessionRes.data?.openid
        const user = sessionRes.data?.user

        console.log('解析出的 openid:', openid)
        console.log('解析出的 user:', user)

        if (openid) {
          // 更新全局用户数据（统一使用小写openid）
          instance.appContext.config.globalProperties.$MpUserData = {
            openid: openid,
            ...user
          }
          console.log('设置 $MpUserData:', instance.appContext.config.globalProperties.$MpUserData)

          // 缓存 openid 到本地存储，避免每次启动都需要重新登录
          try {
            uni.setStorageSync('cached_openid', openid)
            console.log('openid 已缓存到本地')
          } catch (err) {
            console.log('缓存 openid 失败:', err)
          }

          // 立即验证设置是否成功
          console.log('验证 $MpUserData:', instance.appContext.config.globalProperties.$MpUserData?.openid)
        }
      } catch (err) {
        console.log('登录流程出错:', err)
        console.error('无法完成登录，code:', res.code)
        // 不再重复调用 code2Session，因为微信 code 只能使用一次
        // 用户需要重新进入小程序或刷新页面
      }
    },
    fail: err => {
      console.log('login fail:', err)
    }
  })

  // 获取系统信息
  uni.getSystemInfo({
    success: function (e) {
      instance.appContext.config.globalProperties.$StatusBar = e.statusBarHeight
      if (e.platform === 'android') {
        instance.appContext.config.globalProperties.$CustomBar = e.statusBarHeight! + 50
      } else {
        instance.appContext.config.globalProperties.$CustomBar = e.statusBarHeight! + 45
      }

      // #ifdef MP-WEIXIN
      instance.appContext.config.globalProperties.$StatusBar = e.statusBarHeight
      const custom = uni.getMenuButtonBoundingClientRect()
      instance.appContext.config.globalProperties.$Custom = custom
      instance.appContext.config.globalProperties.$CustomBar = custom.bottom + custom.top - e.statusBarHeight!
      // #endif

      // #ifdef MP-ALIPAY
      instance.appContext.config.globalProperties.$StatusBar = e.statusBarHeight
      instance.appContext.config.globalProperties.$CustomBar = e.statusBarHeight! + e.titleBarHeight!
      // #endif
    },
    fail: function (e) {
      console.log(e)
    }
  })
})

onShow(() => {
  console.log('App Show')
})

onHide(() => {
  console.log('App Hide')
})
</script>
<style lang="scss">
@import 'common/colorui/main.css';
@import 'common/colorui/icon.css';
@import 'common/colorui/animation.css';

page {
  height: 100%;
}
image {
  display: block;
}
.animate-ele-warp {
  width: 100%;
  height: 100%;
  transform-origin: center top;
  position: absolute;
  z-index: 3;
  transform: translate3d(0px, 0px, 3px);
  pointer-events: none;
  .animate-ele {
    position: absolute;
    width: 100%;
    height: 100%;
    left: 20rpx;
    top: 20rpx;
    z-index: 3;
    pointer-events: none;
    .animate-img {
      position: absolute;
    }
  }
}
.bg-image {
  position: fixed;
  width: 100%;
  height: 100%;
}
</style>
