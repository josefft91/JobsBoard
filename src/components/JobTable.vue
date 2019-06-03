<template>
    <table class="job-table min-w-full">
        <thead>
            <tr class="bg-gray-200 uppercase font-semibold text-left py-6 border-0">
                <th>Job Title</th>
                <th>Location</th>
                <th>Salary</th>
                <th>Expiry Date</th>
            </tr>
        </thead>
        
        <tbody>
            <JobRow 
                v-for="(job, index) in filteredResults" 
                :job="job"
                :index="index"
                :key="job.job_id" />
        </tbody>
    </table>
</template>

<style>
.job-table th, .job-table td {
    @apply px-2 py-6
}
</style>
<script>
import JobRow from './JobRow.vue'

export default {
    name: 'JobTable',
    props: {
        jobs: Array,
        keywords: String,
        category: String,
    },
    data() {
        return {
            'jobList': this.jobs
        }
    },
    components: {
        JobRow
    },
    computed: {
        filteredResults() {
            return this.jobs.filter(job => {
                return (job.job_title.toLowerCase().includes(this.keywords) === true 
                || job.plain_package_description.toLowerCase().includes(this.keywords) === true) 
                && (this.category != '' ? job.category_id == this.category : true)
            })
        }
    }
}
</script>