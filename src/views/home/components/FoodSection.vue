<script setup lang="ts">
import { reactive, watch } from 'vue';
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
    },
})
const foodItems = reactive([
    {
        title: '平日午餐', show: false, items: [
            {
                imgUrl: "https://picsum.photos/380/380?1",
                price: ['449', '元', '/ 每位'],
                type: '單湯底',
                period: '平日16:00前進場',
                show: false
            }, {
                imgUrl: "https://picsum.photos/380/380?2",
                price: ['499', '元', '/ 每位'],
                type: '雙湯底',
                period: '平日16:00前進場',
                show: false
            },
        ]
    }, {
        title: '晚餐例假日', show: false, items: [
            {
                imgUrl: "https://picsum.photos/380/380?3",
                price: ['599', '元', '/ 每位'],
                type: '單湯底',
                period: '平日16:00後進場/例假日全天',
                show: false
            }, {
                imgUrl: "https://picsum.photos/380/380?4",
                price: ['649', '元', '/ 每位'],
                type: '雙湯底',
                period: '平日16:00後進場/例假日全天',
                show: false
            },
        ]
    },
])


watch(() => props.scrolledDistance, () => {
    const wholeSection = props.titleSectionDistance + props.contentSectionDistance

    if (props.scrolledDistance > props.beginDistance) foodItems[0].show = true
    if (props.scrolledDistance > props.beginDistance + wholeSection) foodItems[1].show = true
    if (props.scrolledDistance > props.beginDistance + props.titleSectionDistance) {
        foodItems[0].items.forEach((item) => {
            item.show = true
        });
    }
    if (props.scrolledDistance > props.beginDistance + wholeSection + props.titleSectionDistance) {
        foodItems[1].items.forEach((item) => {
            item.show = true
        });
    }
})
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

main section {
    padding-top: 20px;
    background: linear-gradient(to bottom, #c7b299, #c7b299 20px, #fff 20px, #fff 100%);

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
        display: flex;
        justify-content: center;

        .item {
            padding: 0 88px;
            transform: translateY(-100px);

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
                    animation-fill-mode: forwards
                }
            }

            &:last-child {
                .circle_wrapper.show {
                    animation: fadeIn 1.3s ease-out 0.3s;
                    animation-fill-mode: forwards
                }
            }

            .info {
                width: $foodImgSize;
                border: 1px solid #c7b299;
                position: relative;
                z-index: 1;

                >div {
                    padding: 20px;

                    &:first-child {
                        background: #c7b299;

                        span {
                            padding-bottom: 4px;
                            border-bottom: 1px solid #fff;
                            color: #fff;
                            font-size: 24px;
                        }
                    }

                    &:nth-child(2) {
                        background: #fff;

                        >span {
                            color: #c1272d;
                            font-size: 24px;
                        }

                        >div {
                            padding-top: 20px;

                            >span {
                                &:first-child {
                                    font-size: 72px;
                                }

                                &:nth-child(2) {
                                    font-size: 24px;
                                }
                            }

                            &:last-child {
                                text-align: end;

                                a {
                                    color: #a9272d;
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
