<script setup>
import { ref } from "vue";
import DeviceItem from "./DeviceItem.vue";

const newDeviceName = ref("");
const devices = ref([
  {
    id: 1,
    name: "Устройство 1",
    nodes: [
      { id: 1, name: "Узел 1" },
      { id: 2, name: "Узел 2" },
      { id: 3, name: "Узел 3" },
    ],
  },
  {
    id: 2,
    name: "Устройство 2",
    nodes: [
      { id: 4, name: "Узел 1" },
      { id: 5, name: "Узел 2" },
      { id: 6, name: "Узел 3" },
    ],
  },
  {
    id: 3,
    name: "Устройство 3",
    nodes: [
      { id: 7, name: "Узел 1" },
      { id: 8, name: "Узел 2" },
      { id: 9, name: "Узел 3" },
    ],
  },
]);

const addDevice = () => {
  if (newDeviceName.value.trim()) {
    devices.value.push({
      id: Date.now(),
      name: newDeviceName.value,
      nodes: [],
    });
    newDeviceName.value = "";
  }
};

const updateDevice = (updatedDevice) => {
  const index = devices.value.findIndex(
    (device) => device.id === updatedDevice.id
  );
  if (index !== -1) devices.value[index] = updatedDevice;
};


const deleteDevice = (deviceId) => {
  devices.value = devices.value.filter((device) => device.id !== deviceId);
};
</script>

<template>
  <div class="device-list">
    <div class="add-device">
      <input v-model="newDeviceName" placeholder="Название устройства" />
      <button @click="addDevice">Добавить устройство</button>
    </div>

    <div
      v-for="device in devices"
      :key="device.id"
      class="device-item"
    >
      <DeviceItem
        :device="device"
        @updateDevice="updateDevice"
        @deleteDevice="deleteDevice"
      />
    </div>
  </div>
</template>

<style scoped>
.device-list {
  padding: 20px;
  background-color: #f8f8f8;
}

.add-device {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.device-item {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background-color: #fff;
  margin-bottom: 10px;
}
</style>
