<!-- eslint-disable vue/multi-word-component-names -->
<script setup lang="ts">
import { RouterLink } from 'vue-router'
import videoBanner from '@/assets/food_banner.mp4'
import { ref } from 'vue'

const nav = [
  { routeName: '', title: '關於我們' },
  { routeName: '', title: '最新消息' },
  { routeName: '', title: '商品項目' },
  { routeName: '', title: '聯絡我們' },
  { routeName: '', title: '會員中心' }
]

const description = [
  {
    imgUrl: 'https://picsum.photos/800/400?1',
    text: '1993年在日本誕生首家Mo-Mo-Paradise壽喜燒・鍋物專賣店，2003年引進台灣，以『創造顧客最美好用餐體驗』為信念價値，採無限量供應方式，提供專業的日式服務與湯頭。在全球8個城市擁有超過40間分店。',
    title: 'JOJO創造美好體驗。'
  },
  {
    imgUrl: 'https://picsum.photos/800/400?2',
    text: '日本東京都原裝進口，精選特選醬油及黑糖蜜為基底的獨家道地壽喜燒，香氣濃郁、芳醇甘甜，搭配各式食材呈現富有層次的滋味。',
    title: '原汁原味壽喜燒。'
  },
  {
    imgUrl: 'https://picsum.photos/800/400?3',
    text: '完美搭配鍋底及沾醬，並嚴選蛋黃結實飽滿的鮮力蛋，撒上些許日本特製七味粉，增添壽喜燒肉片温潤滑嫩的口感，忠實呈現日本道地鍋物滋味。',
    title: '沾醬不馬虎。'
  },
  {
    imgUrl: 'https://picsum.photos/800/400?4',
    text: '從肉品切片到上桌的溫度管理，到新鮮時蔬的挑選及運送，每個細節層層把關，提供種類豐富、安心美味的食材，讓您吃的營養均衡又健康。',
    title: '嚴選新鮮食材。'
  }
]

const isScrolledPastViewport = ref(false)
const checkScrollDistance = (): boolean => {
  const scrollDistance = window.scrollY || document.documentElement.scrollTop
  const viewportHeight = window.innerHeight - 56
  return scrollDistance > viewportHeight
}

window.addEventListener('scroll', () => {
  isScrolledPastViewport.value = checkScrollDistance()
})
</script>

<template>
  <header>
    <video preload="true" :src="videoBanner" loop autoplay muted />
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
  </header>
  <section>
    <div class="item" v-for="{ imgUrl, text, title } in description" :key="imgUrl">
      <div class="pic">
        <img :src="imgUrl" alt="特色圖片" />
      </div>
      <div class="content">
        <h3>{{ title }}</h3>
        <p>{{ text }}</p>
      </div>
    </div>
  </section>
  <main></main>
</template>

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
}

section .item {
  display: flex;
  &:nth-child(even) .pic {
    order: 1;
  }
  .pic {
    flex-grow: 1;
    img {
      width: 100%;
      vertical-align: middle;
      object-fit: cover;
    }
  }

  .content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 0 60px;
    width: 40%;

    h3 {
      padding-bottom: 4px;
      font-size: 28px;
      margin-bottom: 28px;
      position: relative;

      &::after {
        content: '';
        height: 1px;
        position: absolute;
        left: 0;
        right: 28px;
        bottom: -4px;
        background: #000;
      }
    }
    p {
      font-size: 13px;
      line-height: 1.7;
    }
  }
}
</style>
