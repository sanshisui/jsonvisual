<script setup>
import AppTopbar from "./components/AppTopbar.vue";
import JsonVisualizer from "./components/JsonVisualizer.vue"
</script>

<template>
    <div class="bg-surface-50 dark:bg-surface-950 min-h-screen p-8 flex flex-col gap-6">
        <AppTopbar />
    </div>
  <div class="bg-surface-50 dark:bg-surface-950 min-h-screen p-8 flex flex-col gap-6">
    <JsonVisualizer />
  </div>
</template>
