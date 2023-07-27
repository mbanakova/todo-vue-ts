<template>
  <AppHeader />

  <AppFilters :active-filter="activeFilter" @set-filter="setFilter" />

  <main class="app-main">
    <AppTodoList :todos="filteredTodos" @toggle-todo="toggleTodo" @remove-todo="removeTodo" />

    <AppAddTodo @add-todo="addTodo" />
  </main>

  <AppFooter :stats="stats" />
</template>

<script   lang="ts">
import AppHeader from '@/components/AppHeader.vue';
import AppFooter, { Stats } from '@/components/AppFooter.vue';
import AppFilters from '@/components/AppFilters.vue';
import AppTodoList from '@/components/AppTodoList.vue';
import AppAddTodo from '@/components/AppAddTodo.vue';
import { Todo } from './types/Todo';
import { Filter } from './types/Filter';
import { defineComponent } from 'vue';

interface State {
  todos: Todo[],
  activeFilter: Filter
}

export default defineComponent({
  data(): State {
    return {
      todos: [
        { id: 0, isCompleted: true, text: 'Learn the basics of Vue' },
        { id: 1, isCompleted: false, text: 'Learn the basics of Typescript' },
        { id: 2, isCompleted: false, text: 'Subscribe to the channel' },
      ],
      activeFilter: 'All'
    }
  },
  components: { AppHeader, AppFooter, AppFilters, AppTodoList, AppAddTodo },
  computed: {
    filteredTodos(): Todo[] {
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
        done: this.doneTodos.length
      }
    },
    activeTodos(): Todo[] {
      return this.todos.filter(todo => !todo.isCompleted)
    },
    doneTodos(): Todo[] {
      return this.todos.filter(todo => todo.isCompleted)
    }
  },
  methods: {
    addTodo(todo: Todo) {
      this.todos.push(todo)
    },
    toggleTodo(id: number) {
      const targetTodo = this.todos.find((todo: Todo) => todo.id === id)

      if (targetTodo) {
        targetTodo.isCompleted = !targetTodo.isCompleted
      }
    },
    removeTodo(id: number) {
      this.todos = this.todos.filter((todo: Todo) => todo.id !== id)
    },
    setFilter(filter: Filter) {
      this.activeFilter = filter
    }
  },
})

</script>

<style scoped>
</style>
