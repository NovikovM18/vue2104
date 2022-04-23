<template>
  <div class="posts">
    <list-item
      v-for="post in posts"
      :key="post.id"
      :post="post"
    />
  </div>

</template>
<script>
import ListItem from "@/components/ListItem";

export default {
  components: { ListItem },
  props: {
    posts: {
      type: Array
    }
  },

}
</script>
<style>
</style>