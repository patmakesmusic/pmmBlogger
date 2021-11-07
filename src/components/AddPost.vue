<template>
  <div class="submit-form">
      <div v-if="!submitted">
          <div class="form-group">
              <label for="title">Title</label>
              <input
              type="text"
              class="form-control"
              id="title"
              required
              v-model="post.title"
              name="title"
              />
          </div>

          <div class="form-group">
              <label for="date"> Date</label>
              <input
              class="form-control"
              id="date"
              required
              v-model="post.date"
              name="date"
              />
          </div>

          <div class="form-group">
              <label for="Content"> Content</label>
              <textarea
              class="form-control"
              id="Content"
              required
              v-model="post.Content"
              name="Content"
              />
          </div>

          <div class="form-group">
              <label for="Link"> Link</label>
              <input
              class="form-control"
              id="Link"
              required
              v-model="post.Link"
              name="link"
              />
          </div>

          <div class="form-group">
              <label for="Image"> Image</label>
              <input
              class="form-control"
              id="Image"
              required
              v-model="post.Image"
              name="Image"
              />
          </div>

          <button @click="savePost" class="btn btn-success">Submit</button>
      </div>

      <div v-else>
          <h4>You submitted successfully!</h4>
          <button class="btn btn-success" @click="newPost">Add</button>
      </div>
  </div>
</template>

<script>
import PostDataService from "../services/PostDataService";

export default {
    name: "add-post",
    data() {
        return {
            post: {
                id: null,
                title: "",
                description: "",
                Content: "",
                Link: "",
                Image: "",  
              },
            submitted: false
        };
    },
    methods: {
        savePost() {
            var data = {
                title: this.post.title,
                Content: this.post.Content,
                Link: this.post.Link,
                Image: this.post.Image
            };

            PostDataService.create(data)
            .then(response => {
                this.post.id = response.data.id;
                console.log(response.data);
                this.submitted = true;
            })
            .catch(e => {
                console.log(e);
            });
        },

        newPost() {
            this.submitted = false;
            this.post = {};
        }
    }
};
</script>

<style>
.submit-form {
  max-width: 300px;
  margin: auto;
}
</style>