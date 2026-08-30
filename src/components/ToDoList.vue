<script setup>
import { ref, computed } from 'vue'

const todos = ref([])
const text = ref('')

function Submit() {
  if (text.value.trim()) {
    todos.value.push({
      id: Date.now(), 
      text: text.value,
      done: false
    })
    text.value = ''
  }
}

function Remove(id) {
  todos.value = todos.value.filter(todo => todo.id !== id)
}

function Update(id) {
  const targetTodo = todos.value.find(todo => todo.id === id)
  if (targetTodo) {
    const newText = prompt('Update :', targetTodo.text)
    if (newText !== null && newText.trim() !== '') {
      targetTodo.text = newText.trim()
    }
  }
}

function Tick(id) {
  todos.value = todos.value.map(todo => 
    todo.id === id ? { ...todo, done: !todo.done } : todo
  )
}

const remaining1 = computed(() =>
  todos.value.filter(todo => todo.done).length
)
const remaining2 = computed(() =>
  todos.value.filter(todo => !todo.done).length
)
</script>

<template>
  <div class="todo-container">
    <h2>TodoList</h2>
    
    <form class="todo-form" @submit.prevent="Submit">
      <input id="input-text" type="text" v-model="text" />
      <button id="submit" class="btn btn-primary" type="submit">Add</button>
    </form>

    <ul class="todo-list">
      <li v-for="(todo, index) in todos" :key="todo.id">
        <div class="todo-content">
          <input type="checkbox" :checked="todo.done" @change="Tick(todo.id)" />
            {{ index + 1 }}. {{ todo.text }}
        </div>

        <div class="todo-actions">
          <button class="btn btn-primary" @click="Update(todo.id)">Update</button>
          <button class="btn btn-primary" @click="Remove(todo.id)">Remove</button>
        </div>
      </li>
    </ul>

    <div>
      <p><strong>เสร็จแล้ว:</strong> {{ remaining1 }}</p>
      <p><strong>ยังไม่เสร็จ:</strong> {{ remaining2 }}</p>
    </div>
  </div>
</template>

<style scoped>

.todo-container {
  max-width: 500px;
  margin: 40px auto;
  padding: 30px;
  border-radius: 8px;
  font-family: sans-serif;
  color: #ffffff;
}

h2 {
  text-align: center;
  margin-top: 0;
  padding-bottom: 15px;
}

.todo-form {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}

.btn {
  padding: 5px 16px;
  border-radius: 4px;
  font-size: 14px;
}

.btn-primary {
  background-color: #ffffff;
  color: #1a1a1a;
  border: 2px solid #ffffff;
}

.todo-list {
  list-style: none;
  padding: 0;
  margin: 0 0 25px 0;
}

.todo-content {
  display: flex;
  align-items: center;
  gap: 10px;
}

.todo-actions {
  display: flex;
  gap: 5px;
}
</style>