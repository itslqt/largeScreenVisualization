<template>
    <div>
        <div>【云端告警风险】</div>
        <div ref="target" class="w-full h-full"></div>
    </div>
</template>

<script setup>
import {ref, onMounted, watch} from 'vue';
import * as eharts from 'echarts'
const props = defineProps({
    data: {
        type: Object,
        required: true
    }
})
// console.log(props.data);

let mChart = null;
const target = ref(null);

onMounted(() => {
    mChart = eharts.init(target.value);
    renderChart();
})

const renderChart = () => {
    const options = {
        // 雷达图的坐标系配置
        radar: {
            name: {
                textStyle: {
                    color: '#05D5FF',
                    fontSize: 14
                }
            },
            shape: 'polygon',
            center: ['50%', '50%'],
            radius: '80%',
            startAngle: 120,
            axisLine: {
                lineStyle: {
                    color: 'rgba(5, 213, 255, .8)'
                }
            },
            splitLine: {
                show: true,
                lineStyle: {
                    width: 1,
                    color: 'rgba(5, 213, 255, .8)'
                }
            },
            indicator: props.data.risks.map(item => ({name: item.name, max: 100})),
            splitArea: {
                show: false
            }
        },
        // 坐标极点
        polar: {
            center: ['50%', '50%'],
            radius: '0%'
        },
        // 坐标角度
        angleAxis: {
            min: 0,
            interval: 5,
            clockwise: false,
            axisTick: {
				show: false
			},
			// 不显示坐标轴文字
			axisLabel: {
				show: false
			},
			// 不显示坐标轴线
			axisLine: {
				show: false
			},
			// 不显示分割线
			splitLine: {
				show: false
			}
        },
        // 径向轴
        radiusAxis: {
            min: 0,
            interval: 20,
            splitLine: {
                show: true
            }
        },
        // 图表核心配置
        series: [
            {
                type: 'radar',
                symbol: 'circle',
                symbolSize: 10,
                itemStyle: {
                    normal: {
                        color: '#05D5FF'
                    }
                },
                areaStyle: {
                    normal: {
                        color: '#05D5FF',
                        opacity: 0.5
                    }
                },
                lineStyle: {
                    width: 2,
                    color: '#05D5FF'
                },
                label: {
                    normal: {
                        show: true,
                        formatter: (params) => {
                            return params.value
                        },
                        color: '#fff'
                    }
                },
                data: [
                    {
                        value: props.data.risks.map(item => item.value)
                    }
                ]
            }
        ]
    };

    mChart.setOption(options);
}

watch(() => props.data, renderChart);
</script>

<style lang="scss" scoped>

</style>