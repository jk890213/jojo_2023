<script setup lang="ts">
import { reactive, watch } from 'vue'
const props = defineProps({
  scrolledDistance: {
    type: Number,
    required: true
  },
  beginDistance: {
    type: Number,
    required: true
  },
  titleSectionDistance: {
    type: Number,
    required: true
  },
  contentSectionDistance: {
    type: Number,
    required: true
  }
})
const foodItems = reactive([
  {
    title: '平日午餐',
    show: false,
    items: [
      {
        imgUrl: 'https://picsum.photos/380/380?1',
        price: ['449', '元', '/ 每位'],
        type: '單湯底',
        period: '平日16:00前進場',
        show: false
      },
      {
        imgUrl: 'https://picsum.photos/380/380?2',
        price: ['499', '元', '/ 每位'],
        type: '雙湯底',
        period: '平日16:00前進場',
        show: false
      }
    ]
  },
  {
    title: '晚餐例假日',
    show: false,
    items: [
      {
        imgUrl: 'https://picsum.photos/380/380?3',
        price: ['599', '元', '/ 每位'],
        type: '單湯底',
        period: '平日16:00後進場/例假日全天',
        show: false
      },
      {
        imgUrl: 'https://picsum.photos/380/380?4',
        price: ['649', '元', '/ 每位'],
        type: '雙湯底',
        period: '平日16:00後進場/例假日全天',
        show: false
      }
    ]
  }
])

watch(
  () => props.scrolledDistance,
  () => {
    const wholeSection = props.titleSectionDistance + props.contentSectionDistance

    if (props.scrolledDistance > props.beginDistance) foodItems[0].show = true
    if (props.scrolledDistance > props.beginDistance + wholeSection) foodItems[1].show = true
    if (props.scrolledDistance > props.beginDistance + props.titleSectionDistance) {
      foodItems[0].items.forEach((item) => {
        item.show = true
      })
    }
    if (props.scrolledDistance > props.beginDistance + wholeSection + props.titleSectionDistance) {
      foodItems[1].items.forEach((item) => {
        item.show = true
      })
    }
  }
)
</script>

<template>
  <section v-for="{ items, title, show } in foodItems" :key="title">
    <div class="title">
      <h3 :class="{ show }">{{ title }}</h3>
    </div>
    <div class="container">
      <div class="item" v-for="{ type, imgUrl, period, price, show } in items" :key="type">
        <div class="circle_wrapper" :class="{ show }">
          <img :src="imgUrl" alt="鍋物品項圖片" />
        </div>
        <div class="info">
          <div>
            <span>{{ period }}</span>
          </div>
          <div>
            <span>{{ type }}</span>
            <div>
              <span v-for="text in price" :key="text">
                {{ text }}
              </span>
            </div>
            <div>
              <RouterLink :to="'/'">More</RouterLink>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style lang="scss" scoped>
$foodImgSize: 380px;

section .container {
  height: 600px;
  display: flex;
  justify-content: center;

  @include desktops {
    height: 1200px;
    flex-direction: column;
    align-items: center;
  }

  .item {
    padding: 0 88px;
    transform: translateY(-100px);

    @include smallScreen {
      padding: 0 40px;
    }

    @include desktops {
      padding: 0;
    }

    .circle_wrapper {
      width: $foodImgSize;
      height: $foodImgSize;
      border-radius: 50%;
      overflow: hidden;
      transform: translateY(160px);
      opacity: 0;

      img {
        object-fit: cover;
      }
    }

    &:first-child {
      .circle_wrapper.show {
        animation: fadeIn 1.3s ease-out;
        animation-fill-mode: forwards;
      }
    }

    &:last-child {
      .circle_wrapper.show {
        animation: fadeIn 1.3s ease-out 0.4s;
        animation-fill-mode: forwards;
      }
    }

    .info {
      margin: 0 auto;
      width: $foodImgSize;
      border: 1px solid $minorColor;
      position: relative;
      z-index: 1;

      @include desktops {
        width: 278px;
        text-align: center;
      }

      > div {
        padding: 20px;

        &:first-child {
          background: $minorColor;

          span {
            padding-bottom: 4px;
            border-bottom: 1px solid #fff;
            color: #fff;
            font-size: 24px;

            @include desktops {
              font-size: 14px;
            }
          }
        }

        &:nth-child(2) {
          background: #fff;

          > span {
            color: $majorColor;
            font-size: 24px;

            @include desktops {
              font-size: 18px;
            }
          }

          > div {
            padding-top: 20px;

            > span {
              margin-right: 4px;

              &:first-child {
                font-size: 72px;

                @include desktops {
                  font-size: 40px;
                }
              }

              &:nth-child(2) {
                font-size: 24px;

                @include desktops {
                  font-size: 12px;
                }
              }
            }

            &:last-child {
              text-align: end;

              a {
                color: #a9272d;

                @include desktops {
                  font-size: 12px;
                }
              }
            }
          }
        }
      }
    }
  }
}

@keyframes fadeIn {
  0% {
    opacity: 0;
    transform: translateY(160px);
  }

  20% {
    opacity: 1;
  }

  60% {
    transform: translateY(20px);
    opacity: 1;
  }

  100% {
    transform: translateY(40px);
    opacity: 1;
  }
}
</style>
