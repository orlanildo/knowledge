<template>
    <div class="article-by-id">
        <PageTitle icon="fa fa-file-o" :main="article.name" :sub="article.description" />
        <div class="article-content" v-html="article.content"></div>

        <Label>Avaliação</Label>
        <star-rating class="rating-star"
            v-model="rating"
            v-bind:increment="1.0"
            v-bind:max-rating="5"
            inactive-color="#FFF"
            active-color="#6610f2"
            v-bind:star-size="40"
            v-bind:show-rating= "false">
        </star-rating>
        
    </div>
</template>

<script>
import 'highlightjs/styles/dracula.css'
import hljs from 'highlightjs/highlight.pack.js'
import { baseApiUrl } from '@/global'
import axios from 'axios'
import PageTitle from '../template/PageTitle'
import StarRating from 'vue-star-rating'

export default {
    name: 'ArticleById',
    components: { PageTitle, StarRating},
    data: function() {
        rating: 0
        return {
            article: {},
        }
    },
    methods: {
        setRating: function(rating){
            this.rating= rating;
        }
    },
    mounted() {
        const url = `${baseApiUrl}/articles/${this.$route.params.id}`
        axios.get(url).then(res => this.article = res.data)
    },
    updated() {
        document.querySelectorAll('.article-content pre.ql-syntax').forEach(e => {
            hljs.highlightBlock(e)
        })
    }    
}
</script>

<style>
    .rating-star{
        float:right;
        margin-bottom: 20%;
    }
    .article-content {
        background-color: #FFF;
        border-radius: 8px;
        padding: 25px;
    }

    .article-content pre {
        padding: 20px;
        border-radius: 8px;
        font-size: 1.2rem;
        background-color: #1e1e1e;
        color: #FFF;
    }

    .article-content img {
        max-width: 100%;
    }

    .article-content :last-child {
        margin-bottom: 0px;
    }
</style>
