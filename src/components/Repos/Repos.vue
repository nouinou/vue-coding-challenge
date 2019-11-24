<template>
    <div id="Repos">
        <div id="repos_container">
            Repos Component is Working
            <Repo />
        </div>
    </div>
</template>

<script>
import Repo from './Repo/Repo.vue';

export default {
    name: 'Repos',
    components: {
        Repo
    }
}
</script>