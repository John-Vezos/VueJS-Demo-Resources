<template>
  <li>
    <!-- Component <base-card> for render each resource -->
    <base-card>
      <div>

        <!-- Header for each resource -->
        <header>
          <h3> {{ title }} </h3>
          <!-- Delete button for each resource .. set mode -->
          <base-button mode="flat" @click="deleteResource(id)"> Delete </base-button>
        </header>

        <!-- Description for this resource -->
        <p> {{ description }} </p>
        
        <nav>
            <!-- Link for each resource -->
            <a :href="link"> View Resource </a>
        </nav>

      </div>
    </base-card>
  </li>
</template>

<script>
export default {
  // get data title, description and link for each resource -- StoredResources.vue Component
  props: [ 'id', 'title', 'description', 'link' ],
  inject: [ 'deleteResource' ],
}
</script>

<style scoped>
li {
  margin: auto;
  max-width: 40rem;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h3 {
  font-size: 1.25rem;
  margin: 0.5rem 0;
}

p {
  margin: 0.5rem 0;
}

a {
  text-decoration: none;
  color: #ce5c00;
}

a:hover,
a:active {
  color: #c89300;
}
</style>