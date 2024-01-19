
<template>
  <div class="bg-[url('assets/imgs/bg.jpg')] bg-cover bg-center h-screen text-white p-5 flex overflow-hidden"
  v-if="data"
  >
    <!-- left 横向柱状图 关系图 雷达图 -->
    <div class="flex-1 mr-5 bg-opacity-50 bg-slate-800 p-3 flex-col">
      <HorizontalBar class="h-1/3 box-border pb-4" :data="data.regionData"></HorizontalBar>
      <RadBar class="h-1/3 box-border pb-4" :data="data.riskData"></RadBar>
      <Relation class="h-1/3" :data="data.relationData"></Relation>
    </div>
    <!-- middle 数据总览图 地图可视化 -->
    <div class="w-1/2 mr-5 flex flex-col">
      <TotalData class="bg-opacity-50 bg-slate-800 p-3" :data="data.totalData"></TotalData>
      <MapChart class="bg-opacity-50 bg-slate-800 p-3 mt-4 flex-1" :data="data.mapData"></MapChart>
    </div>
    <!-- right 竖向柱状图 环形图 文档云图 -->
    <div class="flex-1 bg-opacity-50 bg-slate-800 p-3 flex-col">
      <VerTicalBar class="h-1/3 box-border pb-4" :data="data.serverData"></VerTicalBar>
      <RingBar class="h-1/3 box-border pb-4" :data="data.abnormalData"></RingBar>
      <WordCloud class="h-1/3" :data="data.wordCloudData"></WordCloud>
    </div>
  </div>
</template>

<script setup>
import HorizontalBar from './components/HorizontalBar.vue';
import MapChart from './components/MapChart.vue';
import RadBar from './components/RadBar.vue';
import Relation from './components/Relation.vue';
import RingBar from './components/RingBar.vue';
import TotalData from './components/TotalData.vue';
import VerTicalBar from './components/VerticalBar.vue';
import WordCloud from './components/WordCloud.vue'

import { ref } from 'vue';
import {getVisualization} from './api/visualization'

const data = ref(null);
const loadData = async () => {
  data.value = await getVisualization();
  console.log(data.value);
}

loadData();

setInterval(() => {
  loadData();
},3000)

</script>

<style>

</style>