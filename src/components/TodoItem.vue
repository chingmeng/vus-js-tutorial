<template>
  <div 
    class="todo-item" 
    v-bind:class="{'is-complete':todo.completed}">
      <input type="checkbox" v-on:change="this.markComplete(this.todo.id)">
      <span @click="this.$router.push({ path: `/todo_form/${todo.id}/${todo.title}/${todo.detail}/${todo.completed}`, params: { todo: todo }})">
         &nbsp;{{ todo.title }}
      </span>
      
      <button @click="$emit('del-todo', todo.id)" class="del">x</button>
  </div>  
</template>

<script>
export default {
  name: "TodoItem",
  props: ["todo", "markComplete"]
} 
</script>

<style scoped>
  .todo-item {
    background: #f4f4f4;
    padding: 10px;
    border-bottom: 1px #ccc dotted;
  }

  .is-complete {
    text-decoration: line-through;
  }

  .del {
    background: #ff0000;
    color: #fff;
    border: none;
    padding: 5px 9px;
    border-radius: 50%;
    cursor: pointer;
    float: right;
  }
</style>
