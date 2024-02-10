<template>
  <div>
    <ListDataItem 
      v-for="item in sortedDataList" 
      :key="item.id" 
      :item="item" 
      @changeColor="changeColor(item.id)"
    />
    <div class="add-item">
      <input type="text" v-model="newItemName" placeholder="Name" />
      <input type="text" v-model="newItemDescription" placeholder="Description" />
      <button @click="addItem">Add Item</button>
    </div>
    <div class="sort-buttons">
      <button @click="sortData('asc')">Sort Ascending</button>
      <button @click="sortData('desc')">Sort Descending</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import ListDataItem from './ListDataItem.vue';
import ListDataData from '@/data/ListDataData';
import { IListData } from '@/interfaces/IListData';
import { ref, computed } from 'vue';

const dataList: IListData[] = ListDataData;
const newItemName = ref('');
const newItemDescription = ref('');

const sortedDataList = computed(() => [...dataList]);

const changeColor = (id: number) => {
  const item = dataList.find(item => item.id === id);
  if (item) {
    item.color = getRandomColor();
    item.bgColor = getRandomColor();
  }
};

const addItem = () => {
  const name = newItemName.value.trim();
  const description = newItemDescription.value.trim();
  if (name.length > 0 && description.length > 0) {
    const id = dataList.length + 1;
    dataList.push({ id, name, description, color: 'black', bgColor: 'white' });
    newItemName.value = '';
    newItemDescription.value = '';
  }
};

const sortData = (order: 'asc' | 'desc') => {
  if (order === 'asc') {
    dataList.sort((a, b) => a.id - b.id);
  } else {
    dataList.sort((a, b) => b.id - a.id);
  }
};

const getRandomColor = () => {
  return '#' + Math.floor(Math.random() * 16777215).toString(16);
};
</script>

<style scoped>
.add-item {
  margin-top: 20px;
}

.add-item input {
  margin-right: 10px;
}

.sort-buttons {
  margin-top: 20px;
}

.sort-buttons button {
  margin-right: 10px;
}
</style>
