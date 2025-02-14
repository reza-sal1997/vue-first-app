<template>
  <base-card>
    <base-button :mode="storedResourcesButtonMode" @click="setSelectedTab('stored-resources')">Store
      Resources
    </base-button>
    <base-button :mode="addResourceButtonMode" @click="setSelectedTab('add-resource')">Add
      Resource
    </base-button>
  </base-card>
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import StoredResources from "./StoredResources.vue";
import AddResource from "./AddResource.vue";

export default {
  components: {
    StoredResources,
    AddResource
  },

  data() {
    return {
      selectedTab: 'stored-resources',
      storedResources: [
        {
          id: 'official-guide',
          title: 'Official Guide',
          description: "The official Vue.js documentation",
          link: "https://vuejs.org",
        },
        {
          id: 'google',
          title: 'Google',
          description: "Google is really important",
          link: "https://google.com",
        },
      ]
    }
  },
  provide() {
    return {
      resources: this.storedResources,
      addResource: this.addResource,
      deleteResource: this.removeResource,
    }
  },
  computed: {
    storedResourcesButtonMode() {
      return this.selectedTab === 'stored-resources' ? null : 'flat';
    },
    addResourceButtonMode() {
      return this.selectedTab === 'add-resource' ? null : 'flat';
    },
  },

  methods: {
    setSelectedTab(tab) {
      this.selectedTab = tab;
    },
    addResource(title, description, url) {
      const newResource = {
        id: new Date().toDateString(),
        title: title,
        description: description,
        link: url
      }
      this.storedResources.unshift(newResource);
      this.selectedTab = 'stored-resources';
    },
    removeResource(resourceId) {
      const resourceIndex = this.storedResources.findIndex(resource => resource.id === resourceId)
      this.storedResources.splice(resourceIndex, 1);
    }
  }
}
</script>
