<template>
  <div class="vue-rss-feed">
    <Feed :feedUrl="feedUrl" :name="name" :limit="limit" @feedmain="getName"/>
  </div>
</template>

<script>
import Feed from "../components/Feed.vue";

export default {
  name: "VueRssFeed",
  components: {
    Feed
  },
  props: {
    feedUrl: String,
    name: String,
    limit: Number
  },
  methods: {
    getName(evt) {
      this.$emit('demomain', evt);
    }
  }
};
</script>

<style scoped>
.vue-rss-feed {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 600px;
  width: 100%;
  text-align: center;
  color: #2c3e50;
}
</style>
