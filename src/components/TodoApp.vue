<template>
  <div class="todo-container">
    <h2>Todoリスト</h2>
    <div class="input-container">
      <input v-model="newTodo" @keyup.enter="addTodo" placeholder="新しいタスクを入力" />
      <button class="add-btn" @click="addTodo">追加</button>
    </div>
    <ul>
      <li v-for="(todo, index) in todos" :key="index" class="todo-item">
        <label>
          <input type="checkbox" v-model="todo.completed" class="checkbox" />
          <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
        </label>
        <button class="delete-btn" @click="removeTodo(index)">削除</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const todos = ref([
  { text: 'Vue3を学ぶ', completed: false },
  { text: 'Todoアプリを作成', completed: false },
]);

const newTodo = ref('');

const addTodo = () => {
  if (newTodo.value.trim() !== '') {
    todos.value.push({ text: newTodo.value, completed: false });
    newTodo.value = '';
  }
};

const removeTodo = (index) => {
  todos.value.splice(index, 1);
};
</script>

<style scoped>
.todo-container {
  max-width: 600px;
  margin: auto;
  padding: 20px;
  background: #f8f9fa;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
}
.input-container {
  display: flex;
  gap: 8px;
  margin-bottom: 16px;
}
input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
.add-btn {
  background: #28a745;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
}
.add-btn:hover {
  background: #218838;
}
ul {
  list-style-type: none;
  padding: 0;
}
.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: white;
  padding: 10px;
  border-radius: 4px;
  margin-bottom: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
.completed {
  text-decoration: line-through;
  color: gray;
}
.checkbox {
  margin-right: 8px;
}
.delete-btn {
  background: #dc3545;
  color: white;
  border: none;
  padding: 6px 10px;
  border-radius: 4px;
  cursor: pointer;
}
.delete-btn:hover {
  background: #c82333;
}
</style>