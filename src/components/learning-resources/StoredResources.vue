<template>

  <p v-if="isLoading"> Loading ... </p>

  <!-- Database and app resources is empty OR null -->
  <p v-else-if="!isLoading && ( !resources || resources.length === 0 )"> Resources is empty! Add some resources first! </p>
  
  <!-- Render all the resources -->
  <ul v-else-if="!isLoading && resources && resources.length > 0">

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
  // get data resources -- from TheResources.vue
  inject: [ 'resources' ],
  props: [ 'isLoading' ],
  components: {
    LearningResource,
  },
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