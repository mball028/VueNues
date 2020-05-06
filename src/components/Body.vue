<template>
  <div id="main-body">
    <h2>Headlines</h2>
    <input class="search" type="text" v-model="search" placeholder="Search Headlines" />
    <h5>Sources</h5>
    <button
      v-for="(source, i) in sourceList"
      :key="i"
      @click="sourceFilter = source; active = i;"
      :class="{active: source == sourceFilter}"
    >{{ source }}</button>
    <div v-for="article in matchedArticles" :key="article.url" class="article">
        <a class="headline" :href="article.url" target="_blank">{{article.title}}</a>
        <p class="source">{{article.source.name.toLowerCase()}}</p>
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
  methods: {
  },
  data() {
    return {
      articles: [],
      search: "",
      sourceList: [
        "All",
        "ABC News",
        "Bleacher Report",
        "Bloomberg",
        "CBS News",
        "CNBC",
        "CNN",
        "ESPN",
        "Fox News",
        "Time",
        "USA Today"
      ],
      sourceFilter: "All"
    };
  },
  mounted() {
    axios
      .get(
        "http://newsapi.org/v2/top-headlines?country=us&apiKey=dd09bb9baa8843d8b2e9bd4705399f79"
      )
      .then(res => (this.articles = res.data.articles));
  },
  computed: {
    matchedArticles: function() {
      return this.articles.filter(article => {
        return (
          article.title.toLowerCase().match(this.search.toLowerCase()) ||
          article.description.toLowerCase().match(this.search.toLowerCase())
        );
      });
    }
  }
};
</script>

<style scoped>
button.active {
  background-color: #42b883;
}
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
.article {
  padding: 15px;
  margin-bottom: 30px;
  box-shadow: 10px 10px #42b8839f;
}
.search {
  width: 60%;
  border: none;
  border-bottom: solid 4px #42b883;
  padding: 5px;
}
.search:focus {
  background-color: #42b88338;
}
</style>