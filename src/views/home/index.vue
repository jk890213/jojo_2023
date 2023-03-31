<!-- eslint-disable vue/multi-word-component-names -->
<script setup lang="ts">
import VideoBanner from './components/VideoBanner.vue'
import Feature from './components/Feature.vue'
import { computed, onBeforeUnmount, onMounted, reactive } from 'vue'

const description = reactive([
  {
    imgUrl: 'https://picsum.photos/800/400?1',
    text: '1993年在日本誕生首家Mo-Mo-Paradise壽喜燒・鍋物專賣店，2003年引進台灣，以『創造顧客最美好用餐體驗』為信念價値，採無限量供應方式，提供專業的日式服務與湯頭。在全球8個城市擁有超過40間分店。',
    title: 'MOMO創造美好體驗。',
    isShow: false
  },
  {
    imgUrl: 'https://picsum.photos/800/400?2',
    text: '日本東京都原裝進口，精選特選醬油及黑糖蜜為基底的獨家道地壽喜燒，香氣濃郁、芳醇甘甜，搭配各式食材呈現富有層次的滋味。',
    title: '原汁原味壽喜燒。',
    isShow: false
  },
  {
    imgUrl: 'https://picsum.photos/800/400?3',
    text: '完美搭配鍋底及沾醬，並嚴選蛋黃結實飽滿的鮮力蛋，撒上些許日本特製七味粉，增添壽喜燒肉片温潤滑嫩的口感，忠實呈現日本道地鍋物滋味。',
    title: '沾醬不馬虎。',
    isShow: false
  },
  {
    imgUrl: 'https://picsum.photos/800/400?4',
    text: '從肉品切片到上桌的溫度管理，到新鮮時蔬的挑選及運送，每個細節層層把關，提供種類豐富、安心美味的食材，讓您吃的營養均衡又健康。',
    title: '嚴選新鮮食材。',
    isShow: false
  }
])

const foodItems = [
  {
    imgUrl: 'https://picsum.photos/800/400?4',
    price: '449元 / 每位',
    title: '單湯底'
  }
]

const viewport = reactive({
  height: window.innerHeight ?? document.documentElement.clientHeight,
  width: window.innerWidth ?? document.documentElement.clientWidth
})

const changeViewport = () => {
  viewport.width = window.innerWidth ?? document.documentElement.clientWidth
  viewport.height = window.innerHeight ?? document.documentElement.clientHeight
}

const navHeight = 56
const featureSectionHeight = 350
const bannerHeight = computed(() => viewport.height - navHeight)

const scrolledStatus = reactive({
  isScrolledPastViewport: false,
  isFirstTitle: false
})

const checkScrolledStatus = () => {
  const scrollDistance = window.scrollY ?? document.documentElement.scrollTop
  scrolledStatus.isScrolledPastViewport = scrollDistance > bannerHeight.value

  if (scrollDistance > 1) description[0].isShow = true
  if (scrollDistance > featureSectionHeight) description[1].isShow = true
  if (scrollDistance > featureSectionHeight * 2) description[2].isShow = true
  if (scrollDistance > featureSectionHeight * 3) description[3].isShow = true

  if (scrollDistance > featureSectionHeight * 4) scrolledStatus.isFirstTitle = true
}

onMounted(() => {
  window.addEventListener('scroll', checkScrolledStatus)
  window.addEventListener('resize', changeViewport)
})

onBeforeUnmount(() => {
  window.addEventListener('scroll', checkScrolledStatus)
  window.addEventListener('resize', changeViewport)
})
</script>

