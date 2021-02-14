<template>
  <div id="app">
    <h1 class="title">DJVue</h1>
    <hr class="ts">
    <div class="container mt-5">
      <TaskComp/>
    </div>
  </div>
</template>

<script>
import TaskComp from './components/TaskComp'
export default {
  name: 'App',
  components: {
    TaskComp
  },
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;800&display=swap');
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
$primary-blue: #010148;
$primary-yellow: rgb(255, 207, 87);

#app {
  font-family: 'Poppins', sans-serif;
  .title {
    font-weight: 800;
    color: rgb(255, 207, 87);
    -webkit-text-stroke: 1px black;
    letter-spacing: 4px;
    margin: 10px 20px 10px 30px;
    font-size: 54px;
  }

}
</style>
