<template>
  <h2 class="text-2xl font-bold mb-4">Todo List</h2>
  <div class="bg-white shadow-md rounded-lg p-4">
    <ul>
      <li v-for="todo in todos" :key="todo.id" class="flex justify-between items-center mb-2 p-2 border-b border-gray-200">
        <div class="flex-grow">
          <h3 class="font-semibold text-lg">{{ todo.title }}</h3>
          <p class="text-gray-600">{{ todo.description }}</p>
          <div class="flex items-center">
            <p class="text-gray-600 mr-2">Date: {{ todo.date }}</p>
            <p class="text-gray-600 mr-2">Time: {{ todo.time }}</p>
            <p class="text-gray-600">Place: {{ todo.place }}</p>
          </div>
        </div>
        <div class="flex items-center">
          <button @click="$emit('editTodo', todo)" class="mr-2 px-3 py-1 bg-blue-500 text-white font-semibold rounded-lg">Edit</button>
          <button @click="$emit('deleteTodo', todo.id)" class="text-red-500 px-3 py-1 font-semibold rounded-lg">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';

interface Todo {
  id: number;
  title: string;
  description: string;
  date: string;
  time: string;
  place: string;
}

export default defineComponent({
  props: {
    todos: {
      type: Array as PropType<Todo[]>,
      required: true
    }
  }
});
</script>
