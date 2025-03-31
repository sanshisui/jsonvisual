<template>
  <div class="nested-flow-container">
    <VueFlow
        v-model="elements"
        :default-zoom="1"
        :min-zoom="0.2"
        :max-zoom="4"
        class="vue-flow-canvas"
        @connect="onConnect"
        @node-drag-stop="onNodeDragStop"
    >
      <!-- 普通节点 -->
      <template #node-default="{ data }">
        <div class="default-node">
          {{ data.label }}
        </div>
      </template>

      <!-- 父节点 -->
      <template #node-parent="{ data, id }">
        <div class="parent-node" :style="{ backgroundColor: data.color || '#8dd3c7' }">
          <div class="parent-label">{{ data.label }}</div>
          <div class="parent-content">
            <slot :name="`node-content-${id}`"></slot>
          </div>
          <div class="node-handle bottom" />
        </div>
      </template>

      <!-- 子节点 -->
      <template #node-child="{ data }">
        <div class="child-node">
          <div>{{ data.label }}</div>
          <div class="node-handle bottom" />
        </div>
      </template>

      <!-- 扩展区域节点 -->
      <template #node-extender="{ data }">
        <div class="extender-node">
          <div>{{ data.label }}</div>
          <div class="node-handle bottom" />
        </div>
      </template>

      <!-- 嵌套父节点 -->
      <template #node-nested-parent="{ data, id }">
        <div class="nested-parent-node" :style="{ backgroundColor: data.color || '#b3cde3' }">
          <div class="parent-label">{{ data.label }}</div>
          <div class="parent-content">
            <slot :name="`node-content-${id}`"></slot>
          </div>
          <div class="node-handle bottom" />
        </div>
      </template>

      <Background pattern-color="#aaa" gap="8" />
      <MiniMap />
      <Controls />
    </VueFlow>
  </div>
</template>

<script setup>
import { ref, onMounted, provide } from 'vue';
import { VueFlow, useVueFlow } from '@vue-flow/core';
import '@vue-flow/core/dist/style.css';
import '@vue-flow/core/dist/theme-default.css';
import {Background} from "@vue-flow/background";
import {MiniMap} from "@vue-flow/minimap";
import {Controls} from "@vue-flow/controls";

// 初始化流程图元素
const elements = ref([]);
const { findNode, addNodes, addEdges } = useVueFlow();

// 提供节点数据给子组件
provide('flowElements', elements);