<template>
  <VideoBanner :isScrolledPastViewport="scrolledStatus.isScrolledPastViewport" />
  <Feature v-if="viewport.width > 996" :description="description" />

  <main>
    <section>
      <div class="title">
        <h3 :class="{ show: scrolledStatus.isFirstTitle }">平日午餐</h3>
      </div>
      <div class="container">
        <div class="circle_wrapper">
          <img src="https://picsum.photos/300/300" alt="鍋物品項圖片" />
        </div>
        <div class="info">
          <p>平日16:00前進場</p>
          <p>單湯底</p>
          <p>449元 / 每位</p>
          <RouterLink :to="'/'">More</RouterLink>
        </div>
      </div>

      <!-- <div class="container">
        <div>
          <p></p>
          <ul>
            <li></li>
          </ul>
        </div>
        <div class="circle_wrapper">
          <img src="https://picsum.photos/300/300" alt="熱銷品項圖片" />
        </div>
        <div class="content">
          <h4>幸福龍眼蜜</h4>
          <p>享用甜蜜，擠出創意</p>
          <p>
            採專利的特色擠壓瓶，讓您享受甜蜜的同時，不沾染您的纖手，立即享用簡單方便的甜蜜。每個細節都為您想好，最用心、最健康的蜂蜜單品，獻給您最珍視的人。
          </p>
          <button>立即下單</button>
        </div>
      </div> -->
    </section>
    <section>
      <div class="title">
        <h3>精選商品</h3>
      </div>
    </section>
  </main>
</template>

<style lang="scss" scoped>
main section {
  &:nth-child(1) .title {
    background-image: url('https://picsum.photos/1200/1200?1');
  }
  &:nth-child(2) .title {
    background-image: url('https://picsum.photos/1200/1200?2');
  }
  &:nth-child(3) .title {
    background-image: url('https://picsum.photos/1200/1200?3');
  }

  .title {
    height: 480px;
    background-position: center center;
    background-repeat: no-repeat;
    background-size: cover;
    background-attachment: fixed;
    position: relative;
    overflow: hidden;

    &::after {
      content: '';
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      background: rgba(0, 0, 0, 0.3);
    }

    h3.show {
      top: 50%;
      opacity: 1;
    }
    h3 {
      position: absolute;
      top: -1px;
      left: 50%;
      opacity: 0;
      transform: translate(-50%, -50%);
      font-size: 60px;
      letter-spacing: 40px;
      color: #fff;
      z-index: 1;

      transition: all 1.3s ease-in-out;
    }
  }

  .container {
    height: 600px;

    .circle_wrapper {
      width: 300px;
      height: 300px;
      border-radius: 50%;
      overflow: hidden;

      img {
        object-fit: cover;
      }
    }
  }
}

// .hot_item .container {
//   display: flex;
//   justify-content: center;
//   padding: 28px 0;
//   .circle_wrapper {
//     margin-right: 40px;
//     width: 300px;
//     height: 300px;
//     border-radius: 50%;
//     overflow: hidden;

//     img {
//       object-fit: cover;
//     }
//   }

//   .content {
//     width: 20%;
//     align-self: center;

//     h4 {
//       margin-bottom: 24px;
//       font-size: 37px;
//       font-weight: bold;
//       color: #ca4e52;
//     }

//     p {
//       margin-bottom: 24px;

//       &:nth-child(2) {
//         font-size: 20px;
//         margin-bottom: 24px;
//       }
//       &:nth-child(3) {
//         font-size: 13px;
//         font-weight: normal;
//         letter-spacing: 1px;
//         line-height: 1.3;
//       }
//     }

//     button {
//       border: none;
//       outline: none;
//       padding: 12px;
//       border-radius: 4px;
//       background: #000;
//       color: #fff;
//       font-size: 16px;
//       cursor: pointer;

//       &:hover {
//         animation: bounce 0.5s linear;
//       }
//     }
//   }
// }

// @keyframes bounce {
//   0% {
//     transform: scale(1);
//   }
//   15% {
//     transform: scale(1.2);
//   }
//   35% {
//     transform: scale(0.8);
//   }
//   65% {
//     transform: scale(1.1);
//   }
//   90% {
//     transform: scale(0.9);
//   }
//   100% {
//     transform: scale(1);
//   }
// }
</style>
