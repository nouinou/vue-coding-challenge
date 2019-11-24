<template>
    <div id="Repos" >
        <div id="repo_date_filter">
            <!-- 2008-02-08 is Github creation date  -->
            <input 
                type="date" 
                v-model="date" 
                min="2008-02-08" 
                :max="today"
                v-on:change=update()
                 />
        </div>
        <div id="repos_container">
            <Repo 
                v-for="repo in repos" 
                v-bind:key="repo.id"
                :name ="repo.name" 
                :description="repo.description" 
                :stars="repo.stargazers_count"
                :issues="repo.open_issues_count"
                :owner_login="repo.owner.login"
                :owner_avatar="repo.owner.avatar_url"
                :owner_url="repo.owner.html_url"
                :url="repo.html_url"
                :created_at="repo.created_at"
                 />
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import Repo from './Repo/Repo.vue';

export default {
    name: 'Repos',
    components: {
        Repo
    },
    data(){
        return {
            source: 'https://api.github.com/search/repositories?q=created:>',
            apiURl: 'https://api.github.com/search/repositories?q=created:>',
            today: this.getTodayDate(),
            date: this.getPastMonth(),
            sort: '&sort=stars',
            order: '&order=desc',
            repos: [],
        }
    },
    mounted(){
        this.apiURl += this.date + this.sort + this.order;
        this.fetch();
    },
    methods: {
        fetch(){
            axios.get(this.apiURl).then(res => {
                this.repos = res.data.items;
            });
        },
        update(){
            //changing date and fetching again
            this.apiURl = this.source + this.date +  this.sort + this.order;
            this.fetch();
        },
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