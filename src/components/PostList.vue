<template>
  <div class="list row">
      <div class="col-md-8">
          <div class="input-group mb-3">
              <input
              type="text"
              class="form-control"
              placeholder="Search by title"
              v-model="title"
              />
              <div class="input-group-append">
                  <button
                  class="btn btn-outline-secondary"
                  type="button"
                  @click="searchTitle"
                  >
                  Search
                  </button>
              </div>
          </div>
        </div>
        <div class="col-md-6">
            <h4>Posts List</h4>
            <ul class="list-group">
                <li
                class="list-group-item"
                :class="{ active: index == currentIndex }"
                v-for="(post, index) in posts"
                :key="index"
                @click="setActivePost(post, index)"
                >
                {{ post.title }}
                </li>
            </ul>
            
        </div>
        <div class="col-md-6">
            <div v-if="currentPost.id">
                <h4>Post</h4>
            <div>
          <label><strong>Title:</strong></label> {{ currentPost.title }}
        </div>
        <div>
          <label><strong>Date:</strong></label>
          {{ currentPost.date }}
        </div>
        <div>
          <label><strong>Content:</strong></label>
          {{ currentPost.Content }}
        </div>
        <div>
          <label><strong>Link:</strong></label>
          {{ currentPost.Link }}
        </div>
        <div>
          <label><strong>Image:</strong></label>
          {{ currentPost.Image }}
        </div>
        <img :src="currentPost.Image">

        <a
        class="m-3 btn btn-sm btn-danger"
        :href="'/posts/' + currentPost.id"
        >
        Edit
        </a>
            </div>
            <div v-else>
                <br />
                <p>Please Click on a Post...</p>
            </div>
        </div>
  </div>
</template>

<script >
import PostDataService from "../services/PostDataService";

export default {
    name: "posts-list",
    data() {
        return {
            posts: [],
            currentPost: {},
            currentIndex: -1,
            title: ""
        };
    },
    methods: {
        retrievePosts() {
            PostDataService.getAll()
            .then(response => {
                this.posts = response.data;
                console.log(response.data);
            })
            .catch(e => {
                console.log(e);
            });
        },

        refreshList() {
            this.retrievePosts();
            this.currentPost = null;
            this.currentIndex = -1;
        },

        setActivePost(post, index) {
            this.currentPost = post;
            this.currentIndex = index;
        },

        searchTitle() {
            PostDataService.findByTitle(this.title)
            .then(response => {
                this.posts = response.data;
                console.log(response.data);
            })
            .catch(e => {
                console.log(e);
            });
        }
    },
    mounted() {
        this.retrievePosts();
    }
};
</script>

<style>

.list {
  text-align: left;
  max-width: 750px;
  margin: auto;
}
img {
    max-width: 155px;
    max-height: 155px;
}

</style>