// 创建嵌套节点结构
onMounted(() => {
  // 顶部普通节点
  const topNode = {
    id: 'node-1',
    type: 'default',
    data: { label: 'NODE' },
    position: { x: 300, y: 50 },
    style: { border: '1px solid #1a192b', borderRadius: '3px' }
  };

  // 左侧父节点
  const leftParentNode = {
    id: 'parent-1',
    type: 'parent',
    data: {
      label: 'PARENT NODE',
      color: '#8dd3c7'
    },
    position: { x: 100, y: 200 },
    style: { width: 250, height: 300 }
  };

  // 左侧父节点中的子节点
  const leftChildNode = {
    id: 'child-1',
    type: 'child',
    data: { label: 'CHILD NODE' },
    position: { x: 125, y: 250 },
    parentNode: 'parent-1',
    extent: 'parent'
  };

  // 左侧父节点中的扩展区域
  const extenderNode = {
    id: 'extender-1',
    type: 'extender',
    data: { label: 'DRAG ME TO EXTEND AREA!' },
    position: { x: 125, y: 330 },
    parentNode: 'parent-1',
    extent: 'parent',
    style: { backgroundColor: '#b3e5fc', color: '#01579b' }
  };

  // 右侧父节点
  const rightParentNode = {
    id: 'parent-2',
    type: 'parent',
    data: {
      label: 'PARENT NODE',
      color: '#8dd3c7'
    },
    position: { x: 450, y: 200 },
    style: { width: 400, height: 450 }
  };

  // 右侧父节点中的子节点1
  const rightChildNode1 = {
    id: 'child-2',
    type: 'child',
    data: { label: 'CHILD NODE' },
    position: { x: 475, y: 250 },
    parentNode: 'parent-2',
    extent: 'parent'
  };

  // 右侧父节点中的子节点2
  const rightChildNode2 = {
    id: 'child-3',
    type: 'child',
    data: { label: 'CHILD NODE' },
    position: { x: 625, y: 250 },
    parentNode: 'parent-2',
    extent: 'parent'
  };

  // 嵌套父节点
  const nestedParentNode = {
    id: 'nested-parent-1',
    type: 'nested-parent',
    data: {
      label: 'NESTED PARENT NODE',
      color: '#b3cde3'
    },
    position: { x: 550, y: 350 },
    parentNode: 'parent-2',
    extent: 'parent',
    style: { width: 300, height: 250 }
  };

  // 嵌套父节点中的子节点1
  const nestedChildNode1 = {
    id: 'nested-child-1',
    type: 'child',
    data: { label: 'NESTED CHILD NODE' },
    position: { x: 575, y: 400 },
    parentNode: 'nested-parent-1',
    extent: 'parent'
  };

  // 嵌套父节点中的子节点2
  const nestedChildNode2 = {
    id: 'nested-child-2',
    type: 'child',
    data: { label: 'NESTED CHILD NODE' },
    position: { x: 575, y: 480 },
    parentNode: 'nested-parent-1',
    extent: 'parent'
  };

  // 添加所有节点
  elements.value = [
    topNode,
    leftParentNode,
    leftChildNode,
    extenderNode,
    rightParentNode,
    rightChildNode1,
    rightChildNode2,
    nestedParentNode,
    nestedChildNode1,
    nestedChildNode2,
    // 连线
    {
      id: 'edge-1-parent-1',
      source: 'node-1',
      target: 'parent-1',
      type: 'smoothstep'
    },
    {
      id: 'edge-1-parent-2',
      source: 'node-1',
      target: 'parent-2',
      type: 'smoothstep'
    }
  ];
});

// 处理节点连接事件
const onConnect = (params) => {
  const newEdge = {
    id: `edge-${params.source}-${params.target}`,
    source: params.source,
    target: params.target,
    type: 'smoothstep'
  };

  addEdges([newEdge]);
};

// 处理节点拖动停止事件
const onNodeDragStop = (event, node) => {
  console.log('节点位置已更新:', node.id, node.position);
};
</script>

<style scoped>
.nested-flow-container {
  width: 100%;
  height: 800px;
}

.vue-flow-canvas {
  width: 100%;
  height: 100%;
}

.default-node {
  padding: 10px 20px;
  border: 1px solid #1a192b;
  border-radius: 3px;
  background: white;
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 150px;
  min-height: 40px;
}

.parent-node {
  padding: 10px;
  border-radius: 5px;
  width: 100%;
  height: 100%;
  position: relative;
}

.nested-parent-node {
  padding: 10px;
  border-radius: 5px;
  width: 100%;
  height: 100%;
  position: relative;
}

.parent-label {
  font-weight: bold;
  margin-bottom: 10px;
  text-align: center;
}

.parent-content {
  width: 100%;
  height: calc(100% - 30px);
  position: relative;
}

.child-node {
  padding: 10px 20px;
  border: 1px solid #1a192b;
  border-radius: 3px;
  background: white;
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 150px;
  min-height: 40px;
  position: relative;
}

.extender-node {
  padding: 10px 20px;
  border: 1px solid #1a192b;
  border-radius: 3px;
  background: white;
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 150px;
  min-height: 40px;
  position: relative;
  text-align: center;
}

.node-handle {
  position: absolute;
  width: 10px;
  height: 10px;
  background: #1a192b;
  border-radius: 50%;
}

.node-handle.bottom {
  bottom: -5px;
  left: 50%;
  transform: translateX(-50%);
}
</style>