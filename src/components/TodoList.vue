<template>
    <div id="app" class="flex flex-col justify-center items-center mt-20">
      <div class="flex flex-col items-center gap-y-3">
        <h1 class="text-3xl font-bold text-red-950">Todos</h1>
        <input class="border border-red-950 pl-2 rounded-xl" type="text" v-model="todoName" @keyup.enter="addTodo" aria-label="Add a new Todo" placeholder="Add a new Todo">
      </div>
      <ul class="flex flex-col justify-center items-center mt-10">
        <li class="text-2xl mb-2 text-tblack flex flex-row items-center gap-x-3" v-for="todo of todos" :key="todo.id">
          {{ todo.name }}
          <button class="text-xl hover:text-red-800" @click="deleteTodo(todo.id)">
            <font-awesome-icon icon="fa-solid fa-trash" />
          </button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  import axios from "axios";

  const baseURL = "http://localhost:3001/todos";
  
  export default {
    name: "TodoList",
    data() {
      return {
        todos: []
      };
    },
    async created() {
      try {
        const res = await axios.get(baseURL);
  
        this.todos = res.data;
      } catch (e) {
        console.error(e);
      }
    },
    methods: {
        async addTodo() {
            try {
                const res = await axios.post(baseURL, {name: this.todoName});

                this.todos = [...this.todos, res.data];

                this.todoName = "";
            } catch (e) {
                console.error(e);
            }
        },
        async deleteTodo(todoId) {
            try {
                await axios.delete(`${baseURL}/${todoId}`);

                this.todos = this.todos.filter(todo => todo.id !== todoId);
            } catch (e) {
                console.error(e);
            }
        } 
    }
  };
  </script>
