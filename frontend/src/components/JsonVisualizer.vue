<template>
  <div class="simple-flow">
    <VueFlow v-model="elements" @connect="onConnect" elevate-edges-on-select>
      <template #node-user="{data, id}">
        <div class="w-60 h-60 bg-surface-0 dark:bg-surface-900 rounded-2xl border border-surface-200 dark:border-surface-700">
          <div class="w-full p-3 bg-primary-100 dark:bg-primary-900 rounded-t-2xl">
            <span class="font-bold">{{data.name}} - {{ data.remark }}</span>
          </div>
          <div class="p-2">
            <!-- 字段列表将作为子节点显示 -->
          </div>
        </div>
      </template>
      <template #node-field="{data, id}">
        <div class="field-node w-full p-2 border-b border-surface-200 dark:border-surface-700 flex justify-between">
          <div>
            <span class="font-medium">{{data.name}}</span>
            <span class="text-sm text-surface-600"> - {{data.remark}}</span>
          </div>
          <div class="text-xs text-surface-500">{{data.type}}{{data.length ? `(${data.length})` : ''}}</div>
        </div>
      </template>
      <Background />
      <Controls />
    </VueFlow>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { VueFlow } from '@vue-flow/core';
import '@vue-flow/core/dist/style.css';
import '@vue-flow/core/dist/theme-default.css';
import {Background} from "@vue-flow/background";
import {Controls} from "@vue-flow/controls";

// 初始化节点和边
const elements = ref([
  // 用户表
  {
    id: '1',
    type: 'user',
    position: { x: 100, y: 100 },
    connectable: false,  // 表本身不可连接
    data: {
      name: 'User',
      remark: '用户表'
    }
  },
  // 用户表的字段
  {
    id: 'field-1-1',
    type: 'field',
    position: { x: 0, y: 0 },
    parentNode: '1',
    sourcePosition: 'right',
    targetPosition: 'left',
    draggable: false,
    extent: 'parent',
    data: {
      name: 'id',
      remark: '用户ID',
      type: 'int',
      length: 11
    }
  },
  {
    id: 'field-1-2',
    type: 'field',
    position: { x: 0, y: 40 },
    parentNode: '1',
    sourcePosition: 'right',
    targetPosition: 'left',
    draggable: false,
    extent: 'parent',
    data: {
      name: 'name',
      remark: '姓名',
      type: 'varchar',
      length: 50
    }
  },
  {
    id: 'field-1-3',
    type: 'field',
    position: { x: 0, y: 80 },
    parentNode: '1',
    sourcePosition: 'right',
    targetPosition: 'left',
    draggable: false,
    extent: 'parent',
    data: {
      name: 'age',
      remark: '年龄',
      type: 'int',
      length: 3
    }
  },
  
  // 订单表
  {
    id: '2',
    type: 'user',
    position: { x: 400, y: 100 },
    connectable: false,  // 表本身不可连接
    data: {
      name: 'Order',
      remark: '订单表'
    }
  },
  // 订单表的字段
  {
    id: 'field-2-1',
    type: 'field',
    position: { x: 0, y: 0 },
    parentNode: '2',
    sourcePosition: 'right',
    targetPosition: 'left',
    draggable: false,
    extent: 'parent',
    data: {
      name: 'id',
      remark: '订单ID',
      type: 'int',
      length: 11
    }
  },
  {
    id: 'field-2-2',
    type: 'field',
    position: { x: 0, y: 40 },
    parentNode: '2',
    sourcePosition: 'right',
    targetPosition: 'left',
    draggable: false,
    extent: 'parent',
    data: {
      name: 'user_id',
      remark: '用户ID',
      type: 'int',
      length: 11
    }
  }
]);

// 处理连线事件
const onConnect = (params) => {
  // 创建新的连线
  elements.value.push({
    id: `e${params.source}-${params.target}`,
    source: params.source,
    target: params.target
  });
};
</script>

<style scoped>
.simple-flow {
  width: 100%;
  height: 700px;
}

/* 隐藏父节点的连接点 */
:deep(.vue-flow__node[data-type="user"] > .vue-flow__handle) {
  display: none !important;
}

.field-node {
  cursor: pointer;
}

.field-node:hover {
  background-color: rgba(0, 0, 0, 0.05);
}
</style>