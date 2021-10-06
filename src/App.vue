<template>
  <div id="app">
    <div class="container">
      <div class="button-wrapper">
        <button class="btn" @click="searchUnsplash('Autumn')">Autumn</button>
        <button class="btn" @click="searchUnsplash('cliff')">Cliff</button>
        <button class="btn" @click="searchUnsplash('ocean')">Ocean</button>
        <button class="btn" @click="searchUnsplash('turkey')">Turkey</button>
      </div>
      <div>
        <button class="btn" @click="prev('turkey','prevpage')">Prevpage</button>
        <button class="btn" @click="next('turkey','nextpage')">Nextpage</button>
      </div>
      <stack
        :column-min-width="300"
        :gutter-width="15"
        :gutter-height="15"
        monitor-images-loaded
      >
        <stack-item
          v-for="(image, i) in images"
          :key="i"
          style="transition: transform 300ms"
        >
          <img :src="image.urls.small" :alt="image.alt_description" />
        </stack-item>
      </stack>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { Stack, StackItem } from "vue-stack-grid";
export default {
  name: "app",
  components: {
    Stack,
    StackItem,
  },
  data: () => ({
    images: [],
    client_id: "BJESsq7e9HMRiyuzRD5aKCxwMY1Xf3SZNJHQPhgauRM",
    Secret: 'SVDmjikADqa8CZhaJqkH4aV2L1ZDwpG2zpFCdKK98fM',
    per_page: 5,
    page:1,
    nextpage: null
  }),
  methods: {
    searchUnsplash(topic) {
      this.images = [];
      axios
        .get(
          `https://api.unsplash.com/search/photos?client_id=${this.client_id}&query=${topic}&per_page=${this.per_page}&page=${this.page}`,
          {
            headers: {
              Authorization: this.Secret,
              "Accept-Version": "v1",
            },
          }
        )
        .then((response) => {
          this.images = response.data.results;
          console.log(response.data.total_pages);
          this.nextpage = this.page+1;
          console.log(this.nextpage);
        })
        .catch(() => {
          this.images = [];
        });
    },

    next(topic, nextpage){
      console.log(nextpage);
      axios
        .get(
          `https://api.unsplash.com/search/photos?client_id=${this.client_id}&query=${topic}&per_page=${this.per_page}&page=${this.nextpage}`,
          {
            headers: {
              Authorization: this.Secret,
              "Accept-Version": "v1",
            },
          }
        )
        .then((response) => {
          this.images = response.data.results;
          console.log(response.data.total_pages);
          this.nextpage = this.nextpage +1;
        })
        .catch(() => {
          this.images = [];
        });
    }
  },
};
</script>
<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  width: 80vw;
  margin: 0 auto;
}
.button-wrapper {
  display: flex;
  justify-content: center;
  margin-bottom: 25px;
}
.btn {
  font-size: 18px;
  background-color: #42b983;
  color: white;
  padding: 10px 20px;
}
.btn:not(:last-child) {
  margin-right: 10px;
}
img {
  width: 100%;
  height: auto;
  border-radius: 12px;
}
</style>
