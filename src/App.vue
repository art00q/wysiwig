<template>
  <div id="app">
    <TextEditor />
  </div>
</template>

<script>
import TextEditor from './components/TextEditor.vue';

export default {
  name: 'App',
  components: {
    TextEditor
  }
};
</script>

<style scoped>
/* Локальные стили для компонента App, если необходимо */
</style>
