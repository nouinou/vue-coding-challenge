<template>
    <div class="Repo">
        <img class="repo_avatar" :src=owner_avatar alt="The avatar image of the repository's owner">
        <div class="repo_info">
            <h2 class="name"><a :href="url" target="_blank">{{name}}</a></h2>
            <p class="description">{{description}}</p> 
            <div class="stars_issues">
                <span class="stars">{{stars}} star</span> 
                <span class="issues">{{issues}} issue</span> 
            </div>
            <p>Created at {{created_at.slice(0,10)}} by 
                <strong><a :href="owner_url" target="_blank"> @{{owner_login}}</a></strong>
            </p> 
        </div>
            <hr>
    </div>
</template>

<script>
export default {
    name: 'name',
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
    }
}
</script>