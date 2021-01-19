<template>
  <ul>
    <!-- provide resource data in Learning Resource Component to render it -->
    <learning-resource
    v-for="rsrc in resources"
    :key="rsrc.id"
    :id="rsrc.id"
    :title="rsrc.title"
    :description="rsrc.description"
    :link="rsrc.link"
    :rsrc1="rsrc.id"
    > </learning-resource>
  </ul>
</template>

<script>
import LearningResource from './LearningResource.vue';

export default {
  // get data resources -- from App.vue
  inject: [ 'resources' ],
  components: {
    LearningResource,
  }
}
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
  margin: auto;
  max-width: 40rem;
}
</style>