<template>
  <div class="container mt-5">
    <h1 class="mb-4">Post</h1>
    <div class="mb-4">
      <input type="text" class="form-control mb-2" v-model="title" placeholder="Enter title" />
      <textarea class="form-control mb-2" v-model="content" placeholder="Enter content"></textarea>
      <button class="btn btn-primary" @click="savePost">Save</button>
    </div>
    <div class="card mt-3" v-for="post in posts" :key="post.id">
      <div class="card-body">
        <h5 class="card-title">{{ post.title }}</h5>
        <p class="card-text">{{ post.content }}</p>
        <button class="btn gg btn-warning btn-sm mr-2" @click="editPost(post)">Edit</button>
        <button class="btn gg btn-danger btn-sm" @click="deletePost(post.id)">Delete</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      title: '',
      content: '',
      posts: [],
      editingPost: null,
    };
  },
  created() {
    this.fetchPosts();
  },
  methods: {
    async fetchPosts() {
      try {
        const response = await axios.get('http://localhost:3000/posts');
        this.posts = response.data;
      } catch (error) {
        console.error('Error fetching posts:', error);
      }
    },
    async savePost() {
      if (this.editingPost) {
        await axios.put(`http://localhost:3000/posts/${this.editingPost.id}`, {
          title: this.title,
          content: this.content,
        });
        this.editingPost = null;
      } else {
        await axios.post('http://localhost:3000/posts', {
          title: this.title,
          content: this.content,
        });
      }
      this.title = '';
      this.content = '';
      this.fetchPosts();
    },
    editPost(post) {
      this.title = post.title;
      this.content = post.content;
      this.editingPost = post;
    },
    async deletePost(id) {
      await axios.delete(`http://localhost:3000/posts/${id}`);
      this.fetchPosts();
    },
  },
};
</script>

<style>
@import 'bootstrap/dist/css/bootstrap.min.css';

.gg{
  margin-left: 10px;
}
</style>
