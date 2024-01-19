<template>
    <div>
        <div>【服务资源占用比】</div>
        <div ref="target" class="w-full h-full"></div>
    </div>
</template>

<script setup>
import {ref, onMounted, watch} from 'vue';
import * as echarts from 'echarts';

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
    mChart = echarts.init(target.value);
    renderChart();
})

const renderChart = () => {
    const options = {
        xAxis: {
            type: 'category',
            data: props.data.servers.map(item => item.name),
            axisLabel: {
                color: '#9EB1C8'
            }
        },
        yAxis: {
            type: 'value',
            show: false,
            max: function(value) {
                return parseInt(value.max * 1.2);
            }
        },
        grid: {
            top: 16,
            right: 0,
            bottom: 26,
            left: -26,
            containLabel: true
        },
        series: [
            {
                type: 'bar',
                data: props.data.servers.map(item => ({name: item.name, value: item.value})),
                showBackground: true,
                backgroundStyle: {
                    color: 'rgba(180, 180, 180, 0.2)'
                },
                itemStyle: {
                    color: '#479AD3',
                    barBorderRadius: 5,
                    shadowColor: 'rgba(0, 0, 0, 0.3)',
                    shadowBlur: 5
                },
                barWidth: 12,
                label: {
                    show: true,
                    position: 'top',
                    textStyle: {
                        color: '#fff'
                    },
                    formatter: '{c}%'
                }
            }
        ]
    };

    mChart.setOption(options);
}

watch(() => props.data,
      () => {renderChart();})
</script>

<style lang="scss" scoped>

</style>