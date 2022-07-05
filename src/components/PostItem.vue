<template>
  <li>
    <span v-bind:class="{done: post.completed}">
      <input type="checkbox"
             v-on:change="post.completed = !post.completed">
      <strong>{{index + 1}}</strong>
      {{post.title | uppercase}}
    </span>
    <button class="rm"
            v-on:click="$emit('remove-post', post.id)"
    >&times;</button>
  </li>
</template>

<script>
export default {
  props: {
    post: {
      type: Object,
      required: true
    },
    index: Number
  },
  filters: {
    uppercase(value) {
      return value.toUpperCase()
    }
  }
}
</script>

<style scoped>
  li {
    border: 1px solid #ccc;
    display: flex;
    justify-content: space-between;
    padding: .5rem 2rem;
    margin-bottom: 1rem;
    min-height: 70px;
  }

  .rm {
    background: red;
    color: #fff;
    border-radius: 50%;
    font-weight: bold;
    max-height: 25px;
  }

  input {
    margin-right: 1rem;
  }

  .done {
    text-decoration: line-through;
  }
</style>