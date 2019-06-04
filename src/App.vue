<template>
  <div class="md:my-8 lg:my-16" id="app">
    <div class="container md:w-11/12 lg:w-4/5 mx-auto px-4 lg:px-16 py-5 bg-white shadow-xl">
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
            <option v-for="category in jobCategories" :key="category.id" :value="category.id">{{ category.name }}</option>
          </select>
          <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
            <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/></svg>
          </div>
        </div>
        <div class="flex justify-end">
          <button v-on:click="resetFilters" class="ml-2 rounded flex-shrink-0 border-transparent bg-red-500 hover:bg-red-700 py-1 px-2 text-white font-bold">
            Reset
          </button>
        </div>
      </form>
      <div class="container">
        <Job 
          v-for="(job, index) in filteredResults" 
          :job="job"
          :index="index"
          :key="job.job_id" />
        </div>
    </div>
    
  </div>
</template>

<script>
import axios from 'axios'
import Job from './components/Job.vue'

export default {
  name: 'app',
  components: {
    Job
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
    resetFilters: function(evt) {
      evt.preventDefault();
      this.keywords = '';
      this.category = '';
    },
  },
  created: function() {
    this.loadData()
  },
  computed: {
    filteredResults() {
      return this.jobs.filter(job => {
          return (job.job_title.toLowerCase().includes(this.keywords) === true 
          || job.plain_package_description.toLowerCase().includes(this.keywords) === true) 
          && (this.category != '' ? job.category_id == this.category : true)
      })
    },
    jobCategories() {
      return this.jobs.map(job => {
        var category = new Object()
        category['id'] = job.category_id;
        category['name'] = job.category_name;
        return category;
      })
      .filter((value, index, self) => self.map(cat => cat.id).indexOf(value.id) == index)
      .sort((a, b)  => a.id - b.id)
    }
  }
}
</script>

<style>
body{
  @apply bg-gray-200
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
