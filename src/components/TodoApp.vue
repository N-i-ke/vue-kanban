<template>
  <div class="todo-container">
    <h2 class="todo-title">Board</h2>
    <div class="kanban-board">
      <!-- Todo Column -->
      <div class="kanban-column">
        <h3>To Do</h3>
        <draggable 
          v-model="todos" 
          group="tasks" 
          @start="drag=true" 
          @end="drag=false"
          :key="todos"
        >
          <template #item="{ element, index }">
            <div class="todo-item" :key="element.text">
              <label>
                <input type="checkbox" v-model="element.completed" class="checkbox" />
                <span :class="{ completed: element.completed }">{{ element.text }}</span>
              </label>
              <button class="trash-btn" @click="moveToTrash(index)">ゴミ箱に入れる</button>
            </div>
          </template>
        </draggable>
      </div>

      <!-- Trash Column -->
      <div class="kanban-column">
        <h3>Done</h3>
        <draggable 
          v-model="trash" 
          group="tasks" 
          @start="drag=true" 
          @end="drag=false"
          :key="trash"
        >
          <template #item="{ element, index }">
            <div class="trash-item" :key="element.text">
              <span>{{ element.text }}</span>
              <button class="restore-btn" @click="restoreFromTrash(index)">復元</button>
            </div>
          </template>
        </draggable>
      </div>
    </div>

    <!-- Input Container -->
    <div class="input-container">
      <input v-model="newTodo" @keyup.enter="addTodo" placeholder="新しいタスクを入力" />
      <button class="add-btn" @click="addTodo">追加</button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import draggable from 'vuedraggable';

const todos = ref([
  { text: 'Vue3を学ぶ', completed: false },
  { text: 'Todoアプリを作成', completed: false },
]);

const trash = ref([]);
const newTodo = ref('');
const drag = ref(false);

const addTodo = () => {
  if (newTodo.value.trim() !== '') {
    todos.value.push({ text: newTodo.value, completed: false });
    newTodo.value = '';
  }
};

const moveToTrash = (index) => {
  trash.value.push(todos.value[index]);
  todos.value.splice(index, 1);
};

const restoreFromTrash = (index) => {
  todos.value.push(trash.value[index]);
  trash.value.splice(index, 1);
};

onMounted(() => {
  document.body.style.background = "#d3d3d3";
});
</script>

<style scoped>
.todo-title {
  text-align: left;
}
.todo-container {
  min-width: 650px;
  margin: auto;
  padding: 20px;
  background: #f8f9fa;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.kanban-board {
  display: flex;
  justify-content: space-between;
}

.kanban-column {
  width: 45%;
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

.todo-item, .trash-item {
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

.trash-btn {
  background: #ffc107;
  color: white;
  border: none;
  padding: 6px 10px;
  border-radius: 4px;
  cursor: pointer;
}

.trash-btn:hover {
  background: #e0a800;
}

.restore-btn {
  background: #17a2b8;
  color: white;
  border: none;
  padding: 6px 10px;
  border-radius: 4px;
  cursor: pointer;
}

.restore-btn:hover {
  background: #138496;
}
</style>
