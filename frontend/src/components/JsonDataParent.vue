<script setup lang="ts">
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

<template>
  <span>{{props.data.label}}</span>
</template>

<style scoped>

</style>