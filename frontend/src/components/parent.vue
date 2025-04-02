<template>
  <div style="width: 800px; height: 600px">
    <VueFlow v-model="elements">
      <template #node-custom="nodeProps">
        <CustomNode :data="nodeProps.data" :id="nodeProps.id" />
      </template>
      <template #node-custom1="nodeProps">
        <CustomNode1 :data="nodeProps.data" :id="nodeProps.id" />
      </template>
    </VueFlow>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { VueFlow, useVueFlow } from '@vue-flow/core';
import '@vue-flow/core/dist/style.css';
import CustomNode from './child.vue';
import CustomNode1 from './child1.vue';

const { addNodes, addEdges } = useVueFlow();

// 定义初始节点
const elements = ref([
  {
    id: '1',
    type: 'custom',
    position: { x: 100, y: 100 },
    data: { label: '节点 1' },
  },
  {
    id: '2',
    type: 'custom1',
    position: { x: 400, y: 100 },
    data: { label: '节点 2' },
  },
]);
</script>