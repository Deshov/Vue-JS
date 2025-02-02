<template>
  <div class="todo-app">
    <h1>To-Do List</h1>
    <div class="input-group">
      <input
        type="text"
        v-model="currentTask"
        placeholder="Enter a new task"
        @keyup.enter="addEditTask"
      />
      <button @click="addEditTask">{{btnLabel}}</button>
    </div>
    <ul class="todo-list">
      <li v-for="task in tasks" :key="task.id">
        <span>{{ task.text }}</span>
        <div class="btn-group">
          <button @click="setToEditTask(task.id)">Edit</button>
          <button @click="removeTask(task.id)">Remove</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref,computed } from 'vue';

const tasks = ref([]);

const currentTask = ref(null);
const currentId = ref(null);

let nextId = 1;

const btnLabel = computed(() => currentId.value ? 'Edit' : 'Add');

const addEditTask = () => {
  if (currentTask.value.trim()) {
    if (currentId.value) {
      const index = tasks.value.findIndex(task => task.id === currentId.value);
      if (index === -1) {
        alert('Task has been deleted!');
        currentTask.value = '';
        currentId.value = null;
        return
      }
      tasks.value[index].text = currentTask.value.trim();
      currentId.value = null;
    } else {
      tasks.value.push({ id: nextId++, text: currentTask.value.trim() });
    }
    currentTask.value = '';
  } else {
    alert('Task cannot be empty.');
  }
};

const removeTask = (id) => {
  tasks.value = tasks.value.filter(task => task.id !== id);
};

const setToEditTask = (id) => {
  currentId.value = id;
  currentTask.value = tasks.value.find(task => task.id === id).text;
};
</script>

<style scoped>
.todo-app {
  max-width: 400px;
  margin: 20px auto;
  font-family: Arial, sans-serif;
  text-align: center;
}

.input-group {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-bottom: 20px;
}

input {
  width: 70%;
  padding: 8px;
  font-size: 1rem;
}

button {
  padding: 8px 16px;
  font-size: 1rem;
  cursor: pointer;
}

.todo-list {
  list-style-type: none;
  padding: 0;
}

.todo-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 10px;
  margin-bottom: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.btn-group {
  display: flex;
  gap: 10px;
}

.btn-group button {
  color: white;
  border: none;
  border-radius: 4px;
  padding: 5px 10px;
  cursor: pointer;
}

.btn-group button:first-child {
  background-color: green; /* Green for the first button */
}

.btn-group button:first-child:hover {
  background-color: darkgreen; /* Dark green on hover */
}

.btn-group button:nth-child(2) {
  background-color: red; /* Red for the second button */
}

.btn-group button:nth-child(2):hover {
  background-color: darkred; /* Dark red on hover */
}
</style>
