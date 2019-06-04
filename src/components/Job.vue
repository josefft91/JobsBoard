<template>
    <div class="hover:bg-gray-100 border-t border-t-2 border-gray-300 py-5 px-2">
        <div class="lg:flex lg:items-center lg:justify-between">
            <h1 class="text-xl font-bold py-2">{{ job.job_title }}</h1>
            <div class="flex items-center mb-2">
                <div class="w-5"><calendar class="w-5" /></div>
                <h2 class="ml-2 text-gray-600">{{ formatDate }}</h2>
            </div>
        </div>
        
        <div class="flex items-center mb-2">
            <div class="w-5"><location class="w-5" /></div>
            <h2 class="text-xl font-bold text-gray-600 ml-2">{{ job.location_city }}</h2>
        </div>
        
        <div class="flex items-center">
            <div class="w-5"><wallet class="w-5" /></div>
            <h2 class="text-xl text-gray-600 ml-2">{{ job.salaryDisplay }}</h2>
        </div>
        
        
    </div>
</template>

<script>
import moment from 'moment'
import location from '../assets/logos/location.svg'
import wallet from '../assets/logos/wallet.svg'
import calendar from '../assets/logos/calendar.svg'

export default {
    name: 'Job',
        props: {
            job: Object
        },
        components: {
            location,
            wallet, 
            calendar
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
                    return 'Apply by ' + date.format('DD MMMM, YYYY');
                } else {
                    return 'N/A';
                }
            }
        }
    }
</script>