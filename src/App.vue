<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')

const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a,b) =>{
  return b.createdAt - a.createdAt
}))

const addTodo = () => {
    if (input_content.value.trim() === '' || input_category.value === null) {
        return    
    }

    todos.value.push({
      content: input_content.value,
      category: input_category.value,
      done: false,
      createdAt: new Date().getTime()
    })

    input_category.value = null
    input_content.value = ''
}

const removeTodo = todo => {
    todos.value = todos.value.filter(t => t !== todo)
}

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })

onMounted(() => {
  name.value = localStorage.getItem('name')
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

</script>



<template>

    <main class="app">

        <section class="greeting">
          <h2 class="title">
            TODO list name: <input type="text" placeholder="Name here" 
            v-model="name">
          </h2>
        </section>

        <section class="create-todo">

          
              <form @submit.prevent="addTodo">
                  <h4>what's on your todo list today?</h4>
                  <input 
                      type="text" 
                      placeholder="e.g. make a video"
                      v-model="input_content" />
                  
                    <h4>pick a category</h4>
                  
                    <div class="options">
                      
                      <label>
                          <input 
                              type="radio"
                              name="category"
                              value="business"
                              v-model="input_category"/>
                          <span class="bubble business"></span>
                          <div>business</div>
                      </label>
                      
                      <label>
                          <input 
                              type="radio"
                              name="category"
                              value="personal"
                              v-model="input_category"/>
                          <span class="bubble personal"></span>
                          <div>personal</div>
                      </label>
                      
                    </div>

                    <input type="submit" value="Add todo"/>
              </form>
        </section>

        <section class="todo-list">
            <h3>TODO list</h3>
            <div class="list">
                <div v-for="todo in todos_asc" :class="`todo-item ${todo.done &&
                  'done'}`">

                    <label>
                      <input type="checkbox" v-model="todo.done"/>
                      <span :class="`bubble ${todo.category}`"></span>
                    </label>

                    <div class="todo-content">
                        <input type="text" v-model="todo.content"/>
                    </div>

                    <div class="actions">
                      <button class="delete" @click="removeTodo(todo)">Delete</button>
                    </div>

                </div>
            </div>
        </section>


    </main>

</template>
