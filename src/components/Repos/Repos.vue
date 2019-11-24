<template>
    <div id="Repos" v-if="repos.length > 0">
        <div id="repo_date_filter">
            <!-- 2008-02-08 is Github creation date  -->
            <input 
                type="date" 
                v-model="date" 
                min="2008-02-08" 
                :max="today"
                v-on:keydown=removeDateError()
                v-on:keyup.enter=update()
                 />
                <button class="update" v-on:click=update()>search</button>
                <br>
                <span class="error" v-if="this.invalidDateError">Please enter a valid date</span>
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
        <img v-if="showLoading" src="../../assets/spinner.gif" alt="loading-spinner">
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
            apiURl: '',
            today: this.getTodayDate(),
            date: this.getPastMonth(),
            sort: '&sort=stars',
            order: '&order=desc',
            perPage: '&per_page=100', // In order to get 100 results per page
            page: 1,
            repos: [],
            busy: false,
            showLoading: false,
            invalidDateError: false
        }
    },
    mounted(){
        let self = this; // in order to use 'this' inside window;
        this.apiURl = this.source + this.date + this.sort + this.order + this.perPage;
        this.fetch();
        window.onscroll = function() {
        if ((window.innerHeight + window.scrollY) >= document.body.offsetHeight && !self.busy) {
                self.loadMore();
            }
        };
    },
    methods: {
        fetch(){
            this.repos = [];
            this.emitFetching();
            axios.get(this.apiURl).then(res => {
                this.repos = res.data.items;
                this.showLoading = false;
                this.emitLoaded();
            });
        },
        update(){
            let time = new Date(this.date).getTime();
            if (isNaN(time) || time < 0) { 
                this.invalidDateError = true;
                return;
            }
            this.apiURl = this.source + this.date +  this.sort + this.order + this.perPage;
            this.fetch();
        },
        removeDateError(){
            this.invalidDateError = false;
        },
        loadMore() {
            this.busy = true;
            this.page++;
            this.showLoading = true;
            axios.get(this.apiURl + '&page=' + this.page + this.perPage).then(res => {
                const append = res.data.items
                this.repos = this.repos.concat(append);
                this.busy = false;
                this.showLoading = false;
            });
        },
        emitFetching(){
            this.$emit('fetching')
        },
        emitLoaded(){
            this.$emit('loaded');
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


<style>
#repo_date_filter input{
    padding: 5px;
    border-radius: 3px;
    border-width: 0;
    font-size: 12px;
}
.update{
    padding: 5px 20px;
    border-radius: 3px;
    font-size: 12px;
    margin-left: 12px;
}
.error {
    color:red;
    font-size: 12px;
}
</style>