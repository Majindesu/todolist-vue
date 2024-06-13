<template>
  <div class="flex flex-col gap-4 border-b p-4">
    <input class="border border-gray-300 p-2" v-model="localTodo.title" placeholder="Title" />
    <textarea class="border border-gray-300 p-2" v-model="localTodo.description" placeholder="Description"></textarea>
    <div class="flex gap-2">
      <input class="border border-gray-300 p-2" type="date" v-model="localTodo.date" />
      <input class="border border-gray-300 p-2" type="time" v-model="localTodo.time" />
    </div>
    <input class="border border-gray-300 p-2" v-model="localTodo.place" placeholder="Place" />
    <button class="bg-blue-500 text-white p-2 rounded" @click="saveTodo">Save</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, PropType } from 'vue';

interface Todo {
  id?: number;
  title: string;
  description: string;
  date: string;
  time: string;
  place: string;
}

export default defineComponent({
  props: {
    todo: {
      type: Object as PropType<Todo>,
      required: true
    }
  },
  emits: ['updateTodo'],
  setup(props, { emit }) {
    const localTodo = ref<Todo>({ ...props.todo });

    const saveTodo = () => {
      emit('updateTodo', localTodo.value);
      const existingTodos = localStorage.getItem('todos');
      let todos = existingTodos ? JSON.parse(existingTodos) : [];
      const index = todos.findIndex((t: Todo) => t.id === localTodo.value.id);
      if (index !== -1) {
        todos[index] = localTodo.value;
      } else {
        todos.push(localTodo.value);
      }
      localStorage.setItem('todos', JSON.stringify(todos));
    };

    return { localTodo, saveTodo };
  }
});
</script>
