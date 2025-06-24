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
            <div class="todo-item" :class="{ 'dragging': drag, 'completed': element.completed }" @click="openTaskModal(element)">
              <div class="task-content">
                <span class="task-text" :class="{ completed: element.completed }">{{ element.title || element.text }}</span>
                <div class="task-meta" v-if="element.assignee || element.memo">
                  <span class="assignee" v-if="element.assignee">👤 {{ element.assignee }}</span>
                  <span class="memo-indicator" v-if="element.memo">📝</span>
                </div>
              </div>
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
            <div class="todo-item" :class="{ 'dragging': drag, 'completed': element.completed }" @click="openTaskModal(element)">
              <div class="task-content">
                <span class="task-text" :class="{ completed: element.completed }">{{ element.title || element.text }}</span>
                <div class="task-meta" v-if="element.assignee || element.memo">
                  <span class="assignee" v-if="element.assignee">👤 {{ element.assignee }}</span>
                  <span class="memo-indicator" v-if="element.memo">📝</span>
                </div>
              </div>
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
            <div class="todo-item" :class="{ 'dragging': drag, 'completed': element.completed }" @click="openTaskModal(element)">
              <div class="task-content">
                <span class="task-text" :class="{ completed: element.completed }">{{ element.title || element.text }}</span>
                <div class="task-meta" v-if="element.assignee || element.memo">
                  <span class="assignee" v-if="element.assignee">👤 {{ element.assignee }}</span>
                  <span class="memo-indicator" v-if="element.memo">📝</span>
                </div>
              </div>
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
            <div class="todo-item" :class="{ 'dragging': drag, 'completed': element.completed }" @click="openTaskModal(element)">
              <div class="task-content">
                <span class="task-text" :class="{ completed: element.completed }">{{ element.title || element.text }}</span>
                <div class="task-meta" v-if="element.assignee || element.memo">
                  <span class="assignee" v-if="element.assignee">👤 {{ element.assignee }}</span>
                  <span class="memo-indicator" v-if="element.memo">📝</span>
                </div>
              </div>
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
            <div class="todo-item" :class="{ 'dragging': drag, 'completed': element.completed }" @click="openTaskModal(element)">
              <div class="task-content">
                <span class="task-text" :class="{ completed: element.completed }">{{ element.title || element.text }}</span>
                <div class="task-meta" v-if="element.assignee || element.memo">
                  <span class="assignee" v-if="element.assignee">👤 {{ element.assignee }}</span>
                  <span class="memo-indicator" v-if="element.memo">📝</span>
                </div>
              </div>
            </div>
          </template>
        </draggable>
      </div>
    </div>

    <!-- Task Detail Modal -->
    <div v-if="showModal" class="modal-overlay" @click="closeModal">
      <div class="modal-content" @click.stop>
        <div class="modal-header">
          <h2>Task Details</h2>
          <button class="close-btn" @click="closeModal">×</button>
        </div>
        <div class="modal-body">
          <div class="form-group">
            <label>Title</label>
            <input 
              v-model="editingTask.title" 
              type="text" 
              class="form-input"
              placeholder="Enter task title..."
            />
          </div>
          <div class="form-group">
            <label>Assignee</label>
            <input 
              v-model="editingTask.assignee" 
              type="text" 
              class="form-input"
              placeholder="Enter assignee name..."
            />
          </div>
          <div class="form-group">
            <label>Memo</label>
            <textarea 
              v-model="editingTask.memo" 
              class="form-textarea"
              placeholder="Enter task memo..."
              rows="4"
            ></textarea>
          </div>
          <div class="form-group">
            <label>Status</label>
            <div class="status-toggle">
              <button 
                class="status-btn" 
                :class="{ 'active': !editingTask.completed }"
                @click="editingTask.completed = false"
              >
                <span class="status-icon">⏳</span>
                In Progress
              </button>
              <button 
                class="status-btn" 
                :class="{ 'active': editingTask.completed }"
                @click="editingTask.completed = true"
              >
                <span class="status-icon">✅</span>
                Completed
              </button>
            </div>
          </div>
        </div>
        <div class="modal-footer">
          <button class="btn-secondary" @click="closeModal">Cancel</button>
          <button class="btn-primary" @click="saveTask">Save Changes</button>
        </div>
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
  { id: generateId(), text: 'Vue3を学ぶ', title: 'Vue3を学ぶ', completed: false, assignee: '', memo: '' },
  { id: generateId(), text: 'Todoアプリを作成', title: 'Todoアプリを作成', completed: false, assignee: '', memo: '' },
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

