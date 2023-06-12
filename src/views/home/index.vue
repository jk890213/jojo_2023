<!-- eslint-disable vue/multi-word-component-names -->
<script setup lang="ts">
import VideoBanner from './components/VideoBanner.vue'
import FeatureSection from './components/FeatureSection.vue'
import FoodSection from './components/FoodSection.vue'
import LatestNews from './components/LatestNews.vue'
// import StoreInfo from './components/StoreInfo.vue'
import { computed, onBeforeUnmount, onMounted, reactive, ref } from 'vue'

const viewport = reactive({
  height: window.innerHeight ?? document.documentElement.clientHeight,
  width: window.innerWidth ?? document.documentElement.clientWidth
})

const changeViewport = () => {
  viewport.width = window.innerWidth ?? document.documentElement.clientWidth
  viewport.height = window.innerHeight ?? document.documentElement.clientHeight
}

const currentScrollDistance = ref(0)

const checkScrolledStatus = () => {
  currentScrollDistance.value = window.scrollY ?? document.documentElement.scrollTop
}

onMounted(() => {
  window.addEventListener('scroll', checkScrolledStatus)
  window.addEventListener('resize', changeViewport)
})

onBeforeUnmount(() => {
  window.addEventListener('scroll', checkScrolledStatus)
  window.addEventListener('resize', changeViewport)
})

const featureSingleSectionHeigh = computed(() => (viewport.width <= 996 ? 0 : 350))
const sectionTitleHeigh = computed(() => (viewport.width <= 996 ? 350 : 500))
const foodContentHeigh = computed(() => (viewport.width <= 996 ? 1200 : 600))

const foodScetionScrollDistanceStatus = {
  begin: featureSingleSectionHeigh.value * 4
}

const newsScetionScrollDistance = computed(
  () =>
    foodScetionScrollDistanceStatus.begin + (sectionTitleHeigh.value + foodContentHeigh.value) * 2
)
// const storeInfoScetionScrollDistance = computed(() => newsScetionScrollDistance.value + 890)
</script>

<template>
  <VideoBanner :scrolledDistance="currentScrollDistance" :viewportHeight="viewport.height" />
  <FeatureSection
    :singleSectionDistance="featureSingleSectionHeigh"
    :scrolledDistance="currentScrollDistance"
    v-if="viewport.width > 996"
  />
  <main>
    <FoodSection
      :scrolledDistance="currentScrollDistance"
      :beginDistance="foodScetionScrollDistanceStatus.begin"
      :titleSectionDistance="sectionTitleHeigh"
      :contentSectionDistance="foodContentHeigh"
    />
    <LatestNews
      :scrolledDistance="currentScrollDistance"
      :showTitleDistance="newsScetionScrollDistance"
    />
    <!-- <StoreInfo
      :scrolledDistance="currentScrollDistance"
      :showTitleDistance="storeInfoScetionScrollDistance"
    /> -->
  </main>
</template>

<style lang="scss">
main section {
  padding-top: 20px;
  background: linear-gradient(to bottom, $minorColor, $minorColor 20px, #fff 20px, #fff 100%);

  @include desktops {
    padding-top: 10px;
    background: linear-gradient(to bottom, $minorColor, $minorColor 10px, #fff 10px, #fff 100%);
  }

  &:nth-child(1) .title {
    background-image: url('https://picsum.photos/1200/1200?1');
  }

  &:nth-child(2) .title {
    background-image: url('https://picsum.photos/1200/1200?2');
  }

  &:nth-child(3) .title {
    background-image: url('https://picsum.photos/1200/1200?3');
  }

  &:nth-child(4) .title {
    background-image: url('https://picsum.photos/1200/1200?4');
  }

  // &:nth-child(5) .title {
  //   background-image: url('https://picsum.photos/1200/1200?5');
  // }

  .title {
    height: 480px;
    background-position: center center;
    background-repeat: no-repeat;
    background-size: cover;
    background-attachment: fixed;
    position: relative;
    overflow: hidden;

    @include desktops {
      height: 340px;
    }

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
      text-align: center;
      letter-spacing: 40px;
      color: #fff;
      z-index: 1;

      transition: top 1.3s ease-in-out;

      @include smallScreen {
        width: 70%;
      }

      @include desktops {
        font-size: 36px;
      }

      @include phone {
        width: 100%;
        font-size: 26px;
      }
    }
  }
}
</style>
