<template>
  <button @click="toggleDarkMode">
    {{ isDarkMode ? 'Light Mode' : 'Dark Mode' }}
  </button>
</template>

<script lang="ts">
import { defineComponent, ref, watchEffect } from 'vue';

export default defineComponent({
  setup() {
    const isDarkMode = ref(localStorage.getItem('darkMode') === 'true');

    const toggleDarkMode = () => {
      isDarkMode.value = !isDarkMode.value;
      localStorage.setItem('darkMode', isDarkMode.value.toString());
    };

    watchEffect(() => {
      document.documentElement.classList.toggle('dark', isDarkMode.value);
    });

    return { isDarkMode, toggleDarkMode };
  }
});
</script>
