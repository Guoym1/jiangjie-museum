<template>
  <div class="home-page">
    <!-- 1. 顶部导航栏 -->
    <div class="top-bar">
      <div class="left-placeholder">
        <img class="topbar-img" :src="topbarImg" alt="logo" @click="goToUrl('/mine')" style="cursor:pointer;" />
      </div>
      <div class="title">故里红梅</div>
      <div class="right-icons">
        <span class="icon">🔔</span>
        <span class="avatar">👤</span>
      </div>
    </div>

    <!-- 2. 自动轮播 Banner（5秒切换 + 黑色占位） -->
    <div class="banner-container">
      <div class="banner-swiper" :style="{ transform: `translateX(-${currentBanner * 100}%)` }">
        <div class="banner-item" v-for="(item, index) in bannerList" :key="index">
          <img class="banner-img" :src="item.img" @click="goToUrl('https://www.baidu.com')" />
        </div>
      </div>
      <div class="banner-indicators">
        <span 
          class="indicator" 
          :class="{ active: currentBanner === index }"
          v-for="(item, index) in bannerList" 
          :key="index"
        ></span>
      </div>
    </div>

    <!-- 3. 今日开放和预约区域 -->
    <div class="combined-section">
      <!-- 今日开放卡片 -->
      <div class="open-card">
        <div class="left">
          <div class="card-title">今日开放</div>
          <div class="ticket">剩余票量 13,720</div>
          <div class="info">九月二十八日，星期六，15℃，出门请携带雨具</div>
        </div>
        <div class="right placeholder-img" @click="goToUrl('https://www.baidu.com')">
          <img class="right-img" :src="welcomeImg" alt="欢迎" />
        </div>
      </div>

      <!-- 预约 / AR / 助农 -->
      <div class="actions-wrapper">
        <div class="btn-item left-btn" @click="goToUrl('/mine/book')">
          <img class="left-btn-img" :src="appointmentImg" alt="立即预约" />
        </div>
        <div class="right-actions">
          <div class="btn-item right-first-btn" @click="goToUrl('https://www.baidu.com')">
            <img class="right-btn-img" :src="arImg" alt="AR游览" />
          </div>
          <div class="btn-item right-second-btn" @click="goToUrl('/travel/shop')">
            <img class="right-btn-img" :src="redFarmImg" alt="红色助农" />
          </div>
        </div>
      </div>
    </div>

    <!-- 5. 快捷功能区（五个独立div） -->
    <div class="func-row">
      <div class="func-block" @click="goToUrl('https://www.baidu.com')">
        <img class="func-block-img" :src="volunteerImg" alt="沉浸游戏" />
        <div class="func-block-name">沉浸游戏</div>
      </div>
      <div class="func-block" @click="goToUrl('https://www.baidu.com')">
        <img class="func-block-img" :src="layer29Img" alt="全景故里" />
        <div class="func-block-name">全景故里</div>
      </div>
      <div class="func-block" @click="goToUrl('/beautiful')">
        <img class="func-block-img" :src="layer3Img" alt="出行指南" />
        <div class="func-block-name">出行指南</div>
      </div>
      <div class="func-block" @click="goToUrl('https://www.baidu.com')">
        <img class="func-block-img" :src="layer30Img" alt="研读经典" />
        <div class="func-block-name">研读经典</div>
      </div>
      <div class="func-block" @click="goToUrl('https://www.baidu.com')">
        <img class="func-block-img" :src="layer5Img" alt="英烈寄语" />
        <div class="func-block-name">英烈寄语</div>
      </div>
    </div>

    <!-- 6. 故里展览 -->
    <div class="exhibit-section">
      <div class="section-title">故里展览</div>
      <div class="exhibit-video-list">
        <div class="exhibit-video-item" v-for="(item, idx) in exhibitionList" :key="idx">
          <video class="exhibit-video" :src="item.src" controls preload="metadata" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import appointmentImg from '@/imgs/立即预约.png'
import arImg from '@/imgs/AR游览.png'
import redFarmImg from '@/imgs/红色助农.png'
import volunteerImg from '@/imgs/volunteer.png'
import layer29Img from '@/imgs/图层 29.png'
import layer3Img from '@/imgs/图层 3.png'
import layer30Img from '@/imgs/图层 30.png'
import layer5Img from '@/imgs/图层 5.png'
import topbarImg from '@/imgs/图层 26.png'
import banner1 from '@/imgs/轮播1.png'
import banner2 from '@/imgs/轮播2.jpg'
import banner3 from '@/imgs/轮播3.jpg'
import banner4 from '@/imgs/轮播4.jpg'
import welcomeImg from '@/imgs/欢迎.png'
import exhibition1 from '@/imgs/展览1.mp4'
import exhibition2 from '@/imgs/展览2.mp4'
import exhibition3 from '@/imgs/展览3.mp4'
import { useRouter } from 'vue-router'

// 轮播图数据（4张图）
const bannerList = ref([
  { img: banner1 },
  { img: banner2 },
  { img: banner3 },
  { img: banner4 }
])
const currentBanner = ref(0)
let bannerTimer = null

// 展览视频列表
const exhibitionList = ref([
  { src: exhibition1 },
  { src: exhibition2 },
  { src: exhibition3 }
])

