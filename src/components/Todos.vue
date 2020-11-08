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
    markComplete(id_) {
      
      const todo = this.todos.filter(e => e.id === id_ )[0];
      todo.completed = !todo.completed; 
      
      const { id, title, completed } = todo;
      axios.post('http://127.0.0.1:8001/todo/api/', {
        id,
        title, 
        completed
      })
      .catch(err => console.log(err));
    },
    deleteTodo(id) {
      axios.delete(`http://127.0.0.1:8001/todo/api/?id=${id}`)
      .then(res => {
         this.todos = this.todos.filter(e => e.id !== id)
      })
      .catch(err => console.log(err));
    },
    addTodo(newTodo) {
      const { title, completed } = newTodo;
      console.log(title);
      axios.post('http://127.0.0.1:8001/todo/api/', {
        title, 
        completed
      })
      .then(res => {
        this.todos = [...this.todos, res.data]
      })
      .catch(err => console.log(err));
      
    }
  },
  created() {
    axios.get('http://127.0.0.1:8001/todo/api/')
      .then(res => {
        this.todos = res.data
      }) 
      .catch(err => console.log(err));
  }
}
</script>

<style scoped>

</style>