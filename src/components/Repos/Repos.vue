<template>
    <div id="Repos" >
        <div id="repo_date_filter">
            <!-- 2008-02-08 is Github creation date  -->
            <input type="date" v-model="date" min="2008-02-08" :max="today" />
        </div>
        <div id="repos_container">
            <Repo v-for="repo in repos" v-bind:key="repo.id" />
        </div>
    </div>
</template>

<script>
import Repo from './Repo/Repo.vue';

export default {
    name: 'Repos',
    components: {
        Repo
    },
    data(){
        return {
            today: this.getTodayDate(),
            date: this.getPastMonth(),
            repos: [],
        }
    },
    methods: {
        getTodayDate: function(){
            return this.formateDate(new Date());
        },
        getPastMonth: function(){
            let today = new Date();
            // Subsctracting 30 days from today's date
            let aMonthBefore = today.setDate(today.getDate() - 30);
            return this.formateDate(aMonthBefore);
        },
        formateDate: function(date) { // Format date to yyyy-mm-dd
            var d = new Date(date),
                month = '' + (d.getMonth() + 1),
                day = '' + d.getDate(),
                year = d.getFullYear();

            if (month.length < 2) 
                month = '0' + month;
            if (day.length < 2) 
                day = '0' + day;

            return [year, month, day].join('-');
        }
    }
}
</script>