<template>
  <div class="todo-app">
    <form @submit.prevent="addTodo">
      <input v-model="newTodo" type="text" placeholder="Add a new task" />
      <button type="submit">Add</button>
    </form>
    <ul>
      <li
        v-for="(todo, index) in todos"
        :key="index"
        :class="{ done: todo.completed }"
      >
        <span @click="toggleComplete(index)">
          {{ todo.text }}
        </span>
        <button @click="removeTodo(index)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";

// Reactive state
// const todos = ref([]);
const newTodo = ref("");

// Initialize todos from localStorage
const todos = ref(JSON.parse(localStorage.getItem("todos") || "[]"));

// Watch for changes and update localStorage
watch(
  todos,
  (newTodos) => {
    localStorage.setItem("todos", JSON.stringify(newTodos));
  },
  { deep: true }
);

// Add a new todo
const addTodo = () => {
  if (newTodo.value.trim() !== "") {
    todos.value.push({ text: newTodo.value, completed: false });
    newTodo.value = "";
  }
};

// Toggle todo completion
const toggleComplete = (index) => {
  todos.value[index].completed = !todos.value[index].completed;
};

// Remove a todo
const removeTodo = (index) => {
  todos.value.splice(index, 1);
};
</script>

<style scoped>
.todo-app {
  max-width: 400px;
  margin: 50px auto;
  padding: 20px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
form {
  display: flex;
  margin-bottom: 20px;
}
input {
  flex: 1;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
button {
  padding: 10px;
  font-size: 16px;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  margin-left: 10px;
  cursor: pointer;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
li.done span {
  text-decoration: line-through;
  color: #6c757d;
}
span {
  cursor: pointer;
}
</style>
