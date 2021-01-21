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
    <component :is="selectedTab" :="currentTabProperties"> </component>
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
      isLoading: false,
      errorMessage: null,
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
    
    // load resources from Firebase -- Database
    loadDbResources() {
      
      this.isLoading = true;
      fetch('https://vuejs-demo-resources-default-rtdb.europe-west1.firebasedatabase.app/resources.json')
        .then( (response) => {
          if(response.ok) {
            return response.json();
          }
        })
        .then( (data)  => {
          this.isLoading = false;
          for (const id in data) {
            this.storedResources.unshift({
              id: data[id].id,
              title: data[id].title,
              description: data[id].description,
              link: data[id].url,
            });
          }
        })
        .catch( (error) => {
          console.log(error);
          this.isLoading = false;
          this.errorMessage = 'Failed to get data from firebase - please try again later!';
        });

    },

    // choose tab
    setSelectedTab(currTab) {
      this.selectedTab = currTab;
    },

    // add a new resource in storedResources
    addResource(id, title, description, url) {
      const newResource = {
        id: id,
        title: title,
        description: description,
        link: url,
      };
      this.storedResources.unshift(newResource);
      
      // select stored resources tab
      this.selectedTab = 'stored-resources';
    },

    // delete resource from storedResources
    removeResource(resId) {
      const resIndex = this.storedResources.findIndex(res => res.id === resId )
      
      // delete if the resource exist
      if (resIndex >= 0) this.storedResources.splice(resIndex, 1);
    },

  },
  computed: {

    // aesthetic options
    storedResButtonMode() {
      return this.selectedTab === 'stored-resources' ? null : 'flat';
    },

    // aesthetic options
    addRessButtonMode() {
      return this.selectedTab === 'add-resource' ? null : "flat";
    },


    // if it's stored-resources props: isLoading for loading info
    currentTabProperties() {
      return this.selectedTab === 'stored-resources' ? { isLoading: this.isLoading, errorMessage: this.errorMessage } : null;
    },

  },
  mounted() {
    this.loadDbResources(); // call the method to get and render all the resources
  }
}

</script>