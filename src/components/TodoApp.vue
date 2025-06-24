<template>
  <div class="todo-container">
    <div class="header">
      <h1 class="main-title">
        <span class="icon">📋</span>
        Kanban Board
      </h1>
      <p class="subtitle">Organize your tasks efficiently</p>
    </div>
    
    <!-- Input Container -->
    <div class="input-container">
      <div class="input-wrapper">
        <input 
          v-model="newTodo" 
          @keyup.enter="addTodo" 
          placeholder="Add a new task..." 
          class="task-input"
        />
        <button class="add-btn" @click="addTodo">
          <span class="btn-icon">+</span>
          Add Task
        </button>
      </div>
    </div>

    <div class="kanban-board">
      <!-- Todo Column -->
      <div class="kanban-column todo-column">
        <div class="column-header">
          <span class="column-icon">📝</span>
          <h3>To Do</h3>
          <span class="task-count">{{ todos.length }}</span>
        </div>
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
            <div class="todo-item" :class="{ 'dragging': drag, 'completed': element.completed }">
              <label>
                <input type="checkbox" v-model="element.completed" class="checkbox" />
                <span class="checkmark"></span>
                <span class="task-text" :class="{ completed: element.completed }">{{ element.text }}</span>
              </label>
            </div>
          </template>
        </draggable>
      </div>

      <!-- In Progress Column -->
      <div class="kanban-column progress-column">
        <div class="column-header">
          <span class="column-icon">⚡</span>
          <h3>In Progress</h3>
          <span class="task-count">{{ inProgressTasks.length }}</span>
        </div>
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
            <div class="todo-item" :class="{ 'dragging': drag, 'completed': element.completed }">
              <label>
                <input type="checkbox" v-model="element.completed" class="checkbox" />
                <span class="checkmark"></span>
                <span class="task-text" :class="{ completed: element.completed }">{{ element.text }}</span>
              </label>
            </div>
          </template>
        </draggable>
      </div>

      <!-- Code Review Column -->
      <div class="kanban-column review-column">
        <div class="column-header">
          <span class="column-icon">🔍</span>
          <h3>Code Review</h3>
          <span class="task-count">{{ codeReviewTasks.length }}</span>
        </div>
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
            <div class="todo-item" :class="{ 'dragging': drag, 'completed': element.completed }">
              <label>
                <input type="checkbox" v-model="element.completed" class="checkbox" />
                <span class="checkmark"></span>
                <span class="task-text" :class="{ completed: element.completed }">{{ element.text }}</span>
              </label>
            </div>
          </template>
        </draggable>
      </div>

      <!-- Test Column -->
      <div class="kanban-column test-column">
        <div class="column-header">
          <span class="column-icon">🧪</span>
          <h3>Test</h3>
          <span class="task-count">{{ testTasks.length }}</span>
        </div>
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
            <div class="todo-item" :class="{ 'dragging': drag, 'completed': element.completed }">
              <label>
                <input type="checkbox" v-model="element.completed" class="checkbox" />
                <span class="checkmark"></span>
                <span class="task-text" :class="{ completed: element.completed }">{{ element.text }}</span>
              </label>
            </div>
          </template>
        </draggable>
      </div>

      <!-- Done Column -->
      <div class="kanban-column done-column">
        <div class="column-header">
          <span class="column-icon">✅</span>
          <h3>Done</h3>
          <span class="task-count">{{ doneTasks.length }}</span>
        </div>
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
            <div class="todo-item" :class="{ 'dragging': drag, 'completed': element.completed }">
              <label>
                <input type="checkbox" v-model="element.completed" class="checkbox" />
                <span class="checkmark"></span>
                <span class="task-text" :class="{ completed: element.completed }">{{ element.text }}</span>
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
* {
  box-sizing: border-box;
}

.todo-container {
  min-width: 1200px;
  margin: auto;
  padding: 30px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 40px;
  color: white;
}

.main-title {
  font-size: 3rem;
  font-weight: 700;
  margin: 0 0 10px 0;
  text-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 15px;
}

.icon {
  font-size: 2.5rem;
  animation: bounce 2s infinite;
}

@keyframes bounce {
  0%, 20%, 50%, 80%, 100% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(-10px);
  }
  60% {
    transform: translateY(-5px);
  }
}

.subtitle {
  font-size: 1.2rem;
  opacity: 0.9;
  margin: 0;
  font-weight: 300;
}

.input-container {
  margin-bottom: 30px;
}

.input-wrapper {
  display: flex;
  gap: 15px;
  max-width: 600px;
  margin: 0 auto;
}

.task-input {
  flex: 1;
  padding: 15px 20px;
  border: none;
  border-radius: 25px;
  font-size: 16px;
  background: rgba(255, 255, 255, 0.95);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(10px);
  transition: all 0.3s ease;
}

