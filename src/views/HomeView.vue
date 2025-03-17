<template>
  <h1>TODO</h1>

  <div class="todo-list">
    <div class="todo-input">
      <input v-model="newTodoDescription" placeholder="Enter new todo" />
      <button @click="createTodo">Add todo</button>
    </div>
    <div class="todo-container">
      <div v-for="todo in todos" :key="todo.id" class="todo-item">
        <template v-if="editingTodoId === todo.id">
          <input v-model="editableTodo.description" />
          <input type="checkbox" v-model="editableTodo.completed" />
        </template>
        <template v-else>
          <span>{{ todo.id }} : {{ todo.description }}</span>
          <span :class="{ done: todo.completed, 'not-done': !todo.completed }">
            {{ todo.completed ? 'Done' : 'Not Done' }}
          </span>
        </template>
        <button
          style="margin-left: auto"
          @click="editingTodoId === todo.id ? saveTodo(todo.id) : editTodo(todo)"
        >
          {{ editingTodoId === todo.id ? 'Save' : 'Edit' }}
        </button>
        <button @click="deleteTodo(todo.id)">Delete</button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue'
import axios from 'axios'

interface Todo {
  id: number
  description: string
  completed: boolean
}

export default defineComponent({
  setup() {
    const todos = ref<Todo[]>([])
    const apiUrl = 'https://todo-api-606352320440.asia-southeast1.run.app/api/todos'
    const editingTodoId = ref<number | null>(null)
    const editableTodo = ref<{ description: string; completed: boolean }>({
      description: '',
      completed: false,
    })
    const newTodoDescription = ref<string>('')

    const fetchTodos = async () => {
      try {
        const response = await axios.get(apiUrl)
        todos.value = response.data.data
      } catch (error) {
        console.error('Error fetching todos:', error)
      }
    }

    const editTodo = (todo: Todo) => {
      editingTodoId.value = todo.id
      editableTodo.value = { description: todo.description, completed: todo.completed }
    }

    const saveTodo = async (id: number) => {
      try {
        await axios.put(`${apiUrl}/${id}`, editableTodo.value)
        editingTodoId.value = null
        fetchTodos()
      } catch (error) {
        console.error('Error updating todo:', error)
      }
    }

    const deleteTodo = async (id: number) => {
      try {
        await axios.delete(`${apiUrl}/${id}`)
        fetchTodos()
      } catch (error) {
        console.error('Error deleting todo:', error)
      }
    }

    const createTodo = async () => {
      if (!newTodoDescription.value.trim()) return
      try {
        await axios.post(apiUrl, { description: newTodoDescription.value })
        newTodoDescription.value = ''
        fetchTodos()
      } catch (error) {
        console.error('Error creating todo:', error)
      }
    }

    onMounted(fetchTodos)

    return {
      todos,
      editingTodoId,
      editableTodo,
      newTodoDescription,
      editTodo,
      saveTodo,
      deleteTodo,
      createTodo,
    }
  },
})
</script>

<style scoped>
.todo-list {
  margin: auto;
}
.todo-input {
  display: flex;
  margin-bottom: 10px;
}
.todo-input input {
  flex: 1;
  padding: 5px;
  margin-right: 5px;
}
.todo-container {
  overflow-y: auto;
  padding: 10px;
}
.todo-item {
  display: flex;
  align-items: center;
  padding: 8px;
  border: 1px solid #ccc;
  margin: 5px 0;
  border-radius: 4px;
  background-color: #222;
  color: white;
}
.done {
  color: green;
}
.not-done {
  color: red;
}
button {
  margin-left: 5px;
  width: 50px;
  height: 30px;
}
</style>
