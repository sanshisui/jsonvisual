<template>
  <div class="custom-node">
    <div class="node-header">{{ data.label }}</div>
    <div class="node-content">
      <!-- 这里是节点内容 -->
      <p>这是一个自定义节点</p>
    </div>

    <!-- 透明的源连接点，位于右侧 -->
    <Handle
        type="target"
        :position="Position.Right"
        :id="id + '-target-right'"
        class="handle"
    />
  </div>
</template>

<script setup>
import { Handle, Position } from '@vue-flow/core';

// 接收从父组件传递的属性
const props = defineProps({
  data: {
    type: Object,
    required: true,
  },
  id: {
    type: String,
    required: true,
  },
});
</script>

<style scoped>
.custom-node {
  background: white;
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 10px;
  width: 200px;
  position: relative;
}

.node-header {
  font-weight: bold;
  margin-bottom: 8px;
  border-bottom: 1px solid #eee;
  padding-bottom: 5px;
}

.node-content {
  font-size: 0.9em;
}

/* 连接点样式 */
.handle {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background-color: #fff;
  border: 2px solid #555;
  transition: all 0.2s;
}
</style>