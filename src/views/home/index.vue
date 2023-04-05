<!-- eslint-disable vue/multi-word-component-names -->
<script setup lang="ts">
import VideoBanner from './components/VideoBanner.vue'
import FeatureSection from './components/FeatureSection.vue'
import FoodSection from './components/FoodSection.vue'
import LatestNews from './components/LatestNews.vue'

import { onBeforeUnmount, onMounted, reactive, ref } from 'vue'

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

const featureSectionScrollDistance = 350
const mainScetionScrollDistanceStatus = {
  begin: featureSectionScrollDistance * 4,
  title: 500,
  content: 600
}
</script>

<template>
  <VideoBanner :scrolledDistance="currentScrollDistance" :viewportHeight="viewport.height" />
  <FeatureSection :singleSectionDistance="featureSectionScrollDistance" :scrolledDistance="currentScrollDistance"
    v-if="viewport.width > 996" />
  <main>
    <FoodSection :scrolledDistance="currentScrollDistance" :beginDistance="mainScetionScrollDistanceStatus.begin"
      :titleSectionDistance="mainScetionScrollDistanceStatus.title"
      :contentSectionDistance="mainScetionScrollDistanceStatus.content" />
  </main>
  <LatestNews :scrolledDistance="currentScrollDistance"
    :showTitleDistance="mainScetionScrollDistanceStatus.begin + (mainScetionScrollDistanceStatus.title + mainScetionScrollDistanceStatus.content) * 2" />
</template>

<style lang="scss" scoped></style>
