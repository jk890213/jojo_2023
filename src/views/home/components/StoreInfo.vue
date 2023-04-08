<script setup lang="ts">
import { reactive, watch } from 'vue';

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
const data = reactive({
    title: { text: '店舖資訊', show: false }, info: {
        searchStore: '',
        selectOptions: [
            { label: '台北市', value: '' },
            { label: '新北市', value: '' },
        ],
        items: [
            {
                name: '',
                address: '',
                phoneNumber: '',
                businessHours: '',
            },
        ]
    }
})

watch(() => props.scrolledDistance, () => {
    if (props.scrolledDistance > props.showTitleDistance) data.title.show = true
})
</script>

<template>
    <section>
        <div class="title">
            <h3 :class="{ show: data.title.show }">{{ data.title.text }}</h3>
        </div>
        <div class="container">
            <div class="areaFilter">
                <h5>台灣全店鋪</h5>
                <select name="" id="">
                    <option v-for="{ label, value } in data.info.selectOptions" :key="value" :value="value">{{ label }}
                    </option>
                </select>
            </div>
        </div>
    </section>
</template>

<style lang="scss" scoped>
.container {
    max-width: 1200px;
    margin: 120px auto;

    .areaFilter {
        h5 {
            display: inline-block;
            padding: 12px 24px;
            margin-right: 8px;
            color: #fff;
            letter-spacing: 4px;
            background: $minorColor;
        }

        select {
            min-width: 200px;

            option {}
        }
    }
}
</style>