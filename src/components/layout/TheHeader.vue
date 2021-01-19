<template>

  <!-- A header component for all the App -->
  <header>
    <h1> {{ title }} </h1>
  </header>

</template>

<script>
export default {
  // get the value of title -- from App.vue 
  props: [ 'title', ],
}
</script>

<style scoped>
header {
  width: 100%;
  height: 5rem;
  background-color: #640032;
  display: flex;
  justify-content: center;
  align-items: center;
}

header h1 {
  color: white;
  margin: 0;
}
</style>