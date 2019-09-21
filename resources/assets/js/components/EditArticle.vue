<template>
<div>
   <h2>Edit Article</h2>
      <form @submit.prevent="editArticle" class="mb-3">
            <div class="form-group">
                <input type="text" class="form-control" v-model="article.title">
            </div>
             <div class="form-group">
                <textarea class="form-control" v-model="article.body"></textarea>
            </div>
            <button type="submit" class="btn btn-primary ml-3">Save</button>
        </form>
</div>
</template>
<script>
export default {
    data(){
        return {
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
        this.fetchArticle(this.$route.params.id)
    },
    methods: {
        fetchArticle(id){
            fetch('api/article/'+id)
                    .then(res => res.json())
                    .then(res  => {
                        this.article = res.data;
                    })
        },
        // editArticle(){
        //     fetch('api/article', {
        //                 method: 'put',
        //                 body: JSON.stringify(this.article),
        //                 headers: {
        //                     'content-type' : 'application/json'
        //                 }
        //             })
        //             .then(res => res.json())
        //             .then(data => {
        //                 this.article.title = '';
        //                 this.article.body = '';
        //             })
        //             .catch(err => console.log(err));
        //             this.$router.push('/');
        // }
    }
}
</script>