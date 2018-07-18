<template>
  <div class="container">
    <input v-model="search" type="text" placeholder="Search">

    <button v-on:click="loadNews" type="button">search</button>

    <p v-if="loading">search in progress...</p>
    <div class="lds-ellipsis" v-if="loading"><div></div><div></div><div></div><div></div></div>

    <div v-for="article in news">

 <img  v-if="article.urlToImage" :src="article.urlToImage" alt="">
 <h3>{{article.title}}</h3>
   <h4>{{article.author}}</h4>
   <p>{{article.description}}</p>
        <a class="button" :href="article.url" target="_blank">read more</a>

    
 
  </div>
  </div>
</template>

<script>
import axios from 'axios'

const baseurl = "https://newsapi.org/v2";
const apiKey = "0c6397c160344228aeeb61a278ff3541"
const endpoint = "/everything"


const data = {
news: [],
search:'',
loading: false
}
export default {
  data: function() {
    return data
  },
  created(){
    this.loadNews();
  },
  
  methods: {
  	loadNews() {
      if (this.search.length <1) {
        return;
      }

      this.loading = true;
      this.news = [];

      let url = baseurl + endpoint + '?q=' + this.search + '&apiKey=' + apiKey;


    axios.get(url).then(function(response) {
    console.log(response.data.articles)
    data.loading = false;
    data.news = response.data.articles
    }).catch(function(error) {
    console.log(error.message)
    })
    }
  }
}
</script>
