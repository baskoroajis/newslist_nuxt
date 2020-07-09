<template>
    <div class="mt-5">
        <div class="card-columns">
            <div class="card"  v-for="item in posts" v-bind:key="item.key" @click="openDetail(item)">
                <img class="card-img-top" :src="item.urlToImage" alt="Card image cap">
                <div class="card-body">
                    <h5 class="card-title">{{ item.title }}</h5>
                    <h6 class="card-subtitle mb-2 text-muted">{{ item.publishedAt }}</h6>
                    <p class="card-text">{{ item.description }}</p>
                </div>
            </div>
        </div>
        <button class="btn btn-primary btn-more" @click="loadMore">Load More</button>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            allpost : [],
            posts : [],
            APIKEY : '',
            current: 9
        };
    },
    methods : {
        loadMore () {
            this.posts = []
            this.current += 9
            this.allPost.map((item, key) => item.description !== null && this.posts.length < this.current ? this.posts.push(item) : '')
        },
        openDetail(data){
            this.$store.commit('setArticle', data)
            this.$router.replace({ 'path': '/detail' })
        }
    },
    mounted () {
    axios('https://newsapi.org/v2/everything?q=programming&domains=techcrunch.com,techinasia.com&apiKey='+this.APIKEY, {
      crossDomain: true
    }).then( ({ data }) => {
      this.allPost = data.articles
      data.articles.map((item, key) => {
        if (item.description !== null && this.posts.length < 9) {
          this.posts.push(item)
        }
      })
    })
  }  
}
</script>


<style lang="scss" scoped>
/*
 * Card Columns Masonry - Bootstrap 4
 * https://codepen.io/JacobLett/pen/oZmWdd
 */
/* Medium devices (tablets, 768px and up) The navbar toggle appears at this breakpoint */
@media (min-width: 768px) {  
  .card-columns {column-count: 3;}
}
/* Large devices (desktops, 992px and up) */
@media (min-width: 992px) { 
 .card-columns {column-count: 3;}
}
 
/* Extra large devices (large desktops, 1200px and up) */
@media (min-width: 1200px) {  
   .card-columns {column-count: 3;} 
}

.btn-more {
  margin: 20px auto;
  display: block;
}
</style>