// Modal state
const showModal = ref(false);
const editingTask = ref({});
const originalTask = ref(null);

// Add new task
const addTodo = () => {
  if (newTodo.value.trim() !== '') {
    todos.value.push({ 
      id: generateId(), 
      text: newTodo.value,
      title: newTodo.value,
      completed: false,
      assignee: '',
      memo: ''
    });
    newTodo.value = '';
  }
};

// Open task modal
const openTaskModal = (task) => {
  editingTask.value = { ...task };
  originalTask.value = task;
  showModal.value = true;
};

// Close modal
const closeModal = () => {
  showModal.value = false;
  editingTask.value = {};
  originalTask.value = null;
};

// Save task changes
const saveTask = () => {
  if (originalTask.value) {
    Object.assign(originalTask.value, editingTask.value);
    // Update text field for backward compatibility
    originalTask.value.text = editingTask.value.title || editingTask.value.text;
  }
  closeModal();
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
  background: rgba(255, 255, 255, 0.1);
  color: white;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
  backdrop-filter: blur(10px);
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.task-input:focus {
  outline: none;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
  transform: translateY(-2px);
  background: rgba(255, 255, 255, 0.15);
  border-color: rgba(255, 255, 255, 0.4);
}

.task-input::placeholder {
  color: rgba(255, 255, 255, 0.7);
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
  background: rgba(255, 255, 255, 0.1);
  border-radius: 20px;
  padding: 20px;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  min-height: 400px;
  transition: all 0.3s ease;
}

.kanban-column:hover {
  transform: translateY(-5px);
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.25);
  background: rgba(255, 255, 255, 0.15);
  border-color: rgba(255, 255, 255, 0.3);
}

.column-header {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 20px;
  padding-bottom: 15px;
  border-bottom: 2px solid rgba(255, 255, 255, 0.2);
}

.column-icon {
  font-size: 1.5rem;
}

.column-header h3 {
  margin: 0;
  color: white;
  font-size: 1.1em;
  font-weight: 600;
  flex: 1;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
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
  background: rgba(255, 255, 255, 0.05);
}

.draggable-container.dragging {
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.2), rgba(255, 255, 255, 0.1));
  border: 2px dashed rgba(255, 255, 255, 0.4);
}

.todo-item {
  background: rgba(255, 255, 255, 0.15);
  padding: 15px;
  border-radius: 12px;
  margin-bottom: 12px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
  cursor: pointer;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.2);
  position: relative;
  overflow: hidden;
  backdrop-filter: blur(5px);
}

.todo-item::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 3px;
  background: linear-gradient(45deg, #ff6b6b, #ee5a24);
  transform: scaleX(0);
  transition: transform 0.3s ease;
}

.todo-item:hover::before {
  transform: scaleX(1);
}

.todo-item:hover {
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
  transform: translateY(-3px);
  background: rgba(255, 255, 255, 0.2);
  border-color: rgba(255, 255, 255, 0.3);
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
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0.05));
  border-color: rgba(255, 255, 255, 0.1);
}

.todo-item.completed::before {
  background: linear-gradient(45deg, #28a745, #20c997);
}

.task-content {
  width: 100%;
}

.task-text {
  display: block;
  width: 100%;
  min-width: 0;
  white-space: pre-wrap;
  word-break: break-all;
  line-height: 1.5;
  font-size: 14px;
  color: white;
  transition: all 0.3s ease;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.3);
  margin-bottom: 8px;
}

.task-text.completed {
  text-decoration: line-through;
  color: rgba(255, 255, 255, 0.6);
  font-style: italic;
  opacity: 0.7;
}

.task-meta {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 12px;
}

.assignee {
  color: rgba(255, 255, 255, 0.8);
  background: rgba(255, 255, 255, 0.1);
  padding: 2px 6px;
  border-radius: 8px;
  font-size: 11px;
}

