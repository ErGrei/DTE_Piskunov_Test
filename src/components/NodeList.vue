<script setup>
import { ref } from "vue";

const props = defineProps({
  nodes: Array,
});

const emit = defineEmits(["updateNodes"]);

const newNodeName = ref("");
const editingNode = ref(null);

const localNodes = ref([...props.nodes]);

const addNode = () => {
  if (newNodeName.value.trim()) {
    localNodes.value.push({ id: Date.now(), name: newNodeName.value });
    newNodeName.value = "";
    emit("updateNodes", localNodes.value);
  }
};

const editNode = (nodeId) => {
  editingNode.value = nodeId;
};

const saveNode = (node) => {
  editingNode.value = null;
  emit("updateNodes", localNodes.value);
};

const removeNode = (nodeId) => {
  localNodes.value = localNodes.value.filter((node) => node.id !== nodeId);
  emit("updateNodes", localNodes.value);
};

const moveNode = (index, direction) => {
  const newIndex = index + direction;
  if (newIndex >= 0 && newIndex < localNodes.value.length) {
    [localNodes.value[index], localNodes.value[newIndex]] = [
      localNodes.value[newIndex],
      localNodes.value[index],
    ];
    emit("updateNodes", localNodes.value);
  }
};
</script>

<template>
  <div class="node-list">
    <div class="add-node">
      <input v-model="newNodeName" placeholder="Название узла" />
      <button @click="addNode">Добавить узел</button>
    </div>

    <ul>
      <li v-for="(node, index) in localNodes" :key="node.id" class="node-item">
        <div v-if="editingNode === node.id">
          <input v-model="node.name" />
          <button @click="saveNode(node)">Сохранить</button>
        </div>
        <div v-else>
          <span>{{ node.name }}</span>
          <button @click="editNode(node.id)">Редактировать</button>
          <button @click="removeNode(node.id)">Удалить</button>
          <button @click="moveNode(index, -1)" :disabled="index === 0">
            Вверх
          </button>
          <button
            @click="moveNode(index, 1)"
            :disabled="index === localNodes.length - 1"
          >
            Вниз
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<style>
.node-list {
  margin-top: 10px;
  padding-top: 10px;
  border-top: 1px solid #ddd;
}

.add-node {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.node-item {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background-color: #fff;
  margin-bottom: 10px;
}

.node-actions {
  display: flex;
  gap: 10px;
}

input,
button {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  background-color: #4a90e2;
  color: white;
  cursor: pointer;
}

button:hover {
  background-color: #357ab8;
}
</style>
