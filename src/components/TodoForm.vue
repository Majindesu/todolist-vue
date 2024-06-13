<template>
  <form @submit.prevent="handleSubmit" class="flex flex-col space-y-4">
    <input v-model="todo.title" placeholder="Title" required class="p-2 border rounded-md" />
    <textarea v-model="todo.description" placeholder="Description" class="p-2 border rounded-md"></textarea>
    <div class="flex space-x-2">
      <input type="date" v-model="todo.date" class="p-2 border rounded-md" />
      <input type="time" v-model="todo.time" class="p-2 border rounded-md" />
    </div>
    <input v-model="todo.place" placeholder="Place" class="p-2 border rounded-md" />
    <button type="submit" class="bg-blue-500 hover:bg-blue-700 text-white px-4 py-2 rounded">{{ isEditMode ? 'Update' : 'Add' }} Todo</button>
  </form>
</template>

<script lang="ts">
import { defineComponent, ref, watch, PropType } from 'vue';

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
      default: () => ({ title: '', description: '', date: '', time: '', place: '' })
    },
    isEditMode: {
      type: Boolean,
      default: false
    }
  },
  emits: ['saveTodo'],
  setup(props, { emit }) {
    const todo = ref<Todo>({ ...props.todo });

    watch(props, (newProps) => {
      todo.value = { ...newProps.todo };
    });

    const handleSubmit = () => {
      emit('saveTodo', todo.value);
    };

    return { todo, handleSubmit };
  }
});
</script>
