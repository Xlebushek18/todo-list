<script lang="ts">
import { defineComponent } from "vue";
import AppAddTodo from "./components/AppAddTodo.vue";
import AppFIlters from "./components/AppFIlters.vue";
import AppFooter, { Stats } from "./components/AppFooter.vue";
import AppHeader from "./components/AppHeader.vue";
import AppTodoList from "./components/AppTodoList.vue";
import { Todo } from "./types/Todo";
import { Filter } from "./types/filter";

interface State {
  todos: Todo[];
  activeFilter: Filter
}

export default defineComponent({
  components: {
    AppHeader,
    AppFIlters,
    AppTodoList,
    AppAddTodo,
    AppFooter,
  },
  data(): State {
    return {
      todos: [
        { id: 0, text: "Изучить JavaScript-фреймворк Vue", completed: true },
        { id: 1, text: "Изучить язык программирования Typescript", completed: false },
        { id: 2, text: "Найти работу по специальности", completed: false },
      ],
      activeFilter: 'All'
    };
  },
  computed: {
    filterdTodos(): Todo[] {
      switch (this.activeFilter) {
        case 'Active':
          return this.activeTodos
        case 'Done':
          return this.doneTodos
        case 'All':
        default:
          return this.todos
      }
    },
    stats(): Stats {
      return {
        active: this.activeTodos.length,
        done: this.doneTodos.length,
      }
    },
    activeTodos(): Todo[] {
      return this.todos.filter(todo => !todo.completed)
    },
    doneTodos(): Todo[] {
      return this.todos.filter(todo => todo.completed)
    }
  },
  methods: {
    addTodo(todo: Todo) {
      this.todos.push(todo)
    },
    toggleTodo(id: number) {
      const targetTodo = this.todos.find((todo) => todo.id === id);

      if (targetTodo) {
        targetTodo.completed = !targetTodo.completed;
      }
    },
    removeTodo(id: number) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
    },
    setFilter(filter: Filter){
      this.activeFilter = filter
    }
  },
});
</script>

<template>
  <AppHeader />

  <AppFIlters :active-filter="activeFilter" @set-filter="setFilter" />

  <main class="app-main">
    <AppTodoList 
      :todos="filterdTodos" 
      @toggle-todo="toggleTodo" 
      @remove-todo="removeTodo" 
    />

    <AppAddTodo @add-todo="addTodo" />
  </main>

  <AppFooter :stats="stats" />
</template>
