<template>
  <div class="custom-node">
    <div class="node-header">{{ data.label }}</div>
    <div class="node-content">
      <!-- 这里是节点内容 -->
      <p>这是一个自定义节点</p>
    </div>

    <!-- 可见的目标连接点，位于左侧 -->
<!--    <Handle-->
<!--        type="source"-->
<!--        :position="Position.Left"-->
<!--        :id="id + '-source-left'"-->
<!--    />-->
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
</style>