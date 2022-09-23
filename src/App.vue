<script setup>
  // This is an example of component registration
  import { ref, onMounted, computed, watch } from 'vue'
  const todos = ref([])
  const name = ref('')
  const input_content = ref('')
  const input_category = ref(null)
  const todos_asc = computed(() => todos.value.sort((a,b) =>{
    return a.createdAt - b.createdAt
  }))

  watch(name, (newVal) => {
    localStorage.setItem('name', newVal)
  })
  watch(todos, (newVal) => {
    localStorage.setItem('todos', JSON.stringify(newVal))
  }, {
    deep: true
  })
  const addTodo = () => {
    if (input_content.value.trim() === '' || input_category.value === null) {
      return
    }
    todos.value.push({
      content: input_content.value,
      category: input_category.value,
      done: false,
      editable: false,
      createdAt: new Date().getTime()
    })
  }
  const removeTodo = (todo) => {
    todos.value = todos.value.filter((t) => t !== todo)
  }
 onMounted(() => {
    name.value = localStorage.getItem('name') || ''
    todos.value = JSON.parse(localStorage.getItem('todos')) || []
  })
  </script>
  
  <template>
    <main class="app">
    <!-- This is an example of Form input Bindings -->
      <section class="greeting">
        <h2 class="title">
          Hi! <input type="text" id="name" placeholder="Add your name" v-model="name"> Welcome back to Brain Dump
        </h2>
      </section>
    
      <section class="create-todo">
        <h3>Add a Brain Dump</h3>

        <form id="new-todo-form" @submit.prevent="addTodo">
          <h4>What do you need to get done?</h4>
          <input 
            type="text" 
            name="content" 
            id="content" 
            placeholder="e.g. Finish SIT120 Assignment"
            v-model="input_content" />
          
          <h4>Choose priority</h4>
          <div class="options">
  
            <label>
              <input 
                type="radio" 
                name="category" 
                id="category1" 
                value="High"
                v-model="input_category" />
              <span class="bubble high"></span>
              <div>High</div>
            </label>
  
            <label>
              <input 
                type="radio" 
                name="category" 
                id="category2" 
                value="Low"
                v-model="input_category" />
              <span class="bubble low"></span>
              <div>Low</div>
            </label>
  
          </div>
   <!-- Allows for reminders to be submitted efficiently -->
          <input type="submit" value="Add Brain Dump" />
        </form>
      </section>
  
      <section class="todo-list">
        <h3>Current Brain Dump's</h3>
        <div class="list" id="todo-list">
  
          <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">

            <!-- This is an example of Computed Properties -->
            <label>
              <input type="checkbox" v-model="todo.done" />
              <span :class="`bubble ${
                todo.category == 'High' 
                  ? 'High' 
                  : 'Low'
              }`"></span>
            </label>
  
            <div class="todo-content">
              <input type="text" v-model="todo.content" />
            </div>

            <!--This is an example of event handling-->
            <div class="actions">
              <button class="delete" @click="removeTodo(todo)">Delete</button>
            </div>

            <button @click="show = !show">Toggle</button>
            <Transition><p v-if="show">Thanks for using Brain Dump</p></Transition>

          </div>

        </div>
      </section>
  
    </main>
  </template>