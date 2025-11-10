<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import { Plus, Trash2, Check, X, Edit2, Save } from 'lucide-vue-next'

const todos = ref([])
const inputValue = ref('')
const filter = ref('all')
const editingId = ref(null)
const editValue = ref('')

// Load todos from localStorage on mount
onMounted(() => {
  const saved = localStorage.getItem('todos')
  if (saved) {
    try {
      todos.value = JSON.parse(saved)
    } catch (e) {
      console.error('Failed to parse todos')
    }
  }
})

// Save todos to localStorage whenever they change
watch(todos, (newTodos) => {
  localStorage.setItem('todos', JSON.stringify(newTodos))
}, { deep: true })

const addTodo = () => {
  if (inputValue.value.trim()) {
    const newTodo = {
      id: Date.now(),
      text: inputValue.value.trim(),
      completed: false,
      createdAt: new Date().toISOString()
    }
    todos.value.unshift(newTodo)
    inputValue.value = ''
  }
}

const toggleTodo = (id) => {
  const todo = todos.value.find(t => t.id === id)
  if (todo) {
    todo.completed = !todo.completed
  }
}

const deleteTodo = (id) => {
  todos.value = todos.value.filter(todo => todo.id !== id)
}

const startEdit = (id, text) => {
  editingId.value = id
  editValue.value = text
}

const saveEdit = (id) => {
  if (editValue.value.trim()) {
    const todo = todos.value.find(t => t.id === id)
    if (todo) {
      todo.text = editValue.value.trim()
    }
    editingId.value = null
    editValue.value = ''
  }
}

const cancelEdit = () => {
  editingId.value = null
  editValue.value = ''
}

const clearCompleted = () => {
  todos.value = todos.value.filter(todo => !todo.completed)
}

const filteredTodos = computed(() => {
  if (filter.value === 'active') return todos.value.filter(t => !t.completed)
  if (filter.value === 'completed') return todos.value.filter(t => t.completed)
  return todos.value
})

const stats = computed(() => ({
  total: todos.value.length,
  active: todos.value.filter(t => !t.completed).length,
  completed: todos.value.filter(t => t.completed).length
}))
</script>

<template>
  <div class="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100">
    <div class="max-w-3xl mx-auto p-6">
      <!-- Header -->
      <div class="text-center mb-8 pt-8">
        <h1 class="text-5xl font-bold text-indigo-900 mb-2">Todo List</h1>
        <p class="text-indigo-600">Stay organized and productive</p>
      </div>

      <!-- Stats -->
      <div class="grid grid-cols-3 gap-4 mb-6">
        <div class="bg-white rounded-lg shadow-md p-4 text-center">
          <div class="text-3xl font-bold text-indigo-600">{{ stats.total }}</div>
          <div class="text-sm text-gray-600">Total</div>
        </div>
        <div class="bg-white rounded-lg shadow-md p-4 text-center">
          <div class="text-3xl font-bold text-amber-600">{{ stats.active }}</div>
          <div class="text-sm text-gray-600">Active</div>
        </div>
        <div class="bg-white rounded-lg shadow-md p-4 text-center">
          <div class="text-3xl font-bold text-green-600">{{ stats.completed }}</div>
          <div class="text-sm text-gray-600">Completed</div>
        </div>
      </div>

      <!-- Input -->
      <div class="bg-white rounded-lg shadow-lg p-4 mb-6">
        <div class="flex gap-2">
          <input
            v-model="inputValue"
            type="text"
            @keyup.enter="addTodo"
            placeholder="What needs to be done?"
            class="flex-1 px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent text-gray-900"
          />
          <button
            @click="addTodo"
            class="bg-red-700 hover:bg-red-700 text-white px-6 py-3 rounded-lg flex items-center gap-2 transition-colors"
          >
            <Plus :size="20" />
            Add
          </button>
        </div>
      </div>

      <!-- Filters -->
      <div class="bg-white rounded-lg shadow-lg p-4 mb-6">
        <div class="flex gap-2 justify-center">
          <button
            v-for="f in ['all', 'active', 'completed']"
            :key="f"
            @click="filter = f"
            :class="[
              'px-6 py-2 rounded-lg font-medium transition-colors',
              filter === f
                ? 'bg-indigo-600 text-white'
                : 'bg-gray-100 text-gray-700 hover:bg-gray-200'
            ]"
          >
            {{ f.charAt(0).toUpperCase() + f.slice(1) }}
          </button>
        </div>
        <button
          v-if="stats.completed > 0"
          @click="clearCompleted"
          class="w-full mt-3 px-4 py-2 bg-red-50 text-red-600 rounded-lg hover:bg-red-100 transition-colors text-sm font-medium"
        >
          Clear Completed ({{ stats.completed }})
        </button>
      </div>

      <!-- Todo List -->
      <div class="space-y-3">
        <div
          v-if="filteredTodos.length === 0"
          class="bg-white rounded-lg shadow-md p-8 text-center text-gray-500"
        >
          <span v-if="filter === 'all'">No todos yet. Add one above!</span>
          <span v-if="filter === 'active'">No active todos!</span>
          <span v-if="filter === 'completed'">No completed todos!</span>
        </div>

        <div
          v-for="todo in filteredTodos"
          :key="todo.id"
          :class="[
            'bg-white rounded-lg shadow-md p-4 transition-all hover:shadow-lg',
            todo.completed ? 'opacity-75' : ''
          ]"
        >
          <!-- Edit Mode -->
          <div v-if="editingId === todo.id" class="flex gap-2">
            <input
              v-model="editValue"
              type="text"
              @keyup.enter="saveEdit(todo.id)"
              class="flex-1 px-3 py-2 border border-gray-300 rounded focus:outline-none focus:ring-2 focus:ring-indigo-500 text-gray-900"
              autofocus
            />
            <button
              @click="saveEdit(todo.id)"
              class="p-2 bg-green-600 text-white rounded hover:bg-green-700 transition-colors"
            >
              <Save :size="20" />
            </button>
            <button
              @click="cancelEdit"
              class="p-2 bg-gray-600 text-white rounded hover:bg-gray-700 transition-colors"
            >
              <X :size="20" />
            </button>
          </div>

          <!-- View Mode -->
          <div v-else class="flex items-center gap-3">
            <button
              @click="toggleTodo(todo.id)"
              :class="[
                'flex-shrink-0 w-6 h-6 rounded border-2 flex items-center justify-center transition-colors',
                todo.completed
                  ? 'bg-green-600 border-green-600'
                  : 'border-gray-300 hover:border-indigo-500'
              ]"
            >
              <Check v-if="todo.completed" :size="16" class="text-white" />
            </button>
            <span
              :class="[
                'flex-1',
                todo.completed ? 'line-through text-gray-500' : 'text-gray-800'
              ]"
            >
              {{ todo.text }}
            </span>
            <button
              @click="startEdit(todo.id, todo.text)"
              class="p-2 text-indigo-600 hover:bg-indigo-50 rounded transition-colors"
            >
              <Edit2 :size="18" />
            </button>
            <button
              @click="deleteTodo(todo.id)"
              class="p-2 text-red-600 hover:bg-red-50 rounded transition-colors"
            >
              <Trash2 :size="18" />
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
