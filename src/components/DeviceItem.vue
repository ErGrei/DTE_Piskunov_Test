<script setup>
import { ref } from "vue";
import NodeList from "./NodeList.vue";

const props = defineProps({
  device: Object,
});

const emit = defineEmits(["updateDevice", "deleteDevice"]);

const isEditing = ref(false);
const localDeviceName = ref(props.device.name); 

const saveDevice = () => {
  emit("updateDevice", { ...props.device, name: localDeviceName.value });
  isEditing.value = false;
};

const updateNodes = (newNodes) => {
  emit("updateDevice", { ...props.device, nodes: newNodes });
};

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
.device-actions {
  display: flex;
  gap: 10px;
  margin-top: 10px;
}
</style>
