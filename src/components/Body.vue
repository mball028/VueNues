<template>
  <div id="main-body">
    <h2>{{ title }}</h2>
    <input class="search" type="text" v-model="search" placeholder="Search Articles" />
    <br />
    <br />
    <input
      type="text"
      class="topic-search"
      v-model="topic"
      placeholder="Find articles by topic"
      @keyup.enter="topicFinder()"
    />
    <button @click="topicFinder()" class="topic-btn">Look Up Articles</button>
    <h5>Sources</h5>
    <button
      v-for="(source, i) in sourceList"
      :key="i"
      @click="filterSources(source); active = i; filterSources"
      :class="{active: sourceFilter.includes(source)}"
      class="source"
    >{{ source.toLowerCase() }}</button>
    <div v-for="article in filteredArticles" :key="article.url" class="article">
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
  data() {
    return {
      title: "",
      articles: [],
      search: "",
      sourceList: ["All"],
      sourceFilter: ["All"],
      topic: ""
    };
  },
  methods: {
    initialLoad() {
      this.sourceList = ["All"];
      axios
        .get(
          "http://newsapi.org/v2/top-headlines?" +
            "country=us&" +
            "apiKey=dd09bb9baa8843d8b2e9bd4705399f79"
        )
        .then(res => {
          res.data.articles.forEach(article =>
            this.sourceList.includes(article.source.name)
              ? this.sourceList
              : this.sourceList.push(article.source.name)
          );
          return (this.articles = res.data.articles);
        });
      this.title = `Top US Headlines`;
    },
    filterSources(src) {
      const allIndex = this.sourceFilter.indexOf("All");
      if (src != "All" && this.sourceFilter.includes(src) === false) {
        if (allIndex > -1) {
          this.sourceFilter.splice(allIndex, 1);
        }
        this.sourceFilter.push(src);
      } else if (this.sourceFilter.includes(src)) {
        this.sourceFilter.splice(this.sourceFilter.indexOf(src), 1);
        if (this.sourceFilter.length === 0) {
          this.sourceFilter = ["All"];
        }
      } else {
        this.sourceFilter = ["All"];
      }
    },
    topicFinder() {
      if (this.topic.length > 0) {
        axios
          .get(
            `https://newsapi.org/v2/everything?q=${this.topic}&apiKey=dd09bb9baa8843d8b2e9bd4705399f79`
          )
          .then(res => {
            this.sourceList = ["All"];
            res.data.articles.forEach(article => {
              this.sourceList.includes(article.source.name)
                ? this.sourceList
                : this.sourceList.push(article.source.name);
            });
            return (this.articles = res.data.articles);
          });
        this.title = `${this.topic.charAt(0).toUpperCase() +
          this.topic.slice(1)} Articles`;
      } else {
        alert("Please insert a topic.");
        this.initialLoad();
      }
    }
  },
  mounted() {
    this.initialLoad();
  },
  computed: {
    matchedArticles: function() {
      return this.articles.filter(article => {
        if (article.description) {
          return (
            article.title.toLowerCase().match(this.search.toLowerCase()) ||
            article.description.toLowerCase().match(this.search.toLowerCase())
          );
        }
        return article.title.toLowerCase().match(this.search.toLowerCase());
      });
    },
    filteredArticles: function() {
      return this.matchedArticles.filter(article => {
        if (this.sourceFilter == "All") {
          return article;
        } else {
          return this.sourceFilter.includes(article.source.name);
        }
      });
    }
  }
};
</script>

<style scoped>
button.source {
  background-color: #42b8833f;
  border: none;
  border-radius: 15px;
  margin: 5px;
  padding: 3px 12px;
  cursor: pointer;
}
button.active,
button:hover {
  background-color: #42b883;
  color: white;
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
  margin-bottom: 50px;
  box-shadow: 10px 10px #42b8839f;
}
.search {
  width: 60%;
  border: none;
  border-bottom: solid 4px #42b883;
  padding: 5px;
}
.search:focus {
  background-color: #42b8833f;
}
</style>