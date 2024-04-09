<template>
  <div class="graph">
    <div ref="charts" style="width: 1000px; height: 500px; margin: auto"></div>
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import * as echarts from "echarts";
import Papa from "papaparse";
import { defineProps } from "vue";

const charts = ref(null);
const fullData = ref([]);
const curData = ref([]);
const option = {
  legend: {
    data: ["MESFOC_nmile", "predict_MESFOC_nmile"],
  },
  xAxis: {
    name: "Date Time",
    type: "category",
    data: ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
  },
  yAxis: {
    // 设置 y 轴的范围
    min: 60,
    max: 90,
    name: "油耗(Kg/NM)",
    type: "value",
  },
  series: [
    {
      name: "MESFOC_nmile",
      data: [],
      type: "line",
    },
    {
      name: "predict_MESFOC_nmile",
      data: [],
      type: "line",
    },
  ],
};
const fetchCsv = async () => {
  return fetch("/../public/LinearRegression_output.csv")
    .then((response) => response.text())
    .then((text) => {
      return Papa.parse(text, { header: true });
    });
};
onMounted(async () => {
  const { data } = await fetchCsv();
  //   令 curData 等于 data 的前 120 条数据
  curData.value = data.slice(0, 60);
  //   令 fullData 等于 data 的剩余数据
  fullData.value = data.slice(60);
  //   每秒钟从fullData中取出一条数据，然后更新图表
  setInterval(() => {
    curData.value.push(fullData.value.shift());
    if (curData.value.length > 60) {
      curData.value.shift();
    }
    option.xAxis.data = curData.value.map(
      (item) => item.PCDate + " " + item.PCTime
    );
    option.series[0].data = curData.value.map((item) => item.MESFOC_nmile);
    option.series[1].data = curData.value.map(
      (item) => item.predict_MESFOC_nmile
    );
    const e = echarts.init(charts.value);
    e.setOption(option);
  }, 1000);
});

defineProps({
  msg: {
    type: String,
    required: true,
  },
});
</script>

<style scoped>
.graph {
  text-align: center;
}

@media (min-width: 1024px) {
  .graph {
    text-align: center;
  }
}
</style>
