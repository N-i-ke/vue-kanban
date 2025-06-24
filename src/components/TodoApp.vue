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
          item-key="id"
          class="draggable-container"
          :class="{ 'dragging': drag }"
        >
          <template #item="{ element }">
            <div class="todo-item" :class="{ 'dragging': drag }">
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
          item-key="id"
          class="draggable-container"
          :class="{ 'dragging': drag }"
        >
          <template #item="{ element }">
            <div class="todo-item" :class="{ 'dragging': drag }">
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
          item-key="id"
          class="draggable-container"
          :class="{ 'dragging': drag }"
        >
          <template #item="{ element }">
            <div class="todo-item" :class="{ 'dragging': drag }">
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
          item-key="id"
          class="draggable-container"
          :class="{ 'dragging': drag }"
        >
          <template #item="{ element }">
            <div class="todo-item" :class="{ 'dragging': drag }">
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
          item-key="id"
          class="draggable-container"
          :class="{ 'dragging': drag }"
        >
          <template #item="{ element }">
            <div class="todo-item" :class="{ 'dragging': drag }">
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

// ユニークIDを生成する関数
const generateId = () => {
  return Date.now() + Math.random().toString(36).substr(2, 9);
};

// Tasks
const todos = ref([
  { id: generateId(), text: 'Vue3を学ぶ', completed: false },
  { id: generateId(), text: 'Todoアプリを作成', completed: false },
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
    todos.value.push({ 
      id: generateId(), 
      text: newTodo.value, 
      completed: false 
    });
    newTodo.value = '';
  }
};
</script>

<style scoped>
.todo-title {
  text-align: left;
}
.todo-container {
  min-width: 1200px;
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
  gap: 12px;
}

.kanban-column {
  width: 20%;
  background: #ffffff;
  border-radius: 8px;
  padding: 16px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  min-height: 200px;
}

.kanban-column h3 {
  margin-top: 0;
  margin-bottom: 16px;
  color: #333;
  font-size: 1.2em;
  font-weight: 600;
}

.draggable-container {
  min-height: 150px;
  padding: 12px;
  border-radius: 6px;
  transition: background-color 0.2s ease;
}

.draggable-container.dragging {
  background-color: #f0f8ff;
  border: 2px dashed #007bff;
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
  align-items: flex-start;
  justify-content: flex-start;
  background: white;
  padding: 16px;
  border-radius: 8px;
  margin-bottom: 12px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  cursor: grab;
  transition: all 0.2s ease;
  border: 1px solid #e9ecef;
  min-height: 60px;
  font-size: 14px;
  line-height: 1.5;
  white-space: pre-wrap;
  word-break: break-all;
  overflow: hidden;
}

.todo-item:hover {
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
  transform: translateY(-2px);
}

.todo-item:active {
  cursor: grabbing;
}

.todo-item.dragging {
  opacity: 0.8;
  transform: rotate(3deg);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
}

.todo-item label {
  display: flex;
  align-items: flex-start;
  width: 100%;
  cursor: grab;
  gap: 8px;
  overflow: hidden;
}

.todo-item label span {
  overflow: hidden;
  white-space: pre-wrap;
  word-break: break-all;
  flex: 1;
}

.todo-item label:active {
  cursor: grabbing;
}

.completed {
  text-decoration: line-through;
  color: #6c757d;
  font-style: italic;
}

.checkbox {
  margin-right: 0;
  transform: scale(1.2);
  cursor: pointer;
  flex-shrink: 0;
  margin-top: 2px;
}

/* ドラッグ中のスタイル */
.sortable-ghost {
  opacity: 0.5;
  background: #f8f9fa;
}

.sortable-chosen {
  background: #e3f2fd;
}
</style>
