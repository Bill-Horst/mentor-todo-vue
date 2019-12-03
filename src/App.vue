<template>
  <md-card>
    <md-field>
      <md-input v-model="currentTodo" @keydown.enter="addTodo()" placeholder="Add a todo"></md-input>
    </md-field>
    <ul class="todos">
      <li v-for="todo in todos" :key="todo.id">
        <span style="display: inline-block;">
          <input
            class="completed-checkbox"
            type="checkbox"
            name="todo.completed"
            value="todo.completed"
            @click="toggleCompleted(todo)"
          />
        </span>
        <span>
          <p v-if="!todo.isEditing" @dblclick="editTodo(todo)">{{ todo.label }}</p>
        </span>
        <span>
          <div v-if="todo.isEditing">
            <md-field>
              <md-input v-model="todoEditValue" @keydown.enter="finishedEditing(todo)"></md-input>
            </md-field>
          </div>
        </span>
        <span class="delete-todo-button" @click="removeTodo(todo)" style="margin-top: 20px;"><md-icon>close</md-icon></span>
      </li>
    </ul>
    <md-button class="md-primary" @click="removeAllTodos()">Remove all</md-button>
    <md-button class="md-primary" @click="removeCompletedTodos()">Remove completed</md-button>
  </md-card>
</template>


<script>
let atomicCounterID = 0; // added this because if an id is generated based on an array's index, there will be duplicates
export default {
  data() {
    return {
      todos: [],
      currentTodo: "",
      todoEditValue: ""
    };
  },
  methods: {
    addTodo() {
      if (this.currentTodo.trim() !== "") {
        this.todos.push({
          id: atomicCounterID++,
          label: this.currentTodo,
          completed: false,
          isEditing: false
        });
        this.currentTodo = "";
      }
    },
    removeTodo(todo) {
      let idx = this.todos.indexOf(todo);
      this.todos.splice(idx, 1);
    },
    editTodo(todo) {
      this.todoEditValue = todo.label;
      let idx = this.todos.indexOf(todo);
      this.todos[idx].isEditing = true;
    },
    finishedEditing(todo) {
      let idx = this.todos.indexOf(todo);
      this.todos[idx].label = this.todoEditValue;
      this.todos.map(todo => (todo.isEditing = false));
      this.todoEditValue = "";
    },
    removeAllTodos() {
      this.todos = [];
    },
    toggleCompleted(todo) {
      let idx = this.todos.indexOf(todo);
      this.todos[idx].completed = !this.todos[idx].completed;
    },
    removeCompletedTodos() {
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  }
};
</script>


<style>
.md-card {
  padding: 20px;
  margin: 20px;
}
.todos {
  list-style-type: none;
}
.todos > li {
  background-color: rgb(255, 153, 255);
  padding: 20px;
  border: 1px solid black;
  border-radius: 10px;
}
.todos p {
  font-family: cursive;
  text-decoration: underline;
  font-size: 2em;
  cursor: pointer;
}
.delete-todo-button {
  float: right;
  margin: 10px;
  cursor: pointer;
}
li > span {
  display: inline-block;
}
.completed-checkbox {
  zoom: 1;
  transform: scale(2);
  margin-right: 15px;
}
</style>
