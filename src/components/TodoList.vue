<template>
  <div>
    <h2>Todos</h2>
    <div v-if="loading">Loading...</div>
    <div v-else-if="error">{{ error }}</div>
    <ul v-else>
      <li v-for="todo in todos" :key="todo.id">
        <h3>{{ todo.title }}</h3>
        <p>Completed: {{ todo.completed ? 'Yes' : 'No' }}</p>
        <button @click="editTodo(todo)">Edit</button>
        <button @click="deleteTodo(todo.id)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

interface Todo {
  id: number;
  title: string;
  completed: boolean;
}

export default defineComponent({
  name: 'TodoList',
  data() {
    return {
      todos: [] as Todo[],
      loading: false,
      error: null as string | null
    };
  },
  mounted() {
    this.fetchTodos();
  },
  methods: {
    fetchTodos() {
      this.loading = true;
      this.$store.dispatch('todos/fetchTodos')
        .then(() => {
          this.todos = this.$store.getters['todos/getTodos'];
        })
        .catch((error: any) => {
          this.error = error.message;
        })
        .finally(() => {
          this.loading = false;
        });
    },
    editTodo(todo: Todo) {
      // Implement edit logic here
      console.log('Edit todo:', todo);
    },
    deleteTodo(todoId: number) {
      this.$store.dispatch('todos/deleteTodo', todoId);
    }
  }
});
</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin: 10px 0;
  padding: 10px;
  border: 1px solid #ccc;
}
button {
  margin-left: 10px;
}
</style> 