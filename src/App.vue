<script setup lang="ts">
import { ref } from 'vue';
import { item } from './types/Todo';

let todoList = ref([]);
const inputText = ref('');
const itemId = ref(0);

const readLocalStorage = () => {
  const data = localStorage.getItem('todoList');
  if (data) {
    todoList.value = JSON.parse(data);
    itemId.value = todoList.value.length 
  }
}

const writeLocalStorage = () => {
  localStorage.setItem('todoList', JSON.stringify(todoList.value))
}

const addTask = () => {
  todoList.value.push({
    id: itemId.value++,
    name: inputText.value || 'New task',
    done: false
  })
  inputText.value = '';
  writeLocalStorage()
}

const doneBtn = (id) => {
  todoList.value = todoList.value.map(item => {
    if (item.id === id) {
      item.done = !item.done
    }
    return item
  })
  writeLocalStorage()
}

readLocalStorage()

</script>

<template>
  <div>
    <h1>ToDo</h1>
    <form @submit.prevent="addTask">
      <input type="text" v-model="inputText">
      <button>Добавить</button>
    </form>
    <div>
      <ul>
        <li :class="{ doneTask: item.done }" v-for="item in todoList" :key="item.id">
          {{ item.name }}
          <button @click="doneBtn(item.id)">Сделано</button>
        </li>
      </ul>
    </div>
  </div>
</template>
