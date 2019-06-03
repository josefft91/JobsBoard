<template>
  <div class="my-16" id="app">
    <div class="container mx-auto px-32">
      <form class="w-full max-w-sm mb-2">
        <div class="flex flex-wrap py-2">
          <label class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2" for="keywords">
            Keywords
          </label>
          <input id="keywords" v-model="keywords" class="appearance-none block w-full bg-gray-200 text-gray-700 border rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white" type="text" placeholder="Keywords" aria-label="Keywords">
          
        </div>
        <label class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2" for="category">
          Category
        </label>
        <div class="relative">
          <select v-model="category" id="category" class="block appearance-none w-full bg-gray-200 border border-gray-200 text-gray-700 py-3 px-4 pr-8 rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500 mb-2">
            <option value="">None</option>
            <option value="1">Accounting/Finance</option>
            <option value="4">Business/Strategic Management</option>
            <option value="8">Education/Training/Instruction</option>
            <option value="11">Food Services/Hospitality</option>
            <option value="12">Human Resources</option>
          </select>
          <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
            <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/></svg>
          </div>
        </div>
        <div class="flex justify-end">
          <!-- <button v-on:click="filterResults" class="rounded flex-shrink-0 border-transparent bg-blue-500 hover:bg-blue-700 py-1 px-2 text-white font-bold">
            Filter
          </button> -->
          <button v-on:click="resetFilters" class="ml-2 rounded flex-shrink-0 border-transparent bg-red-500 hover:bg-red-700 py-1 px-2 text-white font-bold">
            Reset
          </button>
        </div>
      </form>
      <JobTable :jobs="this.jobs" :keywords="this.keywords" :category="this.category" />
    </div>
    
  </div>
</template>

<script>
import axios from 'axios'
import JobTable from './components/JobTable.vue'
//import TribepadData from './assets/data/tribepad'

export default {
  name: 'app',
  components: {
    JobTable
  },
  data() {
    return {
      'jobs': [],
      'keywords': '',
      'category': '',
      'errors': [],
    }
  },
  methods: {
    loadData: function() {
      axios
        .get('tribepad.json')
        .then(response => {
          this.jobs = response.data.jobs.job
        })
        .catch(e => (this.errors.push(e)))
    },
    // filterResults: function(evt) {
    //   evt.preventDefault();
    //   this.jobs = this.jobs.filter(job => job.job_title.toLowerCase().includes(this.keywords) === true && job.category_id == this.category)
    // },
    resetFilters: function(evt) {
      evt.preventDefault();
      this.keywords = '';
      this.category = '';
      this.loadData();
    }
  },
  created: function() {
    this.loadData()
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
