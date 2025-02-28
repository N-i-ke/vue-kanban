<template>
  <div class="todo-container">
    <h2 class="todo-title">Board</h2>
    <!-- Input Container -->
    <div class="input-container">
      <input v-model="newTodo" @keyup.enter="addTodo" placeholder="New Task" />
      <button class="add-btn" @click="addTodo">Add Task</button>
    </div>

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
          <template #item="{ element }">
            <div class="todo-item" :key="element.text">
              <label>
                <input type="checkbox" v-model="element.completed" class="checkbox" />
                <span :class="{ completed: element.completed }">{{ element.text }}</span>
              </label>
            </div>
          </template>
        </draggable>
      </div>

      <!-- In Progress Column -->
      <div class="kanban-column">
        <h3>In Progress</h3>
        <draggable 
          v-model="inProgressTasks" 
          group="tasks" 
          @start="drag=true" 
          @end="drag=false"
          :key="inProgressTasks"
        >
          <template #item="{ element }">
            <div class="todo-item" :key="element.text">
              <label>
                <input type="checkbox" v-model="element.completed" class="checkbox" />
                <span :class="{ completed: element.completed }">{{ element.text }}</span>
              </label>
            </div>
          </template>
        </draggable>
      </div>

      <!-- Code Review Column -->
      <div class="kanban-column">
        <h3>Code Review</h3>
        <draggable 
          v-model="codeReviewTasks" 
          group="tasks" 
          @start="drag=true" 
          @end="drag=false"
          :key="codeReviewTasks"
        >
          <template #item="{ element }">
            <div class="todo-item" :key="element.text">
              <label>
                <input type="checkbox" v-model="element.completed" class="checkbox" />
                <span :class="{ completed: element.completed }">{{ element.text }}</span>
              </label>
            </div>
          </template>
        </draggable>
      </div>

      <!-- Test Column -->
      <div class="kanban-column">
        <h3>Test</h3>
        <draggable 
          v-model="testTasks" 
          group="tasks" 
          @start="drag=true" 
          @end="drag=false"
          :key="testTasks"
        >
          <template #item="{ element }">
            <div class="todo-item" :key="element.text">
              <label>
                <input type="checkbox" v-model="element.completed" class="checkbox" />
                <span :class="{ completed: element.completed }">{{ element.text }}</span>
              </label>
            </div>
          </template>
        </draggable>
      </div>

      <!-- Done Column -->
      <div class="kanban-column">
        <h3>Done</h3>
        <draggable 
          v-model="doneTasks" 
          group="tasks" 
          @start="drag=true" 
          @end="drag=false"
          :key="doneTasks"
        >
          <template #item="{ element }">
            <div class="todo-item" :key="element.text">
              <label>
                <input type="checkbox" v-model="element.completed" class="checkbox" />
                <span :class="{ completed: element.completed }">{{ element.text }}</span>
              </label>
            </div>
          </template>
        </draggable>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import draggable from 'vuedraggable';

// Tasks
const todos = ref([
  { text: 'Vue3を学ぶ', completed: false },
  { text: 'Todoアプリを作成', completed: false },
]);

// In Progress Tasks
const inProgressTasks = ref([]);

// Code Review Tasks
const codeReviewTasks = ref([]);

// Test Tasks
const testTasks = ref([]);

// Done Tasks
const doneTasks = ref([]);
const newTodo = ref('');
const drag = ref(false);

// Add new task
const addTodo = () => {
  if (newTodo.value.trim() !== '') {
    todos.value.push({ text: newTodo.value, completed: false });
    newTodo.value = '';
  }
};
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
  width: 18%;
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
</style>
