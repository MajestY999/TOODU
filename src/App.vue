<template>
  <div class="todo-app">
    <h1>To-Do List</h1>
    <div class="input-group">
      <input 
        v-model="newTask" 
        @keyup.enter="addTask" 
        placeholder="Введите задачу..."
      />
      <button @click="addTask">Добавить</button>
    </div>
    <ul class="task-list">
      <li 
        v-for="(task, index) in tasks" 
        :key="index"
        @click="toggleDone(index)"
        :class="{ 'done': task.done }"
      >
        {{ task.text }}
        <button @click.stop="removeTask(index)">×</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const newTask = ref('');
const tasks = ref([]);


onMounted(() => {
  const savedTasks = localStorage.getItem('tasks');
  if (savedTasks) {
    tasks.value = JSON.parse(savedTasks);
  }
});

const addTask = () => {
  if (newTask.value.trim() === '') return;
  tasks.value.push({ text: newTask.value, done: false });
  newTask.value = '';
  saveTasks();
};

const removeTask = (index) => {
  tasks.value.splice(index, 1);
  saveTasks();
};

const toggleDone = (index) => {
  tasks.value[index].done = !tasks.value[index].done;
  saveTasks();
};

const saveTasks = () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value));
};
</script>

<style scoped>
.todo-app {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

.input-group {
  display: flex;
  margin-bottom: 20px;
}

input {
  flex-grow: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px 0 0 4px;
}

button {
  padding: 10px 15px;
  background: #42b983;
  color: white;
  border: none;
  border-radius: 0 4px 4px 0;
  cursor: pointer;
}

button:hover {
  background: #369f6e;
}

.task-list {
  list-style: none;
  padding: 0;
}

.task-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  margin-bottom: 5px;
  background: #f9f9f9;
  border-radius: 4px;
  cursor: pointer;
}

.task-list li.done {
  text-decoration: line-through;
  color: #888;
}

.task-list li button {
  background: #ff6b6b;
  border-radius: 50%;
  width: 25px;
  height: 25px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.task-list li button:hover {
  background: #ff5252;
}
</style>