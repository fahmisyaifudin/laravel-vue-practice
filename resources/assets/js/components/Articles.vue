<template>
    <div>
    
       <div v-if="notification" class="alert alert-success" role="alert">
            {{ notification }}
        </div>
        <div class="row">
            <div class="col-md-12">
                <router-link to="/create" class="btn btn-primary mb-2 float-right">
                    Create new Article
                </router-link>
            </div>
        </div>
        <div class="card card-body mb-2" v-for="article in articles" v-bind:key="article.id">
            <h3> {{ article.title}}</h3>
            <p> {{ article.body }}</p>
            <div class="row">
               <router-link :to="{ name: 'articleEdit', params: { id: article.id }}" class="btn btn-info ml-3 mr-2">
                   Edit
                </router-link>
                <button @click="deleteArticle(article.id)" class="btn btn-danger">Delete</button>
            </div>
        </div>
         <div>
            <ul class="pagination float-right">
                <li class="page-item">
                    <a class="page-link" @click="fetchArticles(pagination.prev)" tabindex="-1" href="#">Previous</a>
                </li>
                <li class="page-item">
                    <a class="page-link" @click="fetchArticles(pagination.next)" href="#">Next</a>
                </li>
            </ul>
        </div>
         
    </div>
</template>

<script>
export default {
    data(){
        return {
                notification: '',
                articles: [],
                article: {
                    id: '',
                    title: '',
                    body: '',
                },
                article_id: '',
                pagination: {},
                edit: false  
            }
        },
        created(){
            this.fetchArticles('api/articles');
        },
        methods: {
            fetchArticles(link){
                fetch(link)
                    .then(res => res.json())
                    .then(res  => {
                        this.articles = res.data;
                        this.pagination = res.links;
                    })
            },
            deleteArticle(id){
                if(confirm('Are You Sure?')){
                    fetch('api/article/'+id, {
                        method: 'delete'
                    })
                    .then(res => res.json())
                    .then(data => {
                        this.fetchArticles('api/articles');
                    })
                    .catch(err => console.log(err));
                }
            },
            
        },
    }
</script>