<template>
    <div class="Repo">
        <img class="repo_avatar" :src=owner_avatar alt="The avatar image of the repository's owner">
        <div class="repo_info">
            <h2 class="name"><a :href="url" target="_blank">{{name}}</a></h2>
            <p class="description">{{description}}</p> 
            <div class="stars_issues">
                <span class="stars">{{stars_count}} star</span> 
                <span class="issues">{{issues_count}} issue</span> 
            </div>
            <p class="creation">Created at {{created_at.slice(0,10)}} by 
                <strong><a :href="owner_url" target="_blank"> @{{owner_login}}</a></strong>
            </p> 
        </div>
    </div>
</template>

<script>
export default {
    name: 'name',
    data() {
        return {
            stars_count: 0,
            issues_count: 0
        }
    },
    props: {
        name: String,
        description: String,
        stars: Number,
        issues: Number,
        owner_login: String,
        owner_avatar: String,
        owner_url: String,
        url: String,
        created_at: String
    },
    mounted(){
        this.stars >= 1000 ? this.stars_count = (this.stars / 1000).toFixed(1) + 'K' : this.stars_count = this.stars
        this.issues >= 1000 ? this.issues_count = (this.issues / 1000).toFixed(1) + 'K' : this.issues_count = this.issues
    },
    updated(){
        this.stars >= 1000 ? this.stars_count = (this.stars / 1000).toFixed(1) + 'K' : this.stars_count = this.stars
        this.issues >= 1000 ? this.issues_count = (this.issues / 1000).toFixed(1) + 'K' : this.issues_count = this.issues
    }
}
</script>

<style>
a, a:hover{
    text-decoration: none;
    color: #2c3e50;
    cursor: pointer;
}
.Repo{
    text-align: left !important;
    display: flex;
    flex-direction: row;
    background: #fff;
    margin: 2vmax auto;
    border-radius: 5px;
    width: 85vw;
    max-width: 960px;
    padding:15px;
    box-shadow: 15px 15px 25px -25px rgba(0,0,0,0.5);
}
.repo_avatar{
    margin-right:25px;
    width:30%;
    height:30%;
    align-self: center;
    border-radius:20px;
}
.repo_info{
    width:70%;
    display: flex;
    flex-direction:column;
    justify-content: space-between
}
.name{
    font-size: 4vmin;
    margin: 0
}
.description{
    font-size: 2.5vmin;
    margin: 5px 0;
}
.stars_issues{
    display:flex;
    flex-direction: row;
    justify-content: space-around;
    margin: 5px 0;
}
.stars_issues span{
    font-size: 2.2vmin;
    padding: 1vmin 2vmin;
    border-radius: 5px;
    cursor:default;
}
.stars{
    background: gold;
}
.issues{
    background:#FF614D;
}
.creation{
    font-size:2.2vmin;
    margin: 0
}

</style>