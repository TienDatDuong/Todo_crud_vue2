<template>
  <div id="app">
    <TodoList />
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue'
export default {
  name: 'App',
  components: {
    TodoList,
  },

}
</script>

<style>
/*html{*/
/*  background-image: linear-gradient(to right, #3f87a6, #ebf8e1);*/
/*}*/
</style>
