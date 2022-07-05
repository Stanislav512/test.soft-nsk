<template>
  <div>
    <AddTodo
        @add-todo="addTodo"
    />
    <select v-model="filter">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="not-completed">Not Completed</option>
    </select>
    <hr>
    <Loader v-if="loading" />
    <TodoList
        v-else-if="filteredTodos.length"
        v-bind:todos="filteredTodos"
        @remove-todo="removeTodo"
    />
    <p v-else>No todos!</p>
  </div>
</template>

<script>
import TodoList from '@/components/TodoList'
import AddTodo from '@/components/AddTodo'
import Loader from '@/components/Loader'
export default {
  name: 'app',
  data() {
    return {
      todos: [
        {id: 1, title: 'Post 1', text:'text 1', completed: false, rating: 1},
        {id: 2, title: 'Post 2', text:'text 2', completed: false, rating: 5},
        {id: 3, title: 'Post 3', text:'text 3', completed: false, rating: 10}
      ],
      loading: true,
      filter: 'all'
    }
  },
  mounted() {
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=15')
      .then(response => response.json())
      .then(json => {
        setTimeout(() => {
          this.todos = json
          this.loading = false
        }, 1000)

      })
  },
  // watch: {
  //   filter(value) {
  //     console.log(value)
  //   }
  // },
  computed: {
    filteredTodos() {
      if (this.filter === 'all') {
        return this.todos
      }

      if (this.filter === 'completed') {
        return this.todos.filter(t => t.completed)
      }

      if (this.filter === 'not-completed') {
        return this.todos.filter(t => !t.completed)
      }
    }
  },
  methods: {
    removeTodo(id) {
      this.todos = this.todos.filter(t => t.id !== id)
    },
    addTodo(todo) {
      this.todos.push(todo)
    }
  },
  components: {
    TodoList, AddTodo, Loader
  }
}
</script>