<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <section>
    <div
      class="item"
      v-for="{ imgUrl, text, title, isShow } in description"
      :class="{
        show: isShow
      }"
      :key="imgUrl"
    >
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
import type { PropType } from 'vue'

type Feature = {
  imgUrl: string
  text: string
  title: string
  isShow: boolean
}

defineProps({
  description: {
    type: Array as PropType<Feature[]>,
    required: true
  }
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
