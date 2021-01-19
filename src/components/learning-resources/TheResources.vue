<template>
  <base-card>
    <!-- each button for each tab -->
    <base-button 
      @click="setSelectedTab('stored-resources')" 
      :mode="storedResButtonMode">
      Stored Resource 
    </base-button>

    <base-button 
      @click="setSelectedTab('add-resource')"
      :mode="addRessButtonMode"> 
      Stored Resource 
    </base-button>
  </base-card>

  <!-- keep alive save the data in cache -->
  <!-- call the tab -->
  <keep-alive>
    <component :is="selectedTab"> </component>
  </keep-alive>
</template>

<script>
import StoredResources from './StoredResources.vue'
import AddResource from './AddResource.vue'


export default {
  components: {
    StoredResources,
    AddResource,
  },
  data() {
    return {
      selectedTab: 'stored-resources',
      storedResources: [
        {
          id: 'official-guide',
          title: 'Official Guide',
          description: 'The official Vue.js documentation.',
          link: 'https://vuejs.org',
        },
        {
          id: 'google',
          title: 'Google',
          description: 'Learn to google ...',
          link: 'https://google.org',
        },
      ],
    };
  },
  provide() {
    return {
      resources: this.storedResources,
      addResource: this.addResource,
      deleteResource: this.removeResource,
    };
  },
  methods: {
    // choose tab
    setSelectedTab(currTab) {
      this.selectedTab = currTab;
    },
    // add a new resource in storedResources
    addResource(title, description, url) {
      const newResource = {
        id: new Date().toISOString(),
        title: title,
        description: description,
        link: url,
      };
      this.storedResources.unshift(newResource);
      this.selectedTab = 'stored-resources';
    },
    // delete resource from storedResources
    removeResource(resId) {
      const resIndex = this.storedResources.findIndex(res => res.id === resId )
      
      // delete if the resource exist
      if (resIndex >= 0) this.storedResources.splice(resIndex, 1);
    }
  },
  computed: {
    // aesthetic options
    storedResButtonMode() {
      return this.selectedTab === 'stored-resources' ? null : 'flat';
    },
    addRessButtonMode() {
      return this.selectedTab === 'add-resource' ? null : "flat";
    },
  }
}

</script>