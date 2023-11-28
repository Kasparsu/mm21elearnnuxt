<template>
    <div class="columns is-multiline">
        <div class="column is-one-quarter" v-for="article in articles">
            <div class="card">
                <header class="card-header">
                    <p class="card-header-title">
                        {{ article.title }}
                    </p>
                </header>
                <div class="card-content">
                    <div class="content">
                        {{ article.content }}
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';
export default {
    created() {
        axios.get('http://localhost:3001/api/articles').then(res => {
            console.log(res.data);
            this.articles = res.data;
        });
    },
    data() {
        return {
            articles: []
        }
    }
}
</script>