<template>
  <header>
    <video :src="videoBanner" loop autoplay muted />
    <nav aria-label="pc_nav" :class="{ top: isScrolledPastViewport }">
      <div class="logo">JOJO</div>
      <ul>
        <li v-for="{ routeName, title } in nav" :key="routeName">
          <RouterLink to="/">{{ title }}</RouterLink>
        </li>
      </ul>
    </nav>
    <!-- <nav aria-label="mobile_nav">
      <RouterLink to="/">Home</RouterLink>
    </nav> -->
    <p>
      <span>仿製</span>
      <span>MOMO 壽喜燒</span>
      <span>品牌形象官網</span>
    </p>
  </header>
</template>

<script setup lang="ts">
import videoBanner from '@/assets/food_banner.mp4'
import { computed, ref, watch } from 'vue';

const props = defineProps({
  viewportHeight: {
    type: Number,
    required: true
  },
  scrolledDistance: {
    type: Number,
    required: true
  },
})

const isScrolledPastViewport = ref(false)
const navHeight = 56
const bannerHeight = computed(() => props.viewportHeight - navHeight)
const nav = [
  { routeName: '', title: '關於我們' },
  { routeName: '', title: '最新消息' },
  { routeName: '', title: '商品項目' },
  { routeName: '', title: '聯絡我們' },
  { routeName: '', title: '會員中心' }
]


watch(() => props.scrolledDistance, () => {
  isScrolledPastViewport.value = props.scrolledDistance > bannerHeight.value
})
</script>

<style lang="scss" scoped>
header {
  height: 100vh;
  position: relative;
  overflow: hidden;

  video {
    height: 100%;
    width: 100%;
    position: absolute;
    top: 0;
    left: 0;
    object-fit: cover;
  }

  nav[aria-label='pc_nav'] {
    display: flex;
    justify-content: space-around;
    width: 100%;
    height: 56px;
    padding: 8px;
    position: absolute;
    bottom: 0;
    z-index: 10;
    background-color: rgba(193, 39, 45, 0.9);

    .logo {
      line-height: 40px;
      font-size: 40px;
    }

    ul {
      display: flex;
      width: 80%;

      li {
        width: 20%;
        padding: 0 8px;
        line-height: 40px;

        a {
          color: #fff;
          text-decoration-line: none;
        }
      }
    }
  }

  nav[aria-label='pc_nav'].top {
    position: fixed;
    top: 0;
  }

  p {
    width: 30%;
    position: absolute;
    top: 50%;
    left: 50%;
    color: #fff;
    display: flex;
    flex-direction: column;
    align-items: center;
    font-size: 72px;
    line-height: 1.5;
    text-align: center;
    transform: translate(-50%, -50%);
  }
}
</style>
