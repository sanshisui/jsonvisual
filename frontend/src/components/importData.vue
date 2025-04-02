<script setup lang="ts">
import { ref } from 'vue';

const value = ref('');




</script>

<template>
  <Textarea v-model="value" rows="5" cols="30" />
  <Button label="Submit" />
</template>

<style scoped>

</style>