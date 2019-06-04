<template>
<div class="hover:bg-gray-100 border-t border-t-2 border-gray-300">
    <h1>{{ job.job_title }}</h1>
    <td>{{ job.location_city }}</td>
    <td>{{ job.salaryDisplay }}</td>
    <td class="text-center">{{ formatDate }}</td>
</div>
</template>

<script>
import moment from 'moment'

export default {
    name: 'JobRow',
        props: {
            job: Object
        },
        data() {
            return {
                expiry_date: this.job.expiry_date
            }
        },
        computed: {
            formatDate() {
                let date = moment(this.expiry_date);
                if(date.isValid()) {
                    return date.format('DD MMMM, YYYY');
                } else {
                    return '-';
                }
            }
        }
    }
</script>