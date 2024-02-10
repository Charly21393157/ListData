<template>
  <div>
    <div class="add-item">
      <input type="text" v-model="newItemName" placeholder="Name" />
      <input type="text" v-model="newItemDescription" placeholder="Description" />
      <button @click="addItem">Add Item</button>
    </div>
    <div class="sort-buttons">
      <button @click="sortData('asc')">Sort Ascending</button>
      <button @click="sortData('desc')">Sort Descending</button>
    </div>
    <div v-for="item in sortedDataList" :key="item.id" class="list-item" :style="{ color: item.color, backgroundColor: item.bgColor }" @click="changeColor(item)">
      <h3>{{ item.name }}</h3>
      <p>{{ item.description }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import type { IListData } from '@/interfaces/IListData';
import ListDataData from '@/data/ListDataData';

const dataList: IListData[] = ListDataData;
const newItemName = ref('');
const newItemDescription = ref('');

const sortedDataList = computed(() => [...dataList]);

const changeColor = (item: IListData) => {
  item.color = getRandomColor();
  item.bgColor = getRandomColor();
};

const addItem = () => {
  const name = newItemName.value.trim();
  const description = newItemDescription.value.trim();
  if (name && description) {
    const exists = dataList.some(item => item.name === name && item.description === description);
    if (!exists) {
      const id = dataList.length ? dataList[dataList.length - 1].id + 1 : 1;
      dataList.push({ id, name, description, color: 'black', bgColor: 'white' });
      newItemName.value = '';
      newItemDescription.value = '';
      alert('Item added successfully!');
    } else {
      alert('Item already exists!');
    }
  } else {
    alert('Please enter valid data!');
  }
};

const sortData = (order: 'asc' | 'desc') => {
  if (order === 'asc') {
    dataList.sort((a, b) => a.id - b.id);
    alert('Sorted in ascending order');
  } else {
    dataList.sort((a, b) => b.id - a.id);
    alert('Sorted in descending order');
  }
};


const getRandomColor = () => {
  return '#' + Math.floor(Math.random() * 16777215).toString(16);
};
</script>

<style scoped>
.add-item {
  margin-bottom: 10px;
}

.add-item input {
  margin-right: 10px;
}

.sort-buttons {
  margin-bottom: 10px;
}

.sort-buttons button {
  margin-right: 10px;
}

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
