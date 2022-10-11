<template>
  <base-card>
  <!-- VueJS special behaviour -->
  <!-- adding props & event listeners to custom components -->
  <!-- by default they fall-through to the root level HTML element of that custom component -->
    <base-button @click="setSelectedTab('stored-resources')" :mode="storedResButtonMode">
    Stored Resources
    </base-button>
    <base-button @click="setSelectedTab('add-resource')" :mode="addResButtonMode">
    Add Resource
    </base-button>
  </base-card>
  <keep-alive>
    <component :is="selectedTab"></component>
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
    data(){
      return {
        selectedTab: 'stored-resources',
        storedResources: [
        {id: 'official-guide', title: 'Official Guide', description: 'The official VueJS documentation', link: 'https://vuejs.org'},
        {id: 'google', title: 'Google', description: 'Sharpen your Google-Fu', link: 'https://www.google.com'},
      ]
      }
    },
    provide(){
      return {
        resources: this.storedResources,
        addResource: this.addResource
      }
    },
    computed: {
      storedResButtonMode() {
        return this.selectedTab === 'stored-resources' ? null : 'flat'
      },
      addResButtonMode() {
        return this.selectedTab === 'add-resource' ? null : 'flat'
      }
    },
    methods: {
      setSelectedTab(tab) {
        this.selectedTab = tab
      },
      addResource(title, description, url){
        const newResource = {
          id: new Date().toISOString(),
          title,
          description,
          link: url
        }
        this.storedResources.unshift(newResource)
        this.selectedTab = 'stored-resources'
      }
    }
  }
</script>