.task-input:focus {
  outline: none;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
  transform: translateY(-2px);
}

.task-input::placeholder {
  color: #999;
}

.add-btn {
  background: linear-gradient(45deg, #ff6b6b, #ee5a24);
  color: white;
  border: none;
  padding: 15px 25px;
  border-radius: 25px;
  cursor: pointer;
  font-size: 16px;
  font-weight: 600;
  display: flex;
  align-items: center;
  gap: 8px;
  box-shadow: 0 8px 25px rgba(238, 90, 36, 0.3);
  transition: all 0.3s ease;
}

.add-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 12px 35px rgba(238, 90, 36, 0.4);
}

.btn-icon {
  font-size: 18px;
  font-weight: bold;
}

.kanban-board {
  display: flex;
  justify-content: space-between;
  gap: 20px;
}

.kanban-column {
  width: 20%;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20px;
  padding: 20px;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  min-height: 400px;
  transition: all 0.3s ease;
}

.kanban-column:hover {
  transform: translateY(-5px);
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.15);
}

.column-header {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 20px;
  padding-bottom: 15px;
  border-bottom: 2px solid rgba(0, 0, 0, 0.1);
}

.column-icon {
  font-size: 1.5rem;
}

.column-header h3 {
  margin: 0;
  color: #333;
  font-size: 1.1em;
  font-weight: 600;
  flex: 1;
}

.task-count {
  background: linear-gradient(45deg, #667eea, #764ba2);
  color: white;
  padding: 4px 12px;
  border-radius: 15px;
  font-size: 0.8rem;
  font-weight: 600;
  min-width: 25px;
  text-align: center;
}

.draggable-container {
  min-height: 200px;
  padding: 10px;
  border-radius: 15px;
  transition: all 0.3s ease;
}

.draggable-container.dragging {
  background: linear-gradient(135deg, rgba(102, 126, 234, 0.1), rgba(118, 75, 162, 0.1));
  border: 2px dashed #667eea;
}

.todo-item {
  background: white;
  padding: 15px;
  border-radius: 12px;
  margin-bottom: 12px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  cursor: grab;
  transition: all 0.3s ease;
  border: 1px solid rgba(0, 0, 0, 0.05);
  position: relative;
  overflow: hidden;
}

.todo-item::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 3px;
  background: linear-gradient(45deg, #667eea, #764ba2);
  transform: scaleX(0);
  transition: transform 0.3s ease;
}

.todo-item:hover::before {
  transform: scaleX(1);
}

.todo-item:hover {
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
  transform: translateY(-3px);
}

.todo-item:active {
  cursor: grabbing;
}

.todo-item.dragging {
  opacity: 0.8;
  transform: rotate(2deg) scale(1.05);
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
}

.todo-item.completed {
  background: linear-gradient(135deg, #f8f9fa, #e9ecef);
}

.todo-item label {
  display: flex;
  align-items: flex-start;
  width: 100%;
  cursor: grab;
  gap: 12px;
  position: relative;
}

.todo-item label:active {
  cursor: grabbing;
}

.task-text {
  flex: 1;
  min-width: 0;
  white-space: pre-wrap;
  word-break: break-all;
  line-height: 1.5;
  font-size: 14px;
  color: #333;
  transition: all 0.3s ease;
}

.task-text.completed {
  text-decoration: line-through;
  color: #6c757d;
  font-style: italic;
  opacity: 0.7;
}

.checkbox {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

.checkmark {
  height: 20px;
  width: 20px;
  background-color: #f8f9fa;
  border: 2px solid #dee2e6;
  border-radius: 4px;
  position: relative;
  flex-shrink: 0;
  transition: all 0.3s ease;
}

.checkbox:checked ~ .checkmark {
  background: linear-gradient(45deg, #28a745, #20c997);
  border-color: #28a745;
}

.checkmark:after {
  content: "";
  position: absolute;
  display: none;
  left: 6px;
  top: 2px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
}

.checkbox:checked ~ .checkmark:after {
  display: block;
}

/* ドラッグ中のスタイル */
.sortable-ghost {
  opacity: 0.3;
  background: linear-gradient(135deg, #667eea, #764ba2);
  transform: scale(0.95);
}

.sortable-chosen {
  background: linear-gradient(135deg, rgba(102, 126, 234, 0.1), rgba(118, 75, 162, 0.1));
}

/* レスポンシブ対応 */
@media (max-width: 1200px) {
  .todo-container {
    min-width: auto;
    padding: 20px;
  }
  
  .kanban-board {
    flex-direction: column;
    gap: 15px;
  }
  
  .kanban-column {
    width: 100%;
  }
}
</style>
