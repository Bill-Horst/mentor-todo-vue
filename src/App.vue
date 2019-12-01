<template>
  <div>
    <md-field>
      <md-input v-model="currentTodo" @keydown.enter="addTodo()" placeholder="Add a todo"></md-input>
    </md-field>
    <ul class="todos">
      <li v-for="todo in todos" :key="todo.id">{{ todo.label }}</li>
    </ul>
  </div>
</template>


<script>
export default {
  data() {
    return {
      todos: [],
      currentTodo: ""
    };
  },
  methods: {
    addTodo() {
      this.todos.push({
        id: this.todos.length,
        label: this.currentTodo,
        completed: false
      });
      this.currentTodo = "";
    }
  }
};
</script>


<style>
</style>
