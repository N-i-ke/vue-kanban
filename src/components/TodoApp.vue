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
  gap: 16px;
}

.kanban-column {
  width: 18%;
  background: #ffffff;
  border-radius: 8px;
  padding: 16px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.kanban-column h3 {
  margin-top: 0;
  margin-bottom: 16px;
  color: #333;
  font-size: 1.1em;
}

.draggable-container {
  min-height: 100px;
  padding: 8px;
  border-radius: 4px;
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
  align-items: center;
  justify-content: space-between;
  background: white;
  padding: 12px;
  border-radius: 6px;
  margin-bottom: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  cursor: grab;
  transition: all 0.2s ease;
  border: 1px solid #e9ecef;
}

.todo-item:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
  transform: translateY(-1px);
}

.todo-item:active {
  cursor: grabbing;
}

.todo-item.dragging {
  opacity: 0.8;
  transform: rotate(5deg);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}

.completed {
  text-decoration: line-through;
  color: gray;
}

.checkbox {
  margin-right: 8px;
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
