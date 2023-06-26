---
title: Echarts创建中国3D地图
date: 2023-06-15 14:05:14
tags:
  - echarts
  - vue3
categories: [前端]
---

## Echarts创建中国3D地图

客户需要做一版3D中国地图，地图要倾斜角度，然后可以支持点击省份，对地图两侧的图表数据进行切换，此外还有一些纹理，顶牌信息面板的效果，不一一赘述，末尾我会放一张成品的图片。

### 地图数据

Echarts官网迁移后，地图的json数据已经不翼而飞了，所以在开发地图图表时，要先找一份地图的json数据，阿里的网站就有，直接去下载就好了。
链接：<http://datav.aliyun.com/portal/school/atlas/area_selector>
json下载完成后，要在Echarts对象初始化的时候注册一下，代码如下：

```JavaScript
import china from '../../../public/mock/map/china.json' // 导入地图json数据
import * as echarts from 'echarts'

echarts.registerMap('china', china)
```

### 组件代码

```JavaScript
<!-- 点线 -->
<template>
    <div class="container">
        <!-- <div class="bg-up"></div> -->
        <div class="bg"></div>
        <video
            id="v1"
            class="video-bg"
            autoplay
            loop
            muted
            width="7600"
            height="2160"
            style="object-fit: fill;width: 7600px;height: 2160px;"
        >
            <source :src="videos[videoUrl]" type="video/mp4" />
        </video>
        <div
            id="echarts"
            ref="echartRef"
            class="map"
            :style="{ width: config.width + 'px', height: config.height + 'px' }"
        ></div>
        <div class="nine-line"></div>
    </div>
</template>

<script>
import china from '../../../public/mock/map/china.json'
import * as echarts from 'echarts'
import tietu1 from '../../assets/imgs/home/123.jpg'
import video from '../../assets/imgs/map/map_bg.mp4'
import video2 from '../../assets/imgs/map/map_bg2.mp4'
import video3 from '../../assets/imgs/map/map_bg3.mp4'
import video4 from '../../assets/imgs/map/map_bg4.mp4'
import markerImg from '../../assets/imgs/map/3d_marker_bg.png'
import axiosAPI from '../../axios'
import { useModule } from '../../store/useModule'

export default {
    props: {
        config: {
            type: Object,
            default: () => ({ width: 7600, height: 2160 }),
        }, 
    },
    setup() {
        const moduleStore = useModule()
        let dataMaps = [
            { name: '北京',
                value: 457,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '天津',
                value: 120,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '河北',
                value: 421,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '山西',
                value: 413,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '内蒙古',
                value: 484,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '辽宁',
                value: 106,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '吉林',
                value: 18,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '黑龙江',
                value: 98,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '上海',
                value: 331,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '江苏',
                value: 164,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '浙江',
                value: 4,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '安徽',
                value: 26,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '福建',
                value: 492,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '江西',
                value: 48,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '山东',
                value: 132,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '河南',
                value: 309,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '湖北',
                value: 35,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '湖南',
                value: 11,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '重庆',
                value: 16,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '四川',
                value: 383,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '贵州',
                value: 209,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '云南',
                value: 365,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '西藏',
                value: 421,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '陕西',
                value: 144,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '甘肃',
                value: 472,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '青海',
                value: 182,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '宁夏',
                value: 282,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '新疆',
                value: 51,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '广东',
                value: 110,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '广西',
                value: 453,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '海南',
                value: 356,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '台湾',
                value: 342,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '香港',
                value: 406,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
            { name: '澳门',
                value: 223,
                itemStyle: {
                    color: 'rgba(167, 204, 255, 1)',
                    opacity: 1,
                    borderWidth: 2,
                    borderColor: 'rgba(167, 204, 255, 1)', // 地图边配色
                } },
        ]
        const state = reactive({ 
            videos: [video, video2, video3, video4],
            videoUrl: window.yunyingzhihuizhongxin.videoUrl,
            infoData: [],
        })
        const getData = async () => {
            const res = await axiosAPI.get(window.yunyingzhihuizhongxin.URLs.china3D)
            if (res.code === 200) {
                state.infoData = res.data.data
            }
        }
        const timer = setInterval(getData, window.yunyingzhihuizhongxin.looping.china3D * 1000)
        const addcomma = (num) => (num || 0).toString().replace(/(\d)(?=(?:\d{3})+$)/g, '$1,')
        const convertData = (data) => {
            const res = []
            for (let i = 0; i < data.length; i++) {
                res.push({
                    name: data[i].name,
                    value: data[i].lonlat,
                    value1: data[i].value1,
                    value2: data[i].value2,
                    label1: data[i].label1,
                    label2: data[i].label2,
                    label: {
                        show: true,
                        position: 'top',
                        distance: 0,
                        backgroundColor: {
                            image: markerImg,
                        },
                        width: 414,
                        height: 176,
                        formatter(param) {
                            return `{sty1|${param.data.label1}}
                                {sty2|${param.data.label2}}
                                {sty3|${addcomma(param.data.value1)}}
                                {sty4|${addcomma(param.data.value2)}}`
                        },
                        rich: {
                            sty1: {
                                fontSize: 32,
                                padding: [15, 0, 0, 20],
                                color: 'rgba(23, 31, 44, 1)',
                                fontFamily: 'SourceHanSansCN-Bold, sans-serif',
                                fontWeight: 500,
                                align: 'left',
                            },
                            sty2: {
                                color: 'rgba(23, 31, 44, 1)',
                                padding: [-39, 20, 0, 0],
                                fontSize: 32,
                                fontFamily: 'SourceHanSansCN-Bold, sans-serif',
                                fontWeight: 500,
                                align: 'right',
                            },
                            sty3: {
                                fontSize: 58,
                                padding: [15, 0, 0, -95],
                                fontFamily: 'DINPro-Medium, sans-serif',
                                fontWeight: 500,
                                color: '#FFFFFF',
                                align: 'left',
                            },
                            sty4: {
                                fontSize: 58,
                                padding: [-70, 20, 0, 0],
                                fontFamily: 'DINPro-Medium, sans-serif',
                                fontWeight: 500,
                                color: '#FFFFFF',
                                align: 'right',
                            },
                        },
                    },
                })
            }
            return res
        }
        let myChart
        const init = () => {
            if (!myChart) {
                myChart = echarts.init(document.getElementById('echarts'))
                echarts.registerMap('china', china)
                myChart.on('click', 'series.map3D', params => {
                    let newOption = myChart.getOption()
                    newOption.geo3D[0].regions.forEach(region => {
                        if (params.data.name === region.name) {
                            if (region.itemStyle.borderWidth === 6) {
                                region.itemStyle.color = 'rgba(167, 204, 255, 1)' 
                                region.itemStyle.borderColor = 'rgba(167, 204, 255, 1)' 
                                region.itemStyle.borderWidth = 2
                            } else {
                                region.itemStyle.color = '#76ddff'
                                region.itemStyle.borderColor = '#76ddff' 
                                region.itemStyle.borderWidth = 6 
                            }
                        } else {
                            region.itemStyle.color = 'rgba(167, 204, 255, 1)' 
                            region.itemStyle.borderColor = 'rgba(167, 204, 255, 1)' 
                            region.itemStyle.borderWidth = 2
                        }
                    })
                    moduleStore.currentMapChoose === params.data.name 
                        ? moduleStore.changeCurrentMapChoose('china') 
                        : moduleStore.changeCurrentMapChoose(params.data.name)
                    myChart.setOption(newOption, true)
                })
            }
            if (moduleStore.currentMapChoose !== 'china') {
                dataMaps.forEach(region => {
                    if (region.name === moduleStore.currentMapChoose) {
                        region.itemStyle.color = '#76ddff'
                        region.itemStyle.borderColor = '#76ddff' 
                        region.itemStyle.borderWidth = 6 
                    }
                })
            }
            const option = {
                visualMap: {
                    show: false,
                    symbolSize: [15, 30],
                    pieces: [ // 自定义每一段的范围，以及每一段的文字
                        { gte: 300, label: '1000-9999人', color: 'rgba(82, 104, 134, 0.8)' },
                        { gte: 200, lte: 299, label: '500-999人', color: 'rgba(82, 104, 134, 0.8)' },
                        { gte: 100, lte: 199, label: '100-499人', color: 'rgba(82, 104, 134, 0.8)' },
                        { gte: 10, lte: 99, label: '10-99人', color: 'rgba(82, 104, 134, 0.8)' },
                        // 不指定 min，表示 min 为无限大（-Infinity）。
                        { lte: 9, label: '1-9人', color: 'rgba(82, 104, 134, 0.8)' }, 
                    ],
                },
                geo3D: {
                    show: true,
                    type: 'map3D',
                    map: 'china',
                    regionHeight: 4,
                    shading: 'realistic',
                    height: 2800,
                    top: -500,
                    label: { // 标签的相关设置
                        show: true, // (地图上的城市名称)是否显示标签 [ default: false ]
                        formatter(param) {
                            const city = (param.name).substr(0, 3)
                            return city === '香港' ? `{sty1|${city}}` : `{sty2|${city}}`
                        },
                        rich: {
                            sty1: {
                                color: '#ffffff',
                                fontSize: 28,
                                fontFamily: 'SourceHanSansCN-Regular, sans-serif',
                                align: 'right',
                                backgroundColor: 'transparent',
                                width: 120,
                            },
                            sty2: {
                                color: '#ffffff',
                                fontSize: 28,
                                fontFamily: 'SourceHanSansCN-Regular, sans-serif',
                                align: 'center',
                            },
                        },
                    },
                    roam: true,
                    zoom: 2,
                    realisticMaterial: {
                        detailTexture: tietu1, // 纹理贴图
                        textureTiling: 1, // 纹理平铺，1是拉伸，数字表示纹理平铺次数
                        roughness: 1, // 材质粗糙度，0完全光滑，1完全粗糙
                        metalness: 1, // 0材质是非金属 ，1金属
                        roughnessAdjust: 0,
                    },
                    // 鼠标hover高亮
                    emphasis: {
                        label: {
                            show: true, // (地图上的城市名称)是否显示标签 [ default: false ]
                        },
                        itemStyle: {
                            areaColor: '#61A4E4',
                            // color: '#61A4E4',
                            borderColor: '#88BAEA',
                            borderWidth: 2,
                        },
                    },
                    light: { // 光照相关的设置。在 shading 为 'color' 的时候无效。
                        // 光照的设置会影响到组件以及组件所在坐标系上的所有图表。合理的光照设置能够让整个场景的明暗变得更丰富，更有层次。
                        main: { // 场景主光源的设置，在 globe 组件中就是太阳光。
                            color: '#fff', // 主光源的颜色。[ default: #fff ]
                            intensity: 4, // 主光源的强度。[ default: 1 ]
                            shadow: false, // 主光源是否投射阴影。默认关闭。 开启阴影可以给场景带来更真实和有层次的光照效果。但是同时也会增加程序的运行开销。
                            alpha: 150, // 主光源绕 x 轴，即上下旋转的角度。配合 beta 控制光源的方向。[ default: 40 ]
                            beta: 90, // 主光源绕 y 轴，即左右旋转的角度。[ default: 40 ]
                            shadowQuality: 'high',
                        },
                        ambient: { // 全局的环境光设置。
                            color: '#fff', // 环境光的颜色。[ default: #fff ]
                            intensity: 1, // 环境光的强度。[ default: 0.2 ]
                        },
                    },
                    viewControl: {
                        autoRotate: false, // 自动旋转
                        // autoRotateAfterStill: 10, // 静止时间后自动旋转
                        animation: true,
                        alpha: 45, // 视角绕 x 轴，即上下旋转的角度
                        beta: 5, // 视角绕 y 轴，即左右旋转的角度
                        distance: 90, // 视角距离主体的距离
                        rotateSensitivity: 0,
                        zoomSensitivity: 0,
                    },
                    regions: dataMaps,
                    data: [],
                    // silent: true,
                },
                series: [
                    {
                        show: true,
                        type: 'map3D',
                        map: 'china',
                        regionHeight: 4,
                        shading: 'realistic',
                        itemStyle: {
                            opacity: 0,
                        },
                        height: 2800,
                        top: -500,
                        label: { // 标签的相关设置
                            show: true, // (地图上的城市名称)是否显示标签 [ default: false ]
                            formatter(param) {
                                const city = (param.name).substr(0, 3)
                                return city === '香港' ? `{sty1|${city}}` : `{sty2|${city}}`
                            },
                            rich: {
                                sty1: {
                                    color: '#ffffff',
                                    fontSize: 28,
                                    fontFamily: 'SourceHanSansCN-Regular, sans-serif',
                                    align: 'right',
                                    backgroundColor: 'transparent',
                                    width: 120,
                                },
                                sty2: {
                                    color: '#ffffff',
                                    fontSize: 28,
                                    fontFamily: 'SourceHanSansCN-Regular, sans-serif',
                                    align: 'right',
                                    height: 150,
                                    width: 100,
                                    backgroundColor: 'transparent',
                                    verticalAlign: 'top',
                                },
                            },
                        },
                        roam: true,
                        zoom: 2,
                        realisticMaterial: {
                            detailTexture: tietu1, // 纹理贴图
                            textureTiling: 1, // 纹理平铺，1是拉伸，数字表示纹理平铺次数
                            roughness: 1, // 材质粗糙度，0完全光滑，1完全粗糙
                            metalness: 1, // 0材质是非金属 ，1金属
                            roughnessAdjust: 0,
                        },
                        // 鼠标hover高亮
                        emphasis: {
                            label: {
                                show: true, // (地图上的城市名称)是否显示标签 [ default: false ]
                            },
                            itemStyle: {
                                areaColor: '#61A4E4',
                                // color: '#61A4E4',
                                borderColor: '#88BAEA',
                                borderWidth: 2,
                            },
                        },
                        light: { // 光照相关的设置。在 shading 为 'color' 的时候无效。
                        // 光照的设置会影响到组件以及组件所在坐标系上的所有图表。合理的光照设置能够让整个场景的明暗变得更丰富，更有层次。
                            main: { // 场景主光源的设置，在 globe 组件中就是太阳光。
                                color: '#fff', // 主光源的颜色。[ default: #fff ]
                                intensity: 4, // 主光源的强度。[ default: 1 ]
                                shadow: false, // 主光源是否投射阴影。默认关闭。 开启阴影可以给场景带来更真实和有层次的光照效果。但是同时也会增加程序的运行开销。
                                alpha: 150, // 主光源绕 x 轴，即上下旋转的角度。配合 beta 控制光源的方向。[ default: 40 ]
                                beta: 90, // 主光源绕 y 轴，即左右旋转的角度。[ default: 40 ]
                                shadowQuality: 'high',
                            },
                            ambient: { // 全局的环境光设置。
                                color: '#fff', // 环境光的颜色。[ default: #fff ]
                                intensity: 1, // 环境光的强度。[ default: 0.2 ]
                            },
                        },
                        viewControl: {
                            autoRotate: false, // 自动旋转
                            // autoRotateAfterStill: 10, // 静止时间后自动旋转
                            animation: true,
                            alpha: 45, // 视角绕 x 轴，即上下旋转的角度
                            beta: 5, // 视角绕 y 轴，即左右旋转的角度
                            distance: 90, // 视角距离主体的距离
                            rotateSensitivity: 0,
                            zoomSensitivity: 0,
                        },
                        regions: dataMaps,
                        data: [],
                        zlevel: 1,
                    // silent: true,
                    },
                    // 叠加地图上需要显示的数据，插牌
                    {
                        type: 'scatter3D',
                        name: 'scatter3D',
                        coordinateSystem: 'geo3D',
                        symbol: 'pin',
                        symbolSize: 0,
                        label: {},
                        data: convertData(state.infoData),
                    },
                ],
            }
            // 把option设置给myChart实例
            myChart.setOption(option, true)
        }
        watch(() => state.infoData, () => {
            init()
        }) 
        // 加载完就调用的方法 vue3生命周期
        onMounted(() => {
            getData()
        })
        onBeforeUnmount(() => {
            clearInterval(timer)
        })
        return {
            ...toRefs(state),
        }
    },
}
</script>
<style lang="scss" scoped>
.container {
    position: relative;
    width: 7600px;
    height: 2160px;

    .bg-up {
        position: absolute;
        top: 0;
        left: 0;
        z-index: 4;
        width: 7600px;
        height: 2160px;

        @include set-back("../../assets/imgs/home/map-bg.png");
    }

    .bg {
        position: absolute;
        top: 0;
        left: 0;
        z-index: 3;
        width: 7600px;
        height: 2160px;

        @include set-back("../../assets/imgs/home/8kmap_bg.png");
    }

    .video-bg {
        position: relative;
        z-index: 2;
        width: 7600px;
        height: 2160px;
    }

    .map {
        position: absolute;
        top: 0;
        left: 0;
        z-index: 4;
    }

    .nine-line {
        position: absolute;
        top: 1580px;
        left: 5300px;
        z-index: 5;
        width: 150px;
        height: 245px;
        transform: scale(1.5);

        @include set-back("../../assets/imgs/map/nine-line.png");
    }
}
</style>

```

### 成品图

如下所示：
 ![avatar](https://pic.imgdb.cn/item/64995ba71ddac507cc1de943.jpg)
