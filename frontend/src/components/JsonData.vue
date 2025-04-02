<script setup>
import { ref } from 'vue'
import {VueFlow} from '@vue-flow/core'
import { Background } from '@vue-flow/background'
import { Controls } from '@vue-flow/controls'
import { MiniMap } from '@vue-flow/minimap'
/* these are necessary styles for vue flow */
import '@vue-flow/core/dist/style.css';

/* this contains the default theme, these are optional styles */
import '@vue-flow/core/dist/theme-default.css';
import CustomNode from "./JsonDataParent.vue";
import CustomNode1 from "./JsonDataChild.vue";

const nodes = ref([
  {
    id: '2',
    type: 'parent',
    data: { label: 'parent node' },
    position: { x: 100, y: 100 },
    style: { backgroundColor: 'rgba(16, 185, 129, 0.5)', width: '200px', height: '200px' },
  },
  {
    id: '2a',
    type: 'child',
    data: { label: 'child node' },
    position: { x: 10, y: 50 },
    extent: 'parent',
    parentNode: '2',
  },
  {
    id: '4',
    type: 'parent',
    data: { label: 'parent node' },
    position: { x: 320, y: 175 },
    style: { backgroundColor: 'rgba(16, 185, 129, 0.5)', width: '200px', height: '200px' },
  },
  {
    id: '4a',
    type: 'child',
    data: { label: 'child node' },
    position: { x: 10, y: 50 },
    extent: 'parent',
    parentNode: '4',
  },
  {
    id: '4c',
    type: 'child',
    data: { label: 'child node' },
    position: { x: 10, y: 100 },
    extent: 'parent',
    parentNode: '4',
  },
  {
    id: '999',
    type: 'child',
    data: { label: 'Drag me to extend area!' },
    position: { x: 20, y: 100 },
    extent: 'parent',
    parentNode: '2',
  },
])

const edges = ref([
  // { id: 'e1-2', source: '1', target: '2' },
  // { id: 'e1-4', source: '1', target: '4' },
  // { id: 'e1-4c', source: '1', target: '4c' },
  // { id: 'e2a-4a', source: '2a', target: '4a' },
  // { id: 'e4a-4b1', source: '4a', target: '4b1' },
  // { id: 'e4a-4b2', source: '4a', target: '4b2' },
  // { id: 'e4b1-4b2', source: '4b1', target: '4b2' },
])
</script>
<style scoped>
.simple-flow {
    width: 100%;
    height: 700px;
}
</style>

<template>
  <div class="simple-flow">
    <VueFlow :nodes="nodes" :edges="edges" fit-view-on-init elevate-edges-on-select>
      <template #node-parent="nodeProps">
        <CustomNode :data="nodeProps.data" :id="nodeProps.id" />
      </template>
      <template #node-child="nodeProps">
        <CustomNode1 :data="nodeProps.data" :id="nodeProps.id" />
      </template>
      <MiniMap />

      <Controls />

      <Background />
    </VueFlow>
  </div>

</template>
