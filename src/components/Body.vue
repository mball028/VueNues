<template>
  <div id="main-body">
    <h2>Headlines</h2>
    <div v-for="(article, index) in articles" :key="index" class="link">
      <a class="headline" :href="article.url" target="_blank">{{article.title}}</a>
      <p class="source">{{article.source.name}}</p>
      <br />
      <br />
      <a :href="article.url" target="_blank">
        <img
          v-if="article.urlToImage"
          :src="article.urlToImage"
          alt="article-image"
          class="article-pic"
        />
      </a>
      <p>{{article.description}}</p>
      <p class="author" v-if="article.author">written by {{article.author}}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Newsbody",
  data() {
    return {
      articles: []
    };
  },
  mounted() {
    axios
      .get(
        "http://newsapi.org/v2/top-headlines?country=us&apiKey=dd09bb9baa8843d8b2e9bd4705399f79"
      )
      .then(res => (this.articles = res.data.articles));
  }
};
</script>

<style scoped>
h2 {
  color: #35495e;
}
.author {
  font-style: italic;
  font-weight: lighter;
  font-size: 0.8em;
}
.headline {
  color: black;
  text-decoration: none;
  background-color: #42b883;
  color: white;
  padding: 7px;
  line-height: 30px;
}
.source {
  margin-bottom: 0;
}
#main-body {
  text-align: start;
  width: 75%;
  margin: 0 auto 100px;
  color: #35495e;
}
.article-pic {
  width: 150px;
  border: solid #42b883 3px;
  padding: 3px;
}
.link {
  padding: 15px;
  margin-bottom: 30px;
  box-shadow: 10px 10px #42b8839f;
}
</style>