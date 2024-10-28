  <script setup>
  import { ref, toRef } from 'vue';
  
  const props = defineProps({
    nodes: Array,
  });
  
  const emit = defineEmits(['updateNodes']);
  
  const nodes = toRef(props, 'nodes');
  const newNodeName = ref('');
  const editingNode = ref(null);
  
  const addNode = () => {
    if (newNodeName.value.trim()) {
      nodes.value.push({ id: Date.now(), name: newNodeName.value });
      newNodeName.value = '';
      emit('updateNodes', nodes.value);
    }
  };
  
  const editNode = (nodeId) => {
    editingNode.value = nodeId;
  };
  
  const saveNode = (node) => {
    editingNode.value = null;
    emit('updateNodes', nodes.value);
  };
  
  const removeNode = (nodeId) => {
    nodes.value = nodes.value.filter(node => node.id !== nodeId);
    emit('updateNodes', nodes.value);
  };
  
  const moveNode = (index, direction) => {
    const newIndex = index + direction;
    if (newIndex >= 0 && newIndex < nodes.value.length) {
      [nodes.value[index], nodes.value[newIndex]] = [nodes.value[newIndex], nodes.value[index]];
      emit('updateNodes', nodes.value);
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
        <li v-for="(node, index) in nodes" :key="node.id" class="node-item">
          <div v-if="editingNode === node.id">
            <input v-model="node.name" />
            <button @click="saveNode(node)">Сохранить</button>
          </div>
          <div v-else>
            <span>{{ node.name }}</span>
            <button @click="editNode(node.id)">Редактировать</button>
            <button @click="removeNode(node.id)">Удалить</button>
          </div>
  
          <button @click="moveNode(index, -1)" :disabled="index === 0">Вверх</button>
          <button @click="moveNode(index, 1)" :disabled="index === nodes.length - 1">Вниз</button>
        </li>
      </ul>
    </div>
  </template>
  
  <style scoped>
  .node-list {
    margin-top: 10px;
    padding: 10px;
    border-top: 1px solid #ddd;
  }
  
  .add-node {
    display: flex;
    gap: 10px;
    margin-bottom: 10px;
  }
  
  .node-item {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 5px;
  }
  
  button {
    padding: 4px 6px;
    font-size: 14px;
  }
  </style>
  