<template>
  <div v-if="currentPost" class="edit-form">
      <h4>Post</h4>
      <form>
          <div class="form-group">
              <label for="title">Title</label>
              <input type="text" class="form-control" id="title"
              v-model="currentPost.title"
              />
          </div>
          <div class="form-group">
              <label for="date">Date</label>
              <input type="text" class="form-control" id="date"
              v-model="currentPost.date"
              />
          </div>
          <div class="form-group">
              <label for="Content">Content</label>
              <input type="text" class="form-control" id="Content"
              v-model="currentPost.Content"
              />
          </div>
          <div class="form-group">
              <label for="Link">Link</label>
              <input type="text" class="form-control" id="Link"
              v-model="currentPost.Link"
              />
          </div>
          <div class="form-group">
              <label for="Image">Image</label>
              <input type="text" class="form-control" id="Image"
              v-model="currentPost.Image"
              />
          </div>
          <img :src="currentPost.Image">
      </form>

      <button type="submit" class="m-3 btn btn-sm btn-danger"
      @click="deletePost"
      >
      Delete
      </button>

      <button type="submit" class="m-3 btn btn-sm btn-danger"
      @click="updatePost"
      >
      Update
      </button>
      <p>{{ message }}</p>
  </div>

  <div v-else>
      <br />
      <p>Please click on a Post...</p>
  </div>
</template>

<script>
import PostDataService from "../services/PostDataService";

export default {
    name: "posts",
    data() {
        return {
            currentPost: null,
            message: ''
        };
    },
    methods: {
        getPost(id) {
            PostDataService.get(id)
            .then(response => {
                this.currentPost = response.data;
                console.log(response.data);
            })
            .catch(e => {
                console.log(e);
            });
        },
        updatePost() {
            PostDataService.update(this.currentPost.id, this.currentPost)
            .then(response => {
                console.log(response.data);
                this.message = 'The post was updated successfully!';
            })
            .catch(e => {
                console.log(e);
            });
        },
        deletePost() {
            PostDataService.delete(this.currentPost.id)
            .then(response => {
                console.log(response.data);
                this.$router.push({ name: "posts" });
            })
            .catch(e => {
                console.log(e);
            });
        }
    },
    mounted() {
        this.message = '';
        this.getPost(this.$route.params.id);
    }
};
</script>

<style>
.edit-form {
  max-width: 600px;
  margin: auto;
}
img {
    max-width: 155px;
    max-height: 155px;
}

#Content {
    min-width: 300px;
    min-height: 300px;
}

#Link {
    min-width: 500px;
}
#Image {
    min-width: 500px;
}
</style>