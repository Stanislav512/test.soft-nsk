<template>
  <div>
    <AddPost
        @add-post="addPost"
    />
    <select v-model="filter">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="not-completed">Not Completed</option>
    </select>
    <hr>
    <Loader v-if="loading" />
    <PostList
        v-else-if="filteredPosts.length"
        v-bind:posts="filteredPosts"
        @remove-post="removePost"
    />
    <p v-else>No posts!</p>
  </div>
</template>

<script>
import PostList from '@/components/PostList'
import AddPost from '@/components/AddPost'
import Loader from '@/components/Loader'
export default {
  name: 'app',
  data() {
    return {
      posts: [
        {id: 1, title: 'Post 1', text:'text 1', completed: false, rating: 1},
        {id: 2, title: 'Post 2', text:'text 2', completed: false, rating: 5},
        {id: 3, title: 'Post 3', text:'text 3', completed: false, rating: 10}
      ],
      loading: true,
      filter: 'all'
    }
  },
  mounted() {
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=15')
      .then(response => response.json())
      .then(json => {
        setTimeout(() => {
          this.posts = json
          this.loading = false
        }, 1000)

      })
  },

  // watch: {
  //   filter(value) {
  //     console.log(value)
  //   }
  // },
  computed: {
    filteredPosts() {
      if (this.filter === 'all') {
        return this.posts
      }

      if (this.filter === 'completed') {
        return this.posts.filter(t => t.completed)
      }

      if (this.filter === 'not-completed') {
        return this.posts.filter(t => !t.completed)
      }
    }
  },
  methods: {
    removePost(id) {
      this.posts = this.posts.filter(t => t.id !== id)
    },
    addPost(post) {
      this.posts.push(post)
    }
  },
  components: {
    PostList, AddPost, Loader
  }
}
</script>