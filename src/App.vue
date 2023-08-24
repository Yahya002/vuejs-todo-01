<template>
  <header class="header">Today</header>
  <List />
</template>

<script>
import List from './components/List.vue'

export default {
  name: 'App',
  components: {
    List,
  }
}
</script>

<style>

.header{
  margin: 1em auto;
  max-width: 90%;
  text-align: center;

  border-radius: 2px;
  color:white;
  background-color: #8e81f3;
}

body {
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  background-color: #b9caf9;
}


</style>
