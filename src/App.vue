<script setup>
import DeviceList from "./components/DeviceList.vue";
</script>

<template>
  <div id="app">
    <h1>Список устройств и узлов учета</h1>
    <DeviceList />
  </div>
</template>

<style scoped>
#app {
  max-width: 800px;
  margin: auto;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  color: #4a90e2;
}
</style>
