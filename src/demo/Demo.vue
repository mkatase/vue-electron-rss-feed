<template>
  <el-container>
    <el-header>
      <el-row>
        <el-col :span="8">
          <a href="https://rss.app" class="nav-left">
            <img width="30" height="30" alt="rss" src="../assets/logo.png">
            <div>RSS.app</div>
          </a>
        </el-col>
        <el-col :span="8">
          <div class="center">
            <h1>vue-rss-feed demo</h1>
          </div>
        </el-col>
        <el-col :span="8" class="github-button"></el-col>
      </el-row>
    </el-header>
    <el-main>
      <el-row>
        <el-col :span="12">
          <div class="preview-vue-rss-feed">
            <div v-if="feedUrl">
            <VueRssFeed :feedUrl="feedUrl" :name="name" :limit="limit" @demomain="getName"/>
            </div>
          </div>
        </el-col>
        <el-col :span="12">
          <div style="margin: 20px;"></div>
          <el-form
            label-position="top"
            label-width="100px"
            :model="rssFeedForm"
            ref="rssFeedForm"
            @submit.native.prevent
          >
            <el-form-item label="Enter RSS Feed URL">
              <el-input placeholder="Enter RSS feed URL" v-model="rssFeedForm.feedUrl"></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="submitForm('rssFeedForm')">Submit</el-button>
            </el-form-item>
          </el-form>

          <el-row>
            <div class="scroll-down">
              <span>Scroll down to load more</span>
              <span class="icon-down">
                <svg style="width:30px;height:30px" viewBox="0 0 30 30">
                  <path
                    fill="#2c3e50"
                    d="M7.41,8.58L12,13.17L16.59,8.58L18,10L12,16L6,10L7.41,8.58Z"
                  ></path>
                </svg>
              </span>
            </div>
          </el-row>

          <div class="examples">
            <h3>Examples</h3>
            <ul>
              <li v-for="url in urls" :key="url.name">
                <a :href="url.path" @click="loadExample">{{url.name}}</a>
              </li>
            </ul>
          </div>
          <el-row class="powered-rss-app">
            Powered by
            <a href="https://rss.app">RSS.app</a>
          </el-row>
        </el-col>
      </el-row>
    </el-main>
  </el-container>
</template>

<script>
import VueRssFeed from "./VueRssFeed.vue";
import axios from 'axios';

const PIXALS_FROM_BOTTOM = 200;

export default {
  name: "Demo",
  components: {
    VueRssFeed
  },
  data() {
    return {
      feedUrl: null,
      name: "",
      limit: 4,
      rssFeedForm: {
        feedUrl: null
      },
      urls: []
    };
  },
  created() {
    axios.get("urllist.json")
    .then(response => {
      this.urls = response.data;
      this.feedUrl = response.data[0].path;
      this.rssFeedForm.feedUrl = this.feedUrl;
    });
  },
  mounted() {
    this.scrollEvent = window.addEventListener("scroll", () => {
      if (
        window.innerHeight + window.scrollY >=
        document.body.offsetHeight - PIXALS_FROM_BOTTOM
      ) {
        this.increaseLimit();
      }
    });
  },
  destroyed() {
    if (this.scrollEvent) {
      window.removeEventListener(this.scrollEvent);
    }
  },
  methods: {
    submitForm() {
      if (this.rssFeedForm.feedUrl) {
        this.feedUrl = this.rssFeedForm.feedUrl;
        // Reset limit
        this.limit = 5;
      }
    },
    increaseLimit(loadMore = 5) {
      this.limit += loadMore;
    },
    loadExample(evt) {
      evt.preventDefault();
      this.rssFeedForm.feedUrl = evt.toElement.href;
      this.feedUrl = evt.toElement.href;
    },
    getName(evt) {
      var url = this.urls.filter(url => (url.path === evt.path));
      if ( url.length == 0 ) {
        this.urls.push(evt);
      }
    }
  },
  watch: {
    getFeedUrl() {
      return this.feedUrl;
    }
  }
};
</script>

<style>
body {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.el-header {
  padding-top: 10px;
  height: 50px !important;
  border-bottom: 1px solid #eee;
}

.nav-left {
  display: flex;
  align-items: center;
  font-weight: bold;
  color: #333;
  text-decoration: none;
}

.el-button,
.el-button:focus {
  background-color: #ff641b;
  border-color: #ff641b;
}
.el-button:hover {
  background-color: #ff641b;
  border-color: #ff641b;
  opacity: 0.9;
}
.scroll-down {
  display: flex;
  text-align: center;
  flex-direction: column;
  margin-top: -10px;
}

.el-icon-arrow-down {
  font-size: 35px;
}

.examples h3 {
  color: #2c3e50;
  font-weight: normal;
  margin: 0;
}

.examples ul {
  margin: 10px;
}

.examples li {
  margin-bottom: 5px;
}

.examples a {
  color: #ff641b;
  margin-bottom: 5px;
}

a {
  color: #ff641b;
}

.powered-rss-app {
  margin-top: 20px;
  font-size: 13px;
  /* color: #ff641b; */
}

.el-header h1 {
  margin: 0;
  font-weight: 400;
}

.center {
  text-align: center;
}

.nav-left img {
  margin-right: 5px;
}
.el-main {
  margin: auto;
  padding-top: 50px;
  max-width: 1300px;
  width: 100%;
  color: #2c3e50;
}

.el-col-12 {
  padding: 0 20px;
}
.el-form-item {
  margin-top: 30px;
}

.el-form--label-top .el-form-item__label {
  font-size: 20px;
  color: #2c3e50;
  padding-bottom: 0;
}

.github-button {
  text-align: right;
}
</style>
