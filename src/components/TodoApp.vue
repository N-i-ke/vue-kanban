<template>
  <div>
    <h2>Todoリスト</h2>
    <input v-model="newTodo" @keyup.enter="addTodo" placeholder="新しいタスクを入力" />
    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        <label>
          <input type="checkbox" v-model="todo.completed" />
          <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
        </label>
        <button @click="removeTodo(index)">削除</button>
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
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
}
.completed {
  text-decoration: line-through;
  color: gray;
}
button {
  margin-left: 8px;
}
</style>
