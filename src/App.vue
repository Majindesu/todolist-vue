<template>
  <div className="w-screen max-w-96 mx-auto">
    <DarkModeToggle />
    <TodoForm @saveTodo="addTodo" />
    <TodoList :todos="todos" @editTodo="editTodo" @deleteTodo="deleteTodo" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watchEffect } from 'vue';
import TodoForm from './components/TodoForm.vue';
import TodoList from './components/TodoList.vue';
import DarkModeToggle from './components/DarkmodeToggle.vue';
import TodoItem from './components/TodoItem.vue';

interface Todo {
  id: number;
  title: string;
  description: string;
  date: string;
  time: string;
  place: string;
}

export default defineComponent({
  components: {
    TodoForm,
    TodoList,
    DarkModeToggle,
    TodoItem
  },
  setup() {
    const todos = ref<Todo[]>(JSON.parse(localStorage.getItem('todos') || '[]'));

    watchEffect(() => {
      localStorage.setItem('todos', JSON.stringify(todos.value));
    });

    const addTodo = (newTodo: Omit<Todo, 'id'>) => {
      todos.value.push({ ...newTodo, id: Date.now() });
    };

    const editTodo = (todo: Todo) => {
      const index = todos.value.findIndex(t => t.id === todo.id);
      if (index !== -1) {
        todos.value[index] = todo;
      }
    };

    const deleteTodo = (id: number) => {
      todos.value = todos.value.filter(todo => todo.id !== id);
    };

    return { todos, addTodo, editTodo, deleteTodo };
  }
});
</script>
