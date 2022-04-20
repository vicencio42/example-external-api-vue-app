<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Posts!",
      posts: [],
    };
  },
  created: function () {
    this.postsIndex();
    this.newsIndex();
    var code = this.$route.query.code;
    if (code) {
      axios.get("/auth/github/callback?code=" + code).then((response) => {
        localStorage.setItem("github_access_token", response.data.access_token);
        this.$router.push("/about");
      });
    }
  },
  methods: {
    postsIndex: function () {
      axios.get("https://jsonplaceholder.typicode.com/posts").then((response) => {
        this.posts = response.data;
        console.log(response.data);
      });
    },
    newsIndex: function () {
      axios.get("/news_api").then((response) => {
        console.log(response.data);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <a href="https://github.com/login/oauth/authorize?client_id=1d23e1d0b8678f2750a8">Sign in to GitHub</a>
    <div v-for="post in posts" v-bind:key="post.id">
      <h2>{{ post.title }}</h2>
      <p>{{ post.body }}</p>
    </div>
  </div>
</template>

<style></style>