.memo-indicator {
  color: rgba(255, 255, 255, 0.8);
  font-size: 12px;
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
  background-color: rgba(255, 255, 255, 0.2);
  border: 2px solid rgba(255, 255, 255, 0.4);
  border-radius: 4px;
  position: relative;
  flex-shrink: 0;
  transition: all 0.3s ease;
}

.checkbox:checked ~ .checkmark {
  background: linear-gradient(45deg, #ff6b6b, #ee5a24);
  border-color: #ff6b6b;
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

/* Modal Styles */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
  backdrop-filter: blur(5px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

.modal-content {
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20px;
  padding: 0;
  width: 90%;
  max-width: 500px;
  max-height: 80vh;
  overflow-y: auto;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.3);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  animation: slideIn 0.3s ease;
}

@keyframes slideIn {
  from { 
    transform: translateY(-50px) scale(0.9);
    opacity: 0;
  }
  to { 
    transform: translateY(0) scale(1);
    opacity: 1;
  }
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 25px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
  border-radius: 20px 20px 0 0;
}

.modal-header h2 {
  margin: 0;
  font-size: 1.5rem;
  font-weight: 600;
}

.close-btn {
  background: none;
  border: none;
  color: white;
  font-size: 24px;
  cursor: pointer;
  padding: 0;
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  transition: all 0.3s ease;
}

.close-btn:hover {
  background: rgba(255, 255, 255, 0.2);
}

.modal-body {
  padding: 25px;
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: 600;
  color: #333;
  font-size: 14px;
}

.form-input, .form-textarea {
  width: 100%;
  padding: 12px 15px;
  border: 2px solid #e1e5e9;
  border-radius: 8px;
  font-size: 14px;
  transition: all 0.3s ease;
  background: white;
}

.form-input:focus, .form-textarea:focus {
  outline: none;
  border-color: #667eea;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.form-textarea {
  resize: vertical;
  min-height: 100px;
  font-family: inherit;
}

.checkbox-label {
  display: flex !important;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  font-weight: 500;
}

.checkbox-label input[type="checkbox"] {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

.checkbox-label .checkmark {
  height: 18px;
  width: 18px;
  background-color: #f8f9fa;
  border: 2px solid #dee2e6;
  border-radius: 4px;
  position: relative;
  transition: all 0.3s ease;
}

.checkbox-label input:checked ~ .checkmark {
  background: linear-gradient(45deg, #28a745, #20c997);
  border-color: #28a745;
}

.checkbox-label .checkmark:after {
  content: "";
  position: absolute;
  display: none;
  left: 5px;
  top: 2px;
  width: 4px;
  height: 8px;
  border: solid white;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
}

.checkbox-label input:checked ~ .checkmark:after {
  display: block;
}

.modal-footer {
  display: flex;
  justify-content: flex-end;
  gap: 12px;
  padding: 20px 25px;
  border-top: 1px solid rgba(0, 0, 0, 0.1);
  background: #f8f9fa;
  border-radius: 0 0 20px 20px;
}

.btn-secondary, .btn-primary {
  padding: 10px 20px;
  border: none;
  border-radius: 8px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-secondary {
  background: #6c757d;
  color: white;
}

.btn-secondary:hover {
  background: #5a6268;
  transform: translateY(-1px);
}

.btn-primary {
  background: linear-gradient(45deg, #667eea, #764ba2);
  color: white;
}

.btn-primary:hover {
  transform: translateY(-1px);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.3);
}

/* ドラッグ中のスタイル */
.sortable-ghost {
  opacity: 0.3;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.3), rgba(255, 255, 255, 0.1));
  transform: scale(0.95);
}

.sortable-chosen {
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.2), rgba(255, 255, 255, 0.1));
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
  
  .modal-content {
    width: 95%;
    margin: 20px;
  }
}

.status-toggle {
  display: flex;
  gap: 10px;
  margin-top: 8px;
}

.status-btn {
  flex: 1;
  padding: 12px 16px;
  border: 2px solid #e1e5e9;
  border-radius: 8px;
  background: white;
  color: #6c757d;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.status-btn:hover {
  border-color: #667eea;
  color: #667eea;
  transform: translateY(-1px);
}

.status-btn.active {
  background: linear-gradient(45deg, #667eea, #764ba2);
  border-color: #667eea;
  color: white;
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.3);
}

.status-btn.active:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(102, 126, 234, 0.4);
}

.status-icon {
  font-size: 16px;
}
</style>
