<template>
  <div class="list-item" :style="{ color: item.color, backgroundColor: item.bgColor }" @click="emitChangeColor">
    <h3>{{ item.name }}</h3>
    <p>{{ item.description }}</p>
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';
import type { IListData } from '@/interfaces/IListData';

const props = defineProps<{
  item: IListData;
}>();

const emits = defineEmits(['changeColor']);

const emitChangeColor = () => {
  emits('changeColor');
};
</script>

<style scoped>
.list-item {
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 10px;
  cursor: pointer;
  transition: background-color 0.3s, color 0.3s;
}

.list-item:hover {
  background-color: #f0f0f0;
  color: #333;
}
</style>
