<template>
  <div class="simple-flow">
    <VueFlow v-model="elements" @connect="onConnect" elevate-edges-on-select>
      <template #node-table="{data, id}">
        <div class="table-node">
          <div class="table-header">
            {{ data.name }}
          </div>
          <div class="table-body">
            <!-- 字段列表将作为子节点显示 -->
          </div>
        </div>
      </template>
      <Background />
      <Controls />
    </VueFlow>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { VueFlow } from '@vue-flow/core';
import '@vue-flow/core/dist/style.css';
import '@vue-flow/core/dist/theme-default.css';
import { Background } from "@vue-flow/background";
import { Controls } from "@vue-flow/controls";

// 计算字段位置函数
const calculateFieldPositions = (fields, parentId, startY = 50) => {
  return fields.map((field, index) => ({
    id: `${parentId}-field-${index + 1}`,
    // 默认节点类型
    position: { x: 0, y: startY + index * 60 },
    parentNode: parentId,
    sourcePosition: 'right',
    targetPosition: 'left',
    draggable: false,
    extent: 'parent',
    style: { width: '180px' },
    // 确保节点可连接
    connectable: true,
    data: {
      label: field  // 使用label属性，默认节点会显示这个值
    }
  }));
};

// 用户表字段
const userFields = ['字段1', '字段2', '字段3'];
// 商品表字段
const productFields = ['字段1', '字段2', '字段3'];

// 初始化节点和边
const elements = ref([
  // 用户表
  {
    id: 'user-table',
    type: 'table',
    position: { x: 100, y: 100 },
    style: { width: '220px', height: '350px' },
    connectable: false,  // 表本身不可连接
    data: {
      name: '用户表'
    }
  },
  // 商品表
  {
    id: 'product-table',
    type: 'table',
    position: { x: 500, y: 100 },
    style: { width: '220px', height: '350px' },
    connectable: false,  // 表本身不可连接
    data: {
      name: '商品表'
    }
  }
]);

// 添加字段节点
onMounted(() => {
  // 添加用户表字段
  const userFieldNodes = calculateFieldPositions(userFields, 'user-table');
  userFieldNodes.forEach(node => elements.value.push(node));

  // 添加商品表字段
  const productFieldNodes = calculateFieldPositions(productFields, 'product-table');
  productFieldNodes.forEach(node => elements.value.push(node));
});

// 处理连线事件
const onConnect = (params) => {
  console.log('连接创建:', params);
  // 创建新的连线
  elements.value.push({
    id: `e${params.source}-${params.target}`,
    source: params.source,
    target: params.target,
    animated: true
  });
};
</script>

<style scoped>
.simple-flow {
  width: 100%;
  height: 700px;
}

/* 表格节点样式 */
.table-node {
  border: 1px solid #ccc;
  border-radius: 8px;
  overflow: hidden;
  background-color: white;
  height: 100%;
  display: flex;
  flex-direction: column;
}

.table-header {
  background-color: #f0f0f0;
  padding: 10px;
  font-weight: bold;
  text-align: center;
  border-bottom: 1px solid #ccc;
}

.table-body {
  flex: 1;
  padding: 10px;
}

/* 隐藏父节点的连接点 */
:deep(.vue-flow__node[data-type="table"] > .vue-flow__handle) {
  display: none !important;
}

/* 确保子节点在父节点内正确显示 */
:deep(.vue-flow__node:not([data-type="table"])) {
  width: 180px !important;
  left: 20px !important;
}

/* 确保连接点可见 */
:deep(.vue-flow__handle) {
  width: 12px !important;
  height: 12px !important;
  background-color: #1a192b !important;
  opacity: 1 !important;
  visibility: visible !important;
}

:deep(.vue-flow__edge) {
  pointer-events: all !important;
}
</style>