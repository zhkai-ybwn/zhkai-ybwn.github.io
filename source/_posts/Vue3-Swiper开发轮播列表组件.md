---
title: Vue3 + Swiper开发轮播列表组件
date: 2023-03-20 09:42:34
tags: 
	- Vue
	- Swiper
categories: 
	[前端,Vue]
---

## Vue3 + Swiper开发轮播列表组件

前端开发中，轮播列表的场景并不少见，通常使用`Vue+Swiper`来实现，上手比较快。

### 安装依赖

直接执行npm命令

``` shell
    npm i swiper
```

### 组件编写

#### Html模板代码

``` Html
    <template>
        <div class="swiper-list-container">
            <!-- 列标题 -->
            <div class="list-title">
                <div class="title" v-for="(item, index) in column" :key="index">
                    <div class="title-name" :style="{width: `${item.width}px`}">
                        {{item.title}}
                    </div>
                </div>
            </div>
            <!-- 内容 -->
            <div class="swiper-container list-content">
                <div class="swiper-wrapper">
                    <div v-for="(item, index) in data" :key="index" class="swiper-slide list-line">
                        <div v-for="val in column" :key="val.key" :style="{ width: `${val.width}px` }" class="item">
                            {{item[val.key]}}
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </template>
```

#### JS逻辑代码

```JavaScript
    <script>
    import 'swiper/dist/css/swiper.min.css';
    import 'swiper/dist/js/swiper.min';
    import Swiper from 'swiper';
    import { onMounted, reactive } from 'vue';

    export default ({
        props: {
            // 列标题
            column: {
                type: Array,
                default: () => [
                    { title: '姓名', key: 'name', width: 100 },
                    { title: '年龄', key: 'age', width: 100 },
                    { title: '性别', key: 'sex', width: 100 },
                ],
            },
            // 数据
            data: {
                type: Array,
                default: () => [
                    { name: 'Nicholas', age: '18', sex: '男' },
                    { name: 'Hetty', age: '18', sex: '男' },
                    { name: 'Graham', age: '18', sex: '女' },
                    { name: 'Harley', age: '18', sex: '男' },
                    { name: 'Finbar', age: '18', sex: '女' },
                    { name: 'Oliver', age: '18', sex: '男' },
                ],
            },
        },
        setup(props) {
            const data = reactive({
                swipers: null,
            });
            const createSwiper = () => {
                data.swipers = new Swiper('.swiper-container', {
                    height: 150,
                    speed: 2000, // 匀速时间
                    autoplay: {
                        delay: 0,
                        stopOnLastSlide: false,
                        disableOnInteraction: false,
                    },
                    freeMode: true,
                    loop: props.data.length > 5,
                    direction: 'vertical',
                    slidesPerView: 'auto',
                });
            };
            const init = () => {
                if (!data.swipers) {
                    createSwiper();
                } else if ((data.swipers.passedParams.loop === true && props.data.length <= 5)
                    || (data.swipers.passedParams.loop === false && data.listData.length > 5)) {
                    data.swipers.destroy();
                    data.swipers = null;
                    setTimeout(() => {
                        createSwiper();
                    }, 0);
                } else {
                    data.swipers.update();
                }
            };
            onMounted(() => {
                init();
            });
        },
    });
    </script>
```

#### SCSS样式代码

```CSS
    <style lang="scss" scoped>
    .swiper-list-container {
    width: 300px;
    height: 185;

    .list-title {
        display: flex;
        justify-content: flex-start;
        width: 100%;
        height: 30px;
        color: #d59701;
        background-color: #1a6975;

        .title-name {
        width: 120px;
        overflow: hidden;
        line-height: 30px;
        text-align: center;
        text-overflow: ellipsis;
        white-space: nowrap;
        }
    }

    .list-content {
        width: 100%;
        height: 150px;
        overflow: hidden;
        background-color: #e9caa6;

        .list-line {
        display: flex;
        flex-direction: row;
        justify-content: flex-start;
        height: 30px;
        line-height: 30px;
        color: #d59701;

        .item {
            overflow: hidden;
            text-align: center;
            text-overflow: ellipsis;
            white-space: nowrap;
        }
        }
    }
    }
    </style>
```

#### 重点讲解

- class `swiper-container/swiper-wrappe/swiper-slide`依次排序，由此将Swiper的样式引入到Vue组件中
- 初始化Swiper，即createSwiper方法中，new Swiper对象时应该和Html中的class名称相一致，此外如果存在多个场景使用Swiper时，应该将这个class name命名为不同的名称
- 根据接口或者数据更新，要区分创建和更新Swiper两种场景

#### Swiper参数介绍

此处仅介绍上述组件中使用到的参数，其他参数请参照官方文档
<https://www.swiper.com.cn/api/index.html>

- height：强制Swiper的高度(px)，当你的Swiper在隐藏状态下初始化时且切换方向为垂直才用得上。这个参数会使自适应失效
- speed：切换速度，即slider自动滑动开始到结束的时间（单位ms），也是触摸滑动时释放至贴合的时间
- autoplay：设置为true启动自动切换，并使用默认的切换设置
- freeMode：启用自由模式功能，默认情况下Swiper 每次滑动时只滑动一个Slide，并且会自动贴合Wrapper。开启自由模式后，Swiper 会根据惯性滑动可能不止一格且不会贴合
- loop：设置为 true 则开启循环(loop)模式。loop模式：会在原本slide 前后复制若干个slide (默认一个)并在合适的时候切换，让Swiper看起来像是循环的。复制的slide 上有一些额外的类名代表他是生成的
- direction：Swiper的滑动方向，可设置为水平方向切换 horizontal 或垂直方向切换 vertical
- slidesPerView：设置slider容器能够同时显示的slides数量(carousel模式)。可以设置为数字（可为小数，小数不可loop），或者 'auto'则自动根据slides的宽度来设定数量
