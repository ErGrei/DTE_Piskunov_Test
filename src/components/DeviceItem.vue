<script setup>
import { ref } from "vue";
import NodeList from "./NodeList.vue";
import { watchEffect, toRef } from "vue";

const props = defineProps({
  device: Object,
});

const emit = defineEmits(["updateDevice", "deleteDevice"]);

const device = toRef(props, "device");
const isEditing = ref(false);
const localDeviceName = ref(device.value.name);

const saveDevice = () => {
  emit("updateDevice", { ...device.value, name: localDeviceName.value });
  isEditing.value = false;
};

const updateNodes = (newNodes) => {
  emit("updateDevice", { ...device.value, nodes: newNodes });
};

watchEffect(() => {
  localDeviceName.value = device.value.name;
});
</script>

<template>
  <div class="device-item">
    <div v-if="isEditing">
      <input v-model="localDeviceName" />
      <button @click="saveDevice">Сохранить</button>
    </div>
    <div v-else>
      <span>{{ device.name }}</span>
      <button @click="isEditing = true">Редактировать</button>
      <button @click="$emit('deleteDevice', device.id)">Удалить</button>
    </div>

    <NodeList :nodes="device.nodes" @updateNodes="updateNodes" />
  </div>
</template>

<style scoped>
.device-item {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
}

span {
  font-weight: bold;
  margin-right: 10px;
}
</style>
