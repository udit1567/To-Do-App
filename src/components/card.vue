<script setup>
import { ref } from 'vue'

const newTask = ref('')
const tasks = ref([])
const editingId = ref(null)
const editedText = ref('')

const addTask = () => {
  if (newTask.value.trim() === '') return

  tasks.value.push({
    id: Date.now(),
    text: newTask.value,
    completed: false
  })

  newTask.value = ''
}

const deleteTask = (id) => {
  tasks.value = tasks.value.filter(task => task.id !== id)
}

const toggleTask = (task) => {
  task.completed = !task.completed
}

const startEdit = (task) => {
  editingId.value = task.id
  editedText.value = task.text
}

const saveEdit = (task) => {
  if (editedText.value.trim() === '') return
  task.text = editedText.value
  editingId.value = null
}

const cancelEdit = () => {
  editingId.value = null
}
</script>

<template>
  <div class="card">
    
    <!-- Add Task Section -->
    <div class="input-section">
      <input 
        type="text" 
        placeholder="Add a new task"
        v-model="newTask"
        @keyup.enter="addTask"
      />
      <button class="add" @click="addTask">Add</button>
    </div>

    <!-- Task List -->
    <div class="task-list">
      <div 
        v-for="task in tasks" 
        :key="task.id"
        class="task-card"
      >

        <!-- Normal View -->
        <template v-if="editingId !== task.id">
          <span 
            :class="{ done: task.completed }"
            @click="toggleTask(task)"
          >
            {{ task.text }}
          </span>

          <div class="actions">
            <button class="update" @click="startEdit(task)">
              Edit
            </button>
            <button class="delete" @click="deleteTask(task.id)">
              Delete
            </button>
          </div>
        </template>

        <!-- Edit View -->
        <template v-else>
          <input 
            type="text" 
            v-model="editedText"
            @keyup.enter="saveEdit(task)"
          />
          <div class="actions">
            <button class="add" @click="saveEdit(task)">
              Save
            </button>
            <button class="delete" @click="cancelEdit">
              Cancel
            </button>
          </div>
        </template>

      </div>
    </div>

  </div>
</template>

<style scoped>
.card {
  border: 2px solid #90f00a;
  border-radius: 10px;
  padding: 20px;
  margin: 40px auto;
  max-width: 500px;
  background-color: #eafbcf;
}

.input-section {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

input {
  padding: 8px;
  border-radius: 6px;
  border: 1px solid #ccc;
  flex: 1;
}

.task-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.task-card {
  background: white;
  padding: 12px;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.actions {
  display: flex;
  gap: 6px;
}

.add {
  background-color: #36f207;
  border: none;
  color: white;
  padding: 6px 10px;
  cursor: pointer;
  border-radius: 8px;
}

.update {
  background-color: #f2a007;
  border: none;
  color: white;
  padding: 6px 10px;
  cursor: pointer;
  border-radius: 8px;
}

.delete {
  background-color: #f22e07;
  border: none;
  color: white;
  padding: 6px 10px;
  cursor: pointer;
  border-radius: 8px;
}

.done {
  text-decoration: line-through;
  color: gray;
}
</style>
