<template>
  <section>
    <div class="item" v-for="{ imgUrl, text, title, isShow } in description" :class="{
      show: isShow
    }" :key="imgUrl">
      <div class="pic">
        <img :src="imgUrl" alt="特色圖片" />
      </div>
      <div class="content">
        <h3>{{ title }}</h3>
        <p>{{ text }}</p>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { reactive, watch } from 'vue'
const props = defineProps({
  scrolledDistance: {
    type: Number,
    required: true
  },
  singleSectionDistance: {
    type: Number,
    required: true
  },
})

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

watch(() => props.scrolledDistance, () => {
  const sectionDistance = props.singleSectionDistance
  if (props.scrolledDistance > 1) description[0].isShow = true
  if (props.scrolledDistance > sectionDistance) description[1].isShow = true
  if (props.scrolledDistance > sectionDistance * 2) description[2].isShow = true
  if (props.scrolledDistance > sectionDistance * 3) description[3].isShow = true
})
</script>

<style lang="scss" scoped>
section {
  .item.show {
    opacity: 1;
  }

  .item {
    display: flex;
    opacity: 0;
    transition: opacity 1.3s ease-in-out;

    &:nth-child(even) .pic {
      order: 1;
    }

    .pic {
      height: 350px;
      flex-grow: 1;

      img {
        width: 100%;
        height: 100%;
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
}
</style>