// 自动轮播逻辑（5秒切换）
onMounted(() => {
  bannerTimer = setInterval(() => {
    currentBanner.value = (currentBanner.value + 1) % bannerList.value.length
  }, 5000)
})
onUnmounted(() => {
  clearInterval(bannerTimer)
})

// 点击跳转（立即预约、红色助农、出行指南、顶部logo跳路由，其它跳外链）
const goToUrl = (url) => {
  if (url === '/mine/book' || url === '/travel/shop' || url === '/beautiful' || url === '/mine') {
    router.push(url)
  } else {
    window.open(url, '_blank')
  }
}
const router = useRouter()
</script>

<style scoped>
/* 适配 393 × 895 尺寸，全局基础设置 */
.home-page {
  width: 100%;
  max-width: 393px; /* 限制最大宽度为目标尺寸 */
  margin: 0 auto; /* 居中，适配不同屏幕 */
  min-height: 100vh;
  background-color: #fdf6ee;
  padding-bottom: 80px; /* 避开底部导航 */
  box-sizing: border-box;
}

/* 顶部导航 */
.top-bar {
  position: relative;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  padding: 10px 15px;
  background: #fff;
}
.left-placeholder {
  margin-right: 10px;
  display: flex;
  align-items: center;
}
.topbar-img {
  width: 54px;
  height: 54px;
  display: block;
}
.title {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  font-size: 18px;
  font-weight: bold;
  color: #c72c2c;
  font-family: "宋体", serif;
}
.right-icons {
  position: absolute;
  right: 15px;
  display: flex;
  gap: 10px;
  font-size: 16px;
}

/* 轮播容器 */
.banner-container {
  position: relative;
  width: 100%;
  height: 180px; /* 适配 393 宽度的高度 */
  overflow: hidden;
}
.banner-swiper {
  display: flex;
  width: 100%;
  height: 100%;
  transition: transform 0.5s ease;
}
.banner-item {
  flex-shrink: 0;
  width: 100%;
  height: 100%;
}
.banner-img {
  width: 100%;
  height: 100%;
  background-color: #000; /* 黑色占位 */
}
.banner-indicators {
  position: absolute;
  bottom: 5px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 5px;
}
.indicator {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: rgba(255,255,255,0.5);
}
.indicator.active {
  background: #fff;
}

/* 今日开放卡片 */
.open-card {
  display: flex;
  justify-content: space-between;
  background: #fff;
  margin: 0;
  padding: 15px;
  border-radius: 0;
  box-shadow: none;
}
.left { flex: 1; }
.card-title { font-size: 13px; font-weight: bold; margin-bottom: 5px; }
.ticket { font-size: 10px; color: #c72c2c; margin-bottom: 5px; }
.info { font-size: 10px; color: #666; }
.right {
  width: 80px;
  height: 70px;
  border-radius: 6px;
}

/* 组合区域 */
.combined-section {
  margin: 10px 15px;
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.05);
  display: flex;
  flex-direction: column;
  height: 265px;
  overflow: hidden;
}
.actions-wrapper {
  flex: 4;
  display: flex;
}
.left-btn {
  flex: 1;
  background: #fff;
  text-align: center;
  padding: 5px;
  border-radius: 12px;
  font-size: 13px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.left-btn-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  border-radius: 12px;
}
.right-actions {
  flex: 2;
  display: flex;
  flex-direction: column;
}
.right-actions .btn-item {
  flex: 1;
  background: #fff;
  text-align: center;
  padding: 5px;
  border-radius: 12px;
  font-size: 13px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}
.right-first-btn {
  padding: 0;
}
.right-btn-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}
.btn-item .icon {
  display: block;
  font-size: 15px;
  margin-bottom: 5px;
}

/* 功能区新样式 */
.func-row {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  gap: 12px;
  padding: 15px;
  background: #fff;
  margin: 10px 15px;
  border-radius: 10px;
}
.func-block {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-end;
  background: #f5f5f5;
  border-radius: 12px;
  overflow: hidden;
  cursor: pointer;
  min-width: 0;
  box-shadow: 0 2px 8px rgba(0,0,0,0.04);
}
.func-block-img {
  width: 100%;
  aspect-ratio: 1/1;
  object-fit: cover;
  display: block;
}
.func-block-name {
  font-size: 13px;
  color: #333;
  background: rgba(255,255,255,0.85);
  width: 100%;
  text-align: center;
  padding: 6px 0 5px 0;
  font-weight: 500;
  letter-spacing: 1px;
  border-bottom-left-radius: 12px;
  border-bottom-right-radius: 12px;
  box-sizing: border-box;
}

/* 展览模块 */
.exhibit-section {
  padding: 0 15px;
}
.section-title {
  font-size: 17px;
  font-weight: bold;
  margin-bottom: 10px;
}
.exhibit-video-list {
  display: flex;
  flex-direction: row;
  gap: 16px;
  overflow-x: auto;
  padding-bottom: 10px;
}
.exhibit-video-item {
  min-width: 260px;
  flex: 0 0 70vw;
  max-width: 350px;
  border-radius: 12px;
  overflow: hidden;
  background: #000;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
  display: flex;
  align-items: center;
  justify-content: center;
}
.exhibit-video {
  width: 100%;
  height: 38vw;
  max-height: 220px;
  object-fit: cover;
  background: #000;
  display: block;
}
</style>