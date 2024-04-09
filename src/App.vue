<script setup>
import Graph from "./components/GraphComponent.vue";
import { ref } from "vue";

const value = ref("");
const options = [
  {
    value: "Linear Regression",
    label: "Linear Regression",
  },
  {
    value: "Lasso",
    label: "Lasso",
  },
  {
    value: "Ridge",
    label: "Ridge",
  },
  {
    value: "Poly regression",
    label: "Poly regression",
  },
  {
    value: "Decision tree",
    label: "Decision tree",
  },
];
</script>

<template>
  <header>
    <div class="navigation-bar">
      <el-select
        v-model="value"
        clearable
        placeholder="请选择"
        size="large"
        style="width: 240px"
      >
        <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        />
      </el-select>
    </div>
  </header>

  <main>
    <Graph :msg="value" />

  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
  width: 100%;
}
.navigation-bar{
  margin: 32px auto;
} 

@media (min-width: 1024px) {
  header {
    display: flex, block;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }
}
</style>
