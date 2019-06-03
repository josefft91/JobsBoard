<template>
<tr class="hover:bg-gray-100 border-t border-t-2 border-gray-300">
    <td>{{ job.job_title }}</td>
    <td>{{ job.location_city }}</td>
    <td>{{ job.salaryDisplay }}</td>
    <td class="text-center">{{ formatDate }}</td>
</tr>
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
                // if(this.expiry_date == "0000-00-00") {
                //     return '-'
                // } else {
                    //let date = new Date(this.expiry_date).toUTCString();
                   // return date.getDate() + ' ' + 
                   //return moment(this.expiry_date)
                //}
                
            }
        }
    }
</script>