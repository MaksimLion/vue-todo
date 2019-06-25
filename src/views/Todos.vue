<template>
  <div>
    <router-link to="/">Home</router-link>
    <Loader v-if="loading"/>
    <TodoList v-bind:todos="filteredTodos" @remove-todo="removeTodo" v-else-if="filteredTodos.length"/>
    <h1 v-else>No items</h1>
    <AddTodo @add-todo="addTodo"/>
    <select v-model="filter">
        <option value="all">All</option>
        <option value="completed">completed</option>
        <option value="not-completed">Not Completed</option>
    </select>
  </div>
</template>

<script>
import TodoList from "@/components/TodoList";
import AddTodo from "@/components/AddTodo";
import Loader from "@/components/Loader";
import { setTimeout } from "timers";

export default {
  name: "app",
  data() {
    return {
      todos: [],
      loading: true,
      filter: 'all'
    };
  },
//   watch: {
//       filter(value) {
//           console.log(value)
//       }
//   }
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

  mounted() {
    fetch("https://jsonplaceholder.typicode.com/todos?_limit=10")
      .then(response => response.json())
      .then(json => {
        setTimeout(() => {
          this.todos = json;
          this.loading = false;
        }, 1000);
      });
  },

  components: {
    TodoList,
    AddTodo,
    Loader
  },
  methods: {
    removeTodo(id) {
      this.todos = this.todos.filter(t => t.id !== id);
    },
    addTodo(todo) {
      this.todos.push(todo);
    }
  }
};
</script>