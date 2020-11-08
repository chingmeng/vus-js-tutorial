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

export default {
  name: "Todos",
  components: {
    TodoItem,
    AddTodo
  },
  data() {
    return {
      todos: [
        { id: 1, title: "Todo One", completed: true },
        { id: 2, title: "Todo Two", completed: true },
        { id: 3, title: "Todo Three", completed: false }
      ] 
    }
  },
  methods: {
    markComplete(id) {
      this.todos.forEach(e => console.log(e));
      const todo = this.todos.filter(e => e.id === id )[0];
      todo.completed = !todo.completed; 
    },
    deleteTodo(id) {
      this.todos = this.todos.filter(e => e.id !== id);
    },
    addTodo(newTodo) {
      console.log(newTodo);
      this.todos = [...this.todos, newTodo];
    }
  }
}
</script>

<style scoped>

</style>