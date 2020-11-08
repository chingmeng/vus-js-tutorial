<template>

<div>
  <AddTodo v-on:add-todo="addTodo"/>
  <div v-bind:key="todo.id" v-for="todo in todos">
    <TodoItem v-bind:todo="todo" 
        v-bind:markComplete="markComplete"
        v-on:del-todo="deleteTodo"
    />

  </div>
</div>

</template>

<script>
import TodoItem from './TodoItem.vue';
import AddTodo from './AddTodo.vue';
import axios from 'axios';

export default {
  name: "Todos",
  components: {
    TodoItem,
    AddTodo
  },
  data() {
    return {
      todos: [] 
    }
  },
  methods: {
    markComplete(id) {
      this.todos.forEach(e => console.log(e));
      const todo = this.todos.filter(e => e.id === id )[0];
      todo.completed = !todo.completed; 
    },
    deleteTodo(id) {
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
      .then(res => {
         this.todos = this.todos.filter(e => e.id !== id)
      })
      .catch(err => console.log(err));
    },
    addTodo(newTodo) {
      console.log(newTodo);
      const { title, completed } = newTodo;
      axios.post('https://jsonplaceholder.typicode.com/todos', {
        title, 
        completed
      })
      .then(res => this.todos = [...this.todos, res.data])
      .catch(err => console.log(err));
      
    }
  },
  created() {
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=10')
      .then(res => this.todos = res.data) 
      .catch(err => console.log(err));
  }
}
</script>

<style scoped>

</style>