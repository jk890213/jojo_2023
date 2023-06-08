<script setup lang="ts">
import { reactive, watch } from 'vue'

const props = defineProps({
  scrolledDistance: {
    type: Number,
    required: true
  },
  showTitleDistance: {
    type: Number,
    required: true
  }
})

const news = reactive({
  title: { text: '最新消息', show: false },
  items: [
    {
      imgUrl: 'https://picsum.photos/300/200?1',
      title: '2023兒童我最大•好禮1+1',
      content: '活動期間，帶小朋友至【MO-MO-PARADISE】用餐，享受美食金折抵，再送「三澧兒童套票」',
      type: '公告',
      date: '2023/03/27'
    },
    {
      imgUrl: 'https://picsum.photos/300/200?2',
      title: '嘉義秀泰牧場 3/15營業時間調整',
      content: '3/15當日延後至16:00營業',
      type: '公告',
      date: '2023/03/02'
    },
    {
      imgUrl: 'https://picsum.photos/300/200?3',
      title: '重新家樂福牧場 3/2起內部整修暫停營業',
      content: '3/2起重新家樂福牧場進行內部裝潢整修',
      type: '公告',
      date: '2023/03/02'
    }
  ]
})

watch(
  () => props.scrolledDistance,
  () => {
    if (props.scrolledDistance > props.showTitleDistance) news.title.show = true
  }
)
</script>

<template>
  <section>
    <div class="title">
      <h3 :class="{ show: news.title.show }">{{ news.title.text }}</h3>
    </div>
    <div class="container">
      <ul>
        <li v-for="{ imgUrl, title, content, type, date } in news.items" :key="imgUrl">
          <div class="info">
            <div class="pic"><img :src="imgUrl" alt="消息說明圖片" /></div>
            <div class="text">
              <h4>{{ title }}</h4>
              <p>{{ content }}</p>
            </div>
          </div>
          <div class="type">
            <div>{{ type }}</div>
            <div>{{ date }}</div>
          </div>
        </li>
      </ul>
      <RouterLink to="/">查看更多...</RouterLink>
    </div>
  </section>
</template>

<style lang="scss" scoped>
.container {
  max-width: 1200px;
  margin: 120px auto;
  padding: 0 20px;

  li {
    padding: 24px 0;
    display: flex;
    justify-content: space-between;

    .info {
      display: flex;
      width: 100%;

      .pic {
        padding-right: 20px;

        @include desktops {
          width: 45%;
        }

        img {
          width: 100%;
        }
      }

      .text {
        width: 50%;

        @include desktops {
          width: 55%;
        }
        h4 {
          margin-bottom: 24px;
          font-size: 24px;
          color: $majorColor;
        }
      }
    }

    .type {
      padding-left: 20px;

      @include desktops {
        display: none;
      }

      > div {
        padding: 8px;
        border-top: 1px solid $minorColor;
        border-left: 1px solid $minorColor;
        border-right: 1px solid $minorColor;
        text-align: center;
        color: $minorColor;
        font-size: 12px;

        &:last-child {
          border-bottom: 1px solid $minorColor;
        }
      }
    }
  }

  li + li {
    border-top: 1px solid $minorColor;
  }

  a {
    font-size: 14px;
    color: $minorColor;
    text-decoration: none;
    border-bottom: 1px solid $minorColor;
  }
}
</